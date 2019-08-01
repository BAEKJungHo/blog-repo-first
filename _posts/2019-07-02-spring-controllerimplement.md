---
title: "Controller 구현 방법"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 Controller 구현 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 Controller 구현 방법에 대해서 배워 봅시다.

## Controller 구현 방법

### Setting

  ![r1](/images/posts/201906/r1.jpg)

### HttpServletRequest 객체를 이용한 HTTP 전송 정보 얻기

  ![r4](/images/posts/201906/r4.jpg)

### @RequestParam 어노테이션을 이용한 HTTP 전송 정보 얻기

  ![r5](/images/posts/201906/r5.jpg)

### EXAMPLE

  - html

  ```html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
  	<h1>Member Join</h1>
  	<form action="/lec17/memJoin" method="post">
  		ID : <input type="text" name="memId" ><br />
  		PW : <input type="password" name="memPw" ><br />
  		MAIL : <input type="text" name="memMail" ><br />
  		PHONE : <input type="text" name="memPhone1" size="5"> -
  				<input type="text" name="memPhone2" size="5"> -
  				<input type="text" name="memPhone3" size="5"><br />
  		<input type="submit" value="Join" >
  		<input type="reset" value="Cancel" >
  	</form>
  	<a href="/lec17/resources/html/login.html">LOGIN</a> &nbsp;&nbsp; <a href="/lec17/resources/html/index.html">MAIN</a>
  </body>
  </html>
  ```

  - HttpServletRequest 객체를 사용

  ```java
  @RequestMapping(value="/memLogin", method=RequestMethod.POST)
  public String memLogin(Model model, HttpServletRequest request) {

  String memId = request.getParameter("memId");
  String memPw = request.getParameter("memPw");

  }
  ```

  - @RequestParam 어노테이션을 사용

  ```java
  @RequestMapping(value="/memLogin", method=RequestMethod.POST)
  public String memLogin(Model model, @RequestParam("memId") String memId, @RequestParam("memPw") String memPw) {

  // String memId = request.getParameter("memId"); 필요 없음
  // String memPw = request.getParameter("memPw"); 필요 없음

  }
  ```

  `@RequestParam` 어노테이션에는 `required 속성`을 지정할 수 있는데 true일 경우 반드시 값이 넘어와야 하며,
  넘어오지 않을 경우 ERROR를 발생시킵니다. 그리고 false일 경우는 넘어오지 않아도 상관 없습니다.
  false를 줄 경우 `defaultValue` 속성을 주어서 값이 넘어오지 않았을 때에 defaultValue 값을 기본값으로 설정할 수 있습니다.

  하지만 이 속성들은 잘 사용하지 않습니다. 왜냐하면 클라이언트에서 자바스크립트로 미리 체크를 하기 때문에 서버쪽에 이런 업무까지
  담당하게 하지는 않습니다.

  ```java
  // required = true
  @RequestMapping(value="/memLogin", method=RequestMethod.POST)
  public String memLogin(Model model, @RequestParam("memId") String memId, @RequestParam(value="memPw", required="true") String memPw) {
    .....
  }

  // required = false, defaultValue="1234"
  @RequestMapping(value="/memLogin", method=RequestMethod.POST)
  public String memLogin(Model model, @RequestParam("memId") String memId, @RequestParam(value="memPw", required="false", defaltValue="1234") String memPw) {
    .....
  }
  ```

