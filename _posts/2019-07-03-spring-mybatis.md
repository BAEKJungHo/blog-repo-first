---
title: "Spring으로 DB접속 방법"
layout: post
category: Spring
tags: [Spring, DataBase]
excerpt: "Spring에서 사용하는 Mybatis에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring에서 사용하는 Mybatis에 대해서 배워 봅시다.

## 과정

  ![h1](/images/posts/201906/h1.jpg)

  ![h2](/images/posts/201906/h2.jpg)

## Mybatis

  Mybatis도 메이븐 라이브러리에 추가해야 하는 일종의 `라이브러리`입니다.

  Mybatis를 사용하면 기존에 자바코드로 DB쿼리를 작성하고 날리고 하던 것을 `XML` 코드로 간단하게
  사용할 수 있어서 코드의 양이 대폭 줄어듭니다.

## 스프링으로 DB 접속하는 방법

  스프링으로 DB에 접속하는 방법에는 JDBC만을 사용하는 방법과, dataSource를 정의하는방법과 Mybatis를 이용한
  DB접속방법 3가지가 있습니다.

### JDBC만을 사용

  - pom.xml

  ```xml
  <!-- MySQL connector/j -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.39</version>
  </dependency>
  ```

  위 코드를 메이븐에 추가하여 JDBC 드라이버를 설치합니다.

  - DB 커넥션 만들기

  ```java
  public class MySQLConnectionTest {

    private static final String DRIVER = "com.mysql.jdbc.Driver";
    private static final String URL = "jdbc:mysql://192.110.0.1:3307/스키마 이름";
    private static final String USER = "repacat";
    private static final String PW = "repacat";

    @Test
    public void testConnection() throws Exception {
        Class.forName(DRIVER);

        try(Connection conn = DriverManager.getConnection(URL, USER, PW)) {
            System.out.println(conn);
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
  }
  ```

### Spring 에서 dataSource 를 정의하고 이를 통한 접속 테스트

  - pom.xml

  ```xml
  <!-- spring-jdbc : jdbc 프로그래밍하면서 개발하기 지루한 부분을 spring이 대신 해줌 -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
  </dependency>
  ```

  - root-context.xml

  ```xml
  <!-- dataSource 설정, spring-jdbc 모듈 사용, spring 에서 jdbc 를 통해 mysql 에 접속할 수 있게 함 -->
  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://192.110.0.1:3306/스키마 이름"></property>
    <property name="username" value="계정명"></property>
    <property name="password" value="패스워드"></property>
  </bean>
  ```

  root-context.xml에 `dataSource` 빈을 선언하여 DI를 받을 수 있도록 한다.

  dataSource bean 은 spring-jdbc 모듈에 있는 클래스(org.springframework.jdbc.datasource.DriverManagerDataSource)를 이용하여 JDBC 드라이버를 통해 MySQL 서버에 접속할 수 있게한다.

  DataSource 에는 여러 개의 property가 있는데 여기서는 `연결할 DB의 주소, 계정, 비밀번호` 이렇게 3개의 property만 설정하면 된다. 연결 주소에는 로컬일 경우  127.0.0.1을 입력하고 뒤에는 3306을 입력한다. 3306은 MySQL 설치시 지정한 포트인데, 별도로 변경하지 않았다면 3306이 기본 포트이다.

  그 뒤에는 `스키마 이름(DB명)`을 입력한다.

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations={"file:src/main/webapp/WEB-INF/spring/**/*.xml"})
  public class DataSourceTest {

    @Inject
    private DataSource ds;

    @Test
    public void testConnection() throws Exception {
        try(Connection conn = ds.getConnection()) {
            System.out.println(conn);
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
  }
  ```

  dataSourrce 를 DI 받고 이를 통해 커넥션을 만들 수 있다.

### Spring 에서 MyBatis 를 설정하고 이를 이용한 접속 테스트

  ```xml
  <!-- Mybatis -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.2.8</version>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.2.2</version>
  </dependency>
  ```


## 참조

  > MySQL, Mybatis Setting : [https://all-record.tistory.com/175](https://all-record.tistory.com/175)
  >
  > DataSource, sqlSessionFactory : [https://repacat.tistory.com/23](https://repacat.tistory.com/23)

## 참고 사이트

  > https://kookyungmin.github.io/server/2018/08/13/spring_05/
