---
title: "Java 스프링 설정 파일 분리"
layout: post
category: Spring
tags: [Spring]
excerpt: "@Configuration을 적용한 자바 스프링 설정 파일을 기능 단위로 분리하는 방법을 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @Configuration을 적용한 자바 스프링 설정 파일을 기능 단위로 분리하는 방법을 배워 봅시다.

## 스프링 설정 파일 분리

  ![a2](/images/posts/201906/a2.jpg)

  [Java 스프링 설정 파일 전체 소스](https://baekjungho.github.io/spring-annotationsetting/#example-5-%EC%A0%84%EC%B2%B4-%EC%86%8C%EC%8A%A4)를
  각 기능 단위로 분리하는 방법과, Import를 사용하는 방법을 배워 봅시다.

## 기능 단위로 분리

  파일 이름은 1, 2, 3 보다는 기능에 알맞게 설정해주는 것이 좋습니다.

  - Dao와 Service를 묶은 파일

  ```java
  @Configuration
  public class MemberConfig1 {

	@Bean
	public StudentDao studentDao() {
		return new StudentDao();
	}

	@Bean
	public StudentRegisterService registerService() {
		return new StudentRegisterService(studentDao());
	}

	@Bean
	public StudentModifyService modifyService() {
		return new StudentModifyService(studentDao());
	}

	@Bean
	public StudentSelectService selectService() {
		return new StudentSelectService(studentDao());
	}

	@Bean
	public StudentDeleteService deleteService() {
		return new StudentDeleteService(studentDao());
	}

	@Bean
	public StudentAllSelectService allSelectService() {
		return new StudentAllSelectService(studentDao());
	 }
  }
  ```

  - DataBaseConnection과 관련된 파일

  ```java
  @Configuration
  public class MemberConfig2 {

	@Bean
	public DataBaseConnectionInfo dataBaseConnectionInfoDev() {
		DataBaseConnectionInfo infoDev = new DataBaseConnectionInfo();
		infoDev.setJdbcUrl("jdbc:oracle:thin:@localhost:1521:xe");
		infoDev.setUserId("scott");
		infoDev.setUserPw("tiger");

		return infoDev;
	}

	@Bean
	public DataBaseConnectionInfo dataBaseConnectionInfoReal() {
		DataBaseConnectionInfo infoReal = new DataBaseConnectionInfo();
		infoReal.setJdbcUrl("jdbc:oracle:thin:@192.168.0.1:1521:xe");
		infoReal.setUserId("masterid");
		infoReal.setUserPw("masterpw");

		return infoReal;
	 }
  }
  ```

  - 그 외 Util과 관련된 파일

  ```java
  @Configuration
  public class MemberConfig3 {
    
	@Autowired
	DataBaseConnectionInfo dataBaseConnectionInfoDev;

	@Autowired
	DataBaseConnectionInfo dataBaseConnectionInfoReal;

	@Bean
	public EMSInformationService informationService() {
		EMSInformationService info = new EMSInformationService();
		info.setInfo("Education Management System program was developed in 2015.");
		info.setCopyRight("COPYRIGHT(C) 2015 EMS CO., LTD. ALL RIGHT RESERVED. CONTACT MASTER FOR MORE INFORMATION.");
		info.setVer("The version is 1.0");
		info.setsYear(2015);
		info.setsMonth(1);
		info.setsDay(1);
		info.seteYear(2015);
		info.seteMonth(2);
		info.seteDay(28);

		ArrayList<String> developers = new ArrayList<String>();
		developers.add("Cheney.");
		developers.add("Eloy.");
		developers.add("Jasper.");
		developers.add("Dillon.");
		developers.add("Kian.");
		info.setDevelopers(developers);

		Map<String, String> administrators = new HashMap<String, String>();
		administrators.put("Cheney", "cheney@springPjt.org");
		administrators.put("Jasper", "jasper@springPjt.org");
		info.setAdministrators(administrators);

		Map<String, DataBaseConnectionInfo> dbInfos = new HashMap<String, DataBaseConnectionInfo>();
		dbInfos.put("dev", dataBaseConnectionInfoDev);
		dbInfos.put("real", dataBaseConnectionInfoReal);
		info.setDbInfos(dbInfos);

		return info;
	 }
  }
  ```

  위 코드에서 @Autowired로 의존성 자동 주입을 하는 이유는 다음과 같습니다.

  ```java
  // MemberConfig2에서 생성된 bean 객체를 자동주입 하라는 의미

  @Autowired
  DataBaseConnectionInfo dataBaseConnectionInfoDev;

  @Autowired
  DataBaseConnectionInfo dataBaseConnectionInfoReal;

  ```

  MemberConfig2에서 생성된 DataBaseConnection과 관련된 Bean 객체가 MemberConfig3에서

  ```java
  dbInfos.put("dev", dataBaseConnectionInfoDev);
  dbInfos.put("real", dataBaseConnectionInfoReal);
  ```

  다음 코드와 같이 사용되야 하기 때문에 `@Autowired`로 자동주입을 하여 필요한 객체를 얻는 것입니다.

  따라서 dbInfos.put("dev", dataBaseConnectionInfoDev); 이 코드에서 dataBaseConnectionInfoDev() 메소드를 사용하는 것이 아니고
  @Autowired로 참조변수에 의존성이 자동주입 되어 bean 객체가 생성 되었기 때문에 `dataBaseConnectionInfoDev 참조변수`를 적기만 하면 되는 것입니다.

### MainClass

  Java 스프링 설정 파일을 3개로 분리하였기 때문에 AnnotationConfigApplicationContext을 사용할 때 다음과 같이 하면 됩니다.

  ```java
  AnnotationConfigApplicationContext ctx =
    new AnnotationConfigApplicationContext(MemberConfig1.class, MemberConfig2.class, MemberConfig3.class);
  ```

## @Import

  > @Import({파일명1.class, 파일명2.class})

  ```java
  @Configuration
  @Import({MemberConfig2.class, MemberConfig3.class})
  public class MemberConfigImport {
  	@Bean
  	public StudentDao studentDao() {
  		return new StudentDao();
  	}

  	@Bean
  	public StudentRegisterService registerService() {
  		return new StudentRegisterService(studentDao());
  	}

  	@Bean
  	public StudentModifyService modifyService() {
  		return new StudentModifyService(studentDao());
  	}

  	@Bean
  	public StudentSelectService selectService() {
  		return new StudentSelectService(studentDao());
  	}

  	@Bean
  	public StudentDeleteService deleteService() {
  		return new StudentDeleteService(studentDao());
  	}

  	@Bean
  	public StudentAllSelectService allSelectService() {
  		return new StudentAllSelectService(studentDao());
  	}
  }
  ```

### MainClass

  import를 사용했을 경우 `@Import` 어노테이션을 사용한 자바 파일을 AnnotationConfigApplicationContext에 적어주면 됩니다.

  ```java
  AnnotationConfigApplicationContext ctx =
    new AnnotationConfigApplicationContext(MemberConfigImport.class);
  ```

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
