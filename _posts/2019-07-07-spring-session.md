---
title: "Session"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring MVC Framework에서 Session을 사용하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring MVC Framework에서 Session을 사용하는 방법에 대해서 배워 봅시다.

## Connectionless Protocol

  세션과 쿠키는 `Connectionless Protocol(비연결지향 프로토콜)` 입니다.

  > Connectionless Protocol
  >
  > 웹 서비스는 HTTP 프로토콜을 기반으로 하는데, HTTP 프로토콜은 클라이언트와 서버의 관계를 유지하지 않습니다. 클라이언트가 서버쪽에 하나의 REQUEST를 보내면 서버는 그에 대한 RESPONSE를 하고 서버 연결을 해제합니다. 비연결지향인 이유는 클라이언트가 여러개 이므로 연결이 지속된다면, 서버에 과부하가 일어나기 때문에, 서버의 효율성을 위해 비연결지향 프로토콜입니다.

  Connectionless Protocol의 장점은 서버의 부하를 줄일 수는 있으나, 클라이언트의 요청 시마다 서버와 매번 새로운 연결이 생성되기 때문에
  일반적인 로그인 상태유지, 장바구니 등의 기능을 구현하기 어렵습니다. 이러한 단점을 보완하기 위해서 `세션과 쿠키`를 사용하는 것입니다.

  세션과 쿠키는 클라이언트와 서버의 연결 상태를 유지해주는 방법으로, __세션은 서버에서 연결 정보를 관리하는 반면 쿠키는 클라이언트에서 연결 정보를 관리하는데 차이가 있습니다.__


## HttpServletRequest를 이용한 세션 사용

  ```java
// 로그인 처리 : HttpServletRequest를 사용
@RequestMapping(value="/login", method = RequestMethod.POST)
public String login(UsersDTO usersDTO, HttpServletRequest request) {
  HttpSession session = request.getSession();
  session.setAttribute("usersDTO", usersService.select(usersDTO.getId()));
  return "boardList";
}
  ```

  HttpServletRequest의 `getSession()` 메서드를 사용하여 HttpSession의 참조변수로 받아, session.setAttribute()로 사용합니다.
  이 방법보다는 HttpSession만을 이용한 방식이 더 많이 사용됩니다.

## HttpSession을 이용한 세션 사용

  로그인과 로그아웃 처리를 하기위한 컨트롤러 코드를 보겠습니다.

  ```java
// 로그인 처리 : HttpSession을 사용
@RequestMapping(value="/login", method = RequestMethod.POST)
public String login(UsersDTO usersDTO, HttpSession session, RedirectAttributes rttr, Model model) {
  // 비밀번호 처리
  boolean flag = usersService.checkPw(usersDTO.getId(), usersDTO.getPwd());

  if(flag) {
    UsersDTO user = usersService.select(usersDTO);
    session.setAttribute("user", user);
    session.setAttribute("id", user.getId());
    return "redirect:/boardList";
  } else {
    rttr.addFlashAttribute("msg", "아이디 또는 비밀번호가 틀렸습니다");
    return "redirect:/login";
  }
}

// 로그아웃 처리 : invalidate() 사용
@RequestMapping(value="/logout")
public String logout(UsersDTO usersDTO, HttpSession session) throws Exception {
  session.invalidate();
  return "redirect:/login";
}
  ```

  위 코드에서 session.setAttribute("id", user.getId());를 또 준것은 수정 검증 컨트롤러에서 id 값이 다를경우
  막아두기 위해서 준 것입니다.

  파라미터로 HttpSession session을 두면, 바로 session.setAttribute로 Key와 Value를 지정하여 사용할 수 있습니다.
  또한 세션을 없애기 위해서는 `invalidate()` 메소드를 호출하면 됩니다.

## 세션 주요 메소드

## 세션 주요 메소드

  - getId() : 세션 ID를 반환
  - setAttribute() : 세션 객체의 속성을 저장
  - getAttribute() : 세션 객체에 저장된 속성을 반환
  - removeAttribute() : 세션 객체에 저장된 속성을 제거
  - setMaxInactiveInterval() : 세션 객체의 유지시간을 설정
  - getMaxInactiveInterval() : 세션 객체의 유지시간을 반환
  - invalidate() : 세션 객체의 모든 정보를 삭제
