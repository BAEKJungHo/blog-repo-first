---
title: "스프링 폴더 구조와 pom.xml에 대한 이해"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 폴더 구조와 pom.xml에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 폴더 구조와 pom.xml에 대해 배워 봅시다. 

## 폴더 구조

  스프링 프로젝트 폴더를 들어가면 구조가 다음과 같이 되어 있습니다. __폴더 구조는 암기 하는게 좋습니다.__

  - Spring Project
    - src
      - main
        - java : 실제로 자바를 이용해서 기능 구현을 해 나가는 부분(`자바 파일 관리`)
        - resources : 자바 프로그래밍을 함에 있어서 보조적인 파일(`자원 파일 관리`)
      - test
        - java
        - resources

  ![s7](/images/posts/201906/s7.jpg)

  1. java 폴더(testProject/src/main/java)의 경우 특별한 것은 없고, 앞으로 만들어지는 자바 파일들이 관리되는 폴더입니다.
  2. resources 폴더(testProject/src/main/resources)의 경우 자원을 관리하는 폴더로 `스프링 설정 파일(XML)` 또는 프로퍼티 파일 등이 관리됩니다.
  스프링 설정 파일인 applicationContext.xml 등이 있습니다.
  3. java, resources 폴더는 스프링 프레임워크의 기본 구조를 이루는 폴더로 개발자는 이대로 폴더를 구성해야 합니다.

## pom.xml

  ![s8](/images/posts/201906/s8.jpg)

  `pom.xml`은 aop, tx, core등 자신이 필요한 모듈들을 가져오기 위한 파일을 말합니다. 즉, pom.xml 파일은 메이븐 설정파일로 메이븐은 라이브러리를 연결해주고, 빌드를 위한 플랫폼입니다.

  > pom.xml에 의해서 필요한 라이브러리만 다운로드 해서 사용합니다.

## Group Id & Artifact Id

  Group Id는 Artifact Id를 감싸는 큰 프로젝트이고, Artifact Id는 그 안에 있는 작은 프로젝트 입니다. 예를 들어 지하철 프로젝트를 만든다고하면 Group Id는 지하철이며, Artifact Id는 각 노선입니다.

## 라이브러리

  > 라이브러리 경로 : `C:\Users\사용자\.m2\repository\org\springframework`
  >
  > 참고로 모듈의 라이브러리 파일명은 `artifactId + “-” + 버전명 + “.jar”` 로 표시된다.

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
