---
title: "AOP"
layout: post
category: Spring
tags: [Spring]
excerpt: "Aspect Oriented Programming(AOP)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Aspect Oriented Programming(AOP)에 대해 배워 봅시다.

## AOP

  > AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍

  - AOP는 흩어진 코드를 한 곳으로 모으는 코딩 기법을 말합니다.
  - 기능을 `비지니스 로직`과 `공통 모듈`로 구분하고 핵심 비지니스 로직에 영향을 미치지 않고 사이사이에 공통모듈을
  효과적으로 잘 끼워 넣는 개발 방법
  - 공통 모듈(보안 인증 등)을 만든 후에 코드 밖에서 이 모듈을 비지니스 로직에 삽입하는 것

  ![s3](/images/posts/201907/s3.jpg)

  - 주요 개념
    - Aspect(모듈)와 Target(적용이 되는 대상)
    - Advice(모듈안에 들어감, 해야할 일들)
    - Join point(모듈을 끼워 넣을 지점)와 Pointcut(모듈안에 들어감, 어디에 적용해야 하는지)

  - 구현체
    - AspectJ
    - 스프링 AOP

  - AOP 적용 방법
    - Complie
    - Loadtime
    - Runtime

  > [Aspect-Oriented Programming](https://en.wikipedia.org/wiki/Aspect-oriented_programming)

## 프록시 기반 AOP

  - 스프링 AOP의 특징
    - 프록시 기반의 AOP 구현체.
    - 스프링 빈에만 AOP를 적용할 수 있다.
    - 모든 AOP 기능을 제공하는 것이 목적이 아니라, 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제에 대한 해결책을 제공하는 것이 목적.

  - Proxy Pattern

  ![s4](/images/posts/201907/s4.jpg)

  > Subject & Real Subject !!
  >
  > Subject(Inteface) : Service Interface & Real Subject : ServiceImpl(구현체)


## EXAMPLE

  다음 예제를 통해 배워 봅시다.

  - __흩어진 코드__

  ```java
  class A {
    methodA() {
      AAA
      오늘은 7월 4일 미국 독립 기념일이래요.
      BBB
    }

    methodB() {
      AAA
      저는 운동을 좋아합니다.
      BBB
    }
  }

  class B {
    methodC() {
      AAA
      저는 삼겹살을 좋아합니다.
      BBB
    }
  }
  ```

  - __모아놓은 코드__

  ```java
  class A {
    methodA() {
      오늘은 7월 4일 미국 독립 기념일이래요.
    }

    methodB() {
      저는 운동을 좋아합니다.
    }
  }

  class B {
    methodC() {
      저는 삼겹살을 좋아합니다.
    }
  }

  class AAABBB {
    methodAAABBB(JoinPoint point) {
      AAA
      point.execute();
      BBB
    }
  }
  ```

## AOP 적용 예제

### @LogExecutionTime

  > @LogExecutionTime : 어디에 적용할 지 표시 해두는 용도

  - @LogExecutionTime 으로 메소드 처리 시간 로깅하기

  ```java
  @Target(ElementType.METHOD)
  @Retention(RetentionPolicy.RUNTIME)
  public @interface LogExecutionTime { }
  ```

  - 실제 Aspect(@LogExecutionTime 어노테이션이 달린 곳에 적용)

  ```java
  @Component
  @Aspect
  public class LogAspect {
    Logger logger = LoggerFactory.getLogger(LogAspect.class);

    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
      StopWatch stopWatch = new StopWatch();
      stopWatch.start();

      Object proceed = joinPoint.proceed();
      stopWatch.stop();

      logger.info(stopWatch.prettyPrint());
      return proceed;
    }
  }
  ```

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
  >
  > [https://isstory83.tistory.com/90](https://isstory83.tistory.com/90)
