---
title: "[스프링 MVC 게시판] 7. 게시글 상세보기"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 게시글 상세 보기

  게시글 상세 보기도 CRUD의 READ 부분에 해당합니다.

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
</mapper>
  ```

  해당 게시글의 parameterType이 int인 것을 보니 추상메소드 타입이 int형일 것을 추측할 수 있습니다.

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();
	public BoardDTO select(int num);

}
  ```

  게시글 번호로 조회하기 때문에, 번호를 파라미터로 받기 위해 int num을 줬습니다.

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
	public BoardDTO read(int num);

}
  ```

  DAO Inerface의 추상메소드와 Service Interface의 추상메소드 이름을 일치 시켜도 되고 혹은, 기능에 맞게 자신이
  알아 볼 수 있도록 수정해도 상관 없습니다. 어느 쪽이든 개발자 스타일에 따라 다릅니다.

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

}
  ```

  boardList.jsp에서 제목을 클릭했을 경우 /boardRead/{num} 컨트롤러를 타게 만들었으므로, @RequestMapping도 같게 해야합니다.
  또한 @RequestMapping에서 템플릿 변수명을 사용하여 num값을 받아왔으므로, 파라미터에 @PathVariable를 사용하여 @RequestMapping에서 사용한 템플릿 변수명과 일치시켜줍니다.

### boardRead.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>MayEye BAEKJH Board</title>
</head>
<body>
	<table border="1">
		<tr>
			<th>제목</th>
			<td>${boardDTO.title}</td>
		</tr>
		<tr>
			<th>내용</th>
			<td>${boardDTO.contents}</td>
		</tr>
		<tr>
			<th>작성일</th>
			<td>${boardDTO.date}</td>
		</tr>
		<tr>
			<th>조회수</th>
			<td>${boardDTO.count}</td>
		</tr>
	</table>
	<div>
		<a href="<c:url value="/boardEdit/${boardDTO.num}" />">수정</a>
		<a href="<c:url value="/boardDelete/${boardDTO.num}" />">삭제</a>
		<a href="<c:url value="/boardList" />">목록</a>
	</div>
</body>
</html>
  ```
