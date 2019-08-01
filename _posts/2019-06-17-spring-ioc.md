---
title: "Inversion Of Control"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링의 IOC(Inversion Of Control)와 IOC 컨테이너에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링의 IOC(Inversion Of Control)와 IOC 컨테이너에 대해서 배워 봅시다.

## Inversion Of Control

  `IOC(Inversion Of Control)`의 사전적 의미는 제어가 반전되었다, 제어가 뒤바뀌었다 라는
  의미를 지니고 있습니다. (즉, 제어의 반전, 제어의 역전)

  스프링에서 IOC의 의미는 주로 `의존성에 대한 제어 반전`을 의미합니다.

  [Spring PetClinic](https://projects.spring.io/spring-petclinic/) 프로젝트의 예시를 들어 설명하겠습니다.

## EXAMPLE

  Spring PetClinic의 OwnerController를 보면 소스가 다음과 같이 나와 있습니다.

  ```java
  @Controller
  class OwnerController {

  private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";
  private final OwnerRepository owners;

  public OwnerController(OwnerRepository clinicService) {
      this.owners = clinicService;
   }
  }
  ```

  위 코드를 보면 알 수 있듯이, OwnerController는 `OwnerRepository`를 owners라는 참조변수를 통해서 사용합니다.
  만약에 스프링을 사용하지 않았더라면(즉, IOC를 하지 않았더라면) 위 코드가 어떻게 짜여졌을지 봅시다.

  ```java
  // 일반적인 의존성에 대한 제어권 : 내가 쓸 놈은 내가 만들어 쓴다.
  class OwnerController {
    private OwnerRepository repository = new OwnerRepository();
  }
  ```

  즉, OwnerRepository는 OwnerController의 `의존성(Dependency)`입니다. __IOC는 자신 이외의 누군가가
  밖에서 넣어준다는 의미를 지니는데__ 아래의 코드를 보면 쉽게 이해하실 수 있습니다.

  ```java
  // Inversion Of Control
  // 아래와 같은 형태의 의존성 관리가 IOC(Inversion Of Control)입니다.
  class OwnerController {
    private OwnerRepository repo; // repo라는 OwnerRepository 참조변수를 통해

    // 생성자에서 누군가가 넣어준다는 것을 가정(즉, IOC를 하는 과정)
    public OwnerController(OwnerRepository repo) {
      this.repo = repo;
    }

    // repo를 사용하는 코드
  }

  class OwnerControllerTest {
    @Test
    public void create() {
      // 의존성 주입
      // OwnerRepository라는 의존성을 OwnerController에게 주입
      OwnerRepository repo = new OwnerRepository();
      OwnerController controller = new OwnerController(repo);
    }
  }
  ```

## IOC 컨테이너

  스프링 프레임워크는 Inversion Of Control용의 컨테이너를 제공해 주는데, IOC 컨테이너는 스프링에서 객체를 생성하고 조립하는 컨테이너로,
  __IOC 컨테이너는 객체를 생성하고 관리합니다. 즉, 빈(Bean)을 관리합니다.__ 컨테이너의 가장 핵심적인 인터페이스는 `ApplicationContext(BeanFactory)`입니다. 즉, ApplicationContext는 `IOC 컨테이너`라고 불립니다.

  ![d3](/images/posts/201906/d3.jpg)

  위 그림에서 큰 객체안에 작은 객체가 들어있는 동그라미를 볼 수 있는데, 이것은 큰 객체가 작은 객체들한테 의존하고 있다.
  즉, `의존성 주입(Dependency Injection)` 을 나타내는 것입니다.

  > [ApplicationContext API](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)

  __스프링 설정 파일인 applicationContext.xml은 src/main/resources 안에 있습니다.__

  ApplicationContext는 위 IOC 설명을 위해 예제로 들었던 코드가 동작하게 해줍니다. 즉, 위 `OwnerController와 OwnerRepository`는
  ApplicationContext에서 만들어 주는 `빈(Bean)`입니다. 따라서 이 둘의 의존성은 IOC 컨테이너가 관리를 해줍니다.
  오로지 빈(Bean)만 관리를 할 수 있으며, OwnerController와 OwnerRepository는 빈(Bean)이지만, Owner는 빈(Bean)이 아닙니다.

  > 빈(Bean)인지 아닌지 구분하는 방법은 @Controller, @Component, @Service, @Repository 어노테이션이 붙어있으면 `Component Scan`이라는
  방법을 통해서 자동으로 빈으로 등록이 됩니다.

  __빈(Bean)으로 등록 가능한 것들은 IOC 컨테이너 내부에서 객체를 만들고 그 객체의 의존성을 관리를 해줍니다.__

  ```java
  @Controller
  class OwnerController {

  private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";
  private final OwnerRepository owners;

  public OwnerController(OwnerRepository clinicService) {
      this.owners = clinicService;
   }
  }
  ```

  따라서 위 코드를 보면 OwnerController에 필요한 OwnerRepository 타입의 `빈(Bean)`을 찾아서 OwnerController의 생성자에 주입을 해주기 때문에
  this.owners는 null이 아닙니다.

## ApplicationContext.xml

  ApplicationContext는 IOC 컨테이너라고 했습니다. 즉, bean(빈)을 관리합니다.

  다음 예제를 통해서 배워 봅시다. 기존 코드와, ApplicationContext.xml을 사용한 코드를 비교해 봅시다.

### 기존 코드

  ```java
  public class TransportationWalk {
  	public void move() {
  		System.out.println("도보로 이동합니다.");
  	 }
  }

  public class MainApplication {
  	public static void main(String[] args) {
      // TransportationWalk라는 객체를 생성해서 메소드 호출
  		TransportationWalk walk = new TransportationWalk();
  		walk.move();
  	}
  }
  ```

### ApplicationContext.xml 사용한 코드

  applicationContext.xml은 대부분 비슷하기 때문에 재활용해서 사용할 수 있습니다.
  또한 객체(빈)을 생성하는 부분인 `<bean />`이 부분에서 id는 사용자가 임의로 만들어서
  사용할 수 있고, class 부분은 `프로젝트명.클래스명`으로 지정 해야 합니다.

  ```java
  // ApplicationContext.xml
  // 경로 : src/main/resources
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://www.springframework.org/schema/beans
   		http://www.springframework.org/schema/beans/spring-beans.xsd">

    // 이 부분이 bean(빈, 객체)를 생성하는 부분입니다.
    // 스프링 컨테이너 IOC 메모리 부분에 로딩이 됩니다.
    // 따라서 MainApplication에서는 객체를 생성할 필요가 없습니다.
  	<bean id="tWalk" class="lec03Pjt001.TransportationWalk" />

  </beans>
  ```

  스프링 컨테이너(IOC 컨테이너)의 메모리에 등록된 객체를 메인 클래스에서 사용하기 위해서는
  IOC 컨테이너에 접근을 해야 하는데 방법은 다음과 같습니다.

  1. GenericXmlApplicationContext 클래스를 사용
  2. GenericXmlApplicationContext 객체 생성시 초기화를 위해 파라미터에 `applicationContext resources`를 적어줘야 합니다.
  3. `"classpath:파일명"`
  4. getBean(bean Id, 클래스명.class) 메소드로 객체 가져오기

  ```java
  // MainApplication.java
  // 경로 : src/main/java
  import org.springframework.context.support.GenericXmlApplicationContext;
  public class MainApplication {
    public static void main(String[] args) {

  		// 자바 방식
  		// TransportationWalk transportationWalk = new TransportationWalk();
  		// transportationWalk.move();

  		// 스프링 방식 : IOC 컨테이너 생성
  		GenericXmlApplicationContext ctx =
  				new GenericXmlApplicationContext("classpath:applicationContext.xml");

  		// getBean(bean Id, 클래스명.class) 메소드로 객체 가져오기
  		TransportationWalk transportationWalk = ctx.getBean("tWalk", TransportationWalk.class);
  		transportationWalk.move();

  		// 사용한 resources 반환
  		ctx.close();
  	}
  }
  ```

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
  >
  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
