---
title: "Interceptor vs Filter"
layout: post
category: Spring
tags: [Spring]
excerpt: "Interceptor와 Filter에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Interceptor와 Filter에 대해서 배워 봅시다.

## 차이점

  Filter와 Interceptor는 실행되는 시점과 설정하는 곳이 다릅니다.

  __Filter는 Web Application(web.xml)에 등록을 하고, Interceptor는 servlet-context.xml에 등록을 합니다.__

  Filter는 web.xml에서 설정을 하면 구현이 가능하지만, Interceptor는 설정은 물론 메서드 구현이 필요합니다.

  케어할 수 있는 영역(범위)가 다르다. Filter는 같은 웹 어플리케이션 내에서만 접근이 가능하며, Interceptor의 경우 스프링에서 관리되기 때문에 스프링내의 모든 객체에 접근이 가능합니다.

  Filter의 경우 주로 한글처리에 이용되며, Interceptor의 경우 "로그인 처리"에 이용이 됩니다.

  로그인처리에 Interceptor를 이용하는 이유는 만약 인터셉터를 이용하지 않고, 로그인 처리를 한다면, 게시물을 작성, 게시물 수정, 게시물 삭제 등 모든 요청마다 Controller에서 session을 통해 로그인 정보가 남아 있는지를 확인하는 코드 를 중복해서 입력해야 할 것입니다.

  하지만 인터셉터를 이용하면, A, B, C 작업(A,B,C 경로로 요청)을 할 경우에는 XXXInterceptor를 먼저 수행해 session에서 로그인 정보가 있는지 확인해 주는 역할을 한다면, 중복 코드가 확 줄어들 수 있을 것이다. 이러한 장점 때문에 사용합니다.

## Interceptor

  ![s2](/images/posts/201907/s2.jpg)

  > [https://justforchangesake.wordpress.com/2014/05/07/spring-mvc-request-life-cycle/](https://justforchangesake.wordpress.com/2014/05/07/spring-mvc-request-life-cycle/)

  인터셉터의 정식 명칭은 `핸들러 인터셉터(Handler Interceptor)`입니다. 클라이언트의 요청이 컨트롤러에 가기 전에 가로채고, 응답이 클라이언트에게 가기전에 가로챕니다. 즉, 인터셉터는 DispatcherServlet이 컨트롤러를 요청하기 전,후에 요청과 응답을 가로채서 가공할 수 있도록 해줍니다.

  스프링에서는 인터셉터를 지원하기 위해서 `HandlerInterceptor 인터페이스`와 `HandlerInterceptorAdapter 추상 클래스`를 지원합니다.

  HandlerInterceptorAdapter는 위의 인터페이스를 사용하기 쉽게 구현해 놓은 추상클래스를 뜻합니다.

  - HandlerInterceptorAdaptor의 3가지 메서드

  ```java
  /** preHandle()
  * Controller로 요청이 들어가기 전에 수행
  * handler : preHandle() 메서드를 수행하고 수행될 컨트롤러 메서드에 대한 정보를 담고 있다.
  */
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

  /** postHandle()
  * 컨트롤러의 메서드의 처리가 끝나 return 되고 화면을 띄워주는 처리가 되기 직전에 이 메서드가 수행
  * ModelAndView 객체에 컨트롤러에서 전달해 온 Model 객체가 전달됨으로 컨트롤러에서 작업 후
  * postHandle() 에서 작업할 것이 있다면 ModelAndView를 이용하면 된다.
  */
  postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)

  /** afterCompletion()
  * 컨트롤러가 수행되고 화면처리까지 끝난 뒤 호출된다.
  */
  afterCompletion()
  ```

### interceptor-context.xml 생성

  `webapp/WEB-INF/spring/appServlet/` 폴더 밑에 interceptor-context.xml 파일을 만듭니다.

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:beans="http://www.springframework.org/schema/beans"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

  <!-- Interceptor 설정 -->
  <mvc:interceptors>
    <!-- 공통 Interceptor -->
    <mvc:interceptor>
      <mvc:mapping path="/**" />
      <mvc:exclude-mapping path="/resources/**" />
      <beans:bean id="commonInterceptor"
        class="com.test.example.config.interceptor.CommonInterceptor"></beans:bean>
    </mvc:interceptor>
  </mvc:interceptors>

</beans:beans>
  ```

  모든 Path에서 Interceptor를 걸고 resources/ 폴더 밑은 제외 합니다.

  > Spring 3.2버전 부터는 exclude-mapping 기능을 제공 : 해당 interceptor로부터 제외

### servlet-context.xml에서 설정

  위 처럼 interceptor-context.xml을 생성해도 되고, 혹은 DispatcherServlet이 있는
  servlet-context.xml에서 인터셉터를 설정 해줘도 됩니다.

  예를들어 로그인 처리를 위한 인터셉터를 만들었다고 가정하면 아래 코드를 추가하면 됩니다.

  ```xml
<beans:bean id="LoginInterceptor" class="com.ga.common.interceptor.LoginInterceptor"></beans:bean>

<interceptors>
    <interceptor>
        <mapping path="/board/**"/>
        <beans:ref bean="LoginInterceptor"/>
    </interceptor>
</interceptors>
  ```

### LoginInterceptor 생성

  로그인 인터셉터를 구현한다고 하면 다음과 같이 하면 됩니다.

  ```java
  public class LoginInterceptor extends HandlerInterceptorAdapter{
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        HttpSession session = request.getSession();
        LoginVO loginVO = (LoginVO) session.getAttribute("loginVO");  
        // login 처리를 담당하는 사용자 정보를 담고 있는 객체를 가져옴
        // Object obj = session.getAttribute("login");
        // if (obj == null)

        /** return
        * true : 컨트롤러로 가게됨
        * false : 컨트롤러로 못가게 막음
        */
        if(loginVO == null) {
            response.sendRedirect("/main");
            return false;
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
        // TODO Auto-generated method stub
        super.postHandle(request, response, handler, modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        // TODO Auto-generated method stub
        super.afterCompletion(request, response, handler, ex);
    }    
  }
  ```

## Filter

  - 필터를 이용한 한글처리(web.xml)

  ```xml
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
  ```

## 참조

  > [https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/](https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/)
  >
  > [https://rongscodinghistory.tistory.com/2](https://rongscodinghistory.tistory.com/2)
  >
  > [https://to-dy.tistory.com/21](https://to-dy.tistory.com/21)
