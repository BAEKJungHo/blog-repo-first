---
title: "Spring Petclinic"
layout: post
category: Spring
tags: [Spring]
excerpt: "인프런 백기선님의 스프링 프레임 워크 입문을 듣고 정리한 내용 입니다."
author: BAEKJungHo
---

* content
{:toc}

## Spring Petclinic

  > SAMPLE CODE
  >
  > Spring Petclinic : [https://projects.spring.io/spring-petclinic/](https://projects.spring.io/spring-petclinic/)

### Settings 1.

  - JDK 1.8 Version
    - [JDK 설치 방법](https://baekjungho.github.io/java-eclipse/)
  - IDE : IntelliJ(인텔리J, 커뮤니티 버전도 상관 없음)

  > 주의할 점 : wro4j 메이븐 플러그인이 Java 9 이상을 지원하지 않습니다.

  - 실행방법
    - mvn spring-boot:run
    - IDE에서 메인 어플리케이션 실행

  > 소스코드 : [https://github.com/spring-projects/spring-petclinic](https://github.com/spring-projects/spring-petclinic)

  해당 소스코드가 있는 깃허브 홈페이지로 들어가면 세팅 방법이 나와있습니다. 다음과 같습니다.

  ```
  git clone https://github.com/spring-projects/spring-petclinic.git
  cd spring-petclinic
  ./mvnw package
  java -jar target/*.jar

  // 혹은 ./mvnw spring-boot:run 사용 (메이븐 스프링 부트 플러그인 run 사용)
  ```

  - IntelliJ 실행 - Open 클릭 - git clone으로 받은 프로젝트 경로 설정
    - 경로 : ../../spring-petclinic

### Settings 2.

  > JDK Version이 1.8인지 확인하고, 아닐 경우 `JDK 1.8 Version` 으로 수정

  - File - Project Structure 클릭
    - 단축키 : `Ctrl + Alt + Shift + S`

  ![i7](/images/posts/201906/i7.jpg)

  위 화면에서 빨간색 네모 박스에 있는 JDK버전을 수정하고 파란색 네모 박스에 있는 부분은
  프로그래밍 문법을 나타내는 곳입니다.

  - Project Structure - Modules - Dependencies 설정

  ![pet1](/images/posts/201906/pet1.jpg)

  빨간 네모 박스에 있는 부분이 1.8 Version으로 되어 있어야 합니다.

### 실행방법(spring:boot-run)

  ![pet2](/images/posts/201906/pet2.jpg)

  IntelliJ 오른쪽 벽면에 Maven을 클릭하고 `spring:boot-run`을 클릭하여 실행, 실행이 되면
  주소창에 `localhost:8080`을 입력하여 화면이 나오는지 확인합니다.

### 실행방법(IDE 메인 어플리케이션)

  - spring-petclinic 프로젝트
    - src
      - main
        - java
          - org.springframeworksamples.petclinic
            - __PetClinicApplication__ : Main Application

  ```java
  /*
 * Copyright 2012-2019 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

  package org.springframework.samples.petclinic;

  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;

  /**
   * PetClinic Spring Boot Application.
   *
   * @author Dave Syer
   *
   */
  @SpringBootApplication
  public class PetClinicApplication {

      public static void main(String[] args) {
          SpringApplication.run(PetClinicApplication.class, args);
      }

  }
  ```

  위에서 `@SpringBootApplication` 어노테이션을 사용했는데, 이것은 스프링 부트 기반의
  프로젝트에서 메인이 되는 클래스를 나타내며, 실행가능한 어플리케이션을 뜻합니다.

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
