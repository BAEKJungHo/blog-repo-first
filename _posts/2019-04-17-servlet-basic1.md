---
title: "Servlet Basic"
layout: post
category: Servlet
tags: [JSP, Servlet]
excerpt: "Servlet에 대해 배우고 공부합시다."
author: BAEKJungHo
---

* content
{:toc}

JSP, Servlet 공부를 하면서 참고한 서적입니다.

_프로젝트로 배우는 자바 웹 프로그래밍_

## Servlet Basic 1

  Servlet의 기초적인 것에 대해 배우고 공부합시다. Servlet은 자바 플랫폼에서 컴포넌트
  기반 웹 어플리케이션을 개발하기 위한 핵심 기술 입니다.

## API Class Diagram

  ![servlet](/images/posts/201904/servlet.jpg)

  위 사진은 서블릿 API구조를 나타내는 클래스 다이어그램 입니다.

  전체 구조는 위 사진보다 훨씬 복잡하지만 프로그래머로서 위 구조 정도는 반드시 알아야 합니다.
  그림에서 볼 수 있듯이 모든 Servlet은 javax.servlet.Servlet 인터페이스를 구현해야 합니다.

  일반적으로는 미리 정의된 `javax.servlet.http.HttpServlet`클래스를 상속해서 구현합니다.
  GenericServlet을 상속 받은 하위 클래스로서 HTTP 처리와 관련된 부가 기능이 추가되었습니다.

  즉 개발자가 서블릿.java를 만들때 `public class MemberServlet extends HttpServlet`이 방식과 같이
  HttpServlet을 상속받게 만들면 됩니다.

  그리고 클라이언트 요청에 따라 `service()` 메서드를 호출하고 service()메서드는
  요청 방식이 get인지 post인지에 따라 doGet(), doPost()메서드를 호출합니다.

  __개발자는 doGet(), doPost()만 구현__ 하면되고, service()는 컨테이너에서 자동으로 호출합니다.

## Life Cycle

  서블릿 생명 주기(Servlet Life Cycle)는 다음과 같이 구성 되어있습니다.

  ![life](/images/posts/201904/life.jpg)

  처음 한번만 실행되는 서블릿 초기화를 담당하는 init() 메서드는 클라이언트 요청이 왔을때
  해당 서블릿이 메모리에 있는 지 확인하고 초기화 작업을 수행합니다.
  만약 각각의 스레드에서 공통적으로 사용하기 위한 필요한 작업이 있으면 init() 메서드를
  오버라이딩 하여 구현하면됩니다.

  그리고 요청/응답을 위한 service() 메서드는 doGet()이나 doPost()를 실행하게 됩니다.

  ```java
  // 사용자 요청 처리 : request 객체
  // 응답 처리 : response 객체
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { }
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { }
  ```

  doGet()과 doPost()는 위 처럼 구성됩니다.

  그리고 컨테이너로부터 서블릿 종료 요청이 있으면, destroy() 메서드를 호출합니다. init()과 마찬가지로
  한 번만 수행됩니다.

