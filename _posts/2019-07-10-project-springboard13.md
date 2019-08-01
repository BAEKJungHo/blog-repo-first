---
title: "[스프링 MVC 게시판] 13. 검색 구현"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 검색 구현

  검색을 구현하기 위해 기존에 페이징 구현에 사용하던 `Criteria`를 상속받은 `SearchCriteria`를 만들어 사용합니다.
  SearchCriteria는 검색타입(제목, 작성자, 내용)과 키워드를 속성으로 가지고 있습니다.

  그리고 PageMaker에는 makeSearch()라는 메서드를 가지고 있습니다. 해당 메서드는 검색 조건을 유지하기 위해
  (URI로 검색조건을 달고 가기 위해) URI를 생성해주는 메서드입니다.

  만약 제목으로 스프링을 검색하고 검색 결과의 1번 게시물을 클릭하여 상세 정보를 본다음 목록으로 돌아 올때에
  다시 스프링을 검색했던 조건을 유지하여 원래 있던 페이지로 와야 합니다.

  따라서 ?searchType="title"&keyword="스프링" 이런식으로 URI에 KEY와 VALUE를 담아 컨트롤러와 View쪽으로 서로 뿌려 줄 수 있어야 합니다.

### SearchCriteria(VO) 작성

  ```java
public class SearchCriteria extends Criteria {

	private String searchType;
	private String keyword;

	public String getSearchType() {
		return searchType;
	}

	public void setSearchType(String searchType) {
		this.searchType = searchType;
	}

	public String getKeyword() {
		return keyword;
	}

	public void setKeyword(String keyword) {
		this.keyword = keyword;
	}

	@Override
	public String toString() {
		return "SearchCriteria [searchType=" + searchType + ", keyword=" + keyword + "]";
	}

}
  ```

### PageMaker 메서드 추가

  ```java
  /**
 *  검색 조건과 검색 키워드 처리
 *  URI 자동 생성 메서드
 *  페이지와 페이지번호, 검색 타입과 키워드 조건이 URI에 붙어서 간다.
 */
public String makeSearch(int page) {
  UriComponents uriComponents = UriComponentsBuilder.newInstance()
    .queryParam("page", page)
    .queryParam("pagePageNum", cri.getPerPageNum())
    .queryParam("searchType", ((SearchCriteria)cri).getSearchType())
    .queryParam("keyword", encoding(((SearchCriteria)cri).getKeyword()))
    .build();

  return uriComponents.toUriString();
}

/* 검색 키워드 인코딩 처리 */
public String encoding(String keyword) {
  if(keyword == null || keyword.trim().length() == 0) return "";
  try {
    return URLEncoder.encode(keyword, "UTF-8");
  } catch(Exception e) {
    return "";
  }
}
  ```

  위 2개의 메서드를 추가해야 합니다.

