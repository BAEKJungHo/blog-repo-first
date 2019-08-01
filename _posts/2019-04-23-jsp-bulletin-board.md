---
title: "JSP를 사용한 게시판 구현"
layout: post
category: JSP
tags: [JSP, Servlet, MySQL, DataBase]
excerpt: "JSP와 Servlet 사용해서 게시판 기능을 구현 해봅시다."
author: BAEKJungHo
---

* content
{:toc}

JSP와 Servlet 사용해서 게시판 기능을 구현 해봅시다.

## Bulletin Board

  [MVC 구현2](https://baekjungho.github.io/jsp-mvc2/)에 있는 코드에서 게시판 기능을
  추가해 보겠습니다.

  - __BoardProc.java(Controller)__

  게시글 작성, 조회, 수정, 삭제(CRUD)를 담당하는 컨트롤러입니다.

  ```java
  import java.io.IOException;
  import java.net.URLEncoder;
  import java.text.SimpleDateFormat;
  import java.util.ArrayList;
  import java.util.Date;
  import java.util.List;

  import javax.servlet.RequestDispatcher;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import javax.servlet.http.HttpSession;


  @WebServlet("/member/boardServlet")
  public class BoardProc extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public BoardProc() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doAction(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doAction(request, response);
	}

	protected void doAction(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		BbsDAO bDao = new BbsDAO();
		BbsDTO bbs = null;
		RequestDispatcher rd = null;
		SimpleDateFormat format1 = null;
		Date time = null;
		String time1 = null;
		int id = 0;
		String message = null;
		String url = null;
		HttpSession session = request.getSession();
		request.setCharacterEncoding("UTF-8");
		String action = request.getParameter("action");
		System.out.println("action :" + action);

		switch(action) {
		case "update":		// 수정 버튼 클릭 시
			if (!request.getParameter("id").equals("")) {
				id = Integer.parseInt(request.getParameter("id"));
				bbs = bDao.selectOne(id);
			}
			if (bbs.getMemberId() != (Integer)session.getAttribute("memberId")) {
				message = "id = " + id + " 에 대한 수정 권한이 없습니다.";
				url = "board_list.jsp";
				request.setAttribute("message", message);
				request.setAttribute("url", url);
				rd = request.getRequestDispatcher("alertMsg.jsp");
				rd.forward(request, response);
				break;
			}

			bbs = bDao.selectId(id);
			bDao.close();
			request.setAttribute("bbs", bbs);
			rd = request.getRequestDispatcher("board_update.jsp");
	        rd.forward(request, response);
	        break;

		case "delete":		// 삭제 버튼 클릭 시
			if (!request.getParameter("id").equals("")) {
				id = Integer.parseInt(request.getParameter("id"));
				bbs = bDao.selectOne(id);
			}
			if (bbs.getMemberId() != (Integer)session.getAttribute("memberId")) {
				message = "id = " + id + " 에 대한 삭제 권한이 없습니다.";
				url = "board_list.jsp";
				request.setAttribute("message", message);
				request.setAttribute("url", url);
				rd = request.getRequestDispatcher("alertMsg.jsp");
				rd.forward(request, response);
				break;
			}

			bDao.deleteBbs(id);
			bDao.close();

			message = "id = " + id + " 가 작성한 내용이 삭제되었습니다.";
			url = "board_list.jsp";
			request.setAttribute("message", message);
			request.setAttribute("url", url);
			rd = request.getRequestDispatcher("alertMsg.jsp");
			rd.forward(request, response);
			break;

		case "write":		// write 할 때
			id = (Integer)session.getAttribute("memberId");
			System.out.print("id : " +  id);

			String title = request.getParameter("title");
			String contents = request.getParameter("contents");

			// 날짜 변환
			format1 = new SimpleDateFormat ("yy-MM-dd HH:mm");
			time = new Date();
			time1 = format1.format(time);

			// 글쓰기
			bDao.insertBbs(new BbsDTO(id, title, time1, contents));

			message = "게시물 작성이 완료되었습니다.";
			url = "board_list.jsp";
			request.setAttribute("message", message);
			request.setAttribute("url", url);
			rd = request.getRequestDispatcher("alertMsg.jsp");
			rd.forward(request, response);
			break;

		case "execute":			// 게시물 수정정보 입력 후 실행할 때
			if (!request.getParameter("id").equals("")) {
				id = Integer.parseInt(request.getParameter("id"));
			}
			title = request.getParameter("title");
			contents = request.getParameter("contents");

			format1 = new SimpleDateFormat ("yy-MM-dd HH:mm");
			time = new Date();
			time1 = format1.format(time);

			bbs = bDao.selectOne(id);
			bbs.setTitle(title);
			bbs.setContent(contents);
			bbs.setDate(time1);
			// System.out.println(bbs.toString());

			bDao = new BbsDAO();
			bDao.updateBbs(bbs);
			bDao.close();

			message = "다음과 같이 수정하였습니다.\\n" + bbs.toStrOne();
			request.setAttribute("message", message);
			request.setAttribute("url", "board_list.jsp");
			rd = request.getRequestDispatcher("alertMsg.jsp");
	        rd.forward(request, response);
			break;

		case "search":			// 조회버튼 클릭할 때 : 상세조회
			if (!request.getParameter("id").equals("")) {
				id = Integer.parseInt(request.getParameter("id"));
			}

			bbs = bDao.selectId(id);
			bDao.close();
			request.setAttribute("bbs", bbs);
			rd = request.getRequestDispatcher("board_search.jsp");
	        rd.forward(request, response);
	        break;

		default:
		}
	 }
  }
  ```

  - __BbsDTO(Model)__

  ```java
  public class BbsDTO {

	private int id;
	private int memberId;
	private String title;
	private String date;
	private String content;
	private String name;
	private int totalCount;

	public BbsDTO(int id, int memberId, String title, String date, String content) {
		super();
		this.id = id;
		this.memberId = memberId;
		this.title = title;
		this.date = date;
		this.content = content;
	}

	public BbsDTO(int memberId, String title, String date, String content) {
		super();
		this.memberId = memberId;
		this.title = title;
		this.date = date;
		this.content = content;
	}

	public BbsDTO(String title, String date, String content) {
		super();
		this.title = title;
		this.date = date;
		this.content = content;
	}

	public BbsDTO( ) { }

	public int getTotalCount() {
		return totalCount;
	}

	public void setTotalCount(int totalCount) {
		this.totalCount = totalCount;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public int getMemberId() {
		return memberId;
	}

	public void setMemberId(int memberId) {
		this.memberId = memberId;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getDate() {
		return date;
	}

	public void setDate(String date) {
		this.date = date;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "BbsDTO [id=" + id + ", 글쓴이=" + name + ", 제목=" + title + ", 수정일=" + date.substring(0, 16) + ", 내용="
				+ content + "]";
	}

	// DetailSearch
	public String toStrDetail() {
		return "BbsDTO [글쓴이=" + name + ", 제목=" + title + ", 수정일=" + date.substring(0, 16) + ", 내용="
				+ content + "]";
	}
	// Update
	public String toStrOne() {
		return "BbsDTO [id=" + id + ", memberId=" + memberId + ", 제목=" + title + ", 수정일=" + date + ", 내용="
				+ content + "]";
	 }
  }
  ```

  - __BbsDAO(Model)__

  ```java
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.ArrayList;
  import java.util.List;

  import member.MemberDAO;
  import member.MemberDTO;

  public class BbsDAO {

	private Connection conn;
	private static final String USERNAME = "javauser";
	private static final String PASSWORD = "javapass";
	private static final String URL = "jdbc:mysql://localhost:3306/world?verifyServerCertificate=false&useSSL=false";


	// Constructor : JDBC 드라이버를 로딩 & DB Connection 구하기
	public BbsDAO() {
	try {
		Class.forName("com.mysql.jdbc.Driver");
		conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
	} catch(Exception e) {
		e.printStackTrace();
	}
 }

  // INSERT QUERY - 글쓰기
  public void insertBbs(BbsDTO bbs) {
	String query = "insert into bbs(memberId, title, date, content) values (?, ?, ?, ?);";
	PreparedStatement pStmt = null;
	try {
		pStmt = conn.prepareStatement(query);
		pStmt.setInt(1, bbs.getMemberId());
		pStmt.setString(2, bbs.getTitle());
		pStmt.setString(3, bbs.getDate());
		pStmt.setString(4, bbs.getContent());

		pStmt.executeUpdate();
	} catch(Exception e) {
		e.printStackTrace();
	} finally {
		try {
			if(pStmt != null && !pStmt.isClosed())
				pStmt.close();
		} catch(SQLException se) {
			se.printStackTrace();
		}
	 }
  }

	// DELETE QUERY - 삭제
	public void deleteBbs(int id) {
		String query = "delete from bbs where id=?;";
		PreparedStatement pStmt = null;
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setInt(1, id);

			pStmt.executeUpdate();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
	}

	// UPDATE QUERY - 수정
	public void updateBbs(BbsDTO bbs) {
		String query = "update bbs set title=?, date=?, content=? where id=?;";
		PreparedStatement pStmt = null;
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setString(1, bbs.getTitle());
			pStmt.setString(2, bbs.getDate());
			pStmt.setString(3, bbs.getContent());
			pStmt.setInt(4, bbs.getId());

			pStmt.executeUpdate();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
	}

	// selectCondition
	public List<BbsDTO> selectCondition(String query) {
		PreparedStatement pStmt = null;
		List<BbsDTO> bbsList = new ArrayList<BbsDTO>();
		try {
			pStmt = conn.prepareStatement(query);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				BbsDTO bbs = new BbsDTO();
				bbs.setId(rs.getInt(1));
				bbs.setMemberId(rs.getInt(2));
				bbs.setName(rs.getString(3));
				bbs.setTitle(rs.getString(4));
				bbs.setDate(rs.getString(5));
				bbs.setContent(rs.getString(6));
				bbsList.add(bbs);
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return bbsList;
	}

	// selectOne
	// selectOne - id로 상세조회
	public BbsDTO selectOne(int id) {
		String query = "select * from bbs where id=?;";
		PreparedStatement pStmt = null;
		BbsDTO bbs = new BbsDTO();
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setInt(1,  id);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				bbs.setId(rs.getInt(1));
				bbs.setMemberId(rs.getInt(2));
				bbs.setTitle(rs.getString(3));
				bbs.setDate(rs.getString(4));
				bbs.setContent(rs.getString(5));
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return bbs;
	}

	// 전체조회
	// select - 조회
	public List<BbsDTO> selectAll() {
		String sql ="select b.id as 아이디, m.id as 멤버아이디, m.name as 이름, b.title as 제목, b.date as 수정일, b.content as 내용"
				+ " from bbs as b " +
				"inner join member as m " +
				"on b.memberid = m.id " +
				"order by date desc";
		List<BbsDTO> bbsList = selectCondition(sql);
		return bbsList;
	}

	// ID로 상세조회
	// 제목으로 상세조회
	public BbsDTO selectId(int id) {
		String sql ="select b.id as 아이디, m.name as 이름, b.title as 제목, b.date as 수정일, b.content as 내용"
				+ " from bbs as b " +
				"inner join member as m " +
				"on b.memberid = m.id " +
				"where b.id=? " +
				"order by date desc";
		PreparedStatement pStmt = null;
		BbsDTO bbs = new BbsDTO();
		try {
			pStmt = conn.prepareStatement(sql);
			pStmt.setInt(1,  id);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				bbs.setId(rs.getInt(1));
				bbs.setName(rs.getString(2));
				bbs.setTitle(rs.getString(3));
				bbs.setDate(rs.getString(4));
				bbs.setContent(rs.getString(5));
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return bbs;
	}

	public int getTotalCount() {
		String sql ="select count(*) from bbs;";
		PreparedStatement pStmt = null;
		BbsDTO bbs = new BbsDTO();
		try {
			pStmt = conn.prepareStatement(sql);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				bbs.setTotalCount(rs.getInt(1));
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return bbs.getTotalCount();
	}

	// 현재 페이지(startRow)와 한 페이지에 나타날 게시글 수(endRow) 지정
	// startRow번째 페이지에서 endRow행만큼 가져온다.
	public List<BbsDTO> getList(int startRow, int endRow) {
		List<BbsDTO> bbsList = new ArrayList<BbsDTO>();
		String sql ="select b.id as 글번호, m.name as 이름, b.title as 제목, b.date as 수정일"
				+ " from bbs as b " +
				"inner join member as m " +
				"on b.memberid = m.id " +
				"order by date desc limit "+startRow+", "+endRow;
		PreparedStatement pStmt = null;
		BbsDTO bbs = new BbsDTO();
		try {
			pStmt = conn.prepareStatement(sql);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				bbs.setId(rs.getInt(1));
				bbs.setName(rs.getString(2));
				bbs.setTitle(rs.getString(3));
				bbs.setDate(rs.getString(4));
				bbsList.add(bbs);
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return bbsList;
	}

	// Connection Close
 	public void close() {
		try {
			if(conn != null && !conn.isClosed())
				conn.close();
		} catch(SQLException e) {
			e.printStackTrace();
		}
	 }
  }
  ```

  - __board_list.jsp(View)__

  게시글 리스트를 보여주는 View입니다.

  ```java
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*"%>
  <%@ page import="member.*" %>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

  <%
  request.setCharacterEncoding("UTF-8");
  int id = (Integer)session.getAttribute("memberId");
  BbsDAO bDao = new BbsDAO();
  List<BbsDTO> list = bDao.selectAll();
  bDao.close();
  %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <style>
  table { text-align : center; }
  th { height : 30px; }
  td { height : 20px; }
  </style>
  <head>
  <title>Bulletin Board</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css">
  </head>
  <body>
  <div align=center>
  <H3>My Simple Bulletin Board!!</H3>
  <a href="write.html">글쓰기</a>&nbsp;&nbsp;
  <a href="loginMain.jsp">회원 목록</a>&nbsp;&nbsp;
  <a href="twitter_list.jsp">트윗</a>&nbsp;&nbsp;
  <a href="/jspbook/member/memberProcServlet?action=logout">로그아웃</a>
  <HR>
  <table border="1" style="border-collapse:collapse;" width=700>
  <tr bgcolor="pink"><th>글번호</th><th>제목</th><th>글쓴이</th><th>최종수정일</th><th>액션</th></tr>
  <%
  for (BbsDTO bb: list) {
  %>
    <tr><td><%=bb.getId()%></td>
    <td><a href="/jspbook/member/boardServlet?action=search&id=<%=bb.getId()%>"><%=bb.getTitle()%></a></td>
    <td><%=bb.getName()%></td>
    <td><%=bb.getDate()%></td>
    <%
      String updateUri = "boardServlet?action=update&id=" + bb.getId();
      String deleteUri = "boardServlet?action=delete&id=" + bb.getId();
    %>
    <td>
     &nbsp;<button onclick="location.href='<%=updateUri%>'">수정</button>&nbsp;
    <button onclick="location.href='<%=deleteUri%>'">삭제</button>&nbsp;
    </td>
    </tr>
  <%
  }
  %>
  </table>
  </div>
  </body>
  </html>
  ```

  - __board_search.jsp(View)__

  상세조회 기능을 담당하는 View입니다.

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" import="member.*" %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Update</title>
  </head>
  <body>
  <%
  	request.setCharacterEncoding("UTF-8");
  	BbsDTO bbs = (BbsDTO) request.getAttribute("bbs");
  %>
	<h3>상세 조회</h3>
	<hr>
	<form name="updateBoardForm" action="/jspbook/member/boardServlet?action=execute" method=post>
		<input type="hidden" id="id" name="id" value="<%=bbs.getId()%>">
		<label><span>글번호:</span><%=bbs.getId()%></label><br><br>
		<input type="hidden" id="name" name="name" value="<%=bbs.getName()%>">
		<label><span>이름:</span><%=bbs.getName()%></label><br><br>
		<input type="hidden" id="title" name="title" value="<%=bbs.getTitle()%>">
		<label><span>제목:</span><%=bbs.getTitle()%></label><br><br>
		<input type="hidden" id="date" name="date" value="<%=bbs.getDate()%>">
		<label><span>수정일:</span><%=bbs.getDate()%></label><br><br>
		<input type="hidden" id="contents" name="contents" value="<%=bbs.getContent()%>">
		<label><span>내용:</span><%=bbs.getContent()%></label><br><br>
	</form>
		<%
			String updateUri = "boardServlet?action=update&id=" + bbs.getId();
			String deleteUri = "boardServlet?action=delete&id=" + bbs.getId();
		%>
		 &nbsp;<button onclick="location.href='<%=updateUri%>'">수정</button>&nbsp;
		 <button onclick="location.href='<%=deleteUri%>'">삭제</button>&nbsp;
		 <button onclick="location='board_list.jsp'">목록으로</button>&nbsp;  
  </body>
  </html>
  ```

  - __board_update.jsp(View)__

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" import="member.*" %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Update</title>
  </head>
  <body>
  <%
  	//request.setCharacterEncoding("UTF-8");
  	BbsDTO bbs = (BbsDTO) request.getAttribute("bbs");
  %>
  	<h3>게시물 수정</h3>
  	<hr>
  	<form name="updateBoardForm" action="/jspbook/member/boardServlet?action=execute" method=post>
  		<input type="hidden" id="id" name="id" value="<%=bbs.getId()%>">
  		<label><span>글번호:</span>
  			<%=bbs.getId()%></label><br>
  		<input type="hidden" id="name" name="name" value="<%=bbs.getName()%>">
  		<label><span>이름:</span>
  			<%=bbs.getName()%></label><br>
  		<label><span>제목:</span>
  			<input type="text" name="title" value="<%=bbs.getTitle()%>" size="10"></label><br>
  		<label><span>내용:</span>
  			<textarea rows="10" cols="30" name="contents"></textarea>
  			</label><br>
  		<label><span></span><input type="submit" value="수정" name="B1">&nbsp;&nbsp;
  			<input type="reset" value="재작성" name="B2">
  	</form>
  </body>
  </html>
  ```
