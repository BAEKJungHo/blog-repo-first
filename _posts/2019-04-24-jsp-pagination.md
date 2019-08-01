---
title: "게시판 페이징 기능 구현"
layout: post
category: JSP
tags: [JSP, Servlet, MySQL]
excerpt: "JSP와 Servlet 사용해서 게시판 페이징 기능을 구현 해봅시다."
author: BAEKJungHo
---

* content
{:toc}

JSP와 Servlet 사용해서 게시판 페이징 기능을 구현 해봅시다.

## Pagination

  ![bbs](/images/posts/201904/bbs.jpg)

  - __BbsProc.java(Controller)__

  switch문에 다음 코드를 추가 하였습니다.

  ```java
  case "list":
  if (!request.getParameter("page").equals("")) {
    curPage = Integer.parseInt(request.getParameter("page"));
  }
  bDao = new BbsDAO();
  int count = bDao.getCount();
  if (count == 0)			// 데이터가 없을 때 대비
    count = 1;
  int pageNo = (int)Math.ceil(count/10.0);
  if (curPage > pageNo)	// 경계선에 걸렸을 때 대비
    curPage--;
  session.setAttribute("currentBbsPage", curPage);
  // 리스트 페이지의 하단 페이지 데이터 만들어 주기
  String page = null;
  page = "<a href=#>&laquo;</a>&nbsp;";
  pageList.add(page);
  for (int i=1; i<=pageNo; i++) {
    page = "&nbsp;<a href=bbsServlet?action=list&page=" + i + ">" + i + "</a>&nbsp;";
    pageList.add(page);
  }
  page = "&nbsp;<a href=#>&raquo;</a>";
  pageList.add(page);

  List<BbsMember> bmList = bDao.selectJoinAll(curPage);
  request.setAttribute("bbsMemberList", bmList);
  request.setAttribute("pageList", pageList);
  rd = request.getRequestDispatcher("bbsList.jsp");
      rd.forward(request, response);
  break;
  ```

  - __BbsDAO.java(Model)__

  다음 코드를 추가 하였습니다.

  ```java
  public List<BbsMember> selectJoinAll(int page) {
  int offset = 0;
  String query = null;
  if (page == 0) {	// page가 0이면 모든 데이터를 보냄
    query = "select bbs.id, bbs.title, member.name, bbs.date from bbs " +
        "inner join member on bbs.memberId=member.id order by bbs.id desc;";
  } else {			// page가 0이 아니면 해당 페이지 데이터만 보냄
    query = "select bbs.id, bbs.title, member.name, bbs.date from bbs " +
        "inner join member on bbs.memberId=member.id order by bbs.id desc limit ?, 10;";
    offset = (page - 1) * 10;
  }
  PreparedStatement pStmt = null;
  List<BbsMember> bmList = new ArrayList<BbsMember>();
  try {
    pStmt = conn.prepareStatement(query);
    if (page != 0)
      pStmt.setInt(1, offset);
    ResultSet rs = pStmt.executeQuery();
    while (rs.next()) {
      BbsMember bmDto = new BbsMember();
      bmDto.setId(rs.getInt(1));
      bmDto.setTitle(rs.getString(2));
      bmDto.setName(rs.getString(3));
      bmDto.setDate(rs.getString(4));
      bmList.add(bmDto);
    }
    rs.close();
  } catch (Exception e) {
    e.printStackTrace();
  } finally {
    try {
      if (pStmt != null && !pStmt.isClosed())
        pStmt.close();
    } catch (SQLException se) {
      se.printStackTrace();
    }
  }
  return bmList;
  }

  public int getCount() {
  String query = "select count(*) from bbs;";
  PreparedStatement pStmt = null;
  int count = 0;
  try {
    pStmt = conn.prepareStatement(query);
    ResultSet rs = pStmt.executeQuery();
    while (rs.next()) {				
      count = rs.getInt(1);
    }
    rs.close();
  } catch (Exception e) {
    e.printStackTrace();
  } finally {
    try {
      if (pStmt != null && !pStmt.isClosed())
        pStmt.close();
    } catch (SQLException se) {
      se.printStackTrace();
    }
  }
  return count;
  }
  ```

  - __bbsList.jsp(View)__

  페이징을 구현한 게시판 View는 다음과 같습니다.

  ```java
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <%@ page import="java.util.*, member.*" %>
  <%
  	request.setCharacterEncoding("UTF-8");
  	List<BbsMember> bmList = (List<BbsMember>)request.getAttribute("bbsMemberList");
  	List<String> pageList = (List<String>)request.getAttribute("pageList");
  %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  	<title>게시판 목록</title>
  	<style>
  		td, th { text-align: center; }
  		.button {
  			font-weight: bold; font-size: 9pt;
  			border: 1px solid powderblue;
  		}
  		input[type=submit] {
  			width: 5em; height: 2.5em;
  			font-weight: bold; font-size: 10pt;
  		}
  	</style>
  </head>
  <body>
	<center>
	<h3>게시판</h3>
	<%=(String)session.getAttribute("memberName")%> 회원님 반갑습니다.<br>
	<a href="bbsWrite.jsp">글쓰기</a>&nbsp;&nbsp;
	<a href="twitter_list.jsp">트윗</a>&nbsp;&nbsp;
	<a href="loginMain.jsp">회원목록</a>&nbsp;&nbsp;
	<a href="memberProcServlet?action=logout">로그아웃</a>
	<hr>
	<table border="1" style="border-collapse:collapse;" width=600>
	<tr bgcolor="skyblue" height="30"><th>글번호</th><th>제목</th><th>글쓴이</th><th>최종수정일</th><th>액션</th></tr>
	<%
	for (BbsMember bm: bmList) {
	%>
		<tr height="25"><td><%=bm.getId()%></td>
		<td><a href="bbsServlet?action=view&id=<%=bm.getId()%>"><%=bm.getTitle()%></a></td>
		<td><%=bm.getName()%></td>
		<td><%=bm.getDate().substring(0, 16)%></td>
		<%
			String updateUri = "bbsServlet?action=update&id=" + bm.getId();
			String deleteUri = "bbsServlet?action=delete&id=" + bm.getId();
		%>
		<td>&nbsp;<button onclick="location.href='<%=updateUri%>'">수정</button>&nbsp;
			<button onclick="location.href='<%=deleteUri%>'">삭제</button>&nbsp;</td></tr>
	<%
	}
	%>
	</table>
	<%
	for (String bmPage: pageList)
		out.print(bmPage);
	%>
	</center>
  </body>
  </html>
  ```
