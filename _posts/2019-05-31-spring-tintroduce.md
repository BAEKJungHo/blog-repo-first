---
title: "스프링 프레임워크란?"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring FrameWork의 개요에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Spring FrameWork

  Spring FrameWork는 Java 플랫폼을 위한 애플리케이션 프레임워크 입니다.

  > 스프링 프레임워크의 주요 기능은 DI, AOP, MVC, JDBC 등을 제공합니다.

  - 프레임워크의 장점
    - 모듈화(modularity) : 프레임워크는 구현을 인터페이스 뒤에 감추는 캡슐화를 통해서 모듈화를 강화한다.
    - 재사용성(reusability) : 프레임워크가 제공하는 인터페이스는 여러 애플리케이션에서 반복적으로 사용할 수 있는
    일반적인 컴포넌트를 정의할 수 있게 함으로써 재사용성을 높여준다.
    - 확장성(extensibility) : 다형성을 통해서 인터페이스를 확장할 수 있게 한다.

  자바를 이용해 어플리케이션을 개발할 때 `Maven(메이븐)과 Gradle(그래들)` 같은 빌드 도구를 사용하는데,
  이런 빌드 도구들의 주요 특징 중 하나는 `의존 모듈(jar)` 관리에 있습니다.

  예를 들어 Maven의 경우 중앙 리퍼지토리라고 불리는 서버로 부터 필요한 jar 파일을 다운로드 받아 처리 합니다.

## FrameWork란?

  > 단어상의 의미로 틀, 구조, 뼈대, 구성이라는 의미를 가지고 있습니다.
  >
  > GOF의 Design Pattern에서 프레임워크는 소프트웨어의 특정한 클래스에 대하여 재사용할 수 있는
  설계로 구성된 관련된 클래스의 집합이다. 프레임워크는 설계를 추상적인 클래스로 분리하고 그들의 책임과
  협동 관계를 정의함으로써 아키텍처적인 가이드를 제공한다. 여러분은 프레임워크로부터 추상적인 클래스를
  서브클래싱하여 애플리케이션에 특정한 서브 클래스를 생성함으로써 특정한 애플리케이션에 대하여 프레임워크를
  커스터마이징 한다.

## 스프링 모듈

  ![s1](/images/posts/201906/s1.jpg)

  > 스프링 프레임워크에서 제공하고 있는 모듈을 사용하려면, 모듈에 대한 의존설정을 개발 프로젝트에 XML 파일등을 이용해
서 개발자가 직접 하면 된다.

## 역사

  - 2002년 로드 존슨(MVC)
  - 정식 등록 : 2004년 4월
  - Version
    - 2004년 3월 : Version 1.x
    - 2006년 10월 : Version 2.x
    - 2009년 12월 : Version 3.x
    - 2014년 4월 : Version 4.x
    - 2017년 9월 : Version 5.x

  2006년 부터 `SSH(Spring Structure Hibernate)`라는 용어를 사용 2013년 이후에는
  `Struts(스트럿츠)` 2.3버전을 기준으로 서비스를 종료.

  전자 정부 프레임워크는 `스프링 3.0 Version`을 기반으로 구축되었으며, 초기에는
  스프링 서브 프로젝트로 화면 전환의 흐름을 관리하는 `스프링 웹 플로`가 있었고, 그 이후로
  인증, 허가 처리를 관리하는 `스프링 시큐리티` 등이 추가 되었으며, 일괄 처리 용의 `스프링 배치`가
  출시 되었습니다.

  최근에는 애플리케이션 개발을 간단하게 해주는 `스프링 부트`가 주목을 받게 됩니다.

  - __스프링을 설정하는 방법__
    - 1.0 ~ 3.0 Version : Spring Legacy Project(현업에서 가장 많이 사용하는 방법)
      - 기본 페이지로 JSP 사용
    - 4.0 Version 이후 : Spring Boot(최신기술 : 설정을 최소화 한 것)
      - JSP가 아닌 기본 템플릿을 사용

  > 현재에는 Ajax로 웹 브라우저에 풍부한 화면을 구현

## Ajax란?

  Javascript의 비동기 통신(XMLHttpRequest)을 사용해서 웹 브라우저의 화면 이동 없이
  화면 일부를 변경하는 기술, 화면의 편리성을 향상, 대표적으로 Google의 Map의 화면 이동이나
  지도상에서 상점 정보를 표시하는 것 등이 있습니다.

## Runtime

  - __Spring FrameWork Runtime__
    - Data Access/Integration : JDBC, ORM, OXM, JMS, Transactions
    - Web : WebSocket, Servlet, Web, Portlet
    - AOP, Aspects, Instrumentation, Messaging
    - Core Container : Beans, Core, Context, SPEL
    - Test
