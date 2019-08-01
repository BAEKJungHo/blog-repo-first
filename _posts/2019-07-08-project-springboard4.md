---
title: "[스프링 MVC 게시판] 4. MySQL, MyBatis DB 연동"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## MySQL, MyBatis DB 연동

### MySQL, MyBatis, Junit Dependency 설정

  위에서 Maven Dependency 설정을 했으면 Junit으로 DB연동이 되는지 테스트를 해야 합니다.

  STS로 스프링 프로젝트를 생성하면 Junit 버전이 Default로 4.7로 설정되어있습니다. 하지만 위 Dependency를 보면
  Junit이 4.12되어있습니다. 꼭 수정 하셔야 합니다.

  > 필수 : Junit Version 4.12

  - MyBitis 3.4.1 : MyBitis 프레임워크

  - MyBitis-Spring : Spring과 MyBitis를 연결하는 라이브러리

  - Spring-jdbc : jdbc 라이브러리

  - Spring-test : 스프링과 MyBitis가 정상적으로 연동되었는지 확인하기 위해 필요한 라이브러리

  추가 해야할 의존성은 아래와 같습니다.

  ```xml
  <!-- MySQL -->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>6.0.5</version>
  </dependency>

  <!-- MyBatis 3.4.1 -->
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.1</version>
  </dependency>

  <!-- MyBatis-Spring -->
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
  </dependency>

  <!-- Spring-jdbc -->
  <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${org.springframework-version}</version>
  </dependency>

  <!-- Spring-test -->
  <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${org.springframework-version}</version>
  </dependency>
  ```

