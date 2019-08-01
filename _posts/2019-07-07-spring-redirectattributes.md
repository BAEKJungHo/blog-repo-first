---
title: "RedirectAttributes"
layout: post
category: Spring
tags: [Spring]
excerpt: "RedirectAttributes를 통한 redirect 방식의 특징에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > RedirectAttributes를 통한 redirect 방식의 특징에 대해서 배워 봅시다.

## RedirectAttributes

  우리는 페이지 구현하면서 종종 redirect를 해줄 경우가 있습니다.  하지만, 대부분 Parameter들을 url뒤에 붙여서 GET방식으로 값을 넘겨준다.
  get방식으로 보낼경우 `보안에 취약(취약성)`하게 됩니다.

  리다이렉트가 발생하면 원래 요청은 끊어지고, 새로운 HTTP GET 요청이 시작된다.(브라우저에게 이 URL로 리다이렉트해!)  때문에 리다이렉트 실행 이전에 수행된 모델 데이터는 소멸합니다. __따라서 리다이렉트로 모델을 전달하는 것은 의미 없습니다.__

  ![re](/images/posts/201906/re.jpg)

  그러나 리다이렉트 방법으로도 데이터를 전달하는 방법이 있습니다. GET의 특징을 사용하는 것입니다. RedirectAttributes를 사용하면 url뒤에 RedirectAttributes의 addFlashAttribute를 이용하여 url뒤에 Parameter를 추가하지 않아도 화면에 값을 받을수 있습니다.

  RedirectAttributes는 아래 그림처럼 __리다이렉트가 발생하기 전에 모든 플래시 속성을 세션에 복사합니다.__ 리다이렉션 이후에는 저장된 플래시 속성을 세션에서 모델로 이동시킵니다. 헤더에 파라미터를 붙이지 않기 때문에 URL에 노출되지 않습니다.

  ![re3](/images/posts/201906/re3.jpg)

  > 즉, RedirectAttributes의 장점은 GET 방식을 취하면서도, URL에 데이터를 노출시키지 않고도 값을 전달 할 수 있는 장점이 있습니다.
  header가 아닌 session을 통해 전달하는 방식을 가집니다.

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
  ```

  RedirectAttributes rttr 파라미터를 만들고 addFlashAttribute() 메서드를 호출하여 사용하면 됩니다.

## 참조

  > [https://m.blog.naver.com/PostView.nhn?blogId=allkanet72&logNo=220964699929&proxyReferer=https%3A%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=allkanet72&logNo=220964699929&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
