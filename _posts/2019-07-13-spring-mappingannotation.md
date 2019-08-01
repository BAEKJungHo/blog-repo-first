---
title: "Mapping Annotation"
layout: post
category: Spring
tags: [Spring]
excerpt: "@ReqeustMapping을 포함한 @PostMapping, @GetMapping 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @ReqeustMapping을 포함한 @PostMapping, @GetMapping 어노테이션에 대해서 배워 봅시다.

## Mapping Annotation

  ```java
  @RequestMapping(value="/boardWrite", method=RequestMethod.GET)
  public String boardWrite(Model model) {}

  @RequestMapping(value="/boardWrite", method=RequestMethod.POST)
  public String boardWrite(Model model) {}

  @RequestMapping(value="/boardList")
  public String boardList(Model model) {}
  ```

  우리가 @RequestMapping을 사용할 때에는 위 처럼 메소드방식을 GET OR POST로 지정하기도 하고, 지정하지 않으면
  GET과 POST방식을 모두 받을수 있다는 뜻입니다.

  만약, GET과 POST 방식을 지정해서 사용해야 하는 경우 `@PostMapping과 @GetMapping` 어노테이션을 사용하여 줄여 쓸 수 있습니다.

  ```java
  @GetMapping("/boardWrite")
  public String boardWrite(Model model) {}

  @PostMapping("/boardWrite")
  public String boardWrite(Model model) {}
  ```
  
