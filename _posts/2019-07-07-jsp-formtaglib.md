---
title: "form 태그 라이브러리"
layout: post
category: JSP
tags: [JSP]
excerpt: "form 태그 라이브러리에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## form 태그 라이브러리

  가장 먼저 `form 커스텀 태그`를 사용하기 위해서 라이브러리를 추가해야 합니다.

  ```xml
  <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
  ```

  form 커스텀 태그 사용방식은 아래와 같습니다.

  ```html
  <form:form commandName="member"></form:form>
  ```

  commandName에 `커맨드 객체 이름`을 명시해야 합니다. 입력값이 없을시 method는 'post'로, action값은 '현재 요청URL'값이 설정됩니다.

  > 커맨드 객체(Command Object)
  >
  >[https://baekjungho.github.io/spring-controllerimplement/#%EC%BB%A4%EB%A9%98%EB%93%9C-%EA%B0%9D%EC%B2%B4%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-http%EC%A0%84%EC%86%A1-%EC%A0%95%EB%B3%B4-%EC%96%BB%EA%B8%B0](https://baekjungho.github.io/spring-controllerimplement/#%EC%BB%A4%EB%A9%98%EB%93%9C-%EA%B0%9D%EC%B2%B4%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-http%EC%A0%84%EC%86%A1-%EC%A0%95%EB%B3%B4-%EC%96%BB%EA%B8%B0)

### input 태그를 위한 커스텀 태그

  ```
  <form:input> : text 타입의 <input>
  <form:password> : password 타임의 <input>
  <form:hidden>	: hidden 타입의 <input>

  path속성 : 바인딩 될 command객체의 프로퍼티를 지정하는 속성
  ```

### select 태그를 위한 커스텀 태그

  ```
  <form:select>	: <select>태그를 생성한다. <option> 태그를 생성하는데 필요한 컬렉션을 전달 받을 수 있습니다.
  <form:options> : 지정한 컬렉션 객체를 이용해서 <option> 태그를 생성합니다.
  <form:option> : 하나의 <option> 태그를 생성합니다.

  이 태그는 선택 옵션을 제공할 때 주로 사용하는 태그이므로, 여러개의 선택사항들을 제공합니다.

  선택정보는 컨트롤러에서 생성해서 뷰로 전달하는 경우가 일반적이며 @ModelAttribute 어노테이션을 이용해서 전달
  ```

### 작동 방식

  form 커스텀 태그를 사용할 때에 컨트롤러와 View 파일간의 동작에 있어서 중요한 특징이 있습니다.

  먼저 컨트롤러 코드를 보겠습니다.

  - 게시판 수정을 위한 컨트롤러 코드

  ```java
  // 게시글 수정권한 판단
@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.GET)
public String boardEdit(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
  if(session.getAttribute("id").equals(boardService.read(num).getId())) {
     model.addAttribute("boardDTO", boardService.read(num));
    return "boardEdit";
  } else {
    rttr.addFlashAttribute("msg", "수정 권한이 없습니다");
    return "redirect:/boardList";
  }
}

// 수정 검증 : BindingResut + Validator
@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.POST)
public String boardEdit(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
  if(bindingResult.hasErrors()) return "boardEdit";
  else {
    boardService.edit(boardDTO);
    return "redirect:/boardList";
  }
}
  ```

  - 권한이 있는 경우의 동작

  수정 버튼 클릭 -> GET 방식의 컨트롤러로 이동하여 권한 판단 -> View 파일(boardEdit)로 이동 후 수정내용 작성
  -> 수정 실시 후 수정검증 컨트롤러로 이동 -> boardList 컨트롤러로 이동 -> View 파일로 이동(boardList)

  - 권한이 없는 경우의 동작

  수정 버튼 클릭 -> GET 방식의 컨트롤러로 이동하여 권한 판단 -> boardList 컨트롤러로 이동
  -> View 파일로 이동

  이 동작 과정에서 주의 깊게 보셔야 할 부분이 `게시글 수정권한 판단 컨트롤러`에서 다음 코드입니다.

  ```java
  model.addAttribute("boardDTO", boardService.read(num));
  ```

  model.addAttribute()를 사용하는 이유는 JSP에서 값을 받아서 EL로 찍어내기 위함입니다.

  이제 수정 View파일을 한번 보겠습니다.

  ```html
  <form:form commandName="boardDTO" mehtod="post">
  <table border="1">
    <tr>
      <th><form:label path="title">제목</form:label></th>
      <td>
        <form:input path="title" />
        <form:errors path="title" />
      </td>
    </tr>
    <tr>
      <th><form:label path="contents">내용</form:label></th>
      <td>
        <form:input path="contents" />
        <form:errors path="contents" />
      </td>
    </tr>
  </table>
  <div>
    <input type="submit" value="등록">
    <a href="<c:url value="/boardList" />"> 목록</a>
  </div>
</form:form>
  ```

  boardEdit.jsp View 파일을 보면 어디에도 EL(표현언어)가 보이지 않습니다.
  그렇다면 `model.addAttribute()` 코드를 빼도 될 것 같지만, 빼면 오류가 생깁니다.

  그 이유는 다음과 같습니다.

  __form 커맨드 태그의 commandName의 속성명이 model.addAttribute("boardDTO", .....)의 Key값과 일치해야 하며,
  수정 검증 컨트롤러의 파라미터로 있는 BoardDTO boardDTO 커맨드 객체 파라미터 명과 동일 해야 합니다.__

  > 즉, commandName 속성명 = model.addAttribute()의 Key값 = 커맨드 객체 파라미터명

  수정 권한판단 컨트롤러에서 View쪽으로 갈때 __model.addAttribute("boardDTO", boardService.read(num));__
  이 코드가 __View에서 form 형식을 자동 바인딩을 해줍니다. 즉, 컨트롤러가 View쪽으로 뿌려주는 바인딩__ 이라고 생각하시면 됩니다.

  그리고 View의 form태그에서 __<form:input path="title" />__ path 경로의 이름은 커맨드 객체의 필드(속성명)과 자동으로 바인딩 되기 위해 일치를 시켜줘야 합니다. __즉, View에서 컨트롤러로 뿌려주는 바인딩__ 이라고 생각하시면 됩니다. 따라서 제목과, 내용을 작성하고 나면
  수정검증 컨트롤러로 가서 __커맨드 객체를 통해, path 경로명과 일치하는 필드(속성)명을 Setter 메소드를 통해 알아서 바인딩__ 시켜줍니다.

## 결론

  form 커스텀 태그를 사용하게되면 __컨트롤러가 View로 뿌려주는 바인딩과, View에서 컨트롤러로 뿌려주는 바인딩__ 2가지 방식을
  같이 사용하게 됩니다.
