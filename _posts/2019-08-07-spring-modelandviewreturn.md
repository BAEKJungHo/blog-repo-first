---
title: "RETURN TYPE OF SPRING"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링의 마지막에 확정되는 return Type에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링의 마지막에 확정되는 return Type에 대해서 배워 봅시다.

## RETURN TYPE

  결과 부터 말하자면, 스프링의 HandlerMethod에서  ModelAndView로 리턴하든, String이든 Model이든, 최종으로 리턴되는 타입은 `ModelAndView`입니다.
  따라서, `interceptor` 에서 핸들러 메서드의 ModelAndView로 저장된 값을 `postHandle()`에서 꺼내 쓸 수 있습니다.

  - 리턴타입이 String인 경우

  ```java
  @GetMapping
  public String list(Model model) {
    model.addAttribute("XXX", xxx);
    return "list";
  }
  ```

  String을 반환하는 HandlerMethod가 어떻게 ModelAndView로 리턴하냐면, `DispatcherServlet`을 통하는것은 동일 합니다. 이후 적용되는 `ViewNameMethodReturnValueHandler`를 참고하면 String 형식일 경우, `mavContainer`에 `ViewName`을 셋팅하는 작업을 하고 있음을 볼 수 있습니다. 이 코드에서 추가로 볼 수 있는 결국에 이렇게 mavContainer에 셋팅 된 이후 `RequestMappingHandlerAdapter`에서 아래와 같은 방식으로
  ModelAndView를 확정합니다.

  ```java
  ModelMap model = mavContainer.getModel();
  ModelAndView mav = new ModelAndView(mavContainer.getViewName(), model);
  ```

## 참조

  > [https://bistros.tistory.com/entry/스프링MVC에서-return-type이-String-일경우](https://bistros.tistory.com/entry/스프링MVC에서-return-type이-String-일경우)
