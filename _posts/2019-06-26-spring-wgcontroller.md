---
title: "@Controller"
layout: post
category: Spring
tags: [Spring]
excerpt: "@Controller 어노테이션에 대해서 알아 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## @Controller

  ![m6](/images/posts/201906/m6.jpg)

  @Controller 어노테이션을 클래명 위에 적어 주면, 해당 클래스는 특정 클래스를 구현하거나, 상속할 필요 없이
  Controller 클래스가 됩니다.

  하지만 @Controller 어노테이션을 사용하기 위해서는 스프링 설정 파일(servlet-context.xml)에서
  `<annotation-driven />` 태그를 넣어줘야 합니다.

  ```java
  package com.easycompany.controller.annotation;

  @Controller
  public class LoginController {
     ...
  }
  ```

## 참고

  http://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte2:ptl:annotation-based_controller
