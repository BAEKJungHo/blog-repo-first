---
title: "c:url"
layout: post
category: JSP
tags: [JSP]
excerpt: "c:url이 있는 코드를 해석해 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

## c:url

  > `<c:url value="/xxx/xxx/xxx" />"`의 장점은 contextPath가 바뀌어도 따로 변경이나 수정해줄 필요가 없습니다. 왜냐하면 c:url은 기본적으로 contextPath를 포함시키기 때문입니다.

  ```html
  <a href="<c:url value="/boardList" />">
  ```

  위 코드의 의미는 다음과 같습니다.

  웹 site 주소가 http://localhost:8080/board/ 이게 기본일 경우 위 코드는 다음과 같이 변합니다.

  ```html
  <a href="http://localhost:8080/board/boardList">
  ```

  즉, 기본경로/contextPath를 기본으로 설정해주고 뒤에는 컨트롤러를 찾을 @RequestMapping의 value값과 일치시키면 됩니다.

## form 태그 안에 c:url

  a태그와 마찬가지로 방식이 동일합니다.

  ```html
  <form name="searchForm" action="<c:url value="/boardSearchList" />" method="post" >
  ```
