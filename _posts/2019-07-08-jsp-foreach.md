---
title: "forEach"
layout: post
category: JSP
tags: [JSP]
excerpt: "JSTL 태그라이브러리에 있는 forEach 사용방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## forEach

  - 속성

  ![a7](/images/posts/201907/a7.jpg)

  - varStatus

  ![a8](/images/posts/201907/a8.jpg)

  스프링 MVC 게시판에서 검색과 페이징을 구현한 JSP 파일에서, 검색을 하게 되면 기존 번호가
  101, 88, 50, 35, 8 일경우, 검색 결과에 따른 5개 항목의 번호가 1, 2, 3, 4, 5로 다시 매겨져야 합니다.

  ```html
<c:forEach var="board" items="${map.searchList}" varStatus="loop">
<tr>
  <td>${loop.count}</td>
  <td><a href="<c:url value="/boardRead/${board.num}" />"> ${board.title}</a></td>
  <td>${board.name}</td>
  <td>${board.date}</td>
  <td>${board.count}</td>
</tr>
</c:forEach>
  ```

  varStatus="변수명" `변수명.count`를 사용하면 1부터 차례대로 증가해서 위 기능을 구현할 수 있습니다.
