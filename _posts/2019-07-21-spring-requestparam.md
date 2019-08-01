---
title: "@RequestParam"
layout: post
category: Spring
tags: [Spring]
excerpt: "@RequestParam 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @RequestParam 어노테이션에 대해서 배워 봅시다.

## @RequestParam

  `/XXX/fileManage?filePath=1234` filePath=1234를 쿼리 파라미터(Query Parameter)라고 합니다.

  쿼리 파라미터는 @RequestParam 어노테이션으로 받아올 수 있는데, Primitive 기본자료형들은 @RequestParam을 생략할 수 있습니다.

  ```java
  @GetMapping("info")
  public FileManageResponseDto view(@PathVariable FileTypes type, String filePath) { }
  ```

  즉, filePath 파라미터에는 1234가 들어가 있습니다.

### 단일 파라미터 변환

  ```java
  private String requestTest(@RequestParam("test1") int num, @RequestParam("test2") String str) {
    // 위 처럼 하나 이상의 타입을 지정할 수 있습니다.
    // 스프링에서 지원하는 모든 타입 변환 가능.
    // @RequestParam은 하나 이상의 파라미터에서 사용가능.
    return null;
  }
  ```

  만약, @RequestParam으로 넘어온 키 값이 존재하지 않는경우, `Bad Request Error`가 발생합니다.

  이를 방지하기 위해서 DefaultValue를 지정할 수 있습니다.

  ```java
  private String requestTest(@RequestParam("test", required=false, defaultValue="0") int num, @RequestParam("test2") String str) {
    // required=false로 지정하면, 키값이 존재하지 않아도 Bad Request Error가 발생하지 않습니다.
    // 키값이 존재 하지 않는다면, num 파라미터에 0으로 초기화.
    return null;
  }
  ```

### Map을 사용하여 @RequestParam을 지정

  ```java
  private String requestTest(@RequestParam HashMap<String, String> paramMap)) {
    // required=false로 지정하면, 키값이 존재하지 않아도 Bad Request Error가 발생하지 않습니다.
    // 키값이 존재 하지 않는다면, num 파라미터에 0으로 초기화.
    return null;
  } </String, String>
  ```

  Map을 사용하면 개발한 사람외에는 유지보수가 힘들어 질 수 있습니다.

  
