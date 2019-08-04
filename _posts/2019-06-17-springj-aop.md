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

  > AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍, 인터페이스를 기반으로 함

  AOP는 인터페이스를 기반으로하며, 인터페이스는 상수와, 추상메서드의 집합이므로, AOP는 `메서드`에만 적용 가능합니다.

  - AOP는 흩어진 코드를 한 곳으로 모으는 코딩 기법을 말합니다.
  - 기능을 `비지니스 로직`과 `공통 모듈`로 구분하고 핵심 비지니스 로직에 영향을 미치지 않고 사이사이에 공통모듈을
  효과적으로 잘 끼워 넣는 개발 방법
  - 공통 모듈(보안 인증 등)을 만든 후에 코드 밖에서 이 모듈을 비지니스 로직에 삽입하는 것
  - 스프링 DI가 의존성 주입이라면, `스프링 AOP는 로직(code) 주입` 이라고 할 수 있습니다.

  ![s3](/images/posts/201907/s3.jpg)

## 중요한 특징

  > 스프링 AOP는 인터페이스(Interface) 기반
  >
  > 스프링 AOP는 프록시(Proxy) 기반
  >
  > 스프링 AOP는 런타임(Runtime) 기반

### 용어(Term)

  - 주요 개념
    - `Aspect(모듈)`
      - Advice들 + Pointcut들
      - 언제(When) + 어디에(Where) + 무엇을(What)

    - `Target(적용이 되는 대상)`

    - `Advice(모듈안에 들어감, 해야할 일들)`
      - 언제(When), 무엇을(What)을 의미
      - Pointcut에 적용할 로직, 즉, 메서드를 의미
      - Pointcut에 언제, 무엇을 적용할지 정의하는 메서드

    - `Advisor(조언자, 어디서, 언제, 무엇을)`
      - 한 개의 Advice + 한 개의 Pointcut
      - 스프링 AOP에서만 사용하는 용어
      - 스프링 버전이 올라가면서 이제는 쓰지 말라고 권고하는 기능
      - Aspect가 있기 때문에, Advisor는 필요 없음

    - `JoinPoint(모듈을 끼워 넣을 지점)`
      - Aspect 적용이 가능한 모든 지점
      - Aspect를 적용할 수 있는 지점 중 일부가 Pointcut이 되므로, Pointcut은 JoinPoint의 부분집합
      - 스프링 프레임워크가 관리하는 빈의 모든 메서드에 해당
      - `광의의 JoinPoint`
        - Aspect 적용이 가능한 모든 지점
      - `협의의 JoinPoint`
        - 호출된 객체의 메서드

    - `Pointcut(모듈안에 들어감, 어디에 적용해야 하는지)`
      - 어디에(Where)을 의미
      - 횡단 관심사를 적용할 타깃 메서드를 선택하는 지시자(메서드 선택 필터)
      - 타깃 클래스의 타깃 메서드 지정자
      - `메서드 선정 알고리즘`
      - JoinPoint의 부분집합

  - 구현체
    - AspectJ
    - 스프링 AOP

  - AOP 적용 방법
    - Complie
    - Loadtime
    - Runtime

  > [Aspect-Oriented Programming](https://en.wikipedia.org/wiki/Aspect-oriented_programming)

### @어노테이션 기반 AOP

  @어노테이션 기반. MyAspect.java는 `스프링 프레임 워크에 종속 됩니다.`

#### 예시 코드

  - `리팩토링 전 : @Before와 @After 어노테이션을 적용`

  ```java
  // @어노테이션 기반 AOP
  @Aspect
  public class MyAspect {
    @Before("execution(* runSomething())")
    public void before(JoinPoint joinPoint) {
      System.out.println("Blah Blah .....");
    }

    @After("execution(* runSomething())")
    public void lockDoor(JoinPoint joinPoint) {
      System.out.println("주인이 집을 비웠다. 문을 잠궈라!!");
    }
  }
  ```

  `@Before("execution(* runSomething())")`는 `* runSomething()`이 메서드가 실행되기전에
  아래 메서드를 먼저 실행하라는 의미입니다.

  위 코드를 보면 어노테이션 내부에 중복되는 코드가 보입니다. 위 코드를 리팩토링 해보겠습니다.

  - `리팩토링 후 : @Pointput 어노테이션을 적용`

  ```java
  // @어노테이션 기반 AOP
  @Aspect
  public class MyAspect {
    @Pointcut("execution(* runSomething())")
    private void iampc() {
      // 여기에 무엇을 작성해도 의미가 없음.
    }

    @Before("iampc()")
    public void before(JoinPoint joinPoint) {
      System.out.println("Blah Blah .....");
    }

    @After("iampc()")
    public void lockDoor(JoinPoint joinPoint) {
      System.out.println("주인이 집을 비웠다. 문을 잠궈라!!");
    }
  }
  ```
#### AOP 설정 파일

  ```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"

    xsi:schemaLocation="
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
    http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

  <aop:aspectj-autoproxy />

  <bean id-"myAspect" class="aop.MyAspect" />
  <bean id="boy" class="aop.Boy" />
  <bean id="girl" class="aop.Girl" />

</beans>
  ```

  - `<aop:aspectj-autoproxy />`
    - proxy는 프록시 패턴을 이용해서 횡단 관심사를 핵심 관심하에 주입하는 것을 말합니다.
    - 스프링은 AOP를 사용
    - 스프링 프레임워크에게 AOP 프록시를 사용하라고 알려주는 지시자의 역할
    - auto : 자동

### POJO와 XML 기반 AOP

  > [POJO](https://baekjungho.github.io/spring-pojo/)

#### 예시 코드

  ```java
  // POJO & XML 기반 AOP
  public class MyAspect {
    public void before(JoinPoint joinPoint) {
      System.out.println("Blah Blah .....");
    }

    public void lockDoor(JoinPoint joinPoint) {
      System.out.println("주인이 집을 비웠다. 문을 잠궈라!!");
    }
  }
  ```

#### AOP 설정 파일

  - `리팩토링 전`

  ```xml
  <!-- 생략 -->
  <aop:aspectj-autoproxy />

  <bean id-"myAspect" class="aop.MyAspect" />
  <bean id="boy" class="aop.Boy" />
  <bean id="girl" class="aop.Girl" />

  <!-- 추가된 부분 -->
  <aop:config>
    <aop:aspect ref="myAspect">
      <aop:before method="before" pointcut="execution(* runSomething())" />
      <aop:after method="lockDoor" pointcut="execution(* runSomething())" />
    </aop:aspect>
  </aop:config>
  ```

  위 XML 파일애서 중복된 코드가 있으므로 리팩토링을 해보겠습니다.

  - `리팩토링 후`

  ```xml
  <!-- 생략 -->
  <aop:aspectj-autoproxy />

  <bean id-"myAspect" class="aop.MyAspect" />
  <bean id="boy" class="aop.Boy" />
  <bean id="girl" class="aop.Girl" />

  <!-- 추가된 부분 -->
  <aop:config>
    <aop:pointcut expression="execution(* runSomething())" id="iampc" />
    <aop:aspect ref="myAspect">
      <aop:before method="before" pointcut-ref="iampc" />
      <aop:after method="lockDoor" pointcut-ref="iampc" />
    </aop:aspect>
  </aop:config>
  ```

### 횡단 관심사(cross-cutting concern)

  예를들어 입금, 출금, 이체에 관해서, 세 부분 모두 로깅, 보안, 트랜잭션 기능이 반복적으로 나타날때,
  이처럼 __다수의 모듈에 공통적으로 나타나는 부분이 존재하는 것을 횡단 관심사(cross-cutting concern)__ 라고 합니다.

  우리가 DB 연동 프로그램을 작성할 때 아래와 같은 코드를 본 적이 있을 것입니다.

  ```java
  DB 커넥션 준비
  Statement 객체 준비

  try {
    DB 커넥션 연결
    Statement 객체 세팅
    CRUD 실행
  } catch {
    예외처리
  } finally {
    자원 반납
  }
  ```

  위 의사 코드를 보면, 어떤 DB 연산을 하든 공통적으로 나타나는 코드이며, 이를 바로 `횡단 관심사(cross-cutting concern)`라고 합니다.
  그리고 CRUD 실행부분의 코드를 `핵심 관심사`라고 합니다.

  > CODE : 횡단 관심사 + 핵심 관심사

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
