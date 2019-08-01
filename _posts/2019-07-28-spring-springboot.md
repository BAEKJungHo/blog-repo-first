---
title: "Spring Boot(스프링 부트)"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring Boot(스프링 부트)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring Boot(스프링 부트)에 대해서 배워 봅시다.

## Srping Boot

  > Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run". We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.

  - 스프링부트는 단독실행되는, 실행하기만 하면 되는 상용화 가능한 수준의, 스프링 기반 애플리케이션을 쉽게 만들어낼 수 있다.
  - 최소한의 설정으로 스프링 플랫폼과 서드파티 라이브러리들을 사용할 수 있도록 하고 있다.
  - 스프링 기반의 애플리케이션을 개발하기 쉽도록 기본설정되어 있는 설정을 기반으로 해서 빠르게 개발할 수 있도록 해주는 개발플랫폼이다.

  >  Spring Boot takes an opinionated view of building production-ready Spring applications. Spring Boot favors convention over configuration and is designed to get you up and running as quickly as possible.

### 기능

  - Create stand-alone Spring applications
  > 단독실행가능한 스프링애플리케이션을 생성한다.

  - Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
  > 내장형 톰캣, 제티 혹은 언더토우를 내장(WAR 파일로 배포할 경우에는 필요없음)

  - Provide opinionated 'starter' component to simplify your build configuration
  > 기본설정되어 있는 'starter' 컴포넌트들을 쉽게 추가

  - Automatically configure Spring whenever possible
  > 가능한 자동설정되어 있음

  - Provide production-ready features such as metrics, health checks and externalized configuration
  > 상용화에 필요한 통계, 상태 점검 및 외부설정을 제공

  - Absolutely no code generation and no requirement for XML configuration
  > 설정을 위한 XML 코드를 생성하거나 요구하지 않음

