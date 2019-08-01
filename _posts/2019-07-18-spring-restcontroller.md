---
title: "@RestController"
layout: post
category: Spring
tags: [Spring]
excerpt: "@RestController 어노테이션의 사용 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @RestController 어노테이션의 사용 방법에 대해서 배워 봅시다.

## @RestController

  화면을 비동기로 처리하기 위해서는 컨트롤러의 Handler Method에 @ResponseBody를 붙여야 합니다. 하지만 모든 HandelrMethod에 @ResponseBody 어노테이션을 붙인다면 너무 비효율적입니다. HandlerMethod에 @ResponseBody를 붙이지 않아도 비동기 처리를 할 수 있는 방법이 있는데 바로 @RestController 어노테이션을 이용하면 됩니다.

  클래스 위에 @Controller로 되어있는 부분을 @RestController로 수정하면 @ResponseBody를 붙이지 않아도 됩니다.

  > @Controller와는 다르게 `@RestController`는 리턴값에 자동으로 @ResponseBody를 붙게되어 HTTP 응답데이터(body)에 자바 객체가 매핑되어 전달 된다고 합니다.( ※ @Controller인 경우에는 @ResponseBody를 적어줘야 합니다. )