### 커멘드 객체를 이용한 HTTP전송 정보 얻기

  ![r6](/images/posts/201906/r6.jpg)

  커맨드 객체를 이용하기 위해서는 Member에 관한 정보를 담고있는 Member.java를 만들고 그 안에는
  Member가 지니고 있는 속성과 그에 대한 getter, setter를 가지고 있는 자바 파일을 이용하는 것입니다.

  > 커맨드 객체는 Getter와 Setter가 필수로 있어야 한다.

  - Member.java

  ```java
  public class Member {
	private String memId;
	private String memPw;
	private String memMail;
	private String memPhone1;
	private String memPhone2;
	private String memPhone3;

	public String getMemId() {
		return memId;
	}
	public void setMemId(String memId) {
		this.memId = memId;
	}
	public String getMemPw() {
		return memPw;
	}
	public void setMemPw(String memPw) {
		this.memPw = memPw;
	}
	public String getMemMail() {
		return memMail;
	}
	public void setMemMail(String memMail) {
		this.memMail = memMail;
	}
	public String getMemPhone1() {
		return memPhone1;
	}
	public void setMemPhone1(String memPhone1) {
		this.memPhone1 = memPhone1;
	}
	public String getMemPhone2() {
		return memPhone2;
	}
	public void setMemPhone2(String memPhone2) {
		this.memPhone2 = memPhone2;
	}
	public String getMemPhone3() {
		return memPhone3;
	}
	public void setMemPhone3(String memPhone3) {
		this.memPhone3 = memPhone3;
	 }
  }
  ```

  위 필드명들은 html, jsp파일에 있는 input 혹은 form 태그의 name 속성과 일치합니다.

  - memJoin.html

  ```html
  <form action="/lec17/memJoin" method="post">
  ID : <input type="text" name="memId" ><br />
  PW : <input type="password" name="memPw" ><br />
  MAIL : <input type="text" name="memMail" ><br />
  PHONE : <input type="text" name="memPhone1" size="5"> -
      <input type="text" name="memPhone2" size="5"> -
      <input type="text" name="memPhone3" size="5"><br />
  <input type="submit" value="Join" >
  <input type="reset" value="Cancel" >
  </form>
  ```

  커맨드 객체를 이용할 경우 Controller에서는 다음과 같이 설정하면 됩니다.
  폼에 입력된 값들이 커맨드 객체를 통해서 Setter로 들어가서 값이 세팅되고 사용할 때만
  getter로 꺼내오면 됩니다.

  ```java
  @RequestMapping(value="/memJoin", method=RequestMethod.POST)
  public String memJoin(Member member) {
  // String memId = request.getParameter("memId"); // 필요 없음
  // String memPw = request.getParameter("memPw"); // 필요 없음
  // String memMail = request.getParameter("memMail"); // 필요 없음
  // String memPhone1 = request.getParameter("memPhone1"); // 필요 없음
  // String memPhone2 = request.getParameter("memPhone2"); // 필요 없음
  // String memPhone3 = request.getParameter("memPhone3"); // 필요 없음

  service.memberRegister(member.getMemId, member.getMemPw, member.getMemMail, member.getMemPhone1, member.getMemPhone2, member.getMemPhone3);

  // model.addAttribute("memId", memId); // 필요 없음
  // model.addAttribute("memPw", memPw); // 필요 없음
  // model.addAttribute("memMail", memMail); // 필요 없음
  // model.addAttribute("memPhone", memPhone1 + " - " + memPhone2 + " - " + memPhone3); // 필요 없음

  return "memJoinOk";
  }
  ```

  이제 ViewResolver가 적절한 View를 찾아서 데이터를 넘겨줘야 하는데 커맨드 객체를 사용할 때에는
  컨트롤러에서 model.addAttribute를 사용안해도 되니 View쪽에서도 코드가 살짝 바뀝니다. 즉, View쪽에서도
  커맨드 객체를 사용할 수 있습니다.

  - model.addAttribute를 사용한 경우의 View

  ```html
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
  <%@ page session="false" %>
  <html>
  <head>
	   <title>Home</title>
   </head>
   <body>
	    <h1> memJoinOk </h1>
	     ID : ${memId}<br />
	     PW : ${memPw}<br />
	     Mail : ${memMail} <br />
	     Phone : ${memPhone} <br />

	 <a href="/lec17/resources/html/memJoin.html"> Go MemberJoin </a>
  </body>
  </html>
  ```

  - 커맨드 객체를 사용한 경우의 View

  ```html
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
  <%@ page session="false" %>
  <html>
  <head>
     <title>Home</title>
   </head>
   <body>
      <h1> memJoinOk </h1>
       ID : ${member.memId}<br />
       PW : ${member.memPw}<br />
       Mail : ${member.memMail} <br />
       Phone : ${member.memPhone1} ${member.memPhone2} ${member.memPhone3} <br />

   <a href="/lec17/resources/html/memJoin.html"> Go MemberJoin </a>
  </body>
  </html>
  ```

  이때 커맨드 객체의 getter메소드가 작용하여 값을 가져 옵니다.

  > HTML OR JSP -> Controller : 커맨드 객체의 Setter 메소드가 자동으로 작용
  >
  > Controller -> HTML OR JSP : 커맨드 객체의 Getter 메소드가 자동으로 작용

