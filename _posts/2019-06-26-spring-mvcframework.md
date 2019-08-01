---
title: "스프링 MVC 프레임워크"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 MVC 프레임워크 설계 구조와 사용 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 MVC 프레임워크 설계 구조와 사용 방법에 대해서 배워 봅시다.

## 전체적인 웹 프로그래밍 구조

  ![m10](/images/posts/201906/m10.jpg)

## 스프링 MVC 프레임워크 구조

  ![m3](/images/posts/201906/m3.jpg)

  ![mv1](/images/posts/201906/mv1.jpg)

  파란색 테두리의 핵심 적인 역할은 `ModelAndView 작업`이 핵심입니다. 빨간색 테두리의
  핵심적인 역할은 `DB와 연관된 작업`이 핵심입니다.

## 폴더 및 패키지 구성

  ![v1](/images/posts/201906/v1.jpg)

### 동작 방식

  ![v3](/images/posts/201906/v3.jpg)

  가장 먼저 `DispatcherServlet`이 클라이언트 요청을 `HandlerMapping`에게 던지면 HandlerMapping은 여러개의 `Controller` 중에서
  적합한 Controller를 선택하는 역할을 합니다. 그리고 다시 DispatcherServlet으로 와서 DispatcherServlet이 `HandlerAdapter`에게 갑니다.

  `HandlerAdapter`는 HandlerMapping에서 찾은 Controller에서 그 안에 있는 여러개의 메소드(Mehtod) 중에서 클라이언트 요청을 처리하기에
  가장 적합한 `메소드(Method)`를 찾아주는 역할을 합니다. Controller 객체의 메소드가 실행된 후 Controller 객체는 HandlerAdapter 객체에 `ModelAndView` 객체를 반환하는데 ModelAndView 객체에는 사용자 응답에 필요한 데이터정보와 뷰정보(JSP파일)가 담겨있다. 다음으로 HandlerAdapter 객체는 ModelAndView 객체를 다시 DispatcherServlet 객체에 반환합니다.

  그리고 DispatcherServlet으로 다시 와서 DispatcherServlet이 `ViewResolver`를 찾습니다.
  ViewResolver는 클라이언트 요청을 처리한 결과를 출력하기 위한 가장 적합한 `View`를 찾아주는 역할을 합니다.

  마지막으로 응답을 생성하여 View를 찾으면 그 View(.jsp 파일)를 클라이언트 쪽으로 보내 화면을 출력합니다.

## DispatcherServlet

  ![m4](/images/posts/201906/m4.jpg)

  > web.xml : 서블릿 등록 및 초기 파라미터 등록 작업 등

  servlet-class 태그안에 DispatcherServlet을 등록 해야합니다. 그리고 맨 아래쪽에 url-pattern 태그 안에
  `/(루트)`로 지정한 것은 위 스프링 MVC 프레임워크 구조를 보면 가장먼저 거쳐야 하는 곳이 `DispatcherServlet`이기 때문입니다.
  즉, 사용자의 모든 요청을 받기 위해서 서블릿 맵핑 경로를 `/(루트)`로 지정하는 것입니다.

  위 그림을 보면 `init-param(초기화 파라미터)` 태그 안에 param-value 태그의 마지막에 보면 `servlet-context.xml`이 있는데
  이것이 `스프링 설정 파일(스프링 IOC 컨테이너)`을 나타냅니다.

  즉, web.xml에서는 `DispatcherServlet`을 등록하고 `스프링 설정 파일`을 등록하여 스프링 IOC 컨테이너가 만들어져야 합니다.

  __스프링 컨테이너가 생성되면 HandlerMapping, HandlerAdapter, ViewResolver는 자동으로 생성이 됩니다.__

  -----------------------------------------------------------------------------

  web.xml에서 DispatcherServlet을 지정할 때 크게 2가지 방법으로 나뉩니다.

  ![m5](/images/posts/201906/m5.jpg)

  1. init-param(초기화 파라미터)에서 지정한 파일(servley-context.xml)을 이용해서 스프링 컨테이너 생성
  2. init-param(초기화 파라미터)에서 스프링 설정 파일을 지정하지 않은 경우, 서블릿 별칭을 이용해서 스프링 컨테이너 생성

