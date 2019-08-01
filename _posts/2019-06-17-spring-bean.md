---
title: "빈(Bean)"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 IOC 컨테이너가 관리하는 객체인 Bean에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 IOC 컨테이너가 관리하는 객체인 Bean에 대해서 배워 봅시다.

## Bean

  > 빈(Bean) ?
  >
  > IOC 컨테이너가 관리하는 객체를 뜻합니다. 즉, IOC 컨테이너를 통해 생성된 객체를 Bean이라 합니다.

  - __IOC 컨테이너에 빈(Bean)을 등록하는 방법__
    - __Component Scanning__
      - @Component
        - @Repository
        - @Service
        - @Controller
    - __직접 일일이 XML이나 자바 설정파일에 등록__

  - __빈(Bean)을 꺼내 쓰는 방법__
    - @Autowired
    - @Inject
    - ApplicationContext에서 getBean()으로 직접 꺼내기

  > 오직, 빈(Bean)들만 의존성 주입을 해줄 수 있습니다.

## EXAMPLE 1.

  빈을 직접 등록하는 방법은 `@Bean` 어노테이션을 사용하여 할 수 있습니다.
  이때 메소드 이름이 Bean이 됩니다.

  단 @Bean 어노테이션을 쓰기 위해서는 `@Configuration`이라는 어노테이션을 가지고 있는 곳에만
  사용할 수 있으며, @SpringBootApplication 어노테이션은 @Configuration 을 가지고 있습니다.

  ```java
  @SpringBootApplication
  public class PetClinicApplication {

    @Bean
    public String baek() {
        return "baek";
    }

    public static void main(String[] args) {
        SpringApplication.run(PetClinicApplication.class, args);
    }
  }
  ```

  빈(Bean)을 꺼내 쓰는 방법은 다음과 같습니다.

  ```java
  @RestController
  public class SampleController {

    @Autowired
    String baek;

    @GetMapping("/context")
    public String context() {
        return "hello" + baek;
    }
  }
  ```

## EXAMPLE 2.

  직접 `applicationContext.xml` 스프링 설정파일에 bean을 만들고, getBean() 메소드로 꺼내 쓰는 방법

  - applicationContext.xml

  ```java
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="studentDao" class="ems.member.dao.StudentDao" ></bean>

	<bean id="registerService" class="ems.member.service.StudentRegisterService">
		<constructor-arg ref="studentDao" ></constructor-arg>
	</bean>

	<bean id="modifyService" class="ems.member.service.StudentModifyService">
		<constructor-arg ref="studentDao" ></constructor-arg>
	</bean>

	<bean id="deleteService" class="ems.member.service.StudentDeleteService">
		<constructor-arg ref="studentDao" ></constructor-arg>
	</bean>

  .....
  ```

  - MainClassUseXML.java

  ```java
  //		StudentAssembler assembler = new StudentAssembler();
		GenericXmlApplicationContext ctx =
				new GenericXmlApplicationContext("classpath:applicationContext.xml");

		EMSInformationService informationService = ctx.getBean("informationService", EMSInformationService.class);
		informationService.outputEMSInformation();

  //		StudentRegisterService registerService = assembler.getRegisterService();
		StudentRegisterService registerService = ctx.getBean("registerService", StudentRegisterService.class);
		for (int j = 0; j < sNums.length; j++) {
			Student student = new Student(sNums[j], sIds[j], sPws[j], sNames[j],
					sAges[j], sGenders[j], sMajors[j]);
			registerService.register(student);
		}
  ```

  > [IOC 컨테이너 메모리에 등록된 객체를 사용하는 방법 배우기](https://baekjungho.github.io/spring-ioc/#applicationcontextxml-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%BD%94%EB%93%9C)

## 빈(Bean)의 범위

  ![z2](/images/posts/201906/z2.jpg)

### Singleton

  > Default는 싱글톤(Singleton) 입니다.

  우리가 자바 코드에서 `new ClassName()`과 같이 객체를 3번 생성했다고 하면 new ClassName()으로
  생성한 3개의 객체는 서로 다릅니다.

  하지만 스프링에서는 IOC 컨테이너가 미리 객체(즉, Bean)를 생성해 놓은 상태이고 우리는 자바 코드에서
  getBean() 메소드로 호출하여 사용만 하면 되기 때문에 getBean("A")를 두 번 쓰더라도 같은 객체(Bean)가 반환됩니다.

  즉, 자바에서는 `객체를 생성`한 것이고 스프링에서는 미리 만들어진 `객체를 참조` 하는 것입니다. 따라서 이 방식을
  `싱글톤(Singleton)`이라고 합니다.

### Prototype

  프로토타입(Prototype)은 싱글톤(Singleton)과 반대되는 개념으로, __호출할 때마다 같은 데이터 타입이지만 다른 객체가 생성되길 원할 때 사용하며__ , 개발자가 직접 설정해주어야 하며, 스프링 설정 파일에서 Bean 객체를 정의할 때 `scope` 속성을 명시해주면 됩니다.

## EXAMPLE Singleton.

  - applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="injectionBean" class="scope.ex.InjectionBean" />

	<bean id="dependencyBean" class="scope.ex.DependencyBean">
		<constructor-arg ref="injectionBean" />
		<property name="injectionBean" ref="injectionBean" />
	</bean>
  </beans>
  ```

  - DependencyBean.java

  ```java
  public class DependencyBean {

	private InjectionBean injectionBean;

	public DependencyBean(InjectionBean injectionBean) {
		System.out.println("DependencyBean : constructor");
		this.injectionBean = injectionBean;
	}

	public void setInjectionBean(InjectionBean injectionBean) {
		System.out.println("DependencyBean : setter");
		this.injectionBean = injectionBean;
	}
  }
  ```

  - InjectionBean.java

  ```java
  public class InjectionBean { }
  ```

  - MainClass.java

  ```java
  import org.springframework.context.support.GenericXmlApplicationContext;

  public class MainClass {
	public static void main(String[] args) {

		GenericXmlApplicationContext ctx =
				new GenericXmlApplicationContext("classpath:applicationContext.xml");

		InjectionBean injectionBean =
				ctx.getBean("injectionBean", InjectionBean.class);

		DependencyBean dependencyBean01 =
				ctx.getBean("dependencyBean", DependencyBean.class);

		DependencyBean dependencyBean02 =
				ctx.getBean("dependencyBean", DependencyBean.class);

		if(dependencyBean01.equals(dependencyBean02)) {
			System.out.println("dependencyBean01 == dependencyBean02");
		} else {
			System.out.println("dependencyBean01 != dependencyBean02");
		}
		ctx.close();
	 }
  }
  ```

  스프링 설정 파일에서 scope 명시를 안해줬으므로 결과는 dependencyBean01 == dependencyBean02 다음과 같이 나옵니다.
  만약 두 번 호출되게 하고 싶은경우에는 bean 태그에 scope 속성을 명시하고 prototype을 적어주면 됩니다.

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
