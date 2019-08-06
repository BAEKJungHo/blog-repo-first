---
title: "QueryString을 param 키워드를 이용해서 꺼내기"
layout: post
category: Spring
tags: [Spring]
excerpt: "QueryString을 param 키워드를 이용해서 꺼내는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > QueryString을 param 키워드를 이용해서 꺼내는 방법에 대해서 배워 봅시다.

##

  ```
  http://localhost:8080/xxx/categories?mno=08
  ```

  위와 같은 방식으로 쿼리스트링(QueryString)이 붙어있으면 jsp에서 `param.키값` 으로 꺼내서 쓸 수 있습니다.

  ```html
  <input type="hidden" name="mno" id="mno" value="<c:out vlaue="${param.mno}" />" />
  ```

  위 처럼 mno(메뉴번호)가 모든 페이지 마다 붙어서 가야한다면, `인터셉터(Interceptor)`를 이용해서, VO에 속성을 만들지 않고도, mno값을 유지시킬 수 있습니다.

  ```java
  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
      if(!"XMLHttpRequest".equals(request.getHeader("x-requested-with")) && modelAndView != null){ //ajax가 아닐때만 체크
          String mno = request.getParameter("mno");
          modelAndView.getModel().put("mno", mno);
      }
  }
  ```
