---
title: "Spring Boot. webapp 만들기"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring Boot에서 webapp 폴더를 만드는 방법을 배워 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring Boot에서 webapp 폴더를 만드는 방법을 배워 보겠습니다.

## Spring Boot에서 webapp/WEB-INF 만들기

  스프링 부트 프로젝트를 생성하면 src/main/java와 src/main/resources가 있을 것입니다.

  - src
    - main
      - java
        - com
          - xxx
            -xxx
              - Application.java
      - resources
        - static
        - templates
        - application.properties

  스프링 부트의 초기 프로젝트 폴더 구조는 위와 같을 것입니다. 위에서 main아래에 `webapp`을 만들고
  그 안에 `WEB-INF`를 만들고 그안에 `jsp`를 만들어 줍시다.

  - src
    - main
      - java
        - com
          - xxx
            -xxx
              - Application.java
      - resources
        - static
        - templates
        - application.properties
      - webapp
        - WEB-INF
          - jsp

  Spring Boot 는 webapp의 위치를 모르기 때문에 설정파일에 경로를 명시해주어야 합니다.

  - application.properties

  ```
  spring.mvc.view.prefix=/WEB-INF/jsp/
  spring.mvc.view.suffix=.jsp
  ```
