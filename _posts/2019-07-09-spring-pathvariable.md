---
title: "@PathVariable"
layout: post
category: Spring
tags: [Spring]
excerpt: "@PathVariable 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @PathVariable 어노테이션에 대해서 배워 봅시다.

## @PathVariable

  컨트롤러의 메소드에서 @RequestMapping으로 View파일에서 a태그나 form태그에서 /boardList/{num}과 같이 게시물 번호 등, 일련 번호에 따른 매핑을 해줘야 할 경우 @RequestMapping 어노테이션에서 `{템플릿 변수}`를 사용합니다.

  이때 @PathVariable 어노테이션을 이용해서 `{템플릿 변수}` 와 동일한 이름을 갖는 파라미터를 추가하면 됩니다. @RequestMapping으로 템플릿 변수에 저장된 값은, @PathVariable 어노테이션이 적용된 동일한 이름을 갖는 파라미터에 적용이 됩니다.

  그리고 템플릿 변수명은 필드명, DB 컬럼명과도 같아야합니다.

  > @RequestMapping 템플릿 변수명 = @PathVariable 템플릿 변수명 = 필드명 = DB 컬럼명

  @PathVariable도 @ModelAttribute처럼 모델에 담아줍니다. `@RequestMapping("/fileManage/{type}")`로 설정한 type명으로 @PathVariable의 키값을 정해준다.

  ```java
  @GetMapping
    public String list(@PathVariable FileTypes type) {}
  ```

  @PathVariable에 담기는 Map이 있는데, 그쪽에 type이 담기면서, @ModelAttribute처럼 사용할 수 있다. 따라서 type 파라미터를 model에 담지 않아도 jsp에서 ${type}으로 사용할 수 있다.

## EXAMPLE

  - boardList.jsp

  ```html
<c:forEach var="board" items="${boardList}" varStatus="loop">
  <tr>
    <td>${board.num}</td>
    <td><a href="<c:url value="/boardRead/${board.num}" />"> ${board.title}</a></td>
    <td>${board.name}</td>
    <td>${board.date}</td>
    <td>${board.count}</td>
  </tr>
</c:forEach>
  ```

  - BoardController.java

  ```java
// 게시글 상세 내역
@RequestMapping(value="/boardRead/{num}")
public String boardRead(Model model, @PathVariable int num) {
  model.addAttribute("boardDTO", boardService.read(num));
  return "boardRead";
}
  ```
