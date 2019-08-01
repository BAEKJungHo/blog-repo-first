---
title: "Service 및 DAO 객체 구현 방법"
layout: post
category: Spring
tags: [Spring]
excerpt: "Service 및 DAO 객체 구현 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Service 및 DAO 객체 구현 방법에 대해서 배워 봅시다.

## Service 및 DAO 객체 구현 방법

### 전체적인 흐름

  ![o1](/images/posts/201906/o1.jpg)

### 웹 어플리케이션 준비

  ![o2](/images/posts/201906/o2.jpg)

  ![o3](/images/posts/201906/o3.jpg)

### 한글 처리

  ![o4](/images/posts/201906/o4.jpg)

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>

	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

  <!-- 한글처리를 위한 filter -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>
			org.springframework.web.filter.CharacterEncodingFilter     
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>   
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>  
			<param-value>true</param-value>
		</init-param>
	</filter>    

	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>                 
	</filter-mapping>

  </web-app>
  ```

  위에서 `filter 태그`가 바로 한글처리를 위한 것입니다.

### 서비스 구현

  ![o5](/images/posts/201906/o5.jpg)

  `@Service, @Repository, @Component` 어노테이션을 달고 있으면 Controller 클래스에서
  `@Autowired, @Resource` 어노테이션으로 자동주입을 할 수 있습니다.

  > 서비스 클래스에 사용하기 가장 좋은, 추천하는 어노테이션은 `@Service`

  ![mv1](/images/posts/201906/mv1.jpg)

  다음 그림을 보면 알 수 있듯이 Controller에서는 자동주입을 가진 Service 객체를 가지고 있고
  Service에서는 자동주입을 가진 Dao 객체를 가지고 있습니다.

  - Service Class

  ```java
  //@Service
  //@Service("memService")
  //@Component
  //@Component("memService")
  //@Repository
  @Repository("memService")
  public class MemberService implements IMemberService {

	@Autowired
	MemberDao dao;

  }
  ```

  - Controller Class

  ```java
  @Controller
  public class MemberController {

  //	MemberService service = new MemberService();
  //	@Autowired
	@Resource(name="memService")
	MemberService service;

  }
  ```

### DAO 구현

  ![o6](/images/posts/201906/o6.jpg)

  `@Repository, @Component` 어노테이션을 달고 있으면 Controller 클래스에서
  `@Autowired, @Resource` 어노테이션으로 자동주입을 할 수 있습니다.

  > DAO 클래스에 사용하기 가장 좋은, 추천하는 어노테이션은 `@Repository`

  - DAO Class

  ```java
  // @Component
  @Repository
  public class MemberDao implements IMemberDao {

	private HashMap<String, Member> dbMap;

	public MemberDao() {
		dbMap = new HashMap<String, Member>();
	 }
  }
  ```

  - Service Class

  ```java
  @Repository("memService")
  public class MemberService implements IMemberService {

	@Autowired
	MemberDao dao;
  
  }
  ```
## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
  >
  > [https://m.blog.naver.com/scw0531/220988401816](https://m.blog.naver.com/scw0531/220988401816)
