---
title: "Tomcat 설치 및 STS로 스프링 설정"
layout: post
category: Spring
tags: [Spring]
excerpt: "Tomcat 설치 및 STS로 스프링 설정에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Tomcat 설치 및 STS로 스프링 웹 프로젝트 생성 방법에 대해서 배워 봅시다.

## Tomcat 설치

  > [Apache Tomcat 설치 Ver1.](https://baekjungho.github.io/apache-tomcat/)

  ![t1](/images/posts/201906/t1.jpg)

  zip파일을 다운 받고, 압축을 풀 폴더를 선택하고 압축을 해제합니다.

  ![t2](/images/posts/201906/t2.jpg)

  그리고 이클립스와 연동을 하기 위해서 Window - Show view - other - Server를 선택합니다.
  선택을 하고나면 아래에 Server Tab이 생길 것입니다.

  ![t3](/images/posts/201906/t3.jpg)

  그리고 위 그림과 같이 따라합니다. Browse를 클릭해 아파치를 설치한 경로를 지정합니다.
  그리고 JRE 버전을 1.8로 바꿔 줍니다.

  ![t4](/images/posts/201906/t4.jpg)

  이제 몇가지 설정을 해야 하는데 Server Tab에 있는 Tomcat Server를 더블클릭합니다.

  그리고 위 그림과 같이 Servers Location에 있는 `Use Tomcat Installation`을 체크하고
  Servers Option에서 `Publish module contexts to separate XML files`를 체크합니다.

  그리고 DB를 `오라클(Oracle)`을 사용하는 경우는 오른쪽 Ports에 있는 HTTP/1.1 port 번호를 8090으로 수정합니다.
  그 이유는 오라클의 기본 포트번호가 `8080`이기 때문에 Tomcat 포트번호와 중복이 되므로, 중복을 피하기 위해서 수정하는 것입니다.

  만약 `MySQL`을 사용하는 경우는 수정할 필요가 없습니다.

  ![t5](/images/posts/201906/t5.jpg)

  서버를 구동시키고 URL에 localhost:8090을 입력하여 다음 화면이 나오면 설치가 완료된 것입니다.

## STS 플러그인으로 스프링 설정

  > [STS로 스프링 설정 Ver1.](https://baekjungho.github.io/spring-install/#sts%EB%A1%9C-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%84%A4%EC%A0%95-ver1)

  ![st1](/images/posts/201906/st1.jpg)

  먼저 Help - Eclipse MarketPlace에서 STS를 검색합니다. 그리고 Install을 눌러 설치합니다.

  ![st2](/images/posts/201906/st2.jpg)

  그리고 Confirm을 누르고 Accept(라이센스 동의)를 클릭하고 Finish를 클릭하면 됩니다.

### Spring MVC Project 생성

  Spring MVC Project 생성 방법은 다음을 따르면 됩니다.

  ![st3](/images/posts/201906/st3.jpg)

  ![st4](/images/posts/201906/st4.jpg)

  ![st5](/images/posts/201906/st5.jpg)

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
