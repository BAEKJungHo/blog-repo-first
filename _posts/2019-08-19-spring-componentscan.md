---
title: "@ComponentScan"
layout: post
category: Spring
tags: [Spring]
excerpt: "@ComponentScan 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @ComponentScan 어노테이션에 대해서 배워 봅시다.

## @ComponentScan

  @ComponentScan 어노테이션은 @ComponentScan 및 streotype(@Component, @Service, @Repository, @Controller) 어노테이션이 부여된 Class들을 자동으로 Scan하여 Bean으로 등록해주는 역할을 하는 어노테이션 입니다.

### 설정 방식

#### JAVA

```java
@ComponentScan(basePackages = "com.tech")
public class WebConfig extends WebMvcConfigurerAdapter { }
```

#### XML

```xml
<beans>
    .....
    <context:component-scan base-package="com.tech.blog" />
    .....
</beans>
```

### 어떻게 Component-scan이 동작하고 Bean에 주입되는지애 대한 동작 방식

#### Configuration 파싱

    ConfigurationClassParser 가 Configuration 클래스를 파싱한다.

#### ComponentScan 설정 내역을 파싱(핵심)

    __개발자는 basePackages, basePackesClasses, excludeFilters, includeFilters, lazyInit, nameGenerator, resourcePattern, scopedProxy 등 컴포넌트들을 스캔하기 위한 설정을 할 것이다. ComponentScanAnnotationParser가 컴포넌트 후보를 모두 찾고, 스캔하기 위하여 해당 설정을 파싱하여 가져온다.__

#### Class 로딩

    위의 basePackage 설정을 바탕으로 모든 클래스를 로딩 해야 한다.(* .class)

    클래스로더(ClassLoader)를 이용하여 모든 자원을 Resource 인터페이스 형태로 불러온다.

#### Bean 정의 설정

    클래스로더(ClassLoader)가 로딩한 리소스(클래스)를 BeanDefinition으로 정의해 놓는다. 그리고 beanName의 key값으로 BeanDefinitionRegistry에 등록해 놓는다. 생성할 빈에 대한 정의(메타데이터 같은)라고 보면 될 것 같다.

#### 빈 생성 및 주입

    다시 처음으로 돌아가 bstractApplicationContext에서 보이는 finishBeanFactoryInitialization(beanFactory); 메소드에서 빈을 생성한다.

    위에서 설정한 빈 정의를 바탕으로 객체를 생성하고, 주입한다.

#### 요약

    > Configuration 클래스 및 Annotation에 사용하는 설정들을 파싱한다. 그리고 basePackage 밑의 모든 .class 자원을 불러와서 component 후보인지 확인하여 BeanDefinition (빈 생성을 위한 정의)을 만든다. 생성된 빈 정의를 바탕으로 빈을 생성하고 의존성있는 빈들을 주입한다.

### 범위 지정

    ```java
    @ComponentScan(basePackages = "com.tech")
    ```

    위와 같은 방식으로 Scan 범위(basePackages)를 지정할 수 있습니다.

    basePackageClasses의 괄호 안에 적힌 Class가 위치한 곳에서부터 sterotype(@Component, @Service, @Repository, @Controller) 어노테이션이 부여된 Class를 빈으로 등록해 줍니다. Class를 통해 기입하기 때문에 훨씬 Typesafe한 방법입니다.

### 스프링 부트에서 @ComponentScan 사용

    SpringBoot에서 @SpringBootApplication 어노테이션을 사용하지만, 사실 아래 세가지 어노테이션의 조합입니다.

    ```java
    @Configuration
    @EnableAutoConfiguration
    @ComponentScan
    ```

    @ComponentScan을 @SpringBootApplication과 함께 사용할 수 있으며 결과는 동일합니다.

### 제외를 포함한 @ComponentScan

    @ComponentScan에 제외할 클래스의 패턴을 지정하여 필터를 사용할 수 있습니다.

    ```java
    @ComponentScan(excludeFilters = @ComponentScan.Filter(type=FilterType.REGEX, pattern "com.\\.baeeldung\\.componentscan\\.springapp\\..*"))
    ```

## @ServletComponentScan

  > [@ServletComponentScan Documents](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/servlet/ServletComponentScan.html)

  @WebFilter 어노테이션과 같이 톰캣 xml에 등록되는 Filter들을 스캔하기 위해 사용됩니다.
