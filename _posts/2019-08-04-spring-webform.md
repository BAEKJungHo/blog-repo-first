---
title: "Spring MVC WebForm"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring MVC WebForm에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring MVC WebForm에 대해서 배워 봅시다.

  ![w1](/images/posts/201908/w1.jpg)

## RequestParam의 종류

  Request Parameter는 `HTTP Request Message` 안에 담겨서 보내진다.

  ![w2](/images/posts/201908/w2.jpg)

  Request Parameter는 2가지 방식으로 전달된다.

### GET(Query String)

  - url 뒤에 query string 형식으로 붙어서 보내진다.
  - Ex) 조회와 같이 DB를 변경하지 않는 작업에 이 방식을 사용한다.

  ![w3](/images/posts/201908/w3.jpg)

### POST(HTTP Entity Body)

  - HTTP Request Message의 Body에 담겨서 보내진다.
  - Ex) 암호화, 회원가입(password) 등 주로 DB를 변경하는 작업에 이 방식을 사용한다.

  ![w4](/images/posts/201908/w4.jpg)

## DataBinding

  request parameters가 form bean(=command object)에 바인딩되는 것을 말한다.

  ![w5](/images/posts/201908/w5.jpg)

### Native Solution(@RequestParam)

  > [@RequestParam 어노테이션 자세히 알아보기](https://baekjungho.github.io/spring-requestparam/)

  - @RequestParam 어노테이션 이용
    - method 인자에 request parameter을 binding 해준다.
    - 반드시 query string의 key 이름이 동일해야 값을 가져올 수 있다.
    - method 인자에 form bean을 선언하지 않는다.

  ```java
@RequestMapping("/docreate")
public String doCreate(@RequestParam("name") String name,
                    @RequestParam("email") String email,
                    Model model) {

   // we manually populate the Offer object with
   // the data coming from the user
   Offer offer = new Offer();

   offer.setName(name);
   offer.setEmail(email);
   ...
}
  ```

### Spring Data Binidng(자동 Binding)

  - method 인자에 form bean을 선언한다.
    - Spring이 자동으로 request paraters를 객체에 binding 해준다.

  ```java
@RequestMapping(value="/docreate", method=RequestMethod.POST)
public String doCreate(Offer offer) {
// offer object will be automatically populated
// with request parameters
}
  ```

### DataBinding 과정

  ![w6](/images/posts/201908/w6.jpg)

  - form bean 객체(Offer 객체)를 생성한다.
    - request parameter으로부터 form bean에 내용이 자동으로 binding된다.
    - 이때, form bean(POJO)에 setter method가 구현되어 있어야 binding이 가능하다.
    - 또한 controller의 method 인자에 form bean을 선언되어 있어야 한다.

  - controller에서 form bean을 자동으로 model에 넣어준다. (form bean은 model attribute이다.)
  - model을 view에 넘겨준다.
    - controller에서 form bean을 model에 넣어 view에 전달한다.

  - view는 form bean 내용에 접근할 수 있고 이 내용을 rendering 할 수 있다.
    - 즉, controller로부터 전달받은 model에 있는 form bean(Offer 객체, model attribute)를 사용하여 request parameter에 접근할 수 있다.

  ```html
<html>
<head>
	<title>Thanks</title>
</head>
<body>
    Hi, ${offer.name}.<br/>
    You have successfully registered.
</body>
</html>
  ```

## Data Validation

  사용자가 실수로 잘못된 정보를 입력했을 때 잘못된 양식임을 알려주기 위해 사용한다.

### Hibernate-Validator

  > [Hibernate-Validator 자세히 알아보기](https://baekjungho.github.io/spring-vhibernate/)

  - 사용자의 오류를 감지하기 위해 form bean에 캡슐화된 form data의 유효성을 검사해야 한다.

  - Bean Validation API(JSR-303)는 JavaBean 유효성 검사를 위한 API를 정의하는 명세서이다.
  - 이 명세의 구현체가 Hibernate Validator(라이브러리)이다.
  - 검증 제약 조건을 bean properties에 어노테이션을 달아 설정할 수 있다.
  - Ex) @NotNull, @Pattern, @Size, @Email 등

  - pom.xml에 추가

  ```xml
  <dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.2.4.Final</version>  
  </dependency>
  ```

  - EXAMPLE

  ```java
  public class Offer {
    private int id;

    @Size(min=5, max=100)
    @Pattern(regexp="^[A-Z]{1}[a-z]+$") // 정규표현(Regular Expression)
    private String name;

    @NotEmpty
    @Email
    private String email;
}
  ```

### Message Interpolation

  메시지 보간법(삽입 어구)은 위배된 Bean 유효성 검증 제약 조건에 대한 오류 메시지를 생성하는 것이다.

  - 메시지 속성을 통해 각 속성의 message descriptor를 정의할 수 있다.

  - EXAMPLE

  ```java
  public class Offer {

   private int id;

   @Size(min=3, max=100, message="Name must be between 3 and 100 characters")
   private String name;

   @Email(message="please provide a valid email address")
   @NotEmpty(message="the email address cannot be empty")
   private String email;

   @Size(min=5, max=100, message="Text must be between 5 and 100 characters")
   private String text;
 }
  ```

  - 문제점
    - message resources(불변)가 code 안에 들어가 있는 형태이므로 별도의 파일로 관리하는 것이 바람직하다.
    - 사용자의 location(지역)에 맞는 언어로 바꿔줘야 한다.

### Validating Object

  - `@Valid 어노테이션`
    - @Valid 어노테이션을 통해 Hibernate가 자동으로 유효성 검사를 한다.
    - Controller method의 인자 중 검증할 객체 인자 앞에 해당 어노테이션을 단다.
    - @Valid 어노테이션은 객체의 유효성을 먼저 확인한 다음 모델에 객체를 추가한다.

  - `BindingResult Object`
    - 유효성 검사 과정의 결과를 나타낸다.
    - 필요한 경우에 Controller method의 인자에 추가한다.
    - form bean과 같이 model에 들어가서 View에 넘겨준다.
    - 이를 통해 View는 error message를 출력할 수 있다.

  - 즉, @Valid에 의해 Spring이 자동으로 유효성을 검사하고, 그 결과인 BindingResult 객체를 자동으로 Model에 넣어준다.
    - form bean과 BindingResult가 포함된 Model을 View에서 사용할 수 있다.

  - EXAMPLE

  ```java
@RequestMapping(...)
public String doCreate(@Valid Offer offer, BindingResult result) {
    if(result.hasErrors()) {
	List<ObjectError> errors = result.getAllErrors();

       for(ObjectError error:errors) {
    		System.out.println(error.getDefaultMessage());
       }
       return "createoffer";
    }
    ...
}
  ```

## DataBuffering

  사용자가 잘못된 정보를 입력했을 때 처음부터 다시 입력할 필요가 없도록 입력한 정보를 view에 그대로 남아있게 해주는 것이다.

  ![w7](/images/posts/201908/w7.jpg)

### Spring Form Tag Library

  - 사용자의 입력 내용을 web form에 바인딩해야 한다.
    - Spring은 `data binding-aware tags` 세트를 제공한다.
    - 즉, 알아서 사용자가 입력했던 내용을 매칭해서 그려준다.

  - ‘spring form tag library’의 태그를 사용하려면 JSP 페이지의 맨 위에 다음 지시문을 추가해야 한다.
  - `<%@ taglib prefix="sf" uri="http://www.springframework.org/tags/form"%>`
  - prefix=”sf”: 내가 원하는 약어 설정

  - Spring form tag lib	 : HTML
    - `<sf:form>	: <form>`
    - `<sf:input>	: <input  type="text">`
    - `<sf:password> : <input  type="password">`
    - `<sf:checkbox> : <input  type="checkbox">`

### Revised JSP and Error Message

  ![w8](/images/posts/201908/w8.jpg)

  - ‘spring form tag library’ 태그 사용 지시문 선언
  - ‘spring form tag’ 사용
  - ‘spring form tag’ 사용
    - `<sf:errors>` 태그는 유효성 검사 후 BindingResult 객체로부터 받은 error message를 HTML에 랜더링한다.
  - Model에 들어 있는 model attribute의 이름
    - 빈 객체로 넘어온 offer라는 이름의 model attribute(Offer 객체)에 사용자가 입력한 데이터를 넣기 위한 설정
  - name=”name” -> path=”name”으로 사용
  - Offer 객체의 어떤 속성과 binding 할지를 명시

## 전체 과정

![w9](/images/posts/201908/w9.jpg)

## 참조

  > [https://gmlwjd9405.github.io/2019/01/02/spring-mvc-webform.html](https://gmlwjd9405.github.io/2019/01/02/spring-mvc-webform.html)
