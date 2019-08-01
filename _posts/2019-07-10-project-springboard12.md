---
title: "[스프링 MVC 게시판] 12. 페이징 구현"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 페이징 구현

  한 페이지에 보여줄 게시물이 10개일 경우, 총 게시물이 88개이면, 마지막 페이지의 번호는 9이여야 합니다.
  화면에 보여줄 페이지 버튼의 수가 10개 이상이라면 다음 버튼을 이용해 다음 페이지를 이용할 수 있어야 합니다. 또한
  시작 페이지의 번호가 1이 아니라면 이전 버튼을 이용해 이전 페이지를 이용할 수 있어야 합니다.

### 페이징 원칙

  - 페이징 처리는 반드시 GET 방식만을 이용해야 합니다.

  - 게시글 목록 페이지의 하단에 페이지들의 번호를 보여주고 원하는 번호를 선택하면 해당 페이지로 이동해서 목록을 보여줘야 합니다.

  - 페이징은 반드시 필요한 페이지 번호만 출력해야 합니다. 만약 페이지당 10개의 게시글을 출력하는데 전체 게시글의 수가 42개라면 페이지의 번호는 5페이지까지여야 합니다.

  - 이전과 다음 버튼이 존재해야 합니다. 게시글이 1000개인데 1페이지당 10개의 게시글을 볼 수 있게 했다면 페이징 버튼은 100개가 필요합니다. 그런데 이전과 다음 버튼이 존재하지 않는다면 100개의 버튼을 한번에 보여줘야 합니다. 그렇기 때문에 하단 페이지에 보여줄 버튼의 갯수를 정하고 더 많은 데이터가 있다면 이전과 다음 버튼으로 표시해줘야 합니다.

  - 게시글을 조회하거나 수정, 삭제를 하고 난 뒤, 다시 원래의 목록 페이지로 이동해야 합니다. 예를 들면 게시판에 5페이지에 있는 어떠한 게시물을 조회하거나 수정, 삭제를 했다고 한다면, '목록으로'버튼을 이용해 목록으로 돌아갈 땐 5페이지로 이동해야 합니다.

### 페이징 로직

  일단 게시판의 페이징은 크개 두개의 로직으로 나누어 집니다. 하나는 `Criteria` 또 하나는 `PageMaker` 입니다. 참고로 PageMaker는 Criteria를 기반합니다.

### Criteria(VO) 작성

  > Criteria : 특정 페이지의 게시판을 조회하기 위한 도우미 클래스

  ```java
package com.mayeye.board.dto;

/* Criteria
 * 특정 페이지 조회를 위한 클래스
 */
public class Criteria {

	/* 현재 페이지 번호 */
	private int page;

	/* 한 페이지당 보여질 게시글의 갯수 */
	private int perPageNum;

	/* 특정 페이지의 게시글 시작번호, 게시글 시작 행 번호 */
	/* 현재 페이지의 게시글 시작 번호 = (현재 페이지 번호 - 1) * 페이지당 보여줄 게시글 갯수 */
	public int getPageStart() {
		return (this.page-1)*perPageNum;
	}

  /* 생성자로 페이지 번호와, 페이지당 보여줄 게시글의 갯수 초기화 */
	public Criteria() {
		this.page = 1;
		this.perPageNum = 10;
	}

	public int getPage() {
		return page;
	}

	public void setPage(int page) {
		if(page <= 0) {
			this.page = 1;
		} else {
			this.page = page;
		}
	}

	public int getPerPageNum() {
		return perPageNum;
	}

	public void setPerPageNum(int pageCount) {
		int cnt = this.perPageNum;
		if(pageCount != cnt) {
			this.perPageNum = cnt;
		} else {
			this.perPageNum = pageCount;
		}
	}
}
  ```

