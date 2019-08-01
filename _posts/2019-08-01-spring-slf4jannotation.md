---
title: "@Slf4j"
layout: post
category: Spring
tags: [Spring]
excerpt: "@Slf4j 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @Slf4j 어노테이션에 대해서 배워 봅시다.

## Logging

  > [Slf4j와 Logging에 대해서 공부하기](https://baekjungho.github.io/technology-slf4jlog/)

## @Slf4j

  스프링에서 `@Slf4j` 어노테이션을 사용하기 위해서는 `Lombok 라이브러리`외에 의존성 설정 및 log4j.properties가 필요합니다.

  스프링 부트에서는 로깅 설정을 자동적으로 지원합니다. 다음과 같이 `slf4j 로깅 파사드`( 로깅 모듈을 추상화한 것 )를 통해 `logback`을 기본적으로 지원하고 있습니다.

  ```java
  @Component
  public class AppRunner implements ApplicationRunner {
    // slf4j 로깅 파사드를 통해 logback 로깅 모듈을 지원
    private Logger logger = LoggerFactory.getLogger(AppRunner.class);

    @Override
    public void run(ApplicationArguments args) throws Exception {
      logger.info("=============");
      logger.info("This is Spring Boot App");
      logger.info("=============");
  }
}
  ```

  ```java
  @SpringBootApplication
  public class Application {
    public static void main(String[] args)  {
        SpringApplication application = new SpringApplication(Application.class);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }
}
  ```

### application.properties를 통한 로깅 설정

  ```yml
  # application.properties
  # 콘솔 창에 출력되는 로깅 메세지를 색으로 구분해서 출력
  spring.output.ansi.enabled=always
  # 로그 메세지가 저장되는 로그 디렉터리
  logging.path=logs
  # logging.level.{패키지 경로}를 통해 로깅 레벨을 결정할 수 있슴
  logging.level.com.tutorial.springboot=DEBUG
  ```

### 스프링 부트 로깅 커스터마이징( Spring Boot Logging Customizing)

  스프링 부트에서는 기본적으로 `logback` 모듈을 제공합니다. 따라서 logback 모듈을 다음과 같이 xml 파일로 따로 설정 정보를 관리하면서 개발할 수 있습니다.

  ```xml
// logback-spring.xml <?xml version="1.0" encoding="utf-8" ?>
<configuration>
<include resource="org/springframework/boot/logging/logback/base.xml"/>
<logger name="com.tutorial.springboot" level="DEBUG"/>
</configuration>
  ```

  만일 logback을 사용하지 않고 다른 로깅 모듈로 바꾸고 싶을 때는 pom.xml에 다음과 같이 logback 모듈에 대한 의존성을 제거해야 합니다.

  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
  ```

  그 다음 log4j 같은 다른 로깅 모듈에 대한 의존성을 추가합니다.

  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
  ```

### @Slf4j를 사용하기 위한 의존성

#### spring-boot-starter-web

  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
  </dependency>
  ```

#### spring-boot-starter-logging

  ```xml
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>jcl-over-slf4j</artifactId>
  </dependency>
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>jul-to-slf4j</artifactId>
  </dependency>
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>log4j-over-slf4j</artifactId>
  </dependency>
  <dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
  </dependency>
  ```

## 참조

   > [https://engkimbs.tistory.com/767](https://engkimbs.tistory.com/767)
   >
   > [https://www.slideshare.net/whiteship/ss-47273947](https://www.slideshare.net/whiteship/ss-47273947)
