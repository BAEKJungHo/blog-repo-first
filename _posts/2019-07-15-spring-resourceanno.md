---
title: "@Resource"
layout: post
category: Spring
tags: [Spring]
excerpt: "@Resource 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## @Resource

  [DI(Dependency Injection)](https://baekjungho.github.io/spring-dependencyij/#resource%EB%A1%9C-%EC%A3%BC%EC%9E%85%ED%95%98%EA%B8%B0)를 공부할 때 @Resource는 주입하려고 하는 __객체의 이름이 일치하는 객체를 자동으로 주입__ 한다고 배웠습니다.

  @Resource는 필드와 Setter에만 붙일 수 있고, 생성자에는 붙이지 못합니다. @Resource 어노테이션을 컨트롤러가 아닌 Bean으로 등록되지 않은 일반클래스에서 사용시 @Resource는 생성자에 붙이지 못하므로, 따라서 기본 생성자를 항상 명시 해주어야 합니다.

  @Resource를 사용하기 위해서 스프링 설정 파일(IOC 컨테이너, servlet-context.xml)에서 빈(Bean)을 생성해야 합니다.

  예를들어 파일 업로드를 위해, 서버에 저장될 파일 경로를 스프링 설정 파일에서 Bean으로 등록을하면 Controller에서 @Resource 어노테이션을 사용하여
  객체(Bean)를 주입할 수 있습니다.

  ```xml
  <beans:bean id="uploadPath" class="java.lang.String">
    <beans:constructor-arg value="C:\Users\MAYEYE\Desktop\workspace\Github\SpringBoard\Board\src\main\webapp\WEB-INF\uploadFiles"></beans:constructor-arg>
  </beans:bean>
  ```

  ```java
  /** @Resource
* 객체(Bean)의 이름이 일치하는 객체(Bean) 자동주입
* name 속성명에는 IOC 컨테이너에서 설정한 id명으로 입력
* 스프링 설정 파일을 불러올 때 유용하게 쓰임
* name 속성으로 지정한 uploadPath를 사용 할 수 있음
*/
@Resource(name="uploadPath")
private String uploadPath;
  ```

  스프링 설정파일에서 `uploadPath`로 객체 이름을 설정 했으므로, 컨트롤러에서도 @Resource 어노테이션을 사용할 때, 객체의 이름을
  일치 시켜줘야 합니다. 따라서 @Resource 어노테이션이 적용된 uploadPath 변수는 스프링 설정 파일에서 지정한 경로를 가지고 있으므로
  그냥 변수명으로 사용하시면 됩니다.

## @Qualifier

  Dependency Injection을 할 때, 동일한 객체가 여러개 존재 할 경우 스프링 컨테이너(IOC 컨테이너)는 자동 주입 대상 객체를 판단하지 못해서 예외(Exception)를 발생 시킵니다. 따라서 동일한 객체가 여러개 존재할 경우 `<qualifier value="" />` 태그를 사용하여 `우선순위(Priority)`를 주면 예외 발생을 막을 수 있습니다.

  ```java
@Autowired
@Qualifier("usedDao")
private WordDao wordDao;
  ```

  ```xml
<bean id="wordDao1" class="com.word.dao.WordDao" >
  <qualifier value="usedDao"/>
</bean>
<bean id="wordDao2" class="com.word.dao.WordDao" />
<bean id="wordDao3" class="com.word.dao.WordDao" />
  ```

  만약 @Resource 어노테이션을 사용할 경우, 객체의 이름이 같은것으로 주입을 하므로 객체 참조 변수를 선언할 때에 스프링 컨테이너에 등록된 Bean 이름과 일치 시키면 됩니다.

  ```java
  // wordDao1의 객체를 주입
  @Resource
  private WoardDao wordDao1;
  ```
