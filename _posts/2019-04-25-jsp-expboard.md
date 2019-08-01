---
title: "EL로 변경한 View 파일"
layout: post
category: JSP
tags: [JSP]
excerpt: "앞에서 Scriptlet으로 구현된 예제를 EL로 변경 해보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

JSP, Servlet 공부를 하면서 참고한 서적입니다.

_프로젝트로 배우는 자바 웹 프로그래밍_

## EL로 변경한 View 파일

  앞에서 사용한 alertMsg, bbsList, loginMain 등 jsp로 작성한 View파일들을 EL로 변경하겠습니다.

  - __alertMsg__

  ```html
  <c:set var="message" value="${requestScope.message}"/>
  <c:set var="url" value="${requestScope.url}"/>
  <script type="text/javascript">
  	var message = '${message}';
  	var returnUrl = '${url}';
  	alert(message);
  	document.location.href = returnUrl;
  </script>
  ```

  - __loginMain.jsp__

  ```html
  <body>
  	<center>
  	<h3>회원 명단</h3>
  	${memberName} 회원님 반갑습니다.<br>
  	<a href="bbsServlet?action=list&page=1">게시판</a>&nbsp;&nbsp;
  	<a href="twitter_list.jsp">트윗</a>&nbsp;&nbsp;
  	<a href="memberProcServlet?action=logout">로그아웃</a>
  	<hr>
  	<table border="1" style="border-collapse:collapse;" width=700>
  	<tr bgcolor="pink" height="30"><th>아이디</th><th>이름</th><th>생일</th><th>주소</th><th>액션</th></tr>
  	<c:set var="memberList" value="${requestScope.memberList}"/>
  	<c:forEach var="member" items="${memberList}">
  		<tr height="25"><td>${member.id}</td>
  		<td>${member.name}</td>
  		<td>${member.birthday}</td>
  		<td>${member.address}</td>
  		<td>&nbsp;<button onclick="location.href='memberProcServlet?action=update&id=${member.id}'">수정</button>&nbsp;
  			<button onclick="location.href='memberProcServlet?action=delete&id=${member.id}'">삭제</button>&nbsp;</td></tr>
  	</c:forEach>
  	</table>
  	<c:set var="memberPageList" value="${requestScope.memberPageList}"/>
  	<c:forEach var="pageNo" items="${memberPageList}">
  		${pageNo}
  	</c:forEach>
  	</center>
  </body>
  ```

  - __bbsList.jsp__

  ```html
  <body>
  	<center>
  	<h3>게시판</h3>
  	${memberName} 회원님 반갑습니다.<br>
  	<a href="bbsWrite.jsp">글쓰기</a>&nbsp;&nbsp;
  	<a href="twitter_list.jsp">트윗</a>&nbsp;&nbsp;
  	<a href="loginMain.jsp">회원목록</a>&nbsp;&nbsp;
  	<a href="memberProcServlet?action=logout">로그아웃</a>
  	<hr>
  	<table border="1" style="border-collapse:collapse;" width=700>
  	<tr bgcolor="skyblue" height="30"><th>글번호</th><th>제목</th><th>글쓴이</th><th>최종수정일</th><th>액션</th></tr>
  	<c:set var="bmList" value="${requestScope.bbsMemberList}"/>
  	<c:forEach var="bm" items="${bmList}">
  		<tr height="25"><td>${bm.id}</td>
  		<td><a href="bbsServlet?action=view&id=${bm.id}">${bm.title}</a></td>
  		<td>${bm.name}</td>
  		<td>${bm.date}</td>
  		<td>&nbsp;<button onclick="location.href='bbsServlet?action=update&id=${bm.id}'">수정</button>&nbsp;
  			<button onclick="location.href='bbsServlet?action=delete&id=${bm.id}'">삭제</button>&nbsp;</td></tr>
  	</c:forEach>
  	</table>
  	<c:set var="pageList" value="${requestScope.pageList}"/>
  	<c:forEach var="pageNo" items="${pageList}">
  		${pageNo}
  	</c:forEach>
  	</center>
  </body>
  ```

  - __bbsUpdate.jsp__

  ```html
  <body>
	<center>
	<h3>게시글 수정</h3>
	<hr>
	<c:set var="bm" value="${requestScope.bbsMember}"/>
	<table border="1" style="border-collapse:collapse;">
	<tr bgcolor="skyblue"><th height="30" width="80">항목</th><th width="300">내용</th></tr>
		<form name="updateBbs" action="bbsServlet?action=execute&id=${bm.id}" method=post>
		<tr><td height="25">번호</td><td>${bm.id}</td></tr>
		<tr><td height="25">제목</td><td><input type="text" name="title" size="40" value="${bm.title}"></td></tr>
		<tr><td height="25">글쓴이</td><td>${bm.name}</td></tr>
		<tr><td height="25">수정일시</td><td>${bm.date}</td></tr>
		<tr><td height="100">내용</td><td><textarea name="content" rows="10" cols="42">${bm.content}</textarea></td></tr>
		<tr><td colspan="2" height="30">
				<input type="submit" value="수정하기" name="U1">&nbsp;&nbsp;
				<input type="reset" value="재작성" name="U2"></label>
		</td></tr>
		</form>
	</table>
	<a href="bbsServlet?action=list&page=${currentBbsPage}">목록으로</a>
	</center>
  </body>
  ```

  - __bbsView.jsp__

  ```html
  <body>
  	<center>
  	<h3>게시글 상세보기</h3>
  	<hr>
  	<c:set var="bm" value="${requestScope.bbsMember}"/>
  	<table border="1" style="border-collapse:collapse;" width="500">
  	<tr bgcolor="skyblue"><th height="30" width="80">항목</th><th>내용</th></tr>
  		<tr><td height="25">번호</td><td>${bm.id}</td></tr>
  		<tr><td height="25">제목</td><td>${bm.title}</td></tr>
  		<tr><td height="25">글쓴이</td><td>${bm.name}</td></tr>
  		<tr><td height="25">수정일시</td><td>${bm.date}</td></tr>
  		<tr><td height="100">내용</td><td>${bm.content}</td></tr>
  	</table>
  	<br>
  	<button onclick="location.href='bbsServlet?action=update&id=${bm.id}'">수정</button>&nbsp;&nbsp;
  	<button onclick="location.href='bbsServlet?action=delete&id=${bm.id}'">삭제</button>&nbsp;&nbsp;
  	<a href="bbsServlet?action=list&page=1">목록으로</a>
  	</center>
  </body>
  ```

  - __twitter_list.jsp__

  ```html
  <body>
	<div align=center>
	<H3>My Simple Twitter!!</H3>
	<a href="loginMain.jsp">회원 목록으로</a>
	<HR>
	<form action="/jspbook/member/twitServlet" method="POST">
		<!-- 세션에 저장된 이름 출력 -->
		@${memberName}&nbsp;
		<input type="text" name="msg">&nbsp;&nbsp;<input type="submit" value="트윗">
	</form>
	<HR>
	<div align="left">
	<UL>
	<c:set var="msgs" value="${applicationScope.msgs}" />
	<c:if test="${msgs ne null}">
		<c:forEach var="msg" items="${msgs}">
			<li>${msg}</li>
		</c:forEach>
	</c:if>
	</UL>
	</div>
	</div>
  </body>
  ```

  - __update.jsp__

  ```html
  <body>
	<h3>회원수정</h3>
	<hr>
	<form name="updateForm" action="/jspbook/member/memberProcServlet?action=execute" method=post>
	<c:set var="member" value="${requestScope.member}" />
		<input type="hidden" id="id" name="id" value="${member.id}">
		<label><span>아이디:</span>
			${member.id}</label><br>
		<label><span>이름:</span>
			<input type="text" name="name" value="${member.name}" size="10"></label><br>
		<label><span>생일:</span>
			<input type="text" name="birthday" value="${member.birthday}" size="10"></label><br>
		<label><span>주소:</span>
			<input type="text" name="address" value="${member.address}" size="20"></label><br>
		<label><span></span><input type="submit" value="수정" name="B1">&nbsp;&nbsp;
			<input type="reset" value="재작성" name="B2">
	</form>
  </body>
  ```