## 특징

  JSP를 고급 활용하기 위해서는 Servlet을 잘 다룰 줄 알아야 합니다. 또한 Servlet은
  처음부터 순수 자바코드로 작성합니다. 따라서 자바가 제공하는 기능을 거의 다 사용할 수 있습니다.
  그리고 서블릿은 Tomcat(컨테이너)에 의해 생명주기(Life Cycle)가 관리됩니다.

  MVC 패턴을 쉽게 적용할 수 있고 컨테이너와 밀접한 서버 프로그램을 구현할 수 있으며,
  MVC 패턴을 적용할 때 Contents와 Business Logic을 분리할 수 있습니다.

  Spring FrameWork에서 내부적으로 서블릿을 사용합니다.

  Servlet을 생성할때 조심 해야할점은 다음과 같습니다.

  `Html파일에서의 form태그에서 action에 있는 경로명`과 Servlet의  @WebServlet("/경로명");
  이 비슷해야 합니다.

  ```java
  // html 파일의 form태그 action 경로명
  // 프로젝트폴더명/Servlet파일명
  <form name="form1" action=/jspbook/CalcServlet method=post>

  // Servlet파일명
  @WebServlet("/CalcServlet")
  ```  

  /CalcServlet 앞에 생략되어있는 경로는 `localhost:8080/프로젝트폴더명/` 입니다.

  위 두개의 경로를 잘 맞춰줘야 프로그램이 잘 동작합니다.

  Servlet에서도 jsp와 같이 [한글 깨짐 현상](https://baekjungho.github.io/jsp-basic1)을 다음과 같이 해결 할 수 있습니다.

  ```java
  request.setCharacterEncoding("UTF-8");
  ```

  위 코드를 입력해주면 한글 깨짐 현상이 사라집니다.

  - __서블릿 동작과정__

    서블릿 동작과정은 다음과 같습니다. 컨테이너(Tomcat)이 서블릿 클래스를 로딩하면
    서블릿 클래스(Servlet.class)가 생성되고 컨테이너에서 서블리 인스턴스를 생성하고
    init() 메소드가 초기화 작업을 위해 딱 한번만 실행됩니다.
    그리고 사용자 요청에 따라 service()메소드가 반복적으로 수행되고, 그 다음 doGet()이나
    doPost()등의 메서드들이 호출되며, 상태 종료가 될 때
    destroy()메소드가 호출 되면서 서블릿이 없어집니다.

## 구현

  [Servelt으로 Controler 구현 및 MVC 동작과정 이해하기](https://baekjungho.github.io/jsp-question1)

  서블릿 개발에 필요한 클래스들은 주로 `javax.servlet`에 있으며, 일부는 `javax.servlet.http` 패키지에 있습니다.

  다음은 국어, 영어, 수학점수를 입력받고 합계와 평균을 내는 프로그램을 만들어 보겠습니다.

  - __EXAMPLE__

  먼저 html 파일입니다. 프로그램 시작(Run)은 html파일로 시작해야 합니다.

  ```html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>점수 계산</title>
  </head>
  <body>
  	<center>
  	<h3>점수 계산</h3>
  	<hr>
  	<form name="form1" action=/jspbook/ScoreServlet method=post>
  		국어<input type="text" name="kor" width="200" size="10"><br>
  		영어<input type="text" name="eng" width="200" size="10"><br>
  		수학<input type="text" name="math" width="200" size="10"><br>
  		<input type="submit" value="계산" name="B1">
  		<input type="reset" value="다시입력" name="B2">
  	</form>
  </body>
  </html>
  ```

  다음은 java 파일입니다.

  ```java
  public class Score {
	 int sum(int kor, int eng, int math) {
		 return kor + eng + math;
	 }
	 double avg(int kor, int eng, int math) {
		 return (double)(kor+eng+math)/3;
	 }
  }
  ```

  다음은 Servlet파일 입니다.

  ```java
  import java.io.IOException;
  import java.io.PrintWriter;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  @WebServlet("/ScoreServlet") // 이 부분이 form 태그의 action 경로와 같아야 합니다. 단 프로젝트명은 제외
  public class ScoreServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public ScoreServlet() {}

  // form 태그에서 post방식을 사용한다고 했으므로 post만 구현하면됩니다.
  // 단, 반대쪽인 doGet에서 doPost를 호출해야 합니다.
  // 왜냐하면 다른 방식의 요청이 생길경우 서버 오류가 발생할 수 있으니 예방차원에서 호출하는 것입니다.
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doPost(request, response);
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int kor, eng, math;
		int sum;
		double avg;

		response.setContentType("text/html; charset=UTF-8");

		PrintWriter out = response.getWriter();
		kor = Integer.parseInt(request.getParameter("kor"));
		eng = Integer.parseInt(request.getParameter("eng"));
		math = Integer.parseInt(request.getParameter("math"));

		Score score = new Score();
		sum = score.sum(kor, eng, math);
		avg = score.avg(kor, eng, math);

		out.println("<html>");
		out.println("<head><title>점수 계산</title></head>");
		out.println("<body><center>");
		out.println("<h2>계산 결과</h2>");
		out.println("<hr>");
		out.println("합계 : " + sum);
		out.println("평균 : " + avg);
		out.println("</body></html>");
	 }
  }
  ```

  위 쪽의 `@WebServlet("/ScoreServlet")` 어노테이션은 서블릿 매핑을 위한 것이며
  이 클래스가 서블릿이라는 것을 알립니다. [serialVersionUID](https://baekjungho.github.io/java-basic6)는
  객체 직렬화를 나타냅니다.

  doGet()메서드와 doPost()메서드는 각 요청마다 다른 처리가 필요하면 두 가지 모두를 구현해야 하고
  한 가지 방식으로 처리할 수 있으면, 한 가지만 작성하면됩니다. 단, 다른 방식의 요청이 생길 경우
  서버 오류가 발생 할 수 있어서 doPost()를 구현하면 doGet()에서는 doPost() 메서드를 호출하는 방식으로
  위 처럼 사용 합니다.

  그리고 서블릿에서 사용자 요청에 따른 응답(response)을 보낼 때는 PrintWriter 객체를 얻어서 사용합니다.
  HttpServletResponse의 getWriter() 메소드를 통해 얻을 수 있습니다.

  request.getParameter("폼 태그 이름")로 폼 태그에 대한 데이터를 가져오고 PrintWriter 객체로
  HTML 태그를 사용하여 출력합니다.

  서블릿은 HTML처럼 웹 서버가 바로 해석할 수 있는 형태가 아니라서 서블릿 클래스와 웹 콘텐츠에 대한 접근 URL을 서로 매핑 시켜줘야 합니다. 이부분이 상당히 까다로운 부분입니다.

  - __HttpServletRequest Method__

    |메소드명   |특징|
    |-----------|----|
    |getParameterNames()|  매개변수 이름을 열거 형태로 넘겨줍니다.|
    |getParameter(name)|  HTML에서 문자열 name과 같은 이름을 가진 매개변수 값을 가져옵니다.|
    |getParameterValues(name)|  문자열 name과 같은 이름을 가진 매개변수 값을 배열 형태로 가져옵니다. 주로 checkbox, multiple list등에 사용합니다.|
    |getCookies()|  모든 쿠키값을 javax.servlet.http.Cookie의 배열형태로 가져옵니다.|
    |getMethod()|  현재 요청이 Get인지 Post인지 파악해서 가져옵니다.|
    |getSession()|  현재 세션 객체를 가져옵니다.|
    |getRemoteAddr()|  클라이언트의 IP 주소를 알려줍니다.|
    |getProtocol()| 현재 서버의 프로토콜을 문자열 형태로 알려줍니다.|
    |setCharacterEncoding()| 현재 JSP에 전달되는 내용을 지정한 캐릭터셋으로 변환해줍니다. HTML폼에서 한글을 입력할대 정상적으로 처리하려면 반드시 필요합니다.|

    > request.setCharacterEncoding("UTF-8");

  - __HttpServletResponse Method__

    |메소드명   |특징|
    |-----------|----|
    |setContentType(type)| 문자열 형태의 type에 지정된 MIMI Type으로 Content Type을 설정합니다.|
    |setHeader(name, value)|  문자열 name의 이름으로 문자열 value 값을 헤더로 설정합니다.|
    |setDateHeader(name, date)| 문자열 name의 이름으로 date에 설정된 밀리세컨드 시간 값을 헤더에 설정합니다.|
    |sendError(status, msg)|  오류 코드를 설정하고 메시지를 보냅니다.|
    |sendRedirect(url)|  클라이언트 요청을 다른 페이지로 보냅니다.|
