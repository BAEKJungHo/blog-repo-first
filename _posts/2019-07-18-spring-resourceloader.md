---
title: "ResourceLoader"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링에서 제공하는 ResourceLoader에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링에서 제공하는 ResourceLoader에 대해서 배워 봅시다.

## ResourceLoader

  ResourceLoader는 스프링에서 제공해주는 객체로 빈으로 자동으로 등록이 되어있어서 의존성 주입을 받아야 합니다. ResourceLoader를 통해서 가져오는게 Resource 객체입니다.

  ```java
  @Controller
  @RequiredArgsConstructor
  public class ResourceController {
    private final ResourceLoader resourceLoader;
  }
  ```

### 파일 전체 경로 구하기

  파일의 전체 경로를 구하기 위해서는 다음과 같이 할 수 있습니다.

  ```java
  String path = fileType.getPath() // Enum 상수를 가져온다. ex) js, css
  String projectPath = System.getProperty("user.dir") + "/webapp/" + path;
  ```

  하지만 문자열이 하드코딩으로 들어가 있기때문에 좋지 않습니다. 이런경우 스프링의 Resource 객체를 이용하면 됩니다.

  ResourceLoader의 `getResource()` 메서드를 사용하면 `C:\Users\XXX\IdeaProjects\XXX\webapp\` 까지 경로를 잡아줍니다. getResource(path)처럼 사용하면 위 경로에 path를 추가해줍니다.

  ```java
  Resource resource = resourceLoader.getResource(path);
  ```
