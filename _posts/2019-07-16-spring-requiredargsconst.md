---
title: "@RequiredArgsConstructor를 사용한 의존성 주입"
layout: post
category: Spring
tags: [Spring]
excerpt: "@RequiredArgsConstructor를 사용한 의존성 주입에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @RequiredArgsConstructor를 통한 의존성 주입에 대해서 배워 봅시다.

## @RequiredArgsConstructor를 사용한 의존성 주입

  @RequiredArgsConstructor를 사용한 의존성 주입방법에 대해서 배우기 위해 다음 개념을 알아야 합니다.

  > [Dependency Injection](https://baekjungho.github.io/spring-dependencyij/#%EC%9D%98%EC%A1%B4-%EA%B0%9D%EC%B2%B4-%EC%9E%90%EB%8F%99-%EC%A3%BC%EC%9E%85) & [@RequiredArgsConstructor](https://baekjungho.github.io/spring-lombok/#requiredargsconstructor)

  컨트롤러에서 @Autowired나 @Resource, @Inject를 사용하여 의존성을 주입시켰는데 @RequiredArgsConstructor 어노테이션의 특성을 사용하면
  @Autowired나 @Resource @Inject 어노테이션 없이 DI 주입이 가능합니다.

  @RequiredArgsConstructor는 `@NonNull`이나 `final`이 붙은 필드에 대해서 생성자를 생성하는데 Controller에서 `private final XXXService xxxService;` 와 같이 final로 선언해주면 해당 필드를 파라미터로 가지는 생성자가 생성됩니다.

  Dependency Injection에서 배운 개념인, 어떠한 빈에 생성자가 오직 한개만 존재하고, 그 생성자의 파라미터 타입이 빈으로 등록 가능한 존재이면 이 빈은 @Autowired 어노테이션 없이 의존성 주입이 가능합니다.

  코드로 비교하면 다음과 같습니다.

  ```java
  /** Autowired를 사용한 의존성 주입 */
  @Controller
  public class AutowiredUsedController {
    @Autowired
    private XXXService xxxService;
  }

  /** @RequiredArgsConstructor를 사용한 의존성 주입 */
  @Controller
  @RequiredArgsConstructor
  public class RacUseController {
    private final XXXService xxxService;
  }
  ```

## @RequiredArgsConstructor를 사용하여 의존성 주입하는 이유

  @RequiredArgsConstructor를 사용하여 의존성을 주입하는 이유는, 의존성을 주입해야하는 Service 객체가 여러개일 경우, 어노테이션도 여러개 써야하므로, 중복을 피하기 위해서이다. 이것보다 더 중요한 이유는 다음과 같습니다. AService와 BService를 필드로 의존성 주입받는 경우입니다.

  ```java
  // AService > BService
  AService aService = new AService(BService);

  //BService > AService
  BService bService = new BService(AService);
  ```

  __필드 주입 순환참조시 에러발생안함 but 생성자로하면 순환참조하면 에러발생__ , 따라서 생성자로 의존성 주입을 하는게 훨씬 안정적입니다.
