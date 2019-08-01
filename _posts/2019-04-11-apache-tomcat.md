---
title: "Apache & Tomcat Install"
layout: post
category: JSP
tags: [Java, Tomcat, JSP]
excerpt: "Apache와 Tomcat설치 방법과 JSP 프로그래밍을 위한 이클립스 세팅을 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

Apache와 Tomcat설치 방법과 JSP 프로그래밍을 위한 이클립스 세팅을 배워봅시다.

## Apache & Tomcat

  저희가 사용하는 Web Page는 `Apache와 Tomcat`으로 이루어져 있습니다.

  Apache란 Apache SoftWare 재단의 Open Source Project입니다.  
  일명 웹서버로 불리며, `클라이언트 요청이 왔을때만 응답하는 정적 웹페이지`에 사용된다.

  > Apache는 80번 포트로 클라이언트 요청

  Tomcat이란 Apache SoftWare 재단에서 만든 Java Servlet과 JSP 구현을 위한 Open Source

  > Tomcat은 WAS(Web Application Server)에 속하며 동적인 데이터를 처리합니다.
  >
  > DB연결,데이터 조작, 다른 응용프로그램과 상호 작용이 가능하다. 톰캣은 8080포트로 처리한다.

  Web Server와 Web Container의 결합으로, 다양한 기능을 Container에 구현하여 다양한 역할을 수행할 수 있는 서버를 말합니다.
  Web Container란 클라이언트의 요청이 있을 때, 내부의 프로그램을 통해 결과를 만들어내고 이것을 다시 클라이언트에게 전달합니다.

  __Web Server는 정적인 데이터를 처리하는 서버__ 이다.

  이미지나 단순html파일과 같은 리소스를 제공하는 서버는 웹서버를 통하면 WAS 보다 빠르고 안정적이다.

  __WAS는 동적인 데이터를 처리하는 서버__ 이다.

  DB와 연결되어 데이터를 주고받거나 프로그램으로 데이터 조작이 필요한 경우에는 WAS를 활용해야한다.

  __즉, Web Server(Apache) WAS(Tomcat)__

  두 서버의 목적의 차이 때문에 두 개의 서버를 연동해서 사용하면 더욱 효과적인 서비스를 제공할 수 있습니다.

  사용자의 요청은  http 웹 서버를 동해 받고 내부 프로그램은 was를 통해 처리하는 식으로 한다면, 정적인 데이터와 동적인 데이터를 효과적으로 처리가 가능할 것입니다.

  > Reference : [itnewvom](https://itnewvom.tistory.com/37)

## Apache & Tomcat 설치방법 Ver1.

  - [Tomcat Apache org](https://tomcat.apache.org)

    아파치 톰캣 설치 홈페이지에 접속 저는 Tomcat 8 version을 설치 할 것입니다.
    왼쪽 네비게이션바에 있는 Download에서 자신에게 알맞은 버전을 선택해서 설치 해줍시다.

    1. 32-bit/64-bit Windows Service Installer 클릭하여 다운로드(8.5.39 ver)
    2. 별다른 설정할 필요 없이 쭉 Next클릭하여 설치 완료하기
    3. 설치된 Apache 실행 후 주소창에 `localhost:8080`입력하여 Apache가 나오는지 확인

  - __Eclipse 설정__

    - 오른쪽 상단에 있는 Perspective에서 Java EE 클릭

      ![tomcat1](/images/posts/201904/tomcat1.jpg)

    - File - New - Dynamic Web Project 클릭

    - 프로젝트 이름 적고, 폴더 경로를 깃허브(Github)에 올릴 수 있게 .git이 있는 폴더경로로 변경

      ![tomcat2](/images/posts/201904/tomcat2.jpg)

    - 콘솔창이 있는 하단에 Servers클릭

    - No servers are available. Click this link to create a new server.. 클릭

      ![tomcat3](/images/posts/201904/tomcat3.jpg)

    - Apache클릭 - 자신이 설치한 버전 클릭 - Tomcat v8.5 Server

      ![tomcat4](/images/posts/201904/tomcat4.jpg)

    - Browse클릭 후 자신이 아파치 설치한 경로로 설정 후 밑의 JRE클릭하여 JRE 버전 선택

      ![tomcat5](/images/posts/201904/tomcat5.jpg)

    - 생성한 프로젝트를 클릭하고 Add로 오른쪽으로 넘기고 Finish

      ![tomcat6](/images/posts/201904/tomcat6.jpg)

    - cmd를 열고 자신이 Apache를 설치한 경로의 bin디렉터리까지 이동

    - C:\Program Files\Apache Software Foundation\Tomcat 8.5\bin

    - shutdown명령어 입력 후 이클립스 재실행

      ![tomcat7](/images/posts/201904/tomcat7.jpg)

    - Tomcat 서버 실행(실행버튼 클릭)

      ![tomcat8](/images/posts/201904/tomcat8.jpg)

    - 자신이 생성한 프로젝트이름 우클릭

    - Properties - Java Build Path - Libraries - Add Library 클릭

      ![tomcat9](/images/posts/201904/tomcat9.jpg)

    - Server Runtime 클릭

      ![tomcat10](/images/posts/201904/tomcat10.jpg)

    - Apache Tomcat 8.5ver 클릭

      ![tomcat11](/images/posts/201904/tomcat11.jpg)

    - Window - Preferences - General - Workspace - Other - UTF8설정

      ![tomcat12](/images/posts/201904/tomcat12.jpg)

    - Window - Preferences - Web에서 HTML, CSS, JSP - UTF8설정

      ![tomcat13](/images/posts/201904/tomcat13.jpg)

    - WebContent - New - JSP File만들기 (HelloWorld.jsp)

    - <%@ 표시가 빨간 밑줄이 없으면 정상적으로 작동하는 것

    - UTF-8로 설정되어있는지 확인

      ![tomcat14](/images/posts/201904/tomcat14.jpg)

    - Window - Web Browser - Chrome으로 설정

    - JSP실행은 Run As - Run on Server - Finish

  - __특징__

    왼쪽 프로젝트 안에 있는 폴더 중 `Java Resources`의 src폴더는 Java프로그램과
    DB를 담고있는 `model`이고 WebContent는 `JSP 프로그램 및 화면` 즉, `View`를 나타냅니다.
    JSP 프로그래밍에서 어려운 부분이 Web Page끼리 데이터를 교환하게 하는 기술인데
    이를 `컨트롤러(Controler)`라고 합니다. 컨트롤러를 구현하는 방법은 JSP를 이용하는 방법과
    Java Servlet을 이용하는 방법이 있습니다. JSP는 WebContent에 저장되며 Java Servlet은
    Java Resources에 저장됩니다.

  - __Tip__

    폰트사이즈 키우고 줄이는 방법 : Ctrl + & Ctrl -

## Apache & Tomcat 설치방법 Ver2.

  ![t1](/images/posts/201906/t1.jpg)

  zip파일을 다운 받고, 압축을 풀 폴더를 선택하고 압축을 해제합니다.

  ![t2](/images/posts/201906/t2.jpg)

  그리고 이클립스와 연동을 하기 위해서 Window - Show view - other - Server를 선택합니다. 선택을 하고나면 아래에 Server Tab이 생길 것입니다.

  ![t3](/images/posts/201906/t3.jpg)

  그리고 위 그림과 같이 따라합니다. Browse를 클릭해 아파치를 설치한 경로를 지정합니다. 그리고 JRE 버전을 1.8로 바꿔 줍니다.

  ![t4](/images/posts/201906/t4.jpg)

  이제 몇가지 설정을 해야 하는데 Server Tab에 있는 Tomcat Server를 더블클릭합니다.

  그리고 위 그림과 같이 Servers Location에 있는 `Use Tomcat Installation`을 체크하고 Servers Option에서 `Publish module contexts to separate XML files`를 체크합니다.

  그리고 DB를 `오라클(Oracle)`을 사용하는 경우는 오른쪽 Ports에 있는 HTTP/1.1 port 번호를 8090으로 수정합니다. 그 이유는 오라클의 기본 포트번호가 `8080`이기 때문에 Tomcat 포트번호와 중복이 되므로, 중복을 피하기 위해서 수정하는 것입니다.

  만약 `MySQL`을 사용하는 경우는 수정할 필요가 없습니다.

  ![t5](/images/posts/201906/t5.jpg)

  서버를 구동시키고 URL에 localhost:8090을 입력하여 다음 화면이 나오면 설치가 완료된 것입니다.

## JSP 한글 설정

  > Windows - Preferences

  ![s23](/images/posts/201905/s23.jpg)
