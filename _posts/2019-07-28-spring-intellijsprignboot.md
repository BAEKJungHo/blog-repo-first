---
title: "IntelliJ로 Spring Boot 설정하는 방법"
layout: post
category: Spring
tags: [Spring]
excerpt: "IntelliJ로 Spring Boot 설정하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > IntelliJ로 Spring Boot 설정하는 방법에 대해서 배워 봅시다.

## IntelliJ로 Spring Boot 설정하는 방법

  SpringBoot로 인해 확실히 기존의 Spring기반 웹어플리케이션 프로젝트를 셋업하는데 좀 더 편하고 빠르게 개발할 수 있게 되었습니다.
  개인적으로는 STS보다 IntelliJ로 스프링 부트를 사용하여 개발하는 것이 더 편하게 느껴집니다.

  아래 설명 방식은, 2019-07-28일 기준으로 최신버전 `IntelliJ 2019.2`을 기준으로 설명합니다.

  - `New Project - Spring Initailzr` 클릭
    - ![i1](/images/posts/201907/i1.jpg)
    - JAVA SDK Version 설정(JDK가 설치된 경로를 지정)

  - `Group, Artifact, Type, Language, Packaging 설정`
    - ![i2](/images/posts/201907/i2.jpg)
    - Type : Maven Project
    - Language : Java
    - Packaging : Jar
      - Spring Boot에 내장된 Web Container(Tomcat) 사용
      - Stand Alnone 방식으로 구동
      - 톰캣 설정이 필요 없음
    - Packaging : War
      - 외부 Tomcat에 Deploy해서 구동하는 형태
      - 기존 개발환경과 동일
      - 별도의 Tomcat 필요

  > 지금은 Embeded된 톰캣으로 Stand-Alone형태로 구동시키기 위한 목적이므로 Packagin Type을 jar로 선택

  - `Develop Tools에서 다음과 같이 설정`
    - ![i3](/images/posts/201907/i3.jpg)

  - `Web, Template Engine, SQL등 필요한것을 선택해서 설정`
    - ![i4](/images/posts/201907/i4.jpg)

  - `src/main/java/xxx.com.xxx/XXXApplication.java에 추가 설정`
    - ![i5](/images/posts/201907/i5.jpg)

  ```java
  import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
  import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;

  @EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
  ```

  위 설정을 추가해주는 이유는 서버 실행시 __Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.__ 와 같은 오류가 발생하기 때문입니다.
  위 오류는 Spring Boot JDBC 미 설정시 발생하는 오류입니다.

  - `src/main/resources/static/index.html 생성`
    - 서버 실행시, View가 필요하므로 html을 하나 생성해 줍니다.

  - `서버 실행 후 localhost:8080/ 접속`
    - ![i6](/images/posts/201907/i6.jpg)
    - index.html이 나오면 성공적으로 실행된 것입니다.

## Plugins Install

  - Settings(Ctrl+Alt+S) - Plugins - 원하는 플러그인 검색 후 설치
    - ![i7](/images/posts/201907/i7.jpg)

## Font Setting

  -  Settings(Ctrl+Alt+S) - Font - 원하는 폰트 설정
    - ![i8](/images/posts/201907/i8.jpg)
