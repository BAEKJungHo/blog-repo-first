---
title: "Spring FrameWork Install"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring FrameWork의 설치 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Spring FrameWork Install

### 스프링 개발환경 설정

  - Java와 JSP 환경에서 개발자가 좀 더 편안하게 프로젝트를 진행할 수 있도록 만들어 놓은 툴
    - 스프링 개발 툴
      - 기존의 이클립스에서 확장 프로그램을 설치
    - InteliJ
      - 안드로이드 웹 개발에 주로 사용함
    - STS 전용 툴(Spring Tool Suite)
      - 주소 : https://spring.io

## STS로 스프링 설정 Ver1.

  > Spring Tools : [https://spring.io/tools](https://spring.io/tools)

  - 위 홈페이지로 접속
  - Spring Tools 4 for Eclipse에서 원하는 운영체제에 맞게 다운로드 클릭
    - ![s1](/images/posts/201905/s1.jpg)

  - 다운로드한 알집을 풀어 줍니다.
    - sts-4.2.2.RELEASE 폴더를 복사해서 `D: 혹은 F:드라이브`로 옮겨줍니다.
    - 그리고 그 안에 있는 `SpringToolsSuite4.exe`를 실행합니다.
    - 만약 버전이 안맞아서 오류나면 `eclipsec.exe`를 실행하면 됩니다.
    - ![s2](/images/posts/201905/s2.jpg)

  - STS4에서는 Spring Boot만 지원 하기 때문에 Spring Legacy project를 지원하지 않기 때문에
  해당 기능을 추가해 주어야 합니다.
    - HELP 클릭 후 Eclipse MarketPlace... 클릭
    - ![s3](/images/posts/201905/s3.jpg)
    - Spring Tools 검색 후 `Spring Tools 3 Add-On` Install 클릭
    - ![s4](/images/posts/201905/s4.jpg)
    - Confirm 클릭
    - ![s5](/images/posts/201905/s5.jpg)
    - Accept 클릭하고 Finish
    - ![s6](/images/posts/201905/s6.jpg)

  - Worksapce - Preferences 클릭
    - General - Workspace에서 UTF-8로 변경
    - ![s7](/images/posts/201905/s7.jpg)
    - WEB - HTML, CSS, JSP 모두 UTF-8로 변경
      - 변경 방법 : https://baekjungho.github.io/apache-tomcat/

  - Help - Install New SoftWare... 클릭
    - Add 클릭 후 - 2019 - 03 ... 선택 후 - Java Web 검색
    - 맨위에 있는 Eclipse Java Web Developer Tools 설치
    - ![s8](/images/posts/201905/s8.jpg)

  - http://mannaedu.com/bbs/board.php?bo_table=pds&wr_id=74 접속 되는지 확인 후
    - 알집 파일 다운로드

  - File - Import - General - Existing Projects into Workspace
    - 알집을 푼 폴더 Import 하기
    - ![s9](/images/posts/201905/s9.jpg)

  > Sample 프로젝트안에 있는 각종 xml파일들 복사 해서 자신의 프로젝트에 붙여 넣고
  Servlet-Context.xml에서 다음 부분을 수정해야 합니다.

  - ![s24](/images/posts/201905/s24.jpg)

  - Maven 라이브러리 받는 방법
    - 사이트 : http://mvnrepository.com
    - 원하는 라이브러리 검색(ex dbcp)
    - ![s10](/images/posts/201905/s10.jpg)
    - 원하는 버전 클릭
    - ![s11](/images/posts/201905/s11.jpg)
    - ![s12](/images/posts/201905/s12.jpg)

  - 프로젝트 생성
    - New - Other - spring 검색
      - Spring Legacy Project 클릭
      - ![s13](/images/posts/201905/s13.jpg)
      - Spring MVC 프로젝트 클릭
      - ![s14](/images/posts/201905/s14.jpg)
      - 다음과 같은 형식으로 패키지 이름 지정
      - ![s15](/images/posts/201905/s15.jpg)

  - 자바 1.8 Version으로 맞추기

  > 자바 Version을 맞출 때 중요한 점은 4가지를 동시에 확인해야 합니다.
  >
  > pom.xml, Properties의 Proejct Facets, Java Compiler, Java Build Path

  - pom.xml에서 java version을 1.8로 수정
    - ![s16](/images/posts/201905/s16.jpg)

  - 프로젝트명 우클릭 - Properties 클릭 - Java Compiler 클릭하여 1.8로 수정
    - ![s17](/images/posts/201905/s17.jpg)

  - 프로젝트명 우클릭 - Properties 클릭 - Proejct Facets 클릭하여 1.8로 수정
    - ![s18](/images/posts/201905/s18.jpg)

  - 프로젝트명 우클릭 - Properties 클릭 - Java Build Path의 Library에서 JavaSE-1.8 확인
    - ![s19](/images/posts/201905/s19.jpg)

  - C:\Users\714-9\.m2 여기에 Maven으로 받은 파일들이 저장됩니다.
    - C:드라이브 - 사용자 - 사용자이름 - .m2

  > 만약에 라이브러리 충돌이 일어나서 잘 안된다면 C:\Users\714-9\.m2 이 안에 있는
  repository를 삭제하고 다시 실행시키면 됩니다.

## STS로 스프링 설정 Ver2.

  ![st1](/images/posts/201906/st1.jpg)

  먼저 Help - Eclipse MarketPlace에서 STS를 검색합니다. 그리고 Install을 눌러 설치합니다.

  ![st2](/images/posts/201906/st2.jpg)

  그리고 Confirm을 누르고 Accept(라이센스 동의)를 클릭하고 Finish를 클릭하면 됩니다.

### Spring MVC Project 생성

  Spring MVC Project 생성 방법은 다음을 따르면 됩니다.

  ![st3](/images/posts/201906/st3.jpg)

  ![st4](/images/posts/201906/st4.jpg)

  ![st5](/images/posts/201906/st5.jpg)

## 아파치 설정

  > 아파치 : [http://tomcat.apache.org/](http://tomcat.apache.org/)

  - Tomcat 9 Download
    - Core : 64-bit Windows zip (pgp, sha512)
    - ![s20](/images/posts/201905/s20.jpg)

  - 받은 알집을 풀어줍니다.
  - Windows - Preferences - Server - Runtime Environment - Add 클릭
    - 폴더 Import 시키기
    - ![s21](/images/posts/201905/s21.jpg)

  - Maven Dependencies 있는지 확인하기 - 프로젝트 우클릭 - Properties - Deployment Assembly
    - ![s22](/images/posts/201905/s22.jpg)

  - JSP 파일 실행하여 주소 나오는지 확인
    - http://localhost:8080/프로젝트명/
