---
title: "JDK 및 Eclipse 설치"
layout: post
category: Java
tags: [Java]
excerpt: "JDK와 Eclipse 설치 및 설정에 대해 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

JDK와 Eclipse 설치 및 설정에 대해 배워봅시다.

## JDK 설치방법

- [JDK 설치](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

  오라클 홈페이지에 가서 JDK를 설치합니다. 빨간색 네모박스를 클릭하고 자신의 운영체제에 맞게 설치합니다.

  별다른 설정은 필요 없고 계속 `NEXT`를 클릭하여 설치를 해줍시다.

  ![jdk1](/images/posts/201903/jdk1.jpg)

- 환경변수설정

  환경변수를 설정하기 위해 `윈도우 + R`을 누르고 `sysdm.cpl`을 입력다.

  아니면 내컴퓨터->오른쪽마우스->속성->고급->환경변수로 들어가셔도 됩니다.

  여기서 중요한 점은 `사용자 변수(User Variable)와 시스템 변수(System Variable)`두 가지가 있습니다.

  ![jdk2](/images/posts/201903/jdk2.jpg)

  시스템 변수를 설정해보고 cmd에서 `java -version과 java, javac`를 입력해서 잘 동작하는지 확인합시다.
  만약 javac에서 동작이 막힌다면 2번째 방법을 추가로 수행해주세요.


  - 시스템 변수(System variable)

  JAVA_HOME등록을 위하여
  새로 만들기를 클릭하고 경로에 자신이 설치한 jdk\bin까지 들어간 주소를 복사하여 적어줍시다.

  > ex) C:\Program Files\Java\jdk1.8.0_201\bin

  그리고 시스템 변수에 있는 Path를 클릭하여 편집을 누르고 경로 맨마지막에 다음과 같이 적어줍시다.

  > ;JAVA_HOME%\bin;

  그리고 cmd로 `java -version과 java, javac`를 입력해서 잘 동작하는지 확인합시다.

  만약 안된다면 사용자 변수 설정으로 넘어가주세요.


  - 사용자 변수(User Variable)
  사용자 변수에 있는 path를 누르고 경로 맨마지막에 다음과 같이 적어 줍시다.

  > ;C:\Program Files\Java\jdk1.8.0_201\bin

  앞에 세미콜론(;)을 꼭 붙이고 입력해주세요.

## Eclipse 설치방법

  - [Eclipse 설치](https://www.eclipse.org/)

  이클립스 공식 홈페이지에가서 다운로드를 눌러줍시다.

  ![eclipse1](/images/posts/201903/eclipse1.jpg)

  아래로 스크롤하다보면 다음과같이 빨간색 네모안의 Downloads Packages를 눌러줍시다.

  ![eclipse2](/images/posts/201903/eclipse2.jpg)

  또 다시 스크롤하다가 오른쪽 네이게이션 부분을 보면 아래와 같이 여러 버전을 받을 수 있는 링크가 있는데 저는 Oxygen(4.7)을 받았습니다.

  ![eclipse3](/images/posts/201903/eclipse3.jpg)

  설치하고싶은 버전을 선택하고 맨 위에 있는 `Eclipse IDE for Java EE Developers` 버전을 각자 운영체제에 맞게 받아줍시다.

  빨간색 네모 박스에 있는 `Select Another Mirror`를 누르고 곧바로 `Show All`을 눌러줍니다.

  ![eclipse4](/images/posts/201903/eclipse4.jpg)

  ![eclipse5](/images/posts/201903/eclipse5.jpg)

  Korea에서 카카오, 카이스트 등 여러가지가 있는데 한국서버로 받아야 다운로드가 빠릅니다.

  다운을 받고 알집을 푸시면 설치가 끝납니다.

## Eclipse 설정

  자바만 사용하기 위해서는 오른쪽 상단의 `Open Perspective`를 클릭하여 자바를 선택해줍니다.

  ![eclipse6](/images/posts/201903/eclipse6.jpg)

  다음으로 폰트 설정을 위해 상단 메뉴바에 있는 `Window->Preferences`를 클릭합니다. 그리고 다음 그림과 같이 클릭을 하고난 후에
  폰트들을 클릭하여 자신이 원하는 크기와 모양으로 바꿔줍니다.

  ![eclipse7](/images/posts/201903/eclipse7.jpg)

  > 저는 Naver D2 coding ligature를 설치하여 적용했습니다.

  - 한글 적용 방법

  다음으로 한글 설정을 위해 상단 메뉴바에 있는 `Window->Preferences`를 클릭합니다.

  Workspace를 누르고 Text file encoding을 Other로 설정하고 `UTF-8`로 바꿉니다.

  ![eclipse8](/images/posts/201903/eclipse8.jpg)