### root-context.xml 설정 및 dataSource, sqlSessionFactory, sqlSessionTemplate에 대한 이해

  - Namespace Tab을 클릭합니다.

  ![board1](/images/posts/201906/board1.jpg)

  - 사용 가능한 XML 태그의 폭을 넓혀주기 위한 설정. 아래와 동일하게 체크.

  ![board2](/images/posts/201906/board2.jpg)

  다음으로 `dataSource`와 `sqlSessionFactory` 및 `sqlSessionTemplate` 총 3가지를 추가해야 합니다.

  dataSource는 MySQL로 JDBC 드라이버를 찾아서 DB연결을 해주기 위해 필요한 객체입니다.
  dataSource에서는 `연결할 DB의 주소, 계정, 비밀번호`를 적어주고 IP 뒤에 `스키마(DB)`를 적어줍니다.

  마이바티스에서는 `SqlSession`를 생성하기 위해 `SqlSessionFactory`를 사용합니다. 세션을 한번 생성하면 매핑구문을 실행하거나 커밋 또는 롤백을 하기 위해 세션을 사용할수 있습니다. __마지막으로 더 이상 필요하지 않은 상태가 되면 세션을 닫습니다.__ 마이바티스 스프링 연동모듈을 사용하면 SqlSessionFactory를 직접 사용할 필요가 없습니다. 왜냐하면, 스프링 트랜잭션 설정에 따라 자동으로 커밋 혹은 롤백을 수행하고 닫혀지는, 쓰레드에 안전한 SqlSession 개체가 스프링 빈에 주입될 수 있기 때문입니다.

  `SqlSessionTemplate`은 마이바티스 스프링 연동 모듈의 핵심입니다. SqlSessionTemplate은 SqlSession을 구현하고 코드에서 __SqlSession를 대체__ 하는 역할을 합니다. SqlSessionTemplate 은 쓰레드에 안전하고 여러개의 DAO나 매퍼에서 공유할수 있습니다.

  기존에 `JDBC`만을 사용하여 프로그래밍 할때에는 DAO에서 쿼리 구문을 작성하고, 쿼리 구문이 바뀌면 메소드도 여러개 생성하면서 비효율적인 코딩을 했습니다. 또한 항상 `try-catch문`으로 감싸 예외 처리를 해주었는데, `SqlSessionTemplate`을 사용하면 `SQLException`을 SqlSessionTemplate이
  알아서 `RuntimeException`으로 바꿔주면서 `UnchekedException` 상태가되어 예외처리를 해주지 않아도 되서, 스프링에서 보면 DAO나 Service에서
  예외 처리 구문이 없는것을 볼 수 있습니다.

  > UnchekedException : 예외처리를 굳이 안해줘도 상관 없는 예외
  >
  > CheckedException : 예외처리를 꼭 해줘야 하는 예외

  dataSource와 SqlSessionFactory, SqlSessionTemplate을 이해 했으니 root-context.xml에서 설정을 해봅시다.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:jdbc="http://www.springframework.org/schema/jdbc"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xsi:schemaLocation="http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-3.1.xsd
		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd">

	<!-- Root Context: defines shared resources visible to all other web components -->

	<!-- MySQL dataSource -->
	<!-- JDBC 드라이버를 사용하여 MySQL에 접속할 수 있게 해줌  -->
  <bean id="dataSource"
      class="org.springframework.jdbc.datasource.DriverManagerDataSource">
      <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
      <property name="url"
          value="jdbc:mysql://192.168.10.20:3307/test?useSSL=false&amp;serverTimezone=UTC">
      </property>
      <property name="username" value="mayeye"></property>
      <property name="password" value="apdldkdl@@"></property>
  </bean>        

	<!-- mybatis SqlSessionFactoryBean -->
	<!-- SqlSessionFactory는 mybatis 사용시 꼭 필요한 객체(bean) -->
  <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource" />
      <property name="configLocation" value="classpath:mapper/config/mybatis-config.xml" />
      <property name="mapperLocations">
      	<list>
      		<value>classpath:mapper/boardMapper.xml</value>
      	</list>
      </property>
  </bean>

  <!-- sqlSessionTemplate : MyBatis 스프링 연동 모듈의 핵심 -->
  <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg ref="sqlSessionFactory" />
  </bean>

  </beans>
  ```

  root-context.xml 설정을 마쳤으면 src/text/com/xxx/xxx로 가서 클래스 2개를 만들어서 DB연동이 되는지 테스트를 해야 합니다.

  - MySQLConnectionTest.java

  ```java
  package com.mayeye.board;

  import java.sql.Connection;

  import javax.inject.Inject;
  import javax.sql.DataSource;

  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.test.context.ContextConfiguration;
  import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = { "file:src/main/webapp/WEB-INF/spring/**/root-context.xml" })
  public class MySQLConnectionTest {

    @Inject
    private DataSource ds;

    @Test
    public void testConnection() throws Exception {

        try (Connection con = ds.getConnection()) {

            System.out.println("\n >>>>>>>>>> Connection 출력 : " + con + "\n");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }    
  }
  ```

  - MyBatisTest.java

  ```java
  package com.mayeye.board;

  import javax.inject.Inject;

  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.test.context.ContextConfiguration;
  import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations={"file:src/main/webapp/WEB-INF/spring/**/root-context.xml"})
  public class MybatisTest {
    @Inject
    private SqlSessionFactory sqlFactory;

    @Test
    public void testFactory(){
        System.out.println("\n >>>>>>>>>> sqlFactory 출력 : "+sqlFactory);
    }

    @Test
    public void testSession() throws Exception{

        try(SqlSession session = sqlFactory.openSession()){

            System.out.println(" >>>>>>>>>> session 출력 : "+session+"\n");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }  
  }
  ```

  테스트를 위해 MySQLConnectionTest과 MyBatisTest를 각각 마우스 우클릭합니다. 그리고 `[Run As] - [Junit Test]` 를 선택합니다.

  소스 코드가 정상적으로 동작하면 MySQLConnectionTest 클래스는 Connection을, MyBatisTest 클래스는 SqlSessionFactory와 SqlSession의 주소를 출력할 것입니다.

  - MySQLConnectionTest 실행 결과

  ![board3](/images/posts/201906/board3.jpg)

  - MyBatisTest 실행 결과

  ![board4](/images/posts/201906/board4.jpg)

## mybatis-config.xml 설정

  MyBatis 작동 규칙을 정의하는 mybatis-cofing.xml을 설정해야 합니다. 다음과 같습니다.
  mybatis-config.xml은 src/main/resources 밑에 mapper 패키지를 만들고 mapper 패키지 안에
  cofing 패키지를 만들어서 그 안에 넣으면 됩니다.

  > mybatis-config.xml 경로 : src/main/resources/mapper/config

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  	<!-- MyBatis 작동 규칙 정의 -->
  	<settings>
  		<setting name="cacheEnabled" value="false" />
  		<!-- <setting name="useGenereateKeys" value="false" /> -->
  		<setting name="mapUnderscoreToCamelCase" value="true" />
  	</settings>

  	<typeAliases>
  		<typeAlias alias="boardDto" type="com.mayeye.board.dto.BoardDTO"/>
  		<typeAlias alias="usersDto" type="com.mayeye.board.dto.UsersDTO"/>
  	</typeAliases>
  </configuration>
  ```
  mybatis-config.xml을 설정하고 서버 실행시 generateKeys라는 오류가 나면 `<setting name="useGenereateKeys" value="false" />` 태그를 없애야 합니다. mybatis 버전에 따라서 위 태그를 사용할 수도 있고, 사용 못할 수도 있습니다.
