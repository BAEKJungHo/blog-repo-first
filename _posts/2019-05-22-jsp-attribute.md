---
title: "setAttribute & getAttribute"
layout: post
category: JSP
tags: [JSP]
excerpt: "setAttribute와 getAttribute에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## XXXattribute

  request.setParameter()와 request.getParameter()는 String으로 밖에 받지 못하지만
  setAttribute와 getAttribute는 `List`를 받을 수 있습니다.

  __즉, 특정한 단일한 값을 변수로 받으려면 Parameter, 객체를 받으려면 Attribute__

  > request.setAttribute("객체명", 객체); (key, value)
  >
  > request.getAttribute("객체명"); (key)

  setAttribute에서 `객체명(key)과 객체(value)`를 주면 getAttribute에서 `객체명(key)`로 값을
  받아오는 것입니다.

  ```java
  Object errorMessage = .....; // String errorMessage = .....;
  request.setAttribute("error", errorMessage); // Parameter 전달
  RequestDispatcher dispatcher = request.getRequestDispatcher("login.jsp");
  dispatcher.forward(request, response);
  ```

  ---------------------------------------------------------------------------

  위 처럼 Servlet에서 써주면 jsp 파일에서 `"객체명(key)"을 이용해서 객체(value)`를 받을 수 있다.
  여기서 객체값(value)은 타입이 Object입니다.
  따라서 String이 아닌 __Object로 선언해주는것이 중요__ 합니다.
  __만약 String으로 선언해준상태이면 (String)으로 형변환__ 을 해주어야 합니다.
  MemberDTO와 같은 클래스타입이면 클래스타입으로 형변환을 해주어야 합니다.

  > getAttribute를 사용하기 위해서는 형변환이 필수

  ---------------------------------------------------------------------------

  만약에 setAttribute(String name)으로 되어 있으면 String 클래스의 하위 클래스만 매개변수로
  사용할 수 있습니다.

  밑에는 Servlet에서 Parameter를 받아와서 에러메시지를 출력하는 코드입니다
  서블릿에서 request.setAttribute로 Parameter를 받아오면 JSP에서는 getAttribute로
  받아와야 합니다.

  > setParameter는 getParameter와 연결되고 setAttribute는 getAttribute와 연결됩니다.

  ```java
  < %
  Object error = request.getAttribute("error");
  // String error = (String)request.getAttribute("error");
  System.out.println(error);
   if(error != null) {
    out.println("<script>alert('" + error + "')</script>");
  }
  % >
  ```
