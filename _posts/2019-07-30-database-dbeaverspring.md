---
title: "[DBeaver] MySQL연결 및 IntelliJ 연결"
layout: post
category: DataBase
tags: [MySQL, Spring]
excerpt: "DBeaver에서 MySQL DB에 연결하는 방법과 IntelliJ에 연결하여 사용하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > DBeaver에서 MySQL DB에 연결하는 방법과 IntelliJ에 연결하여 사용하는 방법에 대해서 배워 봅시다.

## MySQL 연결

  > MySQL 설치 방법 : [https://baekjungho.github.io/mysql-install/](https://baekjungho.github.io/mysql-install/)

  가장 먼저 MySQL이 설치가 되어있어야합니다. 설치 되었다고 가정한 상태로 진행하겠습니다.

  MySQL 설치를 하고 나서, `MySQL 5.7 Command Line Client`를 실행합니다. 그리고 아래 명령어를 통해 DB를 생성합니다.

  ![sql1](/images/posts/201907/sql1.jpg)

  - DB 생성 : CREATE DATABASE DB명 DEFAULT CHARACTER SET UTF8;
  - DB 삭제 : DROP DATABASE DB명;

  DB를 생성하고 나서 DBeaver를 설치하고 실행합니다.

  - DBeaver 상단에 `데이터베이스` 클릭
    - 새 데이터베이스 연결 클릭
    - MySQL을 찾아서 클릭
    - ![sql2](/images/posts/201907/sql2.jpg)

  - DB에 연결하기 위한 정보를 입력
    - ![sql3](/images/posts/201907/sql3.jpg)
    - `MySQL 기본 포트번호는 3306`

  - 연결 완료된 화면
    - ![sql4](/images/posts/201907/sql4.jpg)

## IntelliJ 연결
