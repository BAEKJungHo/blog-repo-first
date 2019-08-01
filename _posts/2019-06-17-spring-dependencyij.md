---
title: "Dependency Injection"
layout: post
category: Spring
tags: [Spring]
excerpt: "의존성 주입(Dependency Injection)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 의존성 주입(Dependency Injection)에 대해서 배워 봅시다.

## 의존성 주입(DI)

  > 의존성 주입(Dependency Injection)
  >
  > 어떠한 객체에 의존하기 때문에 외부에서 객체를 만들어서 그 객체를 주입하는것
  >
  > 필요한 의존성을 어떻게 받아올 것인가? (생성자, Setter)

  ![d1](/images/posts/201906/d1.jpg)

  위 자동차, 로봇, 라디오 장난감들은 `배터리 객체에 의존하여` 배터리가 있어야 완벽한 객체가 됩니다.

  따라서 배터리에 의존하여 주입하기 때문에 `의존성 주입(DI : Dependency Injection)`이라고 합니다.

  위와 같이 배터리 일체형과 분리형이 있을 경우 이것을 코드로 나타내면 다음과 같습니다.

  ![d2](/images/posts/201906/d2.jpg)

  위 코드에서 `생성자와 Setter메소드가` [IOC(Inversion Of Control)](https://baekjungho.github.io/spring-ioc/) 즉, __의존성에 대한 제어반전__ 을 나타내는 코드이며, 위 생성자에 배터리라는 객체를 넣어 주는 과정이 `의존성 주입(DI)`를 뜻합니다.

  - IOC(Inversion Of Control)와 DI(Dependency Injection)

  ```java
  public class ElectronicRadioToy {
    private Battery battery;

    // 생성자를 통한 IOC : 의존성에 대한 제어 반전
    public ElectronicRadioToy(Battery battery) {
      this.battery = battery;
    }

    // Setter를 통한 IOC : 의존성에 대한 제어 반전
    public void setBattery(Battery battery) {
      this.battery = battery;
    }
  }

  class ElectronicRadioToyTest {
    @Test
    public void create() {
      // 의존성 주입(Dependency Injection)
      // Battery라는 의존성을 ElectronicRadioToy에게 주입
      Battery battery = new Battery();
      ElectronicRadioToy electronicRadioToy = new ElectronicRadioToy(battery);
    }
  }
  ```

## EXAMPLE 1.

  ```java
  @Controller
  class OwnerController {

  private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";
  private final OwnerRepository owners;

  // 생성자가 하나이면서, 파라미터 타입이 빈으로 등록 가능한 존재이므로
  // @Autowired 생략 가능
  // 생성자를 통해 빈(Bean)이 주입됨
  public OwnerController(OwnerRepository clinicService) {
      this.owners = clinicService;
   }
  }
  ```

## EXAMPLE 2.

  ```java
  public class StudentAssembler {

	private StudentDao studentDao;
	private StudentRegisterService registerService;
	private StudentModifyService modifyService;
	private StudentDeleteService deleteService;
	private StudentSelectService selectService;
	private StudentAllSelectService allSelectService;

	public StudentAssembler() {
		studentDao = new StudentDao();
		registerService = new StudentRegisterService(studentDao);
		modifyService = new StudentModifyService(studentDao);
		deleteService = new StudentDeleteService(studentDao);
		selectService = new StudentSelectService(studentDao);
		allSelectService = new StudentAllSelectService(studentDao);
	}

  .....
  }
  ```

  위와 같은 자바 코드에서 StudentAssembler의 생성자 안에 있는 코드를 `applicationContext.xml`에서
  스프링 방식으로 설정하면 다음과 같습니다. 즉, 위 코드와 아래 코드의 기능은 동일합니다.

  ```xml
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

	<bean id="selectService" class="ems.member.service.StudentSelectService">
		<constructor-arg ref="studentDao" ></constructor-arg>
	</bean>

	<bean id="allSelectService" class="ems.member.service.StudentAllSelectService">
		<constructor-arg ref="studentDao" ></constructor-arg>
	</bean>

  .....
  ```

  생성자에 의존성 주입을 위해서 `<constructor-arg ref="참조변수명">`과 같은 방식으로 사용하면
  `의존성 주입(DI)`이 이루어지는 것입니다.

  그리고 [IOC 컨테이너 메모리에 등록된 객체를 사용](https://baekjungho.github.io/spring-ioc/#applicationcontextxml-%EC%82%AC%EC%9A%A9%ED%95%9C-%EC%BD%94%EB%93%9C)하기 위해서 다음과 같이 하면 됩니다.

  1. GenericXmlApplicationContext 클래스를 사용
  2. GenericXmlApplicationContext 객체 생성시 초기화를 위해 파라미터에 `applicationContext resources`를 적어줘야 합니다.
  3. `"classpath:파일명"`
  4. getBean(bean Id, 클래스명.class) 메소드로 객체 가져오기

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

    .....
  ```

  위 코드를 보면 알 수 있듯이 순수 자바와 다르게 `스프링`을 사용하게 되면 `new 연산자` 대신에
  `getBean()`이라는 메소드를 통해 IOC 컨테이너에서 생성된 객체를 가져와서 쓰면 됩니다.

## applicationContext.xml을 사용한 다양한 의존 객체 주입 방법

  직접 applicationContext.xml에 bean을 등록하여 의존 객체를 주입하는 다양한 방법에 대해 배워봅시다.

### 생성자

  생정자를 이용한 의존 객체 주입 방법은 다음 그림과 같습니다.

  ![d4](/images/posts/201906/d4.jpg)

### setter

  setter를 이용한 의존 객체 주입 방법은 다음 그림과 같습니다.

  ![d5](/images/posts/201906/d5.jpg)

  property name은 setter 메소드 이름에서 set을 제거하고 맨 앞 대문자를 소문자로 바꾸면 됩니다.

  value는 파라미터로 넘어오는 값을 적어주면 됩니다.

### List

  List를 이용한 의존 객체 주입 방법은 다음 그림과 같습니다.

  ![d6](/images/posts/201906/d6.jpg)

  property name은 setter 규칙과 동일합니다.

### Map

  Map을 이용한 의존 객체 주입 방법은 다음 그림과 같습니다.

  ![d7](/images/posts/201906/d7.jpg)

  property name은 setter 규칙과 동일합니다.

  ```xml
  <property name="administrators">
  <map>
    <entry>
      <key>
        <value>Cheney</value> // key 값
      </key>
      <value>cheney@springPjt.org</value> // value 값
    </entry>
    <entry>
      <key>
        <value>Jasper</value> // key 값
      </key>
      <value>jasper@springPjt.org</value> // value 값
    </entry>
  </map>
  </property>
  ```

## 의존 객체 자동 주입

  > 의존 객체 자동 주입 ?  
  >
  > 스프링 설정 파일에서 의존 객체를 주입할 때 `<constructor-org>` 또는 `<property>` 태그로 의존 대상 객체를 명시하지 않아도 `스프링 컨테이너`가 자동으로 필요한 의존 대상 객체를 찾아서 의존 대상 객체가 필요한 객체에 주입해 주는 기능이다. 구현 방법은 @Autowired와 @Resource 어노테이션을 이용해서 쉽게 구현할 수 있다.

### @Autowired로 주입하기

  @Autowired 를 사용하면 주입하려고 하는 `객체의 타입`이 일치하는 객체를 자동으로 주입합니다.
  객체를 자동으로 주입하기 위해서는 생성자 등에 @Autowired 어노테이션을 적어 주어야 하며,
  스프링 설정파일(xml)에서 `<context:annotation-config />` 다음 태그를 사용해야 합니다.

  이 태그를 사용하기 위해서는 namespace를 추가 해주어야 하는데 다음과 같습니다.

  ```xml
  <beans xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd">
    <context:annotation-config />
  </beans>
  ```

  따라서 @Autowired 어노테이션을 사용하여 의존 객체를 자동으로 주입하기 위해서는
  위 코드를 기존 `<beans>`태그 안에 있는 스키마와 네임스페이스에 추가 해주어야 합니다.

  - 생성자에 @Autowired 어노테이션을 적용하지 않았을 때의 스프링 설정 파일

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="wordDao" class="com.word.dao.WordDao" />

	<bean id="registerService" class="com.word.service.WordRegisterService">
		<constructor-arg ref="wordDao" />
	</bean>

	<bean id="searchService" class="com.word.service.WordSearchService">
		<constructor-arg ref="wordDao" />
	</bean>
  </beans>
  ```

  생성자에 @Autowired 어노테이션을 적용하지 않았을 때에는 스프링 설정 파일에서
  `<constructor-org>` 태그를 명시해주어야 합니다.

  생성자에 @Autowired 어노테이션을 적용했을 때의 스프링 설정 파일 코드는 다음과 같습니다.

  - 생성자에 @Autowired 어노테이션을 적용했을 때의 스프링 설정 파일

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd
 		http://www.springframework.org/schema/context
 		http://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config />

	<bean id="wordDao" class="com.word.dao.WordDao" >
		<!-- <qualifier value="usedDao"/> -->
	</bean>
	<bean id="wordDao2" class="com.word.dao.WordDao" />
	<bean id="wordDao3" class="com.word.dao.WordDao" />

	<bean id="registerService" class="com.word.service.WordRegisterServiceUseAutowired" />

	<bean id="searchService" class="com.word.service.WordSearchServiceUseAutowired" />
  </beans>
  ```

  생성자에 @Autowired 만 적는다고 해서 의존 객체가 자동으로 주입되는 것이 아니고 스프링 설정파일에서
  `<context:annotation-config />` 태그와 이 것을 사용하기 위한 네임스페이스를 적어주어야 합니다.

  - 필드, 생성자, Setter에 @Autowired 적용

  생성자에 @Autowired 를 사용 할 때에는 그냥 사용하면되지만, 필드나 Setter에 사용할 때에는
  __반드시 기본 생성자(Default Constructor)를 명시 해주어야 합니다.__

  ```java
  package com.word.service;

  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.beans.factory.annotation.Qualifier;

  import com.word.WordSet;
  import com.word.dao.WordDao;

  public class WordRegisterServiceUseAutowired {

  	@Autowired
  	// @Qualifier("usedDao")
  	private WordDao wordDao;

  	public WordRegisterServiceUseAutowired() { }

  	// @Autowired
  	public WordRegisterServiceUseAutowired(WordDao wordDao) {
  		this.wordDao = wordDao;
  	}

  	public void register(WordSet wordSet) {
  		String wordKey = wordSet.getWordKey();
  		if(verify(wordKey)) {
  			wordDao.insert(wordSet);
  		} else {
  			System.out.println("The word has already registered.");
  		}
  	}

  	public boolean verify(String wordKey){
  		WordSet wordSet = wordDao.select(wordKey);
  		return wordSet == null ? true : false;
  	}

  	// @Autowired
  	public void setWordDao(WordDao wordDao) {
  		this.wordDao = wordDao;
  	}
  }
  ```

### @Autowired 생략

  어떠한 빈에 `생성자가 오직 하나만 있고`, 그 생성자에 파라미터 타입에 빈으로 등록 가능한 존재라면, 이 빈(Bean)은 @Autowired 어노테이션이 없더라도 주입을 해줍니다.

  - @Autowired와 @Inject를 붙일 수 있는 곳
    - 필드
    - 생성자
    - Setter

  생성자에 붙이는 걸 추천하고, 만약 Setter와 필드 중에서, Setter가 있으면 Setter에 붙이는걸 추천합니다.

### @Resource로 주입하기

  @Resource 를 사용하면 주입하려고 하는 `객체의 이름`이 일치하는 객체를 자동으로 주입합니다.
  @Autowired 와 같이 `<context:annotation-config />` 태그와 이 것을 사용하기 위한 네임스페이스를 적어 주어야 합니다.

  - @Resource를 붙일 수 있는 곳
    - 필드
    - Setter

  > @Resource는 생성자에 붙이지 못합니다. 따라서 기본 생성자를 항상 명시 해주어야 합니다.

  - @Resource를 적용한 스프링 설정 파일

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd
 		http://www.springframework.org/schema/context
 		http://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config />

	<bean id="wordDao" class="com.word.dao.WordDao" >
		<!-- <qualifier value="usedDao"/> -->
	</bean>
	<bean id="wordDao2" class="com.word.dao.WordDao" />
	<bean id="wordDao3" class="com.word.dao.WordDao" />

	<bean id="registerService" class="com.word.service.WordRegisterServiceUseResource" />

	<bean id="searchService" class="com.word.service.WordSearchServiceUseResource" />
  </beans>
  ```

  위 코드에서 wordDao, wordDao2, wordDao3 이렇게 있는데 이름이 같은 객체를 찾아가서 주입을 해줍니다.

  - 필드와 Setter에 @Resource 적용

  ```java
  package com.word.service;

  import javax.annotation.Resource;
  import org.springframework.beans.factory.annotation.Qualifier;
  import com.word.WordSet;
  import com.word.dao.WordDao;

  public class WordRegisterServiceUseResource {

  	@Resource
  	// @Qualifier("usedDao")
  	private WordDao wordDao;

  	public WordRegisterServiceUseResource() { }

  	// @Resource
  	public WordRegisterServiceUseResource(WordDao wordDao) {
  		this.wordDao = wordDao;
  	}

  	public void register(WordSet wordSet) {
  		String wordKey = wordSet.getWordKey();
  		if(verify(wordKey)) {
  			wordDao.insert(wordSet);
  		} else {
  			System.out.println("The word has already registered.");
  		}
  	}

  	public boolean verify(String wordKey){
  		WordSet wordSet = wordDao.select(wordKey);
  		return wordSet == null ? true : false;
  	}

  	// @Resource
  	public void setWordDao(WordDao wordDao) {
  		this.wordDao = wordDao;
  	}
  }
  ```

## 의존 객체 선택

  다수의 빈(Bean) 객체 중 의존 객체의 대상이 되는 객체를 선택하는 방법에 대해서 배워 봅시다.

  ![c1](/images/posts/201906/c1.jpg)

  동일한 객체가 여러개 존재 할 경우 스프링 컨테이너(IOC 컨테이너)는 자동 주입 대상 객체를 판단하지 못해서
  예외(Exception)를 발생 시킵니다. 따라서 동일한 객체가 여러개 존재할 경우 `<qualifier value="" />` 태그를 사용하여
  `우선순위(Priority)`를 주면 예외 발생을 막을 수 있습니다.

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

  qualifier 태그를 사용하지 않아도 같은 객체가 여러개 있을 경우에도, 의존 객체 자동 주입이 가능하게 할 수 있는데
  `필드명과 bean id의 이름이 같은경우` 이면 오류 없이 의존 객체 자동 주입이 이루어 집니다. 하지만 이 방법은 좋지 않은 방법으로
  추천하지 않고, qualifier 태그를 사용하는 것을 추천 합니다.

## 의존 객체 자동 주입 체크

  ![c2](/images/posts/201906/c2.jpg)

  의존 객체를 @Autowired 어노테이션을 이용해서 하는데, 스프링 설정파일에서 의존 객체가 명시되지 않은 경우
  오류 발생 없이도 가능하게 할 수 있는데 방법은 `@Autowired(required = false)` 를 사용하면 됩니다.
  __즉, 의존 객체가 있으면 주입을 하고 없으면 주입을 하지 말라 라는 의미를 지닙니다.__

  ```java
  @Autowired(required = false)
  private WordDao wordDao;
  ```

  ```xml
  <!-- <bean id="wordDao" class="com.word.dao.WordDao" /> -->
  <bean id="registerService" class="com.word.service.WordRegisterServiceUseAutowired" />
  <bean id="searchService" class="com.word.service.WordSearchServiceUseAutowired" />
  ```

## @Inject

  > @Inject
  >
  > @Autowired와 비슷하게 @Inject 어노테이션을 이용해서 의존 객체를 자동으로 주입을 할 수 있다.
  >
  > @Autowired와 차이점 이라면 @Autowired의 경우 required 속성을 이용해서 의존 대상 객체가 없어도 Exception을 피할 수 있지만, @Inject의 경우 required 속성을 지원하지 않는다.

  @Autowired랑 거의 비슷하다고 보면 되기 때문에 `<context:annotation-config />` 태그와 이 것을 사용하기 위한 네임스페이스를 적어 주어야 합니다.

  실무에서는 @Autowired가 @Inject보다 많이 쓰입니다.

  ![c3](/images/posts/201906/c3.jpg)

  @Autowired에서는 같은 객체가 여러개 있을 경우 `qualifier`라는 태그로 우선순위를 부여하여 Exception을 피할 수 있었는데, @Inject에서는
  `@Named(value="")` 어노테이션을 사용하여 Exception을 피할 수 있습니다.

### @Named

  ```java
  @Inject
  @Named(value="wordDao1")
  private WordDao wordDao;
  ```
  ```xml
  <bean id="wordDao1" class="com.word.dao.WordDao" />
  <bean id="wordDao2" class="com.word.dao.WordDao" />
  <bean id="wordDao3" class="com.word.dao.WordDao" />
  ```

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
  >
  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
