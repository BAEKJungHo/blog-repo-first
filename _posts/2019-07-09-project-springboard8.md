---
title: "[스프링 MVC 게시판] 8. 게시글 작성"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 게시글 작성

  게시글 작성은 CRUD의 CREATE에 해당합니다. 따라서 Mapper.xml에서는 insert 쿼리가 필요합니다.

### VO(DTO) 생성

  boardDTO이므로 6장의 게시판 목록 부분의 DTO랑 같습니다.

### Mapper.xml 작성

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
<mapper namespace="boardDAO">
	<!-- 게시판 목록 -->
	<select id="list" resultType="boardDTO">
	<![CDATA[
		SELECT b.num, b.title, u.name, b.date, b.count, b.id
		FROM b_board as b
		INNER join b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		ORDER BY num DESC
	]]>
	</select>

	<!-- 해당 게시글 보기 -->
	<select id="select" parameterType="int" resultType="boardDTO">
		SELECT *
		FROM b_board
		WHERE num = #{num}
	</select>

	<!-- 삽입 -->
	<insert id="insert" parameterType="boardDTO">
		INSERET INTO b_board (title, contents, id) VALUES(#{title},#{contents},#{id})
	</insert>
</mapper>
  ```

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();
	public void insert(BoardDTO boardDTO);
	public BoardDTO select(int num);

}
  ```

### DAOImpl 작성

  ```java
@Repository
public class BoardDAOImpl implements BoardDAO {

	@Autowired
	private SqlSessionTemplate sqlSessionTemplate;

	@Override
	public List<BoardDTO> list() {
		return sqlSessionTemplate.selectList("boardDAO.list");
	}

	@Override
	public void insert(BoardDTO boardDTO) {
		sqlSessionTemplate.insert("boardDAO.insert", boardDTO);
	}

	@Override
	public BoardDTO select(int num) {
		BoardDTO dto = (BoardDTO)sqlSessionTemplate.selectOne("boardDAO.select", num);
		return dto;
	}

}
  ```

### Service Interface 작성

  ```java
public interface BoardService {

	public List<BoardDTO> list();
	public void write(BoardDTO boardDTO);
	public BoardDTO read(int num);

}
  ```

### ServiceImpl 작성

  ```java
  @Service
  public class BoardServiceImpl implements BoardService {

  	@Autowired
  	private BoardDAO boardDAO;

  	@Override
  	public List<BoardDTO> list() {
  		return boardDAO.list();
  	}

  	@Override
  	public void write(BoardDTO boardDTO) {
  		boardDAO.insert(boardDTO);
  	}

  	@Override
  	public BoardDTO read(int num) {
  		boardDAO.updateReadCount(num);
  		return boardDAO.select(num);
  	}

  }
  ```

### Controller 작성

  ```java
@Controller
public class BoardController {

	private static final Logger Logger = LoggerFactory.getLogger(BoardController.class);

	@Autowired
	private BoardService boardService;

	// 게시판 목록
	@RequestMapping(value="/boardList")
	public String boardList(Model model, HttpSession session) {
		if(session.getAttribute("id") == null) {
			model.addAttribute("msg", "잘못된 접근입니다!!");
			model.addAttribute("url", "login");
			return "redirect";
		} else {
			model.addAttribute("boardList", boardService.list());
			model.addAttribute("user", session.getAttribute("user"));
			return "boardList";
		}
	}

	// 게시글 상세 내역
	@RequestMapping(value="/boardRead/{num}")
	public String boardRead(Model model, @PathVariable int num) {
		model.addAttribute("boardDTO", boardService.read(num));
		return "boardRead";
	}

	// a태그 혹은 주소창 입력시 들어오는 곳
	// 바인딩 에러처리를 위해 Model 객체와 model.addAttribute() 추가
	@RequestMapping(value="/boardWrite", method=RequestMethod.GET)
	public String boardWrite(Model model) {
		model.addAttribute("boardDTO", new BoardDTO());
		return "boardWrite";
	}

	// 게시글 작성
	// Hibernate-validator까지 처리한 코드
	@RequestMapping(value="/boardWrite", method=RequestMethod.POST)
	public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult, HttpSession session) {
		if(bindingResult.hasErrors()) {
			return "boardWrite"; // ViewResolver로 보냄
		} else {
			boardDTO.setId((String)session.getAttribute("id"));
			boardService.write(boardDTO);
			return "redirect:/boardSearchList"; // 새글을 반영하기 위해 컨트롤러로 보냄
		}
	}

}
  ```

  게시글 작성에 대한 처리를 위해 boardWrite라는 이름으로 `메서드 오버로딩`을 이용해서 GET과 POST방식으로 각각 처리 할 수 있게
  만들었습니다.

  boardList에서 글작성을 눌러서 a태그 혹은 주소창으로 들어오게 되면, boardWrite()의 GET 방식을 적용한 메서드로 가게 됩니다.
  아래의 boardWrite.jsp View파일을 보면 `form:form`태그를 사용했는데, boardWrite()의 GET 방식 메서드를 보면
  `model.addAttribute("boardDTO", new BoardDTO());`로 boardDTO 객체를 생성하여 boardDTO라는 Key값으로 JSP로 넘겨주는게 보입니다.

  그 이유는 컨트롤러에서 form:form을 사용한 View쪽으로 __폼(form) 양식을 바인딩 시켜주기 위해__ form:form의 commandName 즉, 커맨드 객체 이름과 동일하게 model.addAttribute()로 커맨드 객체를 생성해서 보내야 합니다.

  반대로 boardWrite.jsp에서 제목과 내용을 작성하고 나서 등록을 누르면 form으로 전송되기 때문에(form은 POST 방식) boardWrite의 POST 방식이 적용된 메서드를 찾아갑니다.

  이 메서드에서는 [Hibernate-validator와 BindingResult](https://baekjungho.github.io/spring-vhibernate/)를 사용하여 검증을 통해 오류가 없을 경우 글을 등록하게 하는 메서드 입니다. DTO 쪽에 보면 어노테이션을 사용한것을 볼 수 있습니다.

### boardWrite.jsp 작성

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!-- form 태그라이브러리 추가 -->
<!-- BindingResult를 사용하기 위해 추가하는 것임  -->
<!-- form 태그 라이브러리를 사용하면 action 속성이 없는데, 브라우저 주소창을 참고해서 스프링이 자동으로 action 정보를 설정해준다. -->
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>MayEye BAEKJH Board</title>
</head>
<body>
	<c:if test="${msg eq null}" >
	<form:form commandName="boardDTO" mehtod="post">
		<table border="1">
			<tr>
				<th><form:label path="title">제목</form:label></th>
				<td>
					<form:input path="title" />
					<form:errors path="title" />
				</td>
			</tr>
			<tr>
				<th><form:label path="contents">내용</form:label></th>
				<td>
					<form:input path="contents" />
					<form:errors path="contents" />
				</td>
			</tr>
		</table>
		<div>
			<input type="submit" value="등록">
			<a href="<c:url value="/boardSearchList" />"> 목록</a>
		</div>
	</form:form>
	</c:if>
</body>
</html>
  ```
