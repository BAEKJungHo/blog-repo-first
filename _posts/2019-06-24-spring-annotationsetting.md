---
title: "어노테이션을 이용한 스프링 설정"
layout: post
category: Spring
tags: [Spring]
excerpt: "XML을 이용한 스프링 설정파일 제작을 Java 파일로 제작할 수 있는 방법에 대해서 학습합니다. "
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 기존에 applicationContext.xml 즉, IOC 컨테이너를 만들어서 스프링 설정을 했던 것을 xml 파일을 만들지 않고, `어노테이션(Annotation)`을 사용하여 스프링 설정을 하는 방법을 배워 봅시다.

## 어노테이션을 이용한 스프링 설정

  ![a1](/images/posts/201906/a1.jpg)

## @Configuration, @Bean

  @Configuration 어노테이션은 스프링 설정 파일임을 명시해주는 어노테이션 입니다.
  즉, applicationContext.xml와 같은 역할을 합니다.

  @Bean 어노테이션은 다음 메소드가 bean 객체로 생성된다는 의미를 지닙니다.

  xml에서는 `<bean id="studentDao" class="ems.member.dao.StudentDao" />`다음과 같이 사용하였는데
  java 파일에서는 `메소드(Method)`를 사용합니다.

  아래 예제를 통해서 xml파일을 자바 메소드로 변환하는 방법에 대해서 배워 봅시다.

### EXAMPLE 1. bean 객체 생성

  ```java
  // <bean id="studentDao" class="ems.member.dao.StudentDao" />
  @Bean
  public StudentDao studentDao() {
    return new StudentDao();
  }
  ```

  __위 studentDao() 메소드의 역할은 StudentDao 객체를 생성(반환)하는 역할입니다.__

### EXAMPLE 2. bean 태그 안에 constructor 태그가 있는 경우

  ```java
  // 	<bean id="registerService" class="ems.member.service.StudentRegisterService">
	//	  <constructor-arg ref="studentDao" ></constructor-arg>
	//  </bean>
  @Bean
  public StudentRegisterService registerService() {
    return new StudentRegisterService(studentDao());
  }
  ```

