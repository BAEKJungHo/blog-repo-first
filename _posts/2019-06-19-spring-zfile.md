---
title: "XML 스프링 설정 파일 분리"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 설정 파일인 applicationContext.xml 파일을 분리하여 관리하는 방법을 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 스프링 설정 파일 분리

  applicationContext.xml 파일 이름은 자신이 원하는 대로 적어도 되지만, 상징적인 의미로 여기서는
  IOC 컨테이너의 핵심 인터페이스인 `applicationContext` 이름을 따서 지었습니다. 이 applicationContext.xml 파일이
  커질 수 있기 때문에 분리하여 관리하는 방법을 배워 봅시다.

  즉, 빈(Bean, 객체)을 관리하는 IOC 컨테이너를 `역할 분담`방식으로 분리하겠다는 의미입니다.

  다시말해, 1개 였던 IOC 컨테이너를 여러개로 쪼개는 것입니다.

  ![z1](/images/posts/201906/z1.jpg)

  파일을 분리하게 되면 기존에 `GenericXmlApplicationContext`를 하나만 사용했는 파일이 3개 이므로
  배열로 classpath를 받아서 `GenericXmlApplicationContext` 생성자에 배열 참조 변수를 넣어서 사용하면 됩니다.

  - 기존 코드

  ```java
  GenericXmlApplicationContext ctx =
    new GenericXmlApplicationContext("classpath:applicationContext.xml");
  ```

  - 변경 코드

  ```java
  String[] appCtxs = {"classpath:appCtx1.xml", "classpath:appCtx2.xml", "classpath:appCtx3.xml"};
  GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(appCtxs);
  ```

  - import를 사용 했을 때의 코드

  ```java
  GenericXmlApplicationContext ctx =
    new GenericXmlApplicationContext("classpath:appCtxImport.xml");
  ```

### 분리 방법

  분리하는 방법은 개발자가 자신이 마음대로 분리하면 되긴 하지만, 일반적으로는 `기능 단위`로 구분지어서
  분리하면 됩니다.

## EXAMPLE

  기존 IOC 컨테이너가 1개인 applicationContext.xml을 3개로 쪼개는 과정입니다.
  생성자 부분, 데이터베이스 관련 부분, 그 외 부분으로 분리한 코드 입니다.

  분리한 파일명은 기능에 알맞게 지어주면 됩니다.

  - applicationContext.xml

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

  - appService.xml(appCtx1)

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
  </beans>
  ```

  - appDatabase.xml(appCtx2)

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

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
  </beans>
  ```

  - appInformation.xml(appCtx3)

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

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

## import

  import를 사용해서 3개로 분리한 appCtx1, appCtx2, appCtx3 파일을 appCtx1 파일에서 appCtx2와 appCtx3파일을 import 하면 __하나의 IOC 컨테이너인 xml 파일이 완성 됩니다.__

  - 추가 해야 하는 소스

  ```xml
  <import resource="classpath:appCtx2.xml"/>
  <import resource="classpath:appCtx3.xml"/>
  ```

  - appCtx1에서 appCtx2와 appCtx3을 import한 하나의 IOC 컨테이너 XML 파일

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<import resource="classpath:appCtx2.xml"/>
	<import resource="classpath:appCtx3.xml"/>

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

  </beans>
  ```

  - 메인 클래스에서 GenericXmlApplicationContext 사용하는 방법

  ```java
  GenericXmlApplicationContext ctx =
    new GenericXmlApplicationContext("classpath:appCtxImport.xml");
  ```

## 참조

  > [자바 스프링 프레임워크(ver.2018)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew#)
