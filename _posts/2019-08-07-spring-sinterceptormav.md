---
title: "Interceptor"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링의 Interceptor에 대해서 자세히 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링의 Interceptor에 대해서 자세히 배워 봅시다.

## Interceptor

  인터셉터의 정식 명칭은 `핸들러 인터셉터(Handler Interceptor)`입니다. 클라이언트의 요청이 컨트롤러에 가기 전에 가로채고, 응답이 클라이언트에게 가기전에 가로챕니다. 즉, 인터셉터는 DispatcherServlet이 컨트롤러를 요청하기 전, 후에 요청과 응답을 가로채서 가공할 수 있도록 해줍니다.

  스프링에서는 인터셉터를 지원하기 위해서 `HandlerInterceptor` 인터페이스와 `HandlerInterceptorAdapter` 추상 클래스를 지원합니다. 따라서 인터셉터를 구현하기 위해서는 implements로 HandlerInterceptor 인터페이스를 구현하거나 HandlerInterceptorAdapter 추상 클래스를 상속 받아야 합니다.

  HandlerInterceptorAdapter는 위의 인터페이스를 사용하기 쉽게 구현해 놓은 추상클래스를 뜻합니다.

  > 참고 : [스프링의 RETURN TYPE에 대해서 공부하기](https://baekjungho.github.io/spring-modelandviewreturn/)

  - `HandlerInterceptorAdapter Method`

  ```java
/** preHandle()
* Controller로 요청이 들어가기 전에 수행
* handler : preHandle() 메서드를 수행하고 수행될 컨트롤러 메서드에 대한 정보를 담고 있다.
* preHandle() 의 결과가 return ture일 경우 : preHandle()에서 저장한 List에 저장된 참조 변수들을, HandlerMethod에서 받아서 쓸 수 있다.
* preHandle() 의 결과가 return false일 경우 : HandlerMethod를 타지 않는다.
* 핸들러 메서드에서 preHandle()에서 사용한 정보들을 꺼내 쓰는 방법
* 1. request.setAttribute("allowUrls", allowUrls);
* 2. 세션
* 3. context 만들기
*/
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

/** postHandle()
* 컨트롤러의 메서드의 처리가 끝나 return 되고 화면을 띄워주는 처리가 되기 직전에 이 메서드가 수행
* ModelAndView 객체에 컨트롤러에서 전달해 온 Model 객체가 전달됨으로 컨트롤러에서 작업 후
* postHandle() 에서 작업할 것이 있다면 ModelAndView를 이용하면 된다.
* EX) int pageIndex = (Integer) modelAndView.getModel().get("pageIndex");
*/
postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)

/** afterCompletion()
* 컨트롤러가 수행되고 화면처리까지 끝난 뒤 호출된다.
* 즉, View return 후에 수행되는 메서드
*/
afterCompletion()
  ```

## 인터셉터를 사용한 예제

  > [QueryString을 param 키워드를 이용해서 꺼내기](https://baekjungho.github.io/spring-querystring/)

## 참고

  > [인터셉터와 필터의 차이 공부하기](https://baekjungho.github.io/spring-interceptorfilter/)
