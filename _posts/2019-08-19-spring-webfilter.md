---
title: "@WebFilter"
layout: post
category: Spring
tags: [Spring]
excerpt: "@WebFilter 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @WebFilter 어노테이션에 대해서 배워 봅시다.

## @WebFilter

  서블릿 스펙 3.0 이전까지는 web.xml을 통해 Servlet과 Filter를 등록하고 URL 맵핑등을 설정하여 사용하였습니다. 그러나 서블릿 3.0 부터는 web.xml에서의 서블릿, 필터 설정을 자바 소스상에서 대체할 수 있는 어노테이션이 추가되었습니다.

  @WebFilter 어노테이션은 필터를 등록하고 설정하는 어노테이션입니다. 서블릿 3.0은 톰캣을 기준으로 7 버전부터 지원하므로 톰캣7 이상의 서블릿컨테이너를 사용한다면 @WebFilter 어노테이션을 사용하여 필터를 등록할 수 있습니다.

### XML로 필터 설정하는 방식

  > [XML로 필터 설정하는 방식](https://baekjungho.github.io/spring-interceptorfilter/#filter)

  XML로 필터를 설정할 때에는 web.xml의 `<filter>` 태그나 `<filter-mapping>` 태그를 통해 필터를 설정하였습니다.

### @WebFilter 어노테이션을 이용한 방식

#### 필터 이름 지정

  web.xml에서 `<filter>` 태그에는 `<filter-name>` 이라는 태그를 지원하여 필터이름을 설정했습니다.

  `@WebFilter은 filterName`이라는 속성을 통해 설정할 수 있습니다. 다만 filterName속성은 필수값이 아닙니다.

  ```java
@WebFilter(filterName ="loggingFilter")
public class LoggingFilter implements Filter {
}
  ```

#### 필터링 경로 지정

  web.xml에서는 `<filter-mapping>` 태그의 `<url-pattern>` 태그등을 통해서 필터링 대상 경로를 설정했었습니다.  @WebFilter 어노테이션의 필터링 대상 설정 방법은 다음과 같이 몇 가지 방법이 있습니다.

  - 어노테이션에 매핑 URL 작성
    - `@WebFilter("/target")`

  - 어노테이션에 와일드카드를 사용
    - `@WebFilter("/*")`

  - 어노테이션의 Value 속성을 이용
    - `@WebFilter(value="/target")`
    - `@WebFilter(value= {"/target", "/target2"})`

  - 어노테이션의 urlPatterns 속성을 이용하는 방법
    - `@WebFilter(urlPatterns="/target")`
    - `@WebFilter(urlPatterns= {"/target", "/target2"})`

  - 서블릿 등록시 사용한 서블릿 이름을 기준으로 지정하는 방법
    - `@WebFilter(servletNames="boardController")`
    - `@WebFilter(servletNames= {"boardController", "boardController2"})`

#### @WebFilter 초기 파라미터 설정 방법

  web.xml에서는 `<filter>` 태그의 `<init-param>` 태그를 통해  초기화 파라미터를 설정해 주었습니다. 만약 필터 초기화시에 설정할 파라미터를 지정하고 싶은 경우에는 initParams 속성에 `@WebInitParam` 어노테이션을 사용하여 속성명과 속성값을 지정해주면 됩니다.

  ```java
  @WebFilter(
    value = {"/target", "/target2"},
    initParams = @WebInitParam(name = "encoding", value = "UTF-8")
  )
  public class EncodingFilter implements Filter {
  }
  ```

  - 배열 형태로 설정값 여러개 주기

  ```java
  @WebFilter(
    initParams = {
        @WebInitParam(name = "encoding", value = "UTF-8"),
        @WebInitParam(name = "encoding2", value = "EUC-KR")
    }
  )
  ```

#### @WebFilter 필터 순서 지정 방법

  아쉽게도 @WebFilter 어노테이션은 필터들 사이의 순서를 지정할 수 있는 속성은 제공하지 않습니다. 따라서 두가지 정도 차선책이 있는데, 하나는 필터를 구현할 때 필터들의 기능을 서로의 순서에 의존하지 않도록 설계 하는것입니다. 즉 어떤 순서로 필터가 수행되어도 상관없이 필터 정책을 사용하는것이죠. 두번째로는 @WebFilter에서 필터이름을 설정해두고 `<filter-mapping>` 태그를 통해 web.xml에서 순서를 지정하는 방법입니다.

  ```java
@WebFilter(filterName= "firstFilter")
public class FirstFilter implements Filter {
}
  ```

  ```java
@WebFilter(filterName= "secondFilter")
public class SecondFilter implements Filter {
}
  ```

  아래와 같이 web.xml을 설정해두면 서블릿컨테이너가 시작될 때 web.xml의 필터 맵핑 태그를 순서대로 읽어 필터체인의 순서를 판단할것입니다.

  ```xml
  <filter-mapping>
      <filter-name>firstFilter</filter-name>
      <url-pattern>/*</url-pattern>
</filter-mapping>
<filter-mapping>
      <filter-name>secondFilter</filter-name>
      <url-pattern>/*</url-pattern>
</filter-mapping>
  ```

#### Dispatcher 지정하기

  필터의 디스패쳐 타입을 지정하고 싶은 경우에는 `dispatcherTypes` 속성을 사용합니다. 기본값은 DispatcherType.REQUEST 이며 배열로 여러개 지정할 수 있습니다.

  ```java
  @WebFilter(
        dispatcherTypes= {
                DispatcherType.REQUEST,
                DispatcherType.INCLUDE
        }
)
public class FirstFilter implements Filter {
}
  ```