## servelt-context.xml

  ![v4](/images/posts/201906/v4.jpg)

  앞에서 DispatcherServlet를 서블릿으로 등록하는 과정을 살펴봤다. 서블릿으로 등록 될 때 contextConfigLocation이름으로 초기화 파라미터를 servlet-context.xml로 지정하고 있는데 이때 지정된 servlet-context.xml파일이 스프링 설정의 역할을 하는 파일이다.

  - 스프링 설정 파일(servlet-context.xml)

  ![v5](/images/posts/201906/v5.jpg)

  - 맵핑 방식

  ![v6](/images/posts/201906/v6.jpg)

  servlet-context.xml의 맵핑 방식은 [JSP 맵핑 방식](https://baekjungho.github.io/jsp-mapping/)과 유사합니다.

## @Controller

  ![m6](/images/posts/201906/m6.jpg)

  DispatcherServlet 설정을 하고나서, Controller를 구현하기 위해서는 클래스명 위에 @Controller 어노테이션을 적어야 합니다.

  단, 적기전에 스프링 설정파일(servlet-context.xml)에서 `<annotation-driven />` 태그를 넣어줘야 합니다.

  ![v7-1](/images/posts/201906/v7-1.jpg)

  실제로 개발자가 구현해야하는 부분은 대부분 Controller, Business Logic(Service), DAO 부분입니다.

### EXAMPLE

  ```java
  package com.bs.lec14;

  import java.text.DateFormat;
  import java.util.Date;
  import java.util.Locale;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestMethod;

  /**
   * Handles requests for the application home page.
   */
  @Controller
  public class HomeController {

  	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);

  	/**
  	 * Simply selects the home view to render by returning its name.
  	 */
  	@RequestMapping(value = "/", method = RequestMethod.GET)
  	public String home(Locale locale, Model model) {
  		logger.info("Welcome home! The client locale is {}.", locale);

  		Date date = new Date();
  		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);

  		String formattedDate = dateFormat.format(date);

  		model.addAttribute("serverTime", formattedDate );

  		return "home";
  	}

  }
  ```

  위에서 model.addAttribute()로 Key와 Value를 지정했으므로 JSP 파일에서는 다음과 같이 사용할 수 있습니다.

  ```html
  ...
  <p> serverTime is ${serverTime} </p>
  ...
  ```

## @RequestMapping

  ![m7](/images/posts/201906/m7.jpg)

  ![v8](/images/posts/201906/v8.jpg)

  @Controller 어노테이션으로 컨트롤러 클래스를 정하고 나서, 그 안에 여러 메소드 들이 있는데, 그 여러 메소드들 중에서
  적합한 메소드를 선택해주는 역할을 담당하는 것이 HandlerAdapter입니다.

  HandlerAdapter가 Controller 안에 있는 여러개의 메소드 중에서 적합한 메소드를 선택하는 역할을 하는데 이때, 메소드 위에 @RequestMapping 어노테이션을 사용하면 위 그림과 같이 success라는 요청이 들어오면 그에 해당하는 메소드를 찾아서 실행 시켜 줍니다.

  위 그림에서 URL을 보면 ch08은 [contextPath](https://baekjungho.github.io/spring-wgcontextpath/)이며,
  success는 @RequestMapping 어노테이션 안에 적혀있는 경로를 나타냅니다.

  ![m8](/images/posts/201906/m8.jpg)

## ViewResolver

  ![m9](/images/posts/201906/m9.jpg)

  ViewResolver는 클라이언트 요청을 처리한 결과를 출력하기 위한 가장 적합한 `View`를 찾아주는 역할을 합니다.

  ViewResolver가 View를 찾는 방식은 다음과 같습니다.

  스프링 설정 파일에서 bean 태그안에 `InternalResourceViewResolveer`를 등록을 해야하고, @RequestMapping을 통해 반환(return)된 값과
  beans property 안에 있는 `prefix`와 `suffix`의 `value값`인 `.jsp`를 합쳐서 View 파일을 찾습니다.

  > JSP 파일명 : prefix+return값+suffix

  ![v9](/images/posts/201906/v9.jpg)

## Tomcat 버전에 따른 스펙

  > [http://tomcat.apache.org/whichversion.html](http://tomcat.apache.org/whichversion.html)
  
## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
  >
  > [https://m.blog.naver.com/scw0531/220988401816](https://m.blog.naver.com/scw0531/220988401816)
