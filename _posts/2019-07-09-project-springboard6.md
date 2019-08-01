---
title: "[스프링 MVC 게시판] 6. 게시판 목록"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 게시판 목록

  게시판 목록을 구현하기 위해서 View 파일의 이름을 boardList.jsp로 생성했습니다.

  게시판 목록은 CRUD에서 Read에 해당하는 부분입니다. 게시판 상세 보기와 게시판 목록(전체 리스트)은 둘다 READ에 해당하는 부분인데
  차이는 상세 보기는 게시물 하나의 번호에 해당하는 DTO를 가져오는 것이고, 게시판 목록은 List를 가져온다는 차이가 있습니다.

### VO(DTO) 생성

  BoardDTO(VO)를 생성하겠습니다.

  ```java
package com.mayeye.board.dto;

import org.hibernate.validator.constraints.Length;
import org.hibernate.validator.constraints.NotEmpty;

public class BoardDTO {

	private int num; // 게시물 번호
	@Length(min=2, max=20, message="제목은 2자 이상, 20자 이하 입력하세요.")
	private String title; // 제목
	private int count; // 조회수
	private String name; // 작성자
	private String date; // 날짜
	@NotEmpty(message="내용을 입력하세요.")
	private String contents; // 내용
	private String id;
	private String del_chk; // 삭제 여부

	public int getNum() {
		return num;
	}
	public void setNum(int num) {
		this.num = num;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public int getCount() {
		return count;
	}
	public void setCount(int count) {
		this.count = count;
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getDate() {
		return date;
	}
	public void setDate(String date) {
		this.date = date;
	}
	public String getContents() {
		return contents;
	}
	public void setContents(String contents) {
		this.contents = contents;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getDel_chk() {
		return del_chk;
	}
	public void setDel_chk(String del_chk) {
		this.del_chk = del_chk;
	}
	@Override
	public String toString() {
		return "BoardDTO [num=" + num + ", title=" + title + ", count=" + count + ", name=" + name + ", date=" + date
				+ ", contents=" + contents + ", id=" + id + ", del_chk=" + del_chk + "]";
	}
}
  ```

### Mapper.xml 작성

  boardMapper.xml을 작성하겠습니다.

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
</mapper>
  ```

  [CDATA](https://baekjungho.github.io/database-cdata/)는 링크를 통해 공부할 수 있습니다.

  resultType은 해당 쿼리문이 반환하는 타입을 말합니다. select 태그의 id속성은
  namespace 속성명과 함께 `namespace.id`로 DAOImpl에서 사용할 수 있습니다.

  아래 DAOImpl 코드를 같이 보시면 이해가 될 것입니다.

  __되도록이면 쿼리태그의 id속성명을 추상메소드명과 일치시키는게 좋습니다.__

  [일치 시키는 이유](https://baekjungho.github.io/spring-mybatisroot/)는 해당 링크를 통해 공부하실 수 있습니다.

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();

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

}
  ```

  SqlSessionTemplate을 @Autowired로 의존성 주입을 통해 사용합니다.

  sqlSessionTemplate에 있는 내장 메소드인 selectList 괄호안의 boardDAO.list는
  `namespace.id`를 뜻합니다.

  Mapper.xml이 하나일 경우 namespace를 생략해도 되지만, 2개이상 존재할 경우에는 꼭 namespace명을 작성하여
  위와같은 방식으로 사용해야 합니다.

### Service Interface 작성

  ```java
public interface BoardService {

	public List<BoardDTO> list();

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

}
  ```

### Controlelr 작성

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

}
  ```

  로그인한 정보를 세션에 담고, 세션에 id값이 null일 경우 redirect.jsp로 이동하여 경고창을 띄웁니다.
  id값이 있을 경우에는 model에 객체를 담아서 boardList.jsp로 반환합니다.

### redirect.jsp

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
	<script type="text/javascript">
		var message = '${msg}';
		var returnUrl = '${url}';
		alert(message);
		document.location.href = returnUrl;
	</script>
</body>
</html>
```

### boardList.jsp

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
	<span>${user.name}님 환영합니다.</span>
	<a href="<c:url value="/logout" />">로그아웃</a>
	<table border="1">
		<tr>
			<th>번호</th>
			<th>제목</th>
			<th>작성자</th>
			<th>작성일</th>
			<th>조회수</th>
		</tr>
	<c:forEach var="board" items="${boardList}" varStatus="loop">
		<tr>
			<td>${board.num}</td>
			<td><a href="<c:url value="/boardRead/${board.num}" />"> ${board.title}</a></td>
			<td>${board.name}</td>
			<td>${board.date}</td>
			<td>${board.count}</td>
		</tr>
	</c:forEach>
	</table>
	<a href="<c:url value="/boardWrite" />">글쓰기</a>
	<c:if test="${msg ne null}">
		<p style="color:#f00;">${msg}</p>
	</c:if>
</body>
</html>
```

  제목을 누르면 해당글의 상세를 볼 수 있습니다. 경로 뒤에 ${board.num}이라는 게시글의 번호를 주어
  컨트롤러에서는 @PathVariable 어노테이션으로 받을 수 있습니다.
