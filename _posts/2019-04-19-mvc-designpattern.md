---
title: "MVC"
layout: post
category: 디자인패턴
tags: [JSP, Servlet, 디자인패턴]
excerpt: "MVC DesignPattern에 대해 배우고 공부합시다."
author: BAEKJungHo
---

* content
{:toc}

## MVC DesignPattern

  소프트웨어 디자인 패턴중 하나인 [MVC(Model View Control)](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC) 디자인 패턴에 대해서 배워보겠습니다.
  웹 개발에 있어서 강력한 디자인 패턴이고 거의 이 방식으로 이루어집니다.
  MVC 패턴의 장점은 서로 분리되어 각자의 역할에 집중할 수 있게끔하여 개발을 하고 그렇게 애플리케이션을 만든다면, 유지보수성, 애플리케이션의 확장성, 그리고 유연성이 증가하고, 중복코딩이라는 문제점 또한 사라지게 되는 것입니다.

  [MVC를 배우기전에 DTO, DAO에 대해 모른다면 배우고 가는게 좋습니다.](https://baekjungho.github.io/jdbc-basic)

  ![model1](/images/posts/201904/model1.jpg)

  - __동작 과정__

    1. 사용자가 웹 사이트에 접속
    2. 사용자가 요청한 데이터 처리를 위해 Controller가 Model을 호출(DAO, DTO)
    3. 모델에서 DB에 접속하여 데이터 처리 후 Controller에게 반환
    4. Controller는 받은 데이터를 처리하여 View에 반환
    5. 사용자가 요청한 데이터를 처리한 화면(View)을 볼 수 있음

    > Model : DataBase Access / View : User Interface / Control : Business Logic

  - __구현 Tip__

    일반적으로 Controller를 만들때는 Servlet을 사용하고 jsp파일과 html파일은 View를 담당합니다.
    view를 구현할때 < jsp:useBean >이나 < jsp:getProperty >혹은 표현식을 사용했지만, `표현언어(EL)와 JSTL`을
    사용할 경우 더욱 편하게 View를 구현할 수 있습니다.
    Model은 DB와 관련해서 DAO, DTO파일이 있습니다. Model, View, Controller가 이클립스에서 저장되는 위치는 다음과 같습니다.

    > Model & Controller : JavaResources의 src밑으로 저장
    >
    > View : WebContent밑으로 저장

    View에서 <form>을 만든다는 것은 사용자가 입력한 데이터를 처리하겠다는 것이고 먼저 [GET/POST](https://baekjungho.github.io/servlet-basic1) 방식 중 하나를 선택해야 합니다. 그리고 Controller가 처리한 데이터가 화면에 정확히 보여지기 위해`(화면간의 데이터 전달)` URL의 주소를 맞춰주는
    [Mapping(매핑)](https://baekjungho.github.io/jsp-mvc1)을 잘해야 합니다. 매핑에서 잘 맞춰 줘야 하는 부분은 다음과 같습니다.

    > JSP, HTML : <form name = “loginForm” action=/jspbook/member/loginProcServlet method=post>
    >
    > Servlet : @WebServlet("/member/loginProcServlet")

    밑에는 추가적으로 알아두면 좋은 사항들입니다.

    > [setAttribute & getAttribute](https://baekjungho.github.io/jsp/jsp-mvc1#2)

## Business Logic

  MVC와 직접적으로 연관된 부분이 Business Logic이 있습니다.

  Business Logic이 궁금하신 분들은 해당 링크를 통해 공부 해주시기 바랍니다.

  > [Business Logic과 MVC 이해하기](https://baekjungho.github.io/business-logic/)
