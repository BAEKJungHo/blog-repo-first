---
title: "객체 지향 설계 5원칙(SOLID)"
layout: post
category: Java
tags: [Java]
excerpt: "객체 지향 설계 5원칙(SOLID)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 객체 지향 설계 5원칙(SOLID)에 대해서 배워 봅시다.

## 객체 지향 설계 5원칙(SOLID)

  1. SRP(Single Responsibility Principle) - 단일 책임 원칙
  2. OCP(Open Closed Principle) - 개방 폐쇄 원칙
  3. LSP(Liskov Substitution Principle) - 리스코프 치환 원칙
  4. ISP(Interface Segregation Principle) - 인터페이스 분리 원칙
  5. DIP(Dependency Inversion Principle) - 의존 역전 원칙

### 결합도와 응집도

  좋은 소프트웨어 설계를 위해서는 `결합도(coupling)`를 낮추고 `응집도(cohesion)`는 높이는 것이 바람직 하다.
  결합도는 `모듈(클래스)간의 상호 의존 정도`로서 결합도가 낮으면 모듈 간의 상호 의존성이 줄어들어 객체의 재사용성이나 수정, 유지보수가 용이하다.

  응집도는 하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성으로, 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져 재사용이나 기능의 수정, 유지보수가 용이하다.

  - 결합도 수준
    - 데이터 결합도, 스탬프 결합도, 컨트롤 결합도, 외부 결합도, 공유 결합도, 내용 결합도
  - 응집도 수준
    - 기능 응집도, 순차 응집도, 통신 응집도, 절차 응집도, 시간 응집도, 논리 응집도, 우연 응집도

## 책 추천

  - 스프링 입문을 위한 자바 객체 지향의 원리와 이해(개구리책)

## 참조

  > [객체 지향 SW 설계의 원칙 : 개방-폐쇄 원칙](http://www.zdnet.co.kr/view/?no=00000039134727)
  >
  > [객체 지향 SW 설계의 원칙 : 사례연국, 단일 책임 원칙](http://www.zdnet.co.kr/news/news_view.asp?article_id=00000039135552)
  >
  > [객체 지향 SW 설계의 원칙 : 인터페이스 분리의 원칙](http://www.zdnet.co.kr/news/news_view.asp?article_id=00000039139151)