### EXAMPLE 3. bean 태그 안에 property 태그가 있는 경우

  [setter 메소드를 property 태그로 나타내는 방법](https://baekjungho.github.io/spring-bean/#singleton)과 비슷합니다.

  ```java
  // <bean id="dataBaseConnectionInfoDev" class="ems.member.DataBaseConnectionInfo">
  //  <property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:xe" />
  //  <property name="userId" value="scott" />
  //  <property name="userPw" value="tiger" />
  // </bean>

  @Bean
	public DataBaseConnectionInfo dataBaseConnectionInfoDev() {
		DataBaseConnectionInfo infoDev = new DataBaseConnectionInfo();
		infoDev.setJdbcUrl("jdbc:oracle:thin:@localhost:1521:xe");
		infoDev.setUserId("scott");
		infoDev.setUserPw("tiger");

		return infoDev;
	}
  ```

### EXAMPLE 4. setter와 ArrayList, Map으로 변환 해야하는 경우

  ```xml
  <bean id="informationService" class="ems.member.service.EMSInformationService">
  <property name="info">
    <value>Education Management System program was developed in 2015.</value>
  </property>
  <property name="copyRight">
    <value>COPYRIGHT(C) 2015 EMS CO., LTD. ALL RIGHT RESERVED. CONTACT MASTER FOR MORE INFORMATION.</value>
  </property>
  <property name="ver">
    <value>The version is 1.0</value>
  </property>
  <property name="sYear">
    <value>2015</value>
  </property>
  <property name="sMonth">
    <value>1</value>
  </property>
  <property name="sDay">
    <value>1</value>
  </property>
  <property name="eYear" value="2015" />
  <property name="eMonth" value="2" />
  <property name="eDay" value="28" />
  <property name="developers">
    <list>
      <value>Cheney.</value>
      <value>Eloy.</value>
      <value>Jasper.</value>
      <value>Dillon.</value>
      <value>Kian.</value>
    </list>
  </property>
  <property name="administrators">
    <map>
      <entry>
        <key>
          <value>Cheney</value>
        </key>
        <value>cheney@springPjt.org</value>
      </entry>
      <entry>
        <key>
          <value>Jasper</value>
        </key>
        <value>jasper@springPjt.org</value>
      </entry>
    </map>
  </property>
  <property name="dbInfos">
    <map>
      <entry>
        <key>
          <value>dev</value>
        </key>
        <ref bean="dataBaseConnectionInfoDev"/>
      </entry>
      <entry>
        <key>
          <value>real</value>
        </key>
        <ref bean="dataBaseConnectionInfoReal"/>
      </entry>
    </map>
  </property>
  </bean>
  ```

  ```java
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
    dbInfos.put("dev", dataBaseConnectionInfoDev());
    dbInfos.put("real", dataBaseConnectionInfoReal());
    info.setDbInfos(dbInfos);

    return info;
  }
  ```  

### EXAMPLE 5. 전체 소스  

  - 기존 applicationContext.xml

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

	<bean id="dataBaseConnectionInfoDev" class="ems.member.DataBaseConnectionInfo">
		<property name="jdbcUrl" value="jdbc:oracle:thin:@localhost:1521:xe" />
		<property name="userId" value="scott" />
		<property name="userPw" value="tiger" />
	</bean>

	<bean id="dataBaseConnectionInfoReal" class="ems.member.DataBaseConnectionInfo">
		<property name="jdbcUrl" value="jdbc:oracle:thin:@192.168.0.1:1521:xe" />
		<property name="userId" value="masterid" />
		<property name="userPw" value="masterpw" />
	</bean>

	<bean id="informationService" class="ems.member.service.EMSInformationService">
		<property name="info">
			<value>Education Management System program was developed in 2015.</value>
		</property>
		<property name="copyRight">
			<value>COPYRIGHT(C) 2015 EMS CO., LTD. ALL RIGHT RESERVED. CONTACT MASTER FOR MORE INFORMATION.</value>
		</property>
		<property name="ver">
			<value>The version is 1.0</value>
		</property>
		<property name="sYear">
			<value>2015</value>
		</property>
		<property name="sMonth">
			<value>1</value>
		</property>
		<property name="sDay">
			<value>1</value>
		</property>
		<property name="eYear" value="2015" />
		<property name="eMonth" value="2" />
		<property name="eDay" value="28" />
		<property name="developers">
			<list>
				<value>Cheney.</value>
				<value>Eloy.</value>
				<value>Jasper.</value>
				<value>Dillon.</value>
				<value>Kian.</value>
			</list>
		</property>
		<property name="administrators">
			<map>
				<entry>
					<key>
						<value>Cheney</value>
					</key>
					<value>cheney@springPjt.org</value>
				</entry>
				<entry>
					<key>
						<value>Jasper</value>
					</key>
					<value>jasper@springPjt.org</value>
				</entry>
			</map>
		</property>
		<property name="dbInfos">
			<map>
				<entry>
					<key>
						<value>dev</value>
					</key>
					<ref bean="dataBaseConnectionInfoDev"/>
				</entry>
				<entry>
					<key>
						<value>real</value>
					</key>
					<ref bean="dataBaseConnectionInfoReal"/>
				</entry>
			</map>
		</property>
	</bean>
  </beans>
  ```

  - xml을 java 파일로 변환

  ```java
  package ems.member.configration;

  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.Map;

  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;

  import ems.member.DataBaseConnectionInfo;
  import ems.member.dao.StudentDao;
  import ems.member.service.EMSInformationService;
  import ems.member.service.StudentAllSelectService;
  import ems.member.service.StudentDeleteService;
  import ems.member.service.StudentModifyService;
  import ems.member.service.StudentRegisterService;
  import ems.member.service.StudentSelectService;

  @Configuration // 스프링 설정 파일임을 명시
  public class MemberConfig {

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
		dbInfos.put("dev", dataBaseConnectionInfoDev());
		dbInfos.put("real", dataBaseConnectionInfoReal());
		info.setDbInfos(dbInfos);

		return info;
	 }
  }
  ```

## MainClass

  메인 클래스에서 xml을 이용한 스프링 설정 파일을 사용했을 때에는 다음 코드로 스프링 컨테이너(IOC 컨테이너)를 생성했습니다.

  ```java
  GenericXmlApplicationContext ctx =
    new GenericXmlApplicationContext("classpath:applicationContext.xml");
  ```

  하지만 `@Configuration` 어노테이션을 이용한 자바 스프링 설정 파일을 사용할 경우에는 다음과 같습니다.

  ```java
  AnnotationConfigApplicationContext ctx =
    new AnnotationConfigApplicationContext(MemberConfig.class);
  ```

  new AnnotationConfigApplicationContext() 괄호 안에는 `@Configuration` 어노테이션이 적용된
  `컴파일된 자바 파일`을 적어주면 됩니다.

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
