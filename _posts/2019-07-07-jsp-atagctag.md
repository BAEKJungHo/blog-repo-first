---
title: "a태그 안에 c:url"
layout: post
category: JSP
tags: [JSP]
excerpt: "a태그 안에 c:url이 있는 코드를 해석해 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

## a태그 안에 c:url

  ```html
  <a href="<c:url value="/boardList" />">
  ```

  코드가 위와 같을 경우 이 코드의 의미는 다음과 같습니다.

  웹 site 주소가 http://localhost:8080/board/ 이게 기본일 경우 위 코드는 다음과 같이 변합니다.

  즉, 기본경로/contextPath를 기본으로 설정해주고 뒤에는 컨트롤러를 찾을 @RequestMapping의 value값과 일치시키면 됩니다.
  따라서 위 코드는 아래의 코드랑 의미가 같습니다.

  ```html
  <a href="http://localhost:8080/board/boardList">
  ```

## form 태그 안에 c:url

  a태그와 마찬가지로 방식이 동일합니다.

  ```html
  <form name="searchForm" action="<c:url value="/boardSearchList" />" method="post" >
  ```