## @ModelAttribute

  커맨드 객체를 이용할 때 바로 위 예제를 보면 JSP에서 `member.memId`이런 방식으로 접근하는 것이 보일 것입니다.
  이때 member를 mem이나 혹은 다른 것으로 줄여 쓰고싶은경우 `@ModelAttribute` 어노테이션을 사용하면 됩니다.

  > 즉, 별칭(alias)의 기능이 있습니다.

  ```java
  @RequestMapping(value="/memJoin", method=RequestMethod.POST)
  public String memJoin(@ModelAttribute("mem") Member member) {

  service.memberRegister(member.getMemId, member.getMemPw, member.getMemMail, member.getMemPhone1, member.getMemPhone2, member.getMemPhone3);

  return "memJoinOk";
  }
  ```

  ```html
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
  <%@ page session="false" %>
  <html>
  <head>
     <title>Home</title>
   </head>
   <body>
      <h1> memJoinOk </h1>
       ID : ${mem.memId}<br />
       PW : ${mem.memPw}<br />
       Mail : ${mem.memMail} <br />
       Phone : ${mem.memPhone1} ${member.memPhone2} ${member.memPhone3} <br />

   <a href="/lec17/resources/html/memJoin.html"> Go MemberJoin </a>
  </body>
  ```

  @ModelAttribute의 또 다른 기능중 하나는 @ModelAttribute가 메소드 위에 붙어 있는 경우는
  그 메소드는 공통으로 무조건 실행되는 메소드를 뜻합니다.

  즉, ControllerA 안에 A, B, C 메소드가 있고 A 메소드에 @ModelAttribute 어노테이션이 붙어있으면
  B를 실행하든 C를 실행하든 A는 무조건 실행됩니다.

  ```java
  @Controller
@RequestMapping("/member")
public class MemberController {

	@Autowired
	MemberService service;

	@ModelAttribute("serverTime")
	public String getServerTime(Locale locale) {

		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);

		return dateFormat.format(date);
	}

	@RequestMapping(value = "/memJoin", method = RequestMethod.POST)
	public String memJoin(Member member) {

		service.memberRegister(member);

		return "memJoinOk";
	}

	@RequestMapping(value = "/memLogin", method = RequestMethod.POST)
	public String memLogin(Member member) {

		service.memberSearch(member);

		return "memLoginOk";
	}
  }
  ```

  위 처럼 @ModelAttribute("serverTime") 붙어있는 getServerTime() 메소드는 공통으로 실행되는 메소드이며
  View에서 다음과 같이 사용할 수 있습니다.

  ```html
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
  <%@ page session="false" %>
  <html>
  <head>
	   <title>Home</title>
   </head>
   <body>
	    <h1> memLoginOk </h1>

	     ID : ${member.memId}<br />
	      PW : ${member.memPw}<br />

	<P>  The time on the server is ${serverTime}. </P>

	<a href="/lec19/resources/html/index.html"> Go Main </a>
  </body>
  </html>
  ```

