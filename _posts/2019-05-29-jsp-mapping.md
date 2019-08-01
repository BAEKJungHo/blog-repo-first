---
title: "Mapping"
layout: post
category: JSP
tags: [JSP, Servlet]
excerpt: "JSP와 Servlet의 Mapping에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Mapping

  Mapping을 예시를 들어 설명 하겠습니다.

  - 프로젝트명 : JspBook
    - Java Resources
      - 패키지명 : section
        - 서블릿명 : AddtionServlet.java
          - 매핑 : @WebServlet("/AdditionServlet")
    - WebContent
      - 폴더명 : calculate
        - JSP명 : add.jsp

  다음과 같은 구조로 매핑과 JSP, Servlet파일을 만들었다고 가정할때 Servlet에서
  코드를 작성하고 서블릿을 실행하여 add.jsp로 값을 넘겨서 출력하는 프로그램을 만든다고 할 때
  조심해야할 점은 다음과 같습니다.

  - 파일이 `WebContent` 바로 밑에 있는 경우에서의 매핑

  ```java
  RequestDispatcher rd = request.getRequestDispatcher("add.jsp");
  ```

  - 파일이 `WebContent/패키지` 패키지 안에 있는 경우에서의 매핑

  ```java
  RequestDispatcher rd = request.getRequestDispatcher("/패키지명/add.jsp");
  ```

  - 파일이 `WebContent/패키지1/패키지2` 패키지 안에 있는 경우에서의 매핑

  ```java
  RequestDispatcher rd = request.getRequestDispatcher("/패키지1/패키지2/add.jsp");
  ```

## Anchor & Form

  앵커태그와 폼태그에서의 매핑 예시를 보겠습니다.

  - 프로젝트명 : FulfillmentService
    - Java Resources
      - 패키지명 : control
        - 서블릿명 : AdminProc.java
          - 매핑 : @WebServlet("/control/adminServlet")
    - WebContent
      - 폴더명 : view
        - 폴더명 : storage
          - JSP명 : storageShipping.jsp

  - Anchor 태그에서의 매핑

  ```html
  <a class="btn btn-primary btn-xs" href ="/FulfillmentService/control/adminServlet?action=release" role="button">출고</a>
  ```

  - Form 태그에서의 매핑

  ```html
  <form action="/FulfillmentService/control/adminServlet?action=invoiceDaily" class="form-horizontal" method="post">
  </form>
  ```

## EXAMPLE

  - Servlet

  ```java
  import java.io.IOException;

  import javax.servlet.RequestDispatcher;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  /**
   * Servlet implementation class AdditionServlet
   */
  @WebServlet("/AdditionServlet")
  public class AdditionServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public AdditionServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int num1 = 20;
		int num2 = 10;
		int add = num1 + num2;
		System.out.println("test");
		request.setAttribute("num1", num1);
		request.setAttribute("num2", num2);
		request.setAttribute("add", add);
    // WebContent 밑에 패키지를 만들어서 그 안에 JSP파일을 저장한 경우
    // 다음과 같이 /패키지명/JSP파일명.jsp로 매핑을 해주어야 한다.
		RequestDispatcher rd = request.getRequestDispatcher("/calculate/add.jsp");
		rd.forward(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	 }
  }
  ```
