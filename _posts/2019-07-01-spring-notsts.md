---
title: "STS를 이용하지 않은 웹 프로젝트"
layout: post
category: Spring
tags: [Spring]
excerpt: "STS를 이용하지 않고 웹 프로젝트를 구현하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > STS를 이용하지 않고 웹 프로젝트를 구현하는 방법에 대해서 배워 봅시다.

## STS를 이용하지 않은 웹 프로젝트 구현 과정

### 스프링 MVC 웹 어플리케이션 제작을 위한 폴더 생성

  ![n1](/images/posts/201906/n1.jpg)

### pom.xml 및 이클립스 import

  ![n2](/images/posts/201906/n2.jpg)

  ![n3](/images/posts/201906/n3.jpg)

### web.xml 작성

  ![n4](/images/posts/201906/n4.jpg)

### servlet-context.xml 작성

  IOC 컨테이너인 servlet-context.xml을 작성해야 합니다.

  ![n5](/images/posts/201906/n5.jpg)

### root-context.xml 작성

  ![n6](/images/posts/201906/n6.jpg)

### 컨트롤러 및 뷰 작성

  ![n7](/images/posts/201906/n7.jpg)

### 실행

  ![n8](/images/posts/201906/n8.jpg)

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