### 스프링과 스프링 부트의 차이점

  ![sb1](/images/posts/201907/sb1.jpg)

  > [https://qph.fs.quoracdn.net/main-qimg-01ffe91237762c5f29b5278383e2156b](https://qph.fs.quoracdn.net/main-qimg-01ffe91237762c5f29b5278383e2156b)

  위 사진이 스프링(Spring)과 스프링 부트(Spring Boot)의 차이점을 잘 설명해 주고 있습니다.

  스프링은 `FrameWork` 입니다. 즉, 틀이라는 의미를 가지고 있는데, 자바를 사용하여 웹 개발을 편하게 도와주는 틀과 같은 것입니다.
  우리는 `자바(Java)`라는 프로그래밍 언어로 `Spring FrameWork`라는 틀에서 제공해주는 `어노테이션`이라는 강력한 기능(이 외에도 여러가지)을 통해서
  웹 개발을 할 수 있습니다.

  반면 `Spring Boot`는 `Srping MVC FrameWork`을 확장해서 더 많은 기본설정들이 내포되어있으며, 더 빠르게 웹 개발을 할 수 있도록 지원하는 FrameWork입니다.
  이 둘은 크게 다르지는 않습니다. 기본적인 스프링에 대한 동작 방식은 같으며, 다만 의존성 설정관리, 여러 라이브러리를 사용하기 위해서 세팅해야하는 부분들을
  Spring Boot에서는 기본으로 잡아주고 있는 부분이 많기 때문에, Spring MVC FrameWork만을 사용할 때보다 `개발 속도가 향상됩니다.`

  > 예를들어, 우리가 `자동차`를 만들어야 한다고 가정할 때, 자동차를 만들기 위해 도구를 챙겨야 하는데, 이 도구를 `Java`라고 할 수 있습니다. 만약, Spring FrameWork를 사용하지 않은 경우에는
  자동차 틀 부터 만들어야 할 것입니다. 하지만 `Spring FrameWork`를 사용하는 경우에는 `자동차 틀`이 기본적으로 제공된다고 보시면됩니다. 이 자동차 틀을 기반으로 자동차를 완성 시키기 위해서는 네이게이션, 핸들, 바퀴, 차 색깔 등등 여러가지가 필요할 것입니다. `Spring Boot`는 `자동차 틀 + 여러가지 기능`이라고 볼 수 있습니다.

  - `스프링만을 사용한 경우(Usage only Spring)`
    - 웹 서비스를 구동시키기 위해서 `Web Container(Tomcat)`을 추가해야한다.
    - 기존 설정 외의 클래스들과 라이브러리들을 사용하기 위해서 의존성을 추가해야한다.
    - 자바, DB, JSP등 서로 호환이 되도록 Maven Dependency에서 버전에 대한 의존성을 맞춰야 한다.
    - 의존성 설정 등 기본적으로 해야할 세팅이 `Spring Boot`보다 훨씬 많다.
    - 즉, 수작업으로 초기 셋업 해야하는 과정이 방대하다.

  - `스프링부트를 사용한 경우(Usage Spring Boot)`
    - 수작업으로 초기 셋업하는 과정 없이 간단히 프로젝트를 띄울 수 있다. 스프링에서 제공하는 `Spring Tool Suite` 개발 도구를 사용하면 마법사를 통해 기본적인 프로젝트 성격과 프로젝트에서 필요로 하는 라이브러리를 선택할 수 있다. 수작업으로 셋업하더라도 이전에 비해 반 이상이 단순해진다고 생각된다.
    - 프로젝트마다 일상적으로 설정하게 되는 사항들을 이미 내부적으로 가지고 있고 개별적으로 차이가 나는 부분만 설정 파일에 집어 넣으면 된다. 예를 들어 DB 연결 설정은 설정대로, 스프링 DB 설정은 설정대로 하지 않고 DB 연결 설정만 설정 파일에 적어놓으면 된다. DB 드라이버니, 트랜잭션이니 하는 것처럼 당연히 들어가야하는 것들은 알아서 처리된다.
    - 스프링 보안(Security), 스프링 데이터 JPA와 같이 다른 스프링 프레임웍 구성 요소를 쉽게 가져다 쓸 수 있으며 이 과정에서 프로토타이핑이나 기능을 시험해보는 시간이 전보다 단축된다.
    - 톰캣(Tomcat)이나 제티(Jetty)를 기본 내장할 수 있으며 웹 프로젝트 띄우는 시간이 독립적인 톰캣으로 띄우는 시간보다 반은 단축된다(예를 들어 30초 -> 15초). 또한 이렇게 서블릿 컨테이너가 내장될 수 있으므로 프로젝트를 .jar 파일 형태로 간단히 만들어 배포할 수 있다.
    - maven pom.xml에서 의존 라이브러리의 버전을 일일이 지정하지 않아도 된다. 스프링 부트가 권장 버전을 관리한다. 또한 Spring Tool Suite을 사용한다면 이클립스의 `컨텐트 어시스트` 기능을 통해 의존 라이브러리를 자동 완성 방식으로 입력할 수 있다.

### EXAMPLE

   Spring Tool Suite을 사용하지 않고 수작업으로 웹프로젝트를 셋업할 경우에는 우선 의존 라이브러리를 `maven pom.xml` 파일에 지정해야할 것이다.
   스프링 부트는 다음 한 가지 라이브러리 의존성만 넣어도 기본적인 웹 개발이 가능하도록 나머지 의존 라이브러리를 준비해준다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

  다음으로는 스프링 환경 셋업이다. 아래와 같이 xml 설정 없이 자바 클래스 하나로 기본 설정이 끝난다. `@Configuration, @EnableAutoConfiguration` 같은 어노테이션이 보인다.
  따라서 Spring Boot에서는 xml 설정보다는 `자바 클래스로 설정`하는 것이 좋다.

```java
package dylan.sample.spring.boot; // 예시 패키지

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

  이제 서비스할 웹 페이지를 만들겠다면 늘 하던 대로 @RequestMapping 어노테이션을 붙인 컨트롤러 메서드를 만들면 된다. 

## 참조

  > [https://gist.github.com/ihoneymon/8a905e1dd8393b6b9298](https://gist.github.com/ihoneymon/8a905e1dd8393b6b9298)
  >
  > [https://start.goodtime.co.kr/2014/10/%EC%9D%B4%EC%A0%9C%EB%8A%94-spring-boot%EB%A5%BC-%EC%8D%A8%EC%95%BC%ED%95%A0-%EB%95%8C%EB%8B%A4/](https://start.goodtime.co.kr/2014/10/%EC%9D%B4%EC%A0%9C%EB%8A%94-spring-boot%EB%A5%BC-%EC%8D%A8%EC%95%BC%ED%95%A0-%EB%95%8C%EB%8B%A4/)
