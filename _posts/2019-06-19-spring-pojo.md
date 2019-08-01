---
title: "POJO"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링의 특징인 POJO(Plain Old Java Object)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링의 특징인 POJO(Plain Old Java Object)에 대해서 배워 봅시다.

## POJO

  > POJO(Plain Old Java Object) : 평범한 자바 객체
  >
  > 주로 특정 자바 모델이나 기능, 프레임워크를 따르지  않는 Java Object를 지칭합니다.

  ```
  Wikipedia.

  Plain Old Java Object, 간단히 POJO는 말 그대로 해석을 하면 오래된 방식의 간단한 자바 오브젝트라는 말로서 Java EE 등의 중량 프레임워크들을 사용하게 되면서 해당 프레임워크에 종속된 "무거운" 객체를 만들게 된 것에 반발해서 사용되게 된 용어이다. 2000년 9월에 마틴 파울러, 레베카 파슨, 조쉬 맥킨지 등이 사용하기 시작한 용어로서 마틴 파울러는 다음과 같이 그 기원을 밝히고 있다.
  ```

  POJO는 `평범한 자바 객체`를 뜻하는데 평범한 자바객체의 특성은 다음과 같습니다.

  1. 클래스 상속을 강제하지 않는다.
  2. 인터페이스 구현을 강제하지 않는다.
  3. 어노테이션 사용을 강제하지 않는다.

  POJO가 아닌 대표적인 객체로는 서블릿 코드를 작성할 때에 HttpServlet을 반드시 상속 받아야 하는 것처럼
  `public XXXServlet extends HttpServlet{ }` 상속을 강제 합니다.

## EXAMPLE

  EJB 등에서 사용되는 Java Bean 이 아닌 Getter 와 Setter 로 구성된 가장 순수한 형태의 기본 클래스를 POJO라 합니다.

  아래와 같은 코드가 POJO를 나타냅니다.

  ```java
  public class PojoClass {
    private String ssn;
    private int age;

    public int getSsn() {
      return ssn;
    }
    public int getAge() {
      return age;
    }
    public void setSsn(String ssn) {
      this.ssn = ssn;
    }
    public void setAge(int age) {
      this.age = age;
    }
  }
  ```

## 참조

  > [Wikipedia](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)
  >
  > [https://simsimjae.tistory.com/213](https://simsimjae.tistory.com/213)
  >
  > [Jins' Dev Inside](https://jins-dev.tistory.com/entry/Spring-의-기본이-되는-POJO-에-대하여?category=760012)