### PageMaker(VO) 작성

  > PageMaker : 페이지를 생성하고, 계산을 하는 클래스

  ```java
package com.mayeye.board.dto;

import java.net.URLEncoder;

import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

public class PageMaker {

	private Criteria cri;
	private int totalCount;
	private int startPage;
	private int endPage;
	private boolean prev;
	private boolean next;
	private int displayPageNum = 5;

	public Criteria getCri() {
		return cri;
	}

	public void setCri(Criteria cri) {
		this.cri = cri;
	}

	public int getTotalCount() {
    return totalCount;
  }

	/* 총 게시글 수를 셋팅할때 calcData() 메서드를 호출하여 페이징 관련 버튼 계산 */
  public void setTotalCount(int totalCount) {
    this.totalCount = totalCount;
    calcData();
  }

  /* 페이징의 버튼들을 생성하는 계산식을 만들었다. 끝 페이지 번호, 시작 페이지 번호, 이전, 다음 버튼들을 구함
   * @cri.getPage() 현재 페이지 번호
   * @cri.getPerPageNum() : 한 페이지당 보여줄 게시글의 갯수
   * 끝 페이지 번호 = (현재 페이지 번호 / 화면에 보여질 페이지 번호의 갯수) * 화면에 보여질 페이지 번호의 갯수
   * 마지막 페이지 번호 = 총 게시글 수 / 한 페이지당 보여줄 게시글의 갯수
   */
  private void calcData() {
    endPage = (int) (Math.ceil(cri.getPage() / (double) displayPageNum) * displayPageNum);
    int tempEndPage = (int) (Math.ceil(totalCount / (double) cri.getPerPageNum()));
    if (endPage > tempEndPage) {
      endPage = tempEndPage;
    }

    startPage = (endPage - displayPageNum) + 1;
    if(startPage <= 0) startPage = 1 ;

    prev = startPage == 1 ? false : true;
    next = endPage * cri.getPerPageNum() >= totalCount ? false : true;
  }

  public int getStartPage() {
      return startPage;
  }

  public void setStartPage(int startPage) {
      this.startPage = startPage;
  }

  public int getEndPage() {
      return endPage;
  }

  public void setEndPage(int endPage) {
      this.endPage = endPage;
  }

  public boolean isPrev() {
      return prev;
  }

  public void setPrev(boolean prev) {
      this.prev = prev;
  }

  public boolean isNext() {
      return next;
  }

  public void setNext(boolean next) {
      this.next = next;
  }

  public int getDisplayPageNum() {
      return displayPageNum;
  }

  public void setDisplayPageNum(int displayPageNum) {
      this.displayPageNum = displayPageNum;
  }

}
  ```

### Mapper.xml 작성

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
<mapper namespace="boardDAO">
<!-- 페이지 적용 -->
<select id="pageList" resultType="hashmap" parameterType="hashmap">
  <![CDATA[
      SELECT b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id
      FROM b_board as b
      INNER join b_users as u
      ON (b.id = u.id)
      WHERE del_chk = 'N'
      ORDER BY num DESC
      LIMIT #{pageStart}, #{perPageNum}
  ]]>
</select>