### Mapper.xml 작성

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
<mapper namespace="boardDAO">
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
	<![CDATA[
		SELECT DISTINCT b.num, b.title, u.name, b.date, b.count, b.contents, b.id
		FROM b_board as b
		INNER join b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N' AND num = #{num}
		ORDER BY num DESC
	]]>
	</select>

	<!-- 삽입(첨부파일 추가)-->
	<insert id="insert" parameterType="boardDTO">
		INSERT INTO b_board (title, contents, id, atch_file_id)
		VALUES (#{title},#{contents},#{id},#{atch_file_id})
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

	<!-- 검색 대한 게시글 수 -->
	<select id="countArticle" resultType="int">
		SELECT count(*)
		FROM b_board as b
		INNER JOIN b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		<include refid="search" />
	</select>

	<!-- 검색 -->
	<select id="searchList" resultType="boardDTO">
		SELECT b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id
		FROM b_board as b
		INNER JOIN b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		<include refid="search" />
		ORDER BY num DESC
		LIMIT #{pageStart}, #{perPageNum}
	</select>

	<!-- MyBatis 동적 sql -->
	<sql id="search">
		<choose>
			<when test="searchType == 'all'">
				AND (u.name LIKE CONCAT('%', #{keyword}, '%')
				OR b.contents LIKE CONCAT('%', #{keyword}, '%')
				OR b.title LIKE CONCAT('%', #{keyword}, '%'))
			</when>
			<when test="searchType == 'writer'">
				AND u.name LIKE CONCAT('%', #{keyword}, '%')
			</when>
			<when test="searchType == 'content'">
				AND b.contents LIKE CONCAT('%', #{keyword}, '%')
			</when>
			<when test="searchType == 'title'">
				AND b.title LIKE CONCAT('%', #{keyword}, '%')
			</when>
		</choose>
	</sql>
</mapper>
  ```

  검색과 페이지를 합쳤기 때문에 전에 작성한 페이지 부분의 쿼리는 필요 없습니다. 그리고 검색에 대한 게시글 수를 구하기 위한
  쿼리를 추가했습니다.

  동적 SQL을 사용하여 검색 조건에 대한 분기를 일일이 나눠 줘야 합니다. #대신 $를 사용하면 분기를 조금만 나눌 수 있는데,
  $는 배제해야합니다.

  > [#과 $의 차이 : SQL Injection](https://baekjungho.github.io/database-mybatisetc/#%EA%B3%BC-%EC%9D%98-%EC%B0%A8%EC%9D%B4)

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public void delete(BoardDTO boardDTO);
	public int update(BoardDTO boardDTO);
	public void insert(BoardDTO boardDTO);
	public BoardDTO select(int num);
	public int updateReadCount(int num);
	public int countBoardList();
	// public List<BoardDTO> searchList(String searchOption, String keyword);
	public List<BoardDTO> searchList(SearchCriteria cri);
	public int countArticle(String searchType, String keyword);

}
  ```

  public List<BoardDTO> searchList(String searchOption, String keyword); 이 부분 대신
  아래의 searchList를 사용한 이유는, 매개변수를 늘여놓는것보다, 현재 매개변수 타입이 SearchCriteria의 속성이므로
  커맨드 객체를 사용하는 편이 낫습니다. SearchCriteria는 Criteria를 상속받았으므로, Criteria의 속성까지 접근하여 사용할 수 있습니다.

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

	@Override
	public int countBoardList() {
		return sqlSessionTemplate.selectOne("boardDAO.countBoardList");
	}

	/* 검색 매개변수 사용 : 비추천
	@Override
	public List<BoardDTO> searchList(String searchOption, String keyword) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("searchOption", searchOption);
		map.put("keyword", keyword);
		return sqlSessionTemplate.selectList("boardDAO.searchList", map);
	}
	 */

	// 검색 페이지까지 적용
	@Override
	public List<BoardDTO> searchList(SearchCriteria cri) {
		// 자동 형변환
		return sqlSessionTemplate.selectList("boardDAO.searchList", cri);
	}

	@Override
	public int countArticle(String searchType, String keyword) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("searchType", searchType);
		map.put("keyword", keyword);
		return sqlSessionTemplate.selectOne("boardDAO.countArticle", map);
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
	public int countBoardList();
	// public List<BoardDTO> searchList(String searchOption, String keyword);
	public List<BoardDTO> searchList(SearchCriteria cri);
	public int countArticle(String searchType, String keyword);
}
  ```

### ServiceImpl 작성

  ```java
@Service
public class BoardServiceImpl implements BoardService {

	@Autowired
	private BoardDAO boardDAO;

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
	public int countBoardList() {
		return boardDAO.countBoardList();
	}

	/* 검색 원본
	@Override
	public List<BoardDTO> searchList(String searchOption, String keyword) {
		return boardDAO.searchList(searchOption, keyword);
	}
	*/

	@Override
	public List<BoardDTO> searchList(SearchCriteria cri) {
		return boardDAO.searchList(cri);
	}

	@Override
	public int countArticle(String searchType, String keyword) {
		return boardDAO.countArticle(searchType, keyword);
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

		Map<String, Object> map = new HashMap<String, Object>();
		map.put("searchList", searchList);
		map.put("count", count);
		map.put("searchOption", cri.getSearchType());
		map.put("keyword", cri.getKeyword());
		mav.addObject("pageMaker", pageMaker);
		mav.addObject("map", map);
		return mav;
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

### boardSearchList.jsp

  기존에 있는 View는 그대로 두고 검색+페이징 기능이 적용된 View를 만들겠습니다.

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
  	<div>
  		<form name="searchForm" action="<c:url value="/boardSearchList" />" method="GET" >
  			<select name="searchType">
  				<option value="all" <c:out value="${searchType =='all'? 'selected':'' }"/>>제목+이름+내용</option>
  				<option value="writer" <c:out value="${searchType =='writer'? 'selected':'' }"/>>이름</option>
  				<option value="content" <c:out value="${searchType =='content'? 'selected':'' }"/>>내용</option>
  				<option value="title" <c:out value="${searchType =='title'? 'selected':'' }"/>>제목</option>
  			</select>
  			<input type="text" name="keyword" value="${keyword}" placeholder="검색">
  			<input id="submit" type="submit" value="검색">
  		</form>
  	</div>
  	<br>
  	<b>${count}</b>개의 게시물이 있습니다.
  	<br><br>
  	<table border="1">
  		<tr>
  			<th>번호</th>
  			<th>제목</th>
  			<th>작성자</th>
  			<th>작성일</th>
  			<th>조회수</th>
  		</tr>
  		<c:forEach var="board" items="${searchList}" varStatus="loop">
  		<tr>
  			<td>${loop.count}</td>
  			<td><a href="<c:url value="/boardRead/${board.num}${pageMaker.makeSearch(pageMaker.cri.page)}" />"> ${board.title}</a></td>
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
  		        <a href='<c:url value="/boardSearchList?${pageMaker.makeSearch(pageMaker.startPage-1)}"/>'>&laquo;</a>
  		    </td>
  		    </c:if>
  		    <c:forEach begin="${pageMaker.startPage}" end="${pageMaker.endPage}" var="idx">
  		    <td>
  		        <a href='<c:url value="/boardSearchList${pageMaker.makeSearch(idx)}"/>'>${idx}</a>
  		    </td>
  		    </c:forEach>
  		    <c:if test="${pageMaker.next && pageMaker.endPage >0}">
  		    <td>
  		        <a href='<c:url value="/boardSearchList${pageMaker.makeSearch(pageMaker.endPage+1)}"/>'>&raquo;</a>
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

  위 View에서 검색을 위한 코드를 보고 다시 Mapper로 가서 글을 읽으면 도움이 됩니다.
