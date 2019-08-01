---
title: "JSP를 사용한 트위터 구현"
layout: post
category: JSP
tags: [JSP, Servlet, MySQL, DataBase]
excerpt: "JSP와 Servlet 사용해서 Twitter 기능을 구현 해봅시다."
author: BAEKJungHo
---

* content
{:toc}

JSP와 Servlet 사용해서 Twitter 기능을 구현 해봅시다.

## Twitter

  [MVC 구현2](https://baekjungho.github.io/jsp-mvc2/)에 있는 코드에서 트위터 기능을
  추가해 보겠습니다. View를 담당하는 twitter_list.jsp(등록된 글의 목록 출력) Controller를 담당하는
  TwitProc.java를 추가 해보겠습니다.

  1. DB를 사용하지 않고 application 내장객체를 사용해 공용 저장소로 활용.
  2. Tomcat 종료시 저장된 데이터도 초기화된다.
  3. 다중 사용자 접속을 지원하며 개별 사용자 id를 유지.

  다른 사람의 홈페이지에 접속하기 위해서는 주소창에 `IP경로/폴더경로`로 입력해주면 됩니다.

  Mapping은 `/member/twitServlet` 경로로 맞추었습니다.

  - __twitter_list.jsp__

  ```java
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <%
  	request.setCharacterEncoding("UTF-8");
  %>
  <html>
  <head>
  <title>ch06 : twitter_list.jsp</title>
  </head>
  <body>
  	<div align=center>
  	<H3>My Simple Twitter!!</H3>
  	<a href="loginMain.jsp">회원 목록으로</a>
  	<HR>
  	<form action="/jspbook/member/twitServlet" method="POST">
  		<!-- 세션에 저장된 이름 출력 -->
  		@<%=session.getAttribute("memberName") %>&nbsp;
  		<input type="text" name="msg">&nbsp;&nbsp;<input type="submit" value="트윗">
  	</form>
  	<HR>
  	<div align="left">
  	<UL>
  	<%
  		// application 내장객체를 통해 msgs 이름으로 저장된 ArrayList를 가지고 옴
  		ArrayList<String>msgs = (ArrayList<String>)application.getAttribute("msgs");

  		//msgs가 null 이 아닌 경우에만 목록 출력
  		if(msgs != null) {
  			for(String msg : msgs) {
  				out.println("<LI>"+msg+"</LI>");
  			}
  		}
  	%>
  	</UL>
  	</div>
  	</div>
  </body>
  </html>
  ```

  - __TwitProc.java__

  ```java
  import java.io.IOException;
  import java.text.SimpleDateFormat;
  import java.util.ArrayList;
  import java.util.Date;
  import java.util.List;
  import java.util.Locale;

  import javax.servlet.ServletContext;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import javax.servlet.http.HttpSession;

  /**
   * Servlet implementation class TwitProc
   */
  @WebServlet("/member/twitServlet")
  public class TwitProc extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public TwitProc() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doPost(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("UTF-8");
		String msg = request.getParameter("msg");
		HttpSession session = request.getSession();
		String username = (String)session.getAttribute("memberName");
		ServletContext application = request.getServletContext();

		// 메시지 저장을 위해 application 에서 msgs 로 저장된 ArrayList 가지고 옴
		List<String> msgs = (ArrayList<String>)application.getAttribute("msgs");

		// null 인 경우 새로운 ArrayList 객체를 생성
		if(msgs == null) {
			msgs = new ArrayList<String>();
			// application 에 ArrayList 저장
			application.setAttribute("msgs",msgs);
		}

		// 사용자 이름, 메시지, 날짜 정보를 포함하여 ArrayList에 추가
		Date date = new Date();
		SimpleDateFormat f = new SimpleDateFormat("MM월 dd일(E) HH:mm", Locale.KOREA);
		msgs.add(username+" :: "+msg+" , "+ f.format(date));

		// 톰캣 콘솔을 통한 로깅
		application.log(msg + ", " + username);

		// 목록 화면으로 리다이렉팅
		response.sendRedirect("twitter_list.jsp");
	 }
  }
  ```

  그리고 loginMain.jsp에서 트윗이라는 단어에 링크를 걸어서 twitter_list.jsp로 이동하게
  만들었습니다. 코드는 다음과 같습니다.

  ```html
  <a href="twitter_list.jsp">트윗</a>&nbsp;&nbsp;&nbsp;
  ```