<!-- 총 게시글 수 -->
<select id="countBoardList" resultType="Integer">
  <![CDATA[
      SELECT count(*)
      FROM b_board
      WHERE del_chk = 'N'
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

<!-- 조회수 증가 -->
<update id="updateCount" parameterType="int">
  UPDATE b_board SET
  count = count+1
  WHERE num = #{num}
</update>

<!-- 삭제 : 상태값 -->
<delete id="delete" parameterType="boardDTO">
<![CDATA[
  UPDATE b_board SET del_chk = 'Y'
  WHERE num = #{num}
]]>
</delete>

</mapper>
  ```

  게시판 목록을 구하기 위한 쿼리를 없애고, 페이지를 적용한 쿼리로 변경 했습니다.
  MySQL에서 limit을 사용하면 페이지 처리가 쉬워집니다. 또한 총 게시글 수를 구하는 쿼리가 추가 되었습니다.

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public void delete(BoardDTO boardDTO);
	public int update(BoardDTO boardDTO);
	public void insert(BoardDTO boardDTO);
	public BoardDTO select(int num);
	public int updateReadCount(int num);
	public List<Map<String, Object>> pageList(Criteria cri);
	public int countBoardList();

}
  ```

### DAOImpl 작성

  ```java
@Repository
public class BoardDAOImpl implements BoardDAO {

	private static final Logger Logger = LoggerFactory.getLogger(BoardController.class);

	@Autowired
	private SqlSessionTemplate sqlSessionTemplate;

	@Override
	public void delete(BoardDTO boardDTO) {
		sqlSessionTemplate.delete("boardDAO.delete", boardDTO);
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

	@Override
	public int updateReadCount(int num) {
		return sqlSessionTemplate.update("boardDAO.updateCount", num);
	}

	// Criteria 객체에 담아서 SQL 매핑에 보낼 파라미터
	// 특정 페이지 게시글의 행(pageStart)과 페이지당 보여줄 게시글의 갯수(perPageNum)
	@Override
	public List<Map<String, Object>> pageList(Criteria cri) {
		return sqlSessionTemplate.selectList("boardDAO.pageList", cri);
	}

	@Override
	public int countBoardList() {
		return sqlSessionTemplate.selectOne("boardDAO.countBoardList");
	}

}
  ```

### Service Interface 작성

  ```java
public interface BoardService {

	public void delete(BoardDTO boardDTO);
	public int edit(BoardDTO boardDTO);
	public void write(BoardDTO boardDTO);
	public BoardDTO read(int num);
	List<Map<String, Object>> pageList(Criteria cri);
	public int countBoardList();

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
	public void delete(BoardDTO boardDTO) {
		boardDAO.delete(boardDTO);
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

	@Override
	public List<Map<String, Object>> pageList(Criteria cri) {
		return boardDAO.pageList(cri);
	}

	@Override
	public int countBoardList() {
		return boardDAO.countBoardList();
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

	// 게시글 수정권한 판단
	@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.GET)
	public String boardEdit(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
		if(session.getAttribute("id").equals(boardService.read(num).getId())) {
			 model.addAttribute("boardDTO", boardService.read(num));
			return "boardEdit";
		} else {
			rttr.addFlashAttribute("msg", "수정 권한이 없습니다");
			return "redirect:/boardSearchList";
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

	// 게시글 삭제
	@RequestMapping(value="/boardDelete/{num}", method=RequestMethod.GET)
	public String boardDelete(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
		if(session.getAttribute("id").equals(boardService.read(num).getId())) {
			boardService.delete(boardService.read(num));
			return "redirect:/boardSearchList";
		} else {
			rttr.addFlashAttribute("msg", "삭제 권한이 없습니다.");
			return "redirect:/boardSearchList";
		}
	}
}
  ```

### boardPageList.jsp

  boardList.jsp는 그냥 두고 새로운 View파일을 만들었습니다.

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
		<c:forEach var="board" items="${list}" varStatus="loop">
		<tr>
			<td>${board.num}</td>
			<td><a href="<c:url value="/boardRead/${board.num}" />"> ${board.title}</a></td>
			<td>${board.name}</td>
			<td>${board.date}</td>
			<td>${board.count}</td>
		</tr>
		</c:forEach>
	</table>
	<table>
		<tr>
		    <c:if test="${pageMaker.prev}">
		    <td>
		        <a href='<c:url value="/boardPageList?page=${pageMaker.startPage-1}"/>'>&laquo;</a>
		    </td>
		    </c:if>
		    <c:forEach begin="${pageMaker.startPage}" end="${pageMaker.endPage}" var="idx">
		    <td>
		        <a href='<c:url value="/boardPageList?page=${idx}"/>'>${idx}</a>
		    </td>
		    </c:forEach>
		    <c:if test="${pageMaker.next && pageMaker.endPage > 0}">
		    <td>
		        <a href='<c:url value="/boardPageList?page=${pageMaker.endPage+1}"/>'>&raquo;</a>
		    </td>
		    </c:if>
		</tr>
	</table>
	<a href="<c:url value="/boardWrite" />">글쓰기</a>
	<c:if test="${msg ne null}">
		<p style="color:#f00;">${msg}</p>
	</c:if>
</body>
</html>
  ```

  그리고 기존에 boardList로 이동하게끔 View에서 매핑한 것을 boardPageList로 수정하면 됩니다.
