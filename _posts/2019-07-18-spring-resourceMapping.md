---
title: "리소스 매핑(Resource Mapping)"
layout: post
category: Spring
tags: [Spring]
excerpt: "리소스 매핑(Resource Mapping)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 리소스 매핑(Resource Mapping)

## 리소스 매핑(Resource Mapping)

  우리가 스프링 MVC에서 주로 사용하는 Mapping 어노테이션은 @RequestMapping, @GetMapping, @PostMapping일 것입니다.
  그리고 서로 다른 요청에 따라 처리하기 위해서 @GetMapping("list")와 같이 처리를 했을 것입니다.

  사실 Mapping을 하기 위한 어노테이션은 다음과 같습니다.

  - GET: 리소스요청  http://localhost:8080/articles
  - GET: 리소스요청  http://localhost:8080/articles/1
  - POST: 리소스 생성 http://localhost:8080/articles
  - PUT: 리소스 전체수정 http://localhost:8080/articles
  - PATCH: 리소스 부분 수정 http://localhost:8080/articles
  - DELETE: 리소스 삭제 http://localhost:8080/articles

  즉, 같은 URI로 요청이 들어와도 Mapping Annotation에 따라서 구분을 지을 수 있습니다.
  GET은 리소스 요청을 나타내는데, 주로 화면에 list를 뿌려줄때 사용하며, POST는 create즉, DB에 Insert 작업이 필요한 요청이 들어오면
  @PostMapping을 타고 HandlerMethod를 실행합니다.

  ```java
  @Controller
  @ReqeustMapping("/XXX/test")
  public class TestController {
    @GetMapping
    public String list(@ModelAttribute("test") TestVo test) {
      return null;
    }

    @PostMapping
    public String create(@ModelAttribute("test") TestVo test) {
      return null;
    }
  }
  ```

  위에서 @ReqeustMapping("/XXX/test")으로 공통 요청 경로를 /XXX/test로 잡았습니다. 따라서 View에서는, 매핑 요청으로 공통 경로만 적을 것입니다.
  그러면, 그 View의 역할(목록, 수정 등)에 따라서 @GetMapping @PostMapping이 알아서 판단해서 HandlerMethod를 실행시킵니다.

  
