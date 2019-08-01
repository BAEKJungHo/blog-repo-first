---
title: "[스프링 MVC 게시판] 9. 게시글 수정"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 게시글 수정

  게시글 수정은 CRUD의 UPDATE에 해당합니다. 따라서 Mapper.xml에서는 update 쿼리가 필요합니다.

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

	<!-- 수정 -->
	<update id="update" parameterType="boardDTO">
		UPDATE b_board SET title = #{title},
		contents = #{contents}
		WHERE num = #{num}
	</update>

</mapper>
  ```

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();
	public int update(BoardDTO boardDTO);
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
	public int update(BoardDTO boardDTO) {
		return sqlSessionTemplate.update("boardDAO.update", boardDTO);
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

### Servcie Interface 작성

  ```java
public interface BoardService {

	public List<BoardDTO> list();
	public int edit(BoardDTO boardDTO);
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
	public int edit(BoardDTO boardDTO) {
		return boardDAO.update(boardDTO);
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

	@Autowired
	private FilesService filesService;

	/* 객체의 이름이 일치하는 객체 자동주입
	* name 속성명에는 IOC 컨테이너에서 설정한 id 명으로 입력
	* 스프링 설정 파일을 불러올 때 유용하게 쓰임
	* name 속성으로 지정한 uploadPath를 사용 할 수 있음
	*/
	@Resource(name="uploadPath")
	private String uploadPath;

	// 게시판 페이징 + 검색
	@RequestMapping(value="/boardSearchList")
	public ModelAndView list(SearchCriteria cri) {
		Logger.info("!!!!!search!!!!!");

		ModelAndView mav = new ModelAndView();

		PageMaker pageMaker = new PageMaker();
		pageMaker.setCri(cri);
		pageMaker.setTotalCount(boardService.countArticle(cri.getSearchType(), cri.getKeyword()));

		List<BoardDTO> searchList = boardService.searchList(cri);
		int count = boardService.countArticle(cri.getSearchType(), cri.getKeyword());
		Logger.info("searchType     " + cri.getSearchType());
		Logger.info("keyword     " + cri.getKeyword());
		Logger.info("svf     " + String.valueOf(count));
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("searchList", searchList);
		map.put("count", count);
		map.put("searchOption", cri.getSearchType());
		map.put("keyword", cri.getKeyword());
		mav.addObject("pageMaker", pageMaker);
		mav.addObject("map", map);
		return mav;
	}

	// 게시판 페이징
	@RequestMapping(value="/boardPageList")
	public ModelAndView boardPageList(Criteria cri, HttpSession session) {
		ModelAndView mav = null;
		if(session.getAttribute("id") == null) {
			mav = new ModelAndView("redirect");
			mav.addObject("msg", "잘못된 접근 입니다!!");
			mav.addObject("url", "login");
			return mav;
		} else {
			mav = new ModelAndView("/boardPageList");
			PageMaker pageMaker = new PageMaker();
			pageMaker.setCri(cri);
			pageMaker.setTotalCount(boardService.countBoardList());

			List<Map<String, Object>> list = boardService.pageList(cri);

			mav.addObject("list", list);
			mav.addObject("pageMaker", pageMaker);

			return mav;
		}
	}

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

	// 게시글 수정권한 판단
	@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.GET)
	public String boardEdit(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
		if(session.getAttribute("id").equals(boardService.read(num).getId())) {
			 model.addAttribute("boardDTO", boardService.read(num));
			return "boardEdit";
		} else {
			rttr.addFlashAttribute("msg", "수정 권한이 없습니다");
			return "redirect:/boardList";
		}
	}

	// 수정 검증 : BindingResut + Validator
	@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.POST)
	public String boardEdit(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
		if(bindingResult.hasErrors()) return "boardEdit";
		else {
			boardService.edit(boardDTO);
			return "redirect:/boardSearchList";
		}
	}

}
  ```

  게시글 수정의 경우도 boardRead.jsp에서 수정 버튼을 누르면 수정 권한이 있는지 판단하는 GET 방식인 메서드인 boardEdit으로 갑니다.
  boardEdit에서도 마찬가지로 @RequestMapping과 @PathVariable의 템플릿 변수를 사용했으며, 수정 권한을 판단하기 위해서
  세션 ID값과 해당 게시글 번호의 작성자 ID값이 일치 할 경우 boardEdit.jsp로 보내고 아닐경우 RedirectAttributes를 사용하여
  GET방식으로 redirect를 사용하여 boardList 컨트롤러로 보냅니다.

  `redirect:/boardList`처럼 사용할 경우 prefix+return값+suffix를 통해 View를 찾는것이 아니라, boardList의 메서드를 찾아갑니다.

  [RedirectAttributes](https://baekjungho.github.io/spring-redirectattributes/)에서 redirect를 사용하게되면(리다이렉트가 발생하게 되면) 원래 요청은 끊어지고 새로운 HTTP GET요청이 생깁니다. 따라서 리다이렉트로 모델에 데이터를 담아서 보내는것은 의미가 없습니다.

  하지만 URL뒤에 데이터를 붙이지 않고도 RedirectAttributes의 addFlashAttribute() 메서드를 사용하면, 데이터를 전달 할 수 있습니다. 헤더에 파라미터를 붙이지 않기 때문에 URL에 노출되지 않습니다. 즉, RedirectAttributes의 장점은 GET 방식을 취하면서도, URL에 데이터를 노출시키지 않고도 값을 전달 할 수 있는 장점이 있습니다. header가 아닌 session을 통해 전달하는 방식을 가집니다.

### boardEdit.jsp

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
<!-- 변경 코드 -->
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
</body>
</html>
  ```
