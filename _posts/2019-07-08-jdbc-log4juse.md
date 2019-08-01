---
title: "스프링 log4j 설정 방법"
layout: post
category: Spring
tags: [Spring, MySQL, MyBatis]
excerpt: "스프링에서 log4j를 설정하고 사용하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링에서 log4j를 설정하고 사용하는 방법에 대해서 배워 봅시다.

## log4j Level

  ![a1](/images/posts/201907/a1.jpg)

## log4j Pattern Option

  ![a5](/images/posts/201907/a5.jpg)

## log4j 주요 클래스

  ![a3](/images/posts/201907/a3.jpg)

## Appender

  ![a4](/images/posts/201907/a4.jpg)

## Dependency 설정

  Dependency를 설정하기 위해 pom.xml에 아래 코드를 추가합니다.

  ```xml
  <!-- Logging -->
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>${org.slf4j-version}</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>jcl-over-slf4j</artifactId>
  <version>${org.slf4j-version}</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${org.slf4j-version}</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.15</version>
  <exclusions>
    <exclusion>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
    </exclusion>
    <exclusion>
      <groupId>javax.jms</groupId>
      <artifactId>jms</artifactId>
    </exclusion>
    <exclusion>
      <groupId>com.sun.jdmk</groupId>
      <artifactId>jmxtools</artifactId>
    </exclusion>
    <exclusion>
      <groupId>com.sun.jmx</groupId>
      <artifactId>jmxri</artifactId>
    </exclusion>
  </exclusions>
  <scope>runtime</scope>
</dependency>
  ```

## log4j.xml 설정

  src/main/resources안에 log4j.xml 설정파일이 있습니다. 아래 코드로 수정합니다.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//APACHE//DTD LOG4J 1.2//EN" "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">

	<!-- Appenders -->
	<appender name="console" class="org.apache.log4j.ConsoleAppender">
		<param name="Target" value="System.out" />
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern" value="[%5p] %d{hh\:mm s} (%F\:%L) %c{1}.%M \: %m%n" />
		</layout>
	</appender>

	<!-- Application Loggers -->
	<logger name="com.mayeye.board">
		<level value="info" />
	</logger>

	<!-- 3rdparty Loggers -->
	<logger name="org.springframework.core">
		<level value="info" />
	</logger>

	<logger name="org.springframework.beans">
		<level value="info" />
	</logger>

	<logger name="org.springframework.context">
		<level value="info" />
	</logger>

	<logger name="org.springframework.web">
		<level value="info" />
	</logger>

	<!-- Root Logger -->
	<root>
		<priority value="warn" />
		<appender-ref ref="console" />
	</root>

</log4j:configuration>
  ```

  만약 debug를 사용하고 싶으면, info 부분을 debug로 수정하면 됩니다.

## Controller에서 LOG 사용

  ```java
  private static final Logger Logger = LoggerFactory.getLogger(XXX.class);
  ```

  위 코드를 삽입하여 `Logger.info()` 방식으로 사용하면 됩니다. 괄호 안은 문자열이 들어가야 합니다.
  XXX.class는 해당 클래스 명을 적어주면 됩니다.
