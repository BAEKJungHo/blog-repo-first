---
title: "JSP를 사용한 장바구니 구현"
layout: post
category: JSP
tags: [JSP, Servlet, MySQL]
excerpt: "JSP를 사용해서 장바구니를 구현해보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

session을 사용하여 장바구니 구현하는 방법을 배워보겠습니다.

## JSP를 사용한 장바구니 구현

  CartDTO를 만들어 품명과 개수를 저장할 수 있게 getter, setter메소드를 만들고
  login.jsp에서 로그인을 하면 selProduct에서 폼을 만들어 과일을 고를 수 있는
  select 태그와 수량을 입력하는 input text태그를 만들고 추가버튼을 만들어서
  추가버튼 클릭 시 add.jsp 컨트롤러가 처리를 하여 checkOut.jsp view로 보여지게 만드는 간단한
  프로그램을 만들어 보겠습니다.

  - __CartDTO__

  ```java
  public class CartDTO {
	private String productName;
	private int quantity;

	public String getProductName() {
		return productName;
	}
	public void setProductName(String productName) {
		this.productName = productName;
	}
	public int getQuantity() {
		return quantity;
	}
	public void setQuantity(int quantity) {
		this.quantity = quantity;
	 }  
  }
  ```

  - __login.jsp__

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>LOGIN</title>
  </head>
  <body>
  	<center>
  		<h2>로그인</h2>
  		<form name ="form1" method="post" action="selProduct.jsp">
  			<input type="text" name="username" />
  			<input type="submit" value="로그인" />
  		</form>
  	</center>
  </body>
  </html>
  ```

  - __selProduct.jsp__

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Product</title>
  </head>
  <%
  	request.setCharacterEncoding("UTF-8"); // euc-kr
  	String username = request.getParameter("username");
  	if (username.equals("")) {
  		out.println("<script>alert('로그인을 하세요');");
  		out.println("history.go(-1);</script>");
  	}
  	session.setAttribute("username", username); // 사용자 이름을 세션에 저장
  %>
  <body>
  	<center>
  		<h2>상품 선택</h2>
  		<hr>
  		<%= session.getAttribute("username") %>님이 로그인한 상태 입니다.
  		<hr>
  		<form name="form1" method="POST" action="add2.jsp">
  			<select name="product">
  				<option>사과</option>
  				<option>자몽</option>
  				<option>포도</option>
  				<option>딸기</option>
  				<option>망고</option>
  			</select>
  			&nbsp;&nbsp;수량: <input type="text" name="quantity" size="4" value="0">&nbsp;&nbsp;
  			<input type="submit" value="추가" />
  		</form>
  		<a href="checkOut2.jsp">계산</a>
  	</center>
  </body>
  </html>
  ```

  - __add.jsp__

  ```java
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.ArrayList, jspbook.ch06.*"%>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Add</title>
  </head>
  <body>
  <%
  	request.setCharacterEncoding("UTF-8");
  	String productname = request.getParameter("product");
  	int quantity = Integer.parseInt(request.getParameter("quantity"));
  	// productList라는 이름으로 세션 설정
  	ArrayList<CartDTO> list = (ArrayList<CartDTO>)session.getAttribute("productList");
    // productList 속성명이 없을 경우 null값을 리턴
  	if(list == null) {
      // ArrayList 객체를 생성하여 setAttribute로 해당 속성에 list값을 지정
  		list = new ArrayList();
  		session.setAttribute("productList", list);
  	}
  	CartDTO cart = new CartDTO();
  	cart.setProductName(productname);
  	cart.setQuantity(quantity);
  	list.add(cart);
  %>
  <script>
  alert("<%=productname %> 이(가) <%=quantity %>개 추가 되었습니다");
  history.go(-1);
  </script>
  </body>
  </html>
  ```

  - __checkOut.jsp__

  ```java
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.ArrayList, jspbook.ch06.*" %>
  <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  	<div align="center">
  	<H2>계산</H2>
  	<%=session.getAttribute("username")%>님이 선택한 상품 목록
  	<HR>
  	<%	// productList라는 이름으로 세션 설정
  		ArrayList<CartDTO> list = (ArrayList<CartDTO>)session.getAttribute("productList");
  		if(list == null) {
  			out.println("선택한 상품이 없습니다.!!!");
  		}
  		else {
  			for(CartDTO cart: list) {
  				out.println(cart.getProductName() + " " + cart.getQuantity() + "개<br>");
  			}
  		}
  	%>
  	</div>
  </body>
  </html>
  ```
