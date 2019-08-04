---
title: "IntelliJ에서 MySQL 연동하기"
layout: post
category: SpringBoot
tags: [SpringBoot]
excerpt: "IntelliJ에서 MySQL 연동하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > IntelliJ에서 MySQL 연동하는 방법에 대해서 배워 봅시다.

## IntelliJ에서 MySQL 연동하기

  > [DBeaver과 MySQL 연동하는 방법](https://baekjungho.github.io/database-dbeaverspring/)

### Dependency 설정

  - `pom.xml`

  ```xml
  <!-- JSP -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <version>9.0.8</version>
</dependency>

<!-- JDBC -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>

<!-- MyBatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.2</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>

<!-- MySQL -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>provided</scope>
</dependency>
  ```

  Dependency를 설정할 때 중요한 점은 `mysql-connector-java`의 Version `8.0.15` 부분을 실제 자신이 가지고 있는 JDBC 드라이버 버전과 맞춰야 합니다.

  pom.xml에 의존성을 추가하고나면 꼭 `Maven-reImport`를 클릭하여 update를 해줍니다.

### application.properties 설정

  ```
spring.view.prefix="/WEB-INF/jsp/"
spring.view.suffix=".jsp"

spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/BAEK_TECH_BLOG?serverTimezone=UTC&useSSL=false
spring.datasource.username=root
spring.datasource.password= root

server.servlet.context-path="/TECH"
  ```

  여기서 driverClassName인 `com.mysql.cj.jdbc.Driver`를 잘 기억해야 합니다.

### MyBatis 설정파일 생성(MybatisConfig.java)

  ```java
package com.technology.baek.blog.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan(basePackages = "com.xxx.*.*.*.repository")
public class MybatisConfig {

}
  ```

  `@Configuration`어노테이션을 통해 해당 클래스가 스프링 설정 파일임을 명시 해줍니다.

  `@MapperScan`으로는 해당 패키지 경로에 있는 자바 파일을 매핑하는 경로로 지정하는 역할을 합니다.

  여기까지 설정을 하셨으면, MySQL에 연결하기위한 준비는 마친것입니다. 아래 과정을 통해 실제로 IntelliJ에서 MySQL을 연결하는 방법을 배워 보겠습니다.

### IntelliJ. DB연결 설정

  - `오른쪽 벽면에 있는 DataBase 클릭`
    - ![s1](/images/posts/201908/s1.jpg)

  - `왼쪽 메뉴의 MySQL 클릭 후 MySQL Connector 다운로드`
    - 다운로드한 버전을 pom.xml의 Connector 의존성 버전과 맞춰야 한다.
    - `ClassName`은 application.properties에서 설정한 driverClassName과 일치해야 한다.
      - `com.mysql.cj.jdbc.Driver`
    - ![s2](/images/posts/201908/s2.jpg)

  - `왼쪽 메뉴의 UserDriver 클릭 후 다운로드 받은 MySQL Connector 설정`
    - `ClassName`은 application.properties에서 설정한 driverClassName과 일치해야 한다.
      - `com.mysql.cj.jdbc.Driver`
    - ![s4](/images/posts/201908/s4.jpg)

  - `왼쪽 메뉴의 Vertica 클릭 후 에러가 없는지 확인`
    - ![s5](/images/posts/201908/s5.jpg)

  - `DB명을 클릭하고 접속하기 위한 정보를 입력`
    - Name : DB명
    - user : userName
    - password : password
    - URL : application.properties에서 설정한 spring.datasource.url의 경로
      - `jdbc:mysql://localhost:3306/BAEK_TECH_BLOG?serverTimezone=UTC&useSSL=false`
    - ![s6](/images/posts/201908/s6.jpg)

  - `연결 성공 화면`
    - ![s7](/images/posts/201908/s7.jpg)

### IntelliJ에서 DB 쿼리 사용 방법

  - `연필 모양 클릭`
    - ![s8](/images/posts/201908/s8.jpg)

  - `Ctrl + Enter로 해당 블럭의 쿼리 실행`
    - ![s9](/images/posts/201908/s9.jpg)
