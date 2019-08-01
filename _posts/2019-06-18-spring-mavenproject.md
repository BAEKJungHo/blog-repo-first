---
title: "Maven을 이용한 스프링 프로젝트 생성"
layout: post
category: Spring
tags: [Spring]
excerpt: "Maven을 이용한 스프링 프로젝트 생성 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Maven을 이용한 스프링 프로젝트 생성 방법에 대해서 배워 봅시다.

## Maven을 이용한 스프링 프로젝트 생성

  - 마우스 우클릭 - 프로젝트 클릭 - Maven 검색 후 Maven Project 클릭

  ![s2](/images/posts/201906/s2.jpg)

  - Group Id와 Artifact Id 적기

  ![ma1](/images/posts/201906/ma1.jpg)

  > Group Id는 Artifact Id를 감싸는 큰 프로젝트이고, Artifact Id는 그 안에 있는 작은 프로젝트 입니다.
  예를 들어 지하철 프로젝트를 만든다고하면 Group Id는 지하철이며, Artifact Id는 각 노선입니다.

  ![s3](/images/posts/201906/s3.jpg)

## Maven 프로젝트 Import 하기

  - New - Import - Maven 검 - Existing Maven Project 클릭 - 폴더 선택

  ![s4](/images/posts/201906/s4.jpg)

  ![s5](/images/posts/201906/s5.jpg)

## 의존 모듈 관리

  자바를 이용해 어플리케이션을 개발할 때 `Maven(메이븐)과 Gradle(그래들)` 같은 빌드 도구를 사용하는데, 이런 빌드 도구들의 주요 특징 중 하나는 `의존 모듈(jar) 관리`에 있습니다.

  예를 들어 Maven의 경우 중앙 리퍼지토리라고 불리는 서버로 부터 필요한 jar 파일을 다운로드 받아 처리 합니다.

## pom.xml

  > pom.xml 작성 방법에 대해서 배워 봅시다.

  `pom.xml`은 aop, tx, core등 자신이 필요한 모듈들을 가져오기 위한 파일을 말합니다. 즉,
  pom.xml 파일은 메이븐 설정파일로 메이븐은 라이브러리를 연결해주고, 빌드를 위한 플랫폼이다

### pom.xml 작성 후 오류 발생

  ![s6](/images/posts/201906/s6.jpg)

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
