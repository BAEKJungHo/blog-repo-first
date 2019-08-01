---
title: "메이븐(Maven)"
layout: post
category: Spring
tags: [Spring]
excerpt: "메이븐(Maven)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 메이븐(Maven)에 대해서 배워 봅시다.

## Maven을 이용한 프로젝트 생성

  Maven을 이용한 프로젝트 생성방법은 다음 링크를 통해 확인하시면 됩니다.

  > [https://baekjungho.github.io/spring-mavenproject/](https://baekjungho.github.io/spring-mavenproject/)

## 메이븐(Maven) 이란?

  Maven은 여러 라이브러리에 관한 버전 관리를 해주는 `Build Tool`입니다. Maven에서는 Maven을
  `소프트웨어 프로젝트 관리 및 도구`라고 설명합니다.

  > Build란 ?
  >
  > 개발한 것을 WebServer에 배포하고 테스트하기 위해서는 필수로 거쳐야하는 관문이며,
  Build라는 단어는 Compile, Testing, Inspection, deploy 과정등이 포함 될 수 있습니다.
  즉, 소프트웨어가 하나의 단위로써 동작하는지 확인하는 과정이며, 소프트웨어를 생성하고 테스트하고 검사하여
  배포하기 위해 수행하는 행위의 집합을 의미합니다.

  Maven `pom.xml`로 만드는데 이 것을 통해서 jar 파일의 의존성관리, 빌드, 배포, 문서생성, Release 등의 과정을 수행할 수 있습니다.
  pom.xml은 자기가 직접 작성해도 되지만, 주로 이미 작성되어있는 것을 재사용하는 편이 낫습니다.

  기존에 `.jar` 라이브러리 파일을 사용하기 위해서는 `Dynamic Web Project`로 프로젝트를 진행할 때 `lib` 폴더 안에 넣어줬는데
  Maven을 사용하면 `<dependencies> 태그안에 <dependency> 태그`를 사용하여 관련 라이브러리를 적어주기만 하면 자동으로 설치가 되면서
  관리가 됩니다.

  `%HOME%/.m2/repository` 폴더는 메이븐이 라이브러리와 플러그인을 다운로드해서 저장하는 폴더입니다.

## LifeCycle

  메이븐의 라이프 사이클은 다음과 같습니다.

  ![maven1](/images/posts/201906/maven1.jpg)

  > 출처 : [https://www.slideshare.net/ssuser5445b7/ss-56566336?qid=927855f5-7c8a-4f88-a834-d31292324fd2&v=&b=&from_search=4](https://www.slideshare.net/ssuser5445b7/ss-56566336?qid=927855f5-7c8a-4f88-a834-d31292324fd2&v=&b=&from_search=4)

  메이븐의 모든 기능은 `플러그인(plugin)`을 기반으로 동작합니다. 플러그인에서 실행할 수 있는 각각의 작업을 `골(goal)`이라하고 하나의 `페이즈(phase)`는 하나의 골과 연결되며, 하나의 플러그인에는 여러 개의 골이 있을 수 있습니다.

  빌드 단계(Build Phase)에는 그 단계가 실행하는 골이 정해져있다. 예를 들면, compile 빌드 단계는 compiler 플러그인의 compile 골을 실행하고, package 빌드 단계는 jar 플러그인의 jar 골을 실행한다.

## pom.xml

  ![s8](/images/posts/201906/s8.jpg)

  pom.xml에 의해서 필요한 라이브러리만 다운로드 해서 사용할 수 있습니다.

  에러가 생길경우 pom.xml 수정한 사항이 프로젝트에 반영이 제대로 안 된 것이니 프로젝트에서 마우스 오른쪽 클릭 후 나오는 메뉴에서 `Maven > Update Dependencies 와 Update Project Configuration` 을 실행하여 반영시켜야 합니다.

### EXAMPLE

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.bs</groupId>
	<artifactId>lec17</artifactId>
	<name>lec17Pjt001</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>4.1.0.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				 </exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

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

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>        
	</dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-eclipse-plugin</artifactId>
                <version>2.9</version>
                <configuration>
                    <additionalProjectnatures>
                        <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
                    </additionalProjectnatures>
                    <additionalBuildcommands>
                        <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
                    </additionalBuildcommands>
                    <downloadSources>true</downloadSources>
                    <downloadJavadocs>true</downloadJavadocs>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <compilerArgument>-Xlint:all</compilerArgument>
                    <showWarnings>true</showWarnings>
                    <showDeprecation>true</showDeprecation>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>org.test.int1.Main</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
  </project>
  ```

## 참조

  > [http://blog.naver.com/PostView.nhn?blogId=angkeloss&logNo=70166883984](http://blog.naver.com/PostView.nhn?blogId=angkeloss&logNo=70166883984)
  >
  > [https://jeong-pro.tistory.com/168](https://jeong-pro.tistory.com/168)
  >
  > [https://kjunine.tistory.com/entry/getting-started-with-maven-2](https://kjunine.tistory.com/entry/getting-started-with-maven-2)
