---
title: "@RequestMapping"
layout: post
category: Spring
tags: [Spring]
excerpt: "@RequestMapping의 어노테이션을 이용한 매핑 방식에 대해서 알아 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Summary

  ![m7](/images/posts/201906/m7.jpg)

  HandlerAdapter가 Controller 안에 있는 여러개의 메소드 중에서 적합한 메소드를 선택하는
  역할을 하는데 이때, 메소드 위에 @RequestMapping 어노테이션을 사용하면 위 그림과 같이
  success라는 요청이 들어오면 그에 해당하는 메소드를 찾아서 실행 시켜 줍니다.

  위 그림에서 URL을 보면 ch08은 [contextPath](https://baekjungho.github.io/spring-wgcontextpath/)이며,
  success는 @RequestMapping 어노테이션 안에 적혀있는 경로를 나타냅니다.

### Model 타입의 파라미터

  ![m8](/images/posts/201906/m8.jpg)

  개발자는 Model 객체에 데이터를 담아서 DispatcherServlet으로 보낼 수 있고 DispatcherServlet으로 전달된 Model 데이터는 View에서 가공되어
  클라이언트한테 응답 처리 됩니다.

  그림에서 볼 수 있듯이 Controller에서 Model 객체에 데이터를 담으면 View 파일에서 `model.setAttribute()`로 사용할 수 있습니다.

## @RequestMapping

  > 컨트롤러의 구현 : 클라이언트 요청을 처리하기 위한 메서드를 구현
  >
  > 클라이언트는 URL로 요청을 전송
  >
  > 요청 URL을 어떤 메서드가 처리할 것인지 결정 하는 것이 @RequestMapping 입니다.

  - RequestMapping은 a태그나 form태그의 경로에 `{변수}?key=value`와 같이 값을 사용할 수 있습니다.
  - URL 경로 변수의 값을 타입에 맞게 알아서 변환 시켜 줍니다.

  - ![s25](/images/posts/201905/s25.jpg)

### 일반 경로 지정

  - @RequestMapping을 사용한 경로 지정

  ```java
  @Controller
  public class MainController {
	// @RequestMapping(value = "/", method = RequestMethod.GET)

	// URL Pattern Mapping으로 get이나 post방식 모두 사용 가능
	@RequestMapping("/")
	public String main(Model model) {
		// Model : 데이터를 담을 그릇 역할, map(K, V)
		// model.addAttribute("변수", "값")
		model.addAttribute("message", "저의 홈페이지 입니다.");
		return "main";
	 }
  }
  ```

  ![r2](/images/posts/201906/r2.jpg)

  - mehtod 방식 지정

  ```java
  @RequestMapping(value = "/", method = RequestMethod.GET)
  ```

  메소드 방식을 지정할때에 GET방식은 기본적으로 생략이 가능합니다.

  따라서 GET 방식은 주로 생략하고, POST 방식일 때에 RequestMethod.POST로 나타냅니다.

  만약 가독성이 떨어지는거 같다고 느끼시면, RequestMethod.GET을 적어주셔도 상관 없습니다.

  ![r3](/images/posts/201906/r3.jpg)

  - get방식 post방식 모두 사용

  ```java
  @RequestMapping("/")
  ```

### form 태그에서의 매핑

  @RequestMapping에 따른 form 태그에서의 매핑은 다음과 같습니다.

  - @RequestMapping

  ```java
  @RequestMapping(value="/memJoin", method=RequestMethod.POST)
  public String memJoin(Model model, HttpServletRequest request) {
  String memId = request.getParameter("memId");
  String memPw = request.getParameter("memPw");
  String memMail = request.getParameter("memMail");
  String memPhone1 = request.getParameter("memPhone1");
  String memPhone2 = request.getParameter("memPhone2");
  String memPhone3 = request.getParameter("memPhone3");

  service.memberRegister(memId, memPw, memMail, memPhone1, memPhone2, memPhone3);

  model.addAttribute("memId", memId);
  model.addAttribute("memPw", memPw);
  model.addAttribute("memMail", memMail);
  model.addAttribute("memPhone", memPhone1 + " - " + memPhone2 + " - " + memPhone3);

  return "memJoinOk";
  }
  ```

  - form 태그

  ```html
  <form action="/lec17/memJoin" method="post"> </form>
  ```

  즉, lec17은 contextPath이며, 뒤에 있는 memJoin은 @RequestMapping 의 value값이며,
  memJoin이라는 메소드를 가리킵니다.

  만약 폼 태그 경로가 다음과 같을경우(중간 경로 추가)에는 @RequestMapping의 경로도 추가해야 합니다.

  ```html
  <form action="/lec17/member/memJoin" method="post"> </form>
  ```

  ```java
  @RequestMapping(value="/member/memJoin", method=RequestMethod.POST)
  public String memJoin(Model model, HttpServletRequest request) { ..... }

  @RequestMapping(value="/member/memLogin", method=RequestMethod.POST)
  public String memLogin(Model model, HttpServletRequest request) { ..... }
  ```

  위 코드는 메소드가 2개 이상인 경우 `공통된 경로(중간경로)`를 메소드마다 @RequestMapping안에 넣어주는 것은 비효율적이므로
  이를 대체하기 위해서 클래스 위에 `@RequestMapping(/중간경로)`를 추가 해주면 됩니다.

  즉, Class의 @RequestMapping 어노테이션 + Method의 @RequestMapping 어노테이션 경로를 합쳐서 HanlderMapping이 적절한 컨트롤러를 찾고
  HandlerAdapter가 적절한 메소드를 찾아서 실행시키는 것입니다.

  - 전체 소스

  ```java
  @Controller
  @RequestMapping("/member")
  public class MemberController {

//	MemberService service = new MemberService();
//	@Autowired
	@Resource(name="memService")
	MemberService service;

	@RequestMapping(value="/memJoin", method=RequestMethod.POST)
	public String memJoin(Model model, HttpServletRequest request) {
		String memId = request.getParameter("memId");
		String memPw = request.getParameter("memPw");
		String memMail = request.getParameter("memMail");
		String memPhone1 = request.getParameter("memPhone1");
		String memPhone2 = request.getParameter("memPhone2");
		String memPhone3 = request.getParameter("memPhone3");

		service.memberRegister(memId, memPw, memMail, memPhone1, memPhone2, memPhone3);

		model.addAttribute("memId", memId);
		model.addAttribute("memPw", memPw);
		model.addAttribute("memMail", memMail);
		model.addAttribute("memPhone", memPhone1 + " - " + memPhone2 + " - " + memPhone3);

		return "memJoinOk";
	}

	@RequestMapping(value="/memLogin", method=RequestMethod.POST)
	public String memLogin(Model model, HttpServletRequest request) {

		String memId = request.getParameter("memId");
		String memPw = request.getParameter("memPw");

		Member member = service.memberSearch(memId, memPw);

		try {
			model.addAttribute("memId", member.getMemId());
			model.addAttribute("memPw", member.getMemPw());
		} catch (Exception e) {
			e.printStackTrace();
		}

		return "memLoginOk";
	}	 
  }
  ```

### 배열로 경로 지정

  경로를 한 메서드에서 처리하고 싶은 경우에는 배열로 경로 목록을 지정하면 됩니다.

  ```java
  @RequestMapping({"/main", "/index"})
  public String main(Model model) {
    ...
  }
  ```

### 클래스와 메서드에 지정

  클래스와 메서드에 @RequestMapping 을 지정하여 사용하면 클래스에 적용한 값과
  메서드에 적용한 값을 조합해서 매핑될 경로를 지정할 수 있습니다.

  ```java
  @Controller
  @RequestMapping("/main")
  public class MainController {
	// @RequestMapping(value = "/", method = RequestMethod.GET)

	// URL Pattern Mapping으로 get이나 post방식 모두 사용 가능
	@RequestMapping("/index")
	public String index(Model model) {
		...
	}

  @RequestMapping("/list")
  public String list(Model model) {
    ...
  }
  }
  ```

### HTTP 전송 방식 지정

  GET 방식인지, POST 방식인지 method 방식을 지정 할 수 있습니다.

  동일한 경로를 값으로 갖고 method 속성만 다를 수도 있습니다.

  ```java
  @Controller
  public class MainController {
  // @RequestMapping(value = "/", method = RequestMethod.GET)

  // URL Pattern Mapping으로 get이나 post방식 모두 사용 가능
  @RequestMapping("/main/list", method=RequestMethod.GET)
  public String index(Model model) {
    ...
  }

  @RequestMapping("/main/list", method=RequestMethod.POST)
  public String list(Model model) {
    ...
  }
  }
  ```

### @PathVariable을 사용한 경로 지정

  <a href="${path}/gugu.do?dan=3">구구단</a> 다음과 같은 방식으로 {변수}?key=value의 형태로 URL이 넘어가는 경우에
  @PathVariable을 사용하면 경로 변수의 값을 Parameter로 전달 받을 수 있습니다.

  ```java
  @RequestMapping("/members/{memberId}")
  public String memberDetail(@PathVariable String memberId, Model model) {
    ...
  }
  ```

  - 경로 변수는 한 개 이상 사용 가능

  ```java
  @RequestMapping("/members/{memberId}/orders/{orderId}")
  public String memberOrderDetail(
  @PathVariable ("memberId") String memberId,
  @PathVariable ("orderId") Long orderId, Model model) {
    ...
  }
  ```

## EXAMPLE

  먼저 예시를 들기 위해 include를 할 header.jsp, menu.jsp 그리고 메인페이지인 main.jsp를 만들고
  이것을 다루는 `MainController`를 만들겠습니다.

  - header.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>

  <c:set var="path" value="${pageContext.request.contextPath}" />
  <script src="http://code.jquery.com/jquery-3.3.1.min.js"></script>
  ```

  - menu.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  <c:set var="path" value="${pageContext.request.contextPath}" />
  </head>
  <body>
  <div style="text-align: center;">
  	<a href="${path}/">main</a>
  	<a href="${path}/gugu.do">구구단</a>
  	<a href="${path}/test.do">테스트</a>
  	<a href="${path}/member/list.do">회원관리 테스트</a>
  </div>
  </body>
  </html>
  ```

  - main.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  <%@ include file="include/header.jsp" %>
  </head>
  <body>
  <%@ include file="include/menu.jsp" %>
  <hr>
  <h2>${message}</h2>
  </body>
  </html>
  ```

  - MainController.java

  ```java
  package com.example.spring01.controller;

  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;

  @Controller
  public class MainController {
  	// @RequestMapping(value = "/", method = RequestMethod.GET)

  	// URL Pattern Mapping으로 get이나 post방식 모두 사용 가능
  	@RequestMapping("/")
  	public String main(Model model) {
  		// Model : 데이터를 담을 그릇 역할, map(K, V)
  		// model.addAttribute("변수", "값")
  		model.addAttribute("message", "저의 홈페이지 입니다.");
  		return "main";
  	}

  	@RequestMapping(value="gugu.do", method=RequestMethod.GET)
  	public String gugu(Model model) {
  		int dan = 7;
  		String result = "";
  		for(int i=1; i<=9; i++) {
  			result += dan + "X" + i + "=" + (dan*i) + "<br>";
  		}
  		model.addAttribute("result", result);
  		return "text/gugu";
  	}
  }
  ```

  여기서 Mapping을 하기 위해서 눈 여겨 봐야 할 것은 다음과 같습니다.

  1. menu.jsp의 a태그의 경로방식
  2. 컨트롤러의 @RequestMapping 어노테이션과 메소드의 리턴값

  ```java
  // menu.jsp
  <a href="${path}/">main</a>
  <a href="${path}/gugu.do?dan=3">구구단</a>
  <a href="${path}/test.do">테스트</a>
  <a href="${path}/member/list.do">회원관리 테스트</a>

  // MainController.java
  @RequestMapping("/")
  public String main(Model model) {
    // Model : 데이터를 담을 그릇 역할, map(K, V)
    // model.addAttribute("변수", "값")
    model.addAttribute("message", "저의 홈페이지 입니다.");
    return "main";
  }

  @RequestMapping(value="gugu.do", method=RequestMethod.GET)
	public String gugu(int dan, Model model) {
		String result = "";
		for(int i=1; i<=9; i++) {
			result += dan + "X" + i + "=" + (dan*i) + "<br>";
		}
		model.addAttribute("result", result);
		return "text/gugu";
	}
  ```

  이렇게 두 가지를 잘 살펴야 하는데 `pageContext.request.contextPath`방식을 사용해서
  menu.jsp의 a태그의 경로는 `${path}/gugu.do?dan=3` 처럼 되어있습니다.

  여기서 `gugu.do`는 @RequestMapping의 value값 입니다. gugu.do 뒤에 준 uri 값은

  @RequestMapping 을 통해서 자동으로 형 변환도 되면서 값을 구분 지어 줍니다.

  만약에 위 a태그 처럼 uri에 `?key=value`형태로 값을 줄 경우 gugu 메소드에서 매개변수로
  해당 value의 타입에 맞게 선언하면 됩니다.

  __자동으로 알아서 형 변환이 되기 때문에 value가 문자열이면 String, 정수면 int로 하면됩니다.__

  그리고 gugu 메소드의 return 값인 `text/gugu`는 src/main/webapp/WEB-INF/views/ 의 경로 아래에
  text라는 폴더를 만들고 gugu.jsp를 만들어야 정확히 매핑이 된다는 의미입니다.

  위에서 main메소드의 return 값인 `main`은 폴더를 만들지 않아도 되므로 views 바로 밑에 있는
  jsp 파일이라는 것을 알 수 있습니다.

## 참고

  http://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte2:ptl:annotation-based_controller