## 데이터가 중첩 커맨드 객체를 이용한 List 구조인 경우

  ![b3](/images/posts/201906/b3.jpg)

  - View

  ```html
  <form action="/lec19/member/memJoin" method="post">
  ID : <input type="text" name="memId" ><br />
  PW : <input type="password" name="memPw" ><br />
  MAIL : <input type="text" name="memMail" ><br />
  PHONE1 : <input type="text" name="memPhones[0].memPhone1" size="5"> -
       <input type="text" name="memPhones[0].memPhone2" size="5"> -
       <input type="text" name="memPhones[0].memPhone3" size="5"><br />
  PHONE2 : <input type="text" name="memPhones[1].memPhone1" size="5"> -
       <input type="text" name="memPhones[1].memPhone2" size="5"> -
       <input type="text" name="memPhones[1].memPhone3" size="5"><br />
  AGE : <input type="text" name="memAge" size="4" value="0"><br />
  ADULT : <input type="radio" name="memAdult" value="true">Yes,
      <input type="radio" name="memAdult" value="false">No <br />
  GENDER : <input type="radio" name="memGender" value="M">Men,
       <input type="radio" name="memGender" value="W">Women<br />
  FAVORITE SPORT : <input type="checkbox" name="memFSports" value="soccer">Soccer,
      <input type="checkbox" name="memFSports" value="baseball">Baseball,
      <input type="checkbox" name="memFSports" value="basketball">Basketball,
      <input type="checkbox" name="memFSports" value="volleyball">Volleyball,
      <input type="checkbox" name="memFSports" value="billiards">Billiards <br />
  <input type="submit" value="Join" >
  <input type="reset" value="Cancel" >
  </form>
  ```

  다음과 같이 전화번호를 여러개 받아야 하는경우는 MemPhone이라는 커맨드 객체를 만들고 Member 커맨드 객체에서
  List<MemPhone> memPhones; 와 같이 사용하면 됩니다.

  - MemPhone.java

  ```java
  public class MemPhone {

	private String memPhone1;
	private String memPhone2;
	private String memPhone3;

	public String getMemPhone1() {
		return memPhone1;
	}
	public void setMemPhone1(String memPhone1) {
		this.memPhone1 = memPhone1;
	}
	public String getMemPhone2() {
		return memPhone2;
	}
	public void setMemPhone2(String memPhone2) {
		this.memPhone2 = memPhone2;
	}
	public String getMemPhone3() {
		return memPhone3;
	}
	public void setMemPhone3(String memPhone3) {
		this.memPhone3 = memPhone3;
	}
  }
  ```

  - Member.java

  ```java
  public class Member {

	private String memId;
	private String memPw;
	private String memMail;
	private List<MemPhone> memPhones;
	private int memAge;
	private boolean memAdult;
	private String memGender;
	private String[] memFSports;

	public String getMemId() {
		return memId;
	}
	public void setMemId(String memId) {
		this.memId = memId;
	}
	public String getMemPw() {
		return memPw;
	}
	public void setMemPw(String memPw) {
		this.memPw = memPw;
	}
	public String getMemMail() {
		return memMail;
	}
	public void setMemMail(String memMail) {
		this.memMail = memMail;
	}
	public List<MemPhone> getMemPhones() {
		return memPhones;
	}
	public void setMemPhones(List<MemPhone> memPhones) {
		this.memPhones = memPhones;
	}
	public int getMemAge() {
		return memAge;
	}
	public void setMemAge(int memAge) {
		this.memAge = memAge;
	}
	public boolean isMemAdult() {
		return memAdult;
	}
	public void setMemAdult(boolean memAdult) {
		this.memAdult = memAdult;
	}
	public String getMemGender() {
		return memGender;
	}
	public void setMemGender(String memGender) {
		this.memGender = memGender;
	}
	public String[] getMemFSports() {
		return memFSports;
	}
	public void setMemFSports(String[] memFSports) {
		this.memFSports = memFSports;
	 }
  }
  ```
  
## TIP

  가장 많이 사용되는 것은 `커맨드 객체`입니다.

  왜냐하면 코드의 양이 많이 줄어들기 때문입니다.

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
