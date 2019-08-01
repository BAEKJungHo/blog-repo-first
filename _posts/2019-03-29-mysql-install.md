---
title: "MySQL & HeidiSQL Install"
layout: post
category: DataBase
tags: [DataBase, MySQL]
excerpt: "MySQL의 설치방법과 HeidiSQL 설치방법을 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

MySQL의 설치방법과 HeidiSQL 설치방법을 배워봅시다.

## MySQL & HeidiSQL

  MySQL의 설치방법과 HeidiSQL 설치방법을 배워봅시다.

  HeidiSQL은 MySQL을 사용하기 위한 Tool입니다.

  [HeidiSQL 이란?](https://ko.wikipedia.org/wiki/HeidiSQL)

## MySQL 설치방법

  MySQL 5.7ver을 설치하는 방법을 배워보겠습니다.

### 기존에 설치된 MySQL 삭제

  MySQL을 설치하기 전에 한번 설치 한 적이 있었다면, 설치 되어있는 MySQL을 삭제해야합니다.

  1. 제어판 - 프로그램 및 기능 - MySQL관련된 프로그램 삭제
  2. 윈도우+R - regedit 입력 - Ctrl + F - MySQL 검색 후 레지스트리 전부 삭제

  MySQL검색이 더 이상 되지 않을 때까지 삭제 해주셔야 합니다.

  -----------------------------------------------------------------------------

### MySQL 설치과정

  먼저 [MySQL](https://www.mysql.com/) 홈페이지에 들어갑니다.

  ![mysql1](/images/posts/201904/mysql1.jpg)

  다운로드를 클릭하고 아래쪽으로 쭉 내려와서 MySQL Community Edition을 클릭합니다.

  ![mysql2](/images/posts/201904/mysql2.jpg)

  그 다음 맨위에 있 MySQL Community Server의 Downloads를 클릭해줍니다.

  ![mysql3](/images/posts/201904/mysql3.jpg)

  다음으로 중간에 내려와서 Looking for previous GA versions?에서 5.7버전을 클릭합니다.

  > 저희는 MySQL Community Server 5.7을 사용할 것입니다.

  ![mysql4](/images/posts/201904/mysql4.jpg)

  그리고 아래 빨간 네모 박스안의 아이콘을 클릭해서 들어갑니다.

  ![mysql5](/images/posts/201904/mysql5.jpg)

  그리고 여기서 가장 중요한데 16.4M버전이 아닌 `387.7M 버전`을 다운 받아야 합니다.
  16.4M 버전을 받게되면 나중에 `MySQL Notifier`에서 재시작을할때 StartPending상태로
  무한 대기상태가 되서 다시 설치해야 합니다.

  ![mysql6](/images/posts/201904/mysql6.jpg)

  그리고 No thanks, just start my download를 눌러서 다운로드를 해줍시다.

  ![mysql7](/images/posts/201904/mysql7.jpg)

  다운로드가 완료되었으면 설치를 진행 해주면됩니다.

  먼저 Default로 설치 해줍시다.

  ![mysql8](/images/posts/201904/mysql8.jpg)

  다음으로는 Excute를 클릭하여 필요 요소들을 설치해줍시다.

  ![mysql9](/images/posts/201904/mysql9.jpg)

  다음 화면이 나오면 또 Excute를 클릭해줍니다.

  ![mysql10](/images/posts/201904/mysql10.jpg)

  설치가 완료되면 다음 화면에서 Standard로 클릭하고 넘어갑니다.

  ![mysql11](/images/posts/201904/mysql11.jpg)

  다음 화면도 Next로 넘어갑니다.

  ![mysql12](/images/posts/201904/mysql12.jpg)

  이제 비밀번호를 설정해주고 넘어갑니다.

  ![mysql13](/images/posts/201904/mysql13.jpg)

  여기서 중요한점은 Windows Service Name을 기억해야합니다.
  설치하고 나서 __C:\user\사용자\AppData\Roaming\Oracle\MySQL Notifier__ 에 가서
  `settings.config`파일을 열고 ServiceName이 초기에 설정한 Name과 같은지 확인합니다.
  대소문자도 같아야합니다. DisplayName은 MySQL Notifier에 보이는 이름이라서
  ServiceName과 달라도 상관이 없습니다.

  만약 초기에 설정한 ServiceName과 settings.config에 있는 이름이 다르면 제대로 작동하지
  않습니다.

  설치 완료 후 Notifier 우클릭 - Actions - Manage Monitored Items - Services 작동상태확인하고나서
  MySQL Notifier `start-stop-restart`가 제대로 작동하는지 확인합니다.

  ![mysql14](/images/posts/201904/mysql14.jpg)

  그리고 쭉 Next로 넘기고 다시 Excute를 클릭합니다.

  ![mysql15](/images/posts/201904/mysql15.jpg)

  Finish를 누르고 또 Next로 넘깁니다. 그리고 MySQL Router Configuration에서도
  아무것도 건드리지 마시고 Finish를 클릭합니다. 또 Next로 넘기고
  비밀번호 입력 후에 Check를 클릭합니다.

  ![mysql16](/images/posts/201904/mysql16.jpg)

  다음으로 또 Excute를 클릭합니다.

  마지막으로 Finish - Next - Finish를 클릭하면 끝납니다.

  -----------------------------------------------------------------------------

### 한글설정

  MySQL 설치가 끝났으면 한글 설정을 해보도록 하겠습니다.

  숨김파일을 표시하기위해 C드라이브에 들어가고 Alt클릭 후 폴더 옵션 - 보기에서
  숨김파일 및 폴더 표시를 클릭합니다.

  `ProgramData`에 들어가서 MySQL - MySQL Servier 5.7로 들어가고 `my.ini`를 열어줍니다.

  그리고 MySQL Notifier를 Stop해줍니다.

  ```
  [client]
  default-character-set = utf8

  [mysqld]
  character-set-client-handshake=FALSE
  init_connect="SET collation_connection = utf8_general_ci"
  init_connect="SET NAMES utf8"
  character-set-server = utf8
  collation-server = utf8_general_ci

  [mysqldump]
  default-character-set = utf8

  [mysql]
  default-character-set = utf8
  ```

  위 코드를 my.ini 맨 마지막에 붙이고 저장하고 빠져 나옵니다.

  그리고 MySQL Notifier를 다시 Start해줍니다.

  시작 - 검색 - MySQL Command Line Client를 클릭하고 비밀번호 입력 후 status를 입력합니다.

  ![mysql17](/images/posts/201904/mysql17.jpg)

  다음 화면 처럼 utf-8로 설정되어있으면 완료 된것입니다.

  -----------------------------------------------------------------------------

### 사용자 등록

  사용자 등록을 위해 다음과 같이 입력해줍니다.

  > create user 'userID'@'%' identified by 'userpassword';
  >
  >create user 'java'@'localhost' identified by 'java';
  >
  > 다음은 권한 부여입니다.
  >
  > grant all privileges on *.* to 'java'@'localhost';
  >
  > flush privileges;

  사용자 등록과 권한부여를 마치면 MySQL Command Line Client 사용은 끝이 났습니다.

## HeidiSQL 설치방법

  [HeidiSQL](https://www.heidisql.com/download.php)홈페이지를 방문합니다.

  버전은 자신이 설치하고 싶은 버전을 설치하면 됩니다.

  저는 9.5 버전을 설치했습니다.

  ![HeidiSQL1](/images/posts/201904/heidiSQL1.jpg)

  그냥 쭉 Next를 눌러 설치를 완료 해주면 됩니다.

  ![HeidiSQL2](/images/posts/201904/heidiSQL2.jpg)

  설치가 완료되고나서 신규를 클릭후 user와 password에 초기에 설정했던 자신의
  아이디와 비밀번호를 입력해서 mysql과 연동을 해줍니다.
