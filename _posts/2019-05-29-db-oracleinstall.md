---
title: "Oracle Install"
layout: post
category: DataBase
tags: [DataBase, Oracle]
excerpt: "Oracle 설치 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Oracle Install

  > ORACLE : [https://www.oracle.com/](https://www.oracle.com/)

  - 오라클 홈페이지에서 회원가입

  - 홈페이지 메인에서 다운로드 버튼 클릭
    - ![o1](/images/posts/201905/o1.jpg)  

  - DataBase 11g Enterprise/Standard Editions 클릭
    - ![o2](/images/posts/201905/o2.jpg)  

  - Accept License Agreement 클릭
    - ![o3](/images/posts/201905/o3.jpg)  

  - Oracle Database 11g Release 2에서 64비트, File1, File2 클릭
    - ![o4](/images/posts/201905/o4.jpg)  

  - 바탕화면 혹은 아무 곳이나 `Oracle`폴더를 만들고 그 안에 설치한 알집 파일들 넣어두기
    - File1(1of2.zip)을 `여기에 풀기` 클릭하면 database 폴더가 생긴다
    - File2(2of2.zip)도 `여기에 풀기`를 클릭하여 알집을 풀어준다.
    - ![o5](/images/posts/201905/o5.jpg)  

  - 생성된 databse 폴더
    - ![o6](/images/posts/201905/o6.jpg)  

  - database 폴더 안에 setup을 클릭하여 설치 시작
    - ![o7](/images/posts/201905/o7.jpg)  

  - Next
    - ![o8](/images/posts/201905/o8.jpg)  

  - Next
    - ![o9](/images/posts/201905/o9.jpg)  

  - Next
    - ![o10](/images/posts/201905/o10.jpg)  

  - 비밀 번호 설정 후 Next
    - ![o11](/images/posts/201905/o11.jpg)

  - 완료 클릭
    - ![o12](/images/posts/201905/o12.jpg)   

  - 설치 화면
    - ![o13](/images/posts/201905/o13.jpg)   

  - 다음 화면에서 비밀번호 관리 클릭
    - ![o14](/images/posts/201905/o14.jpg)  

  - SYS 비밀번호 설정
    - ![o15](/images/posts/201905/o15.jpg)    

  - SCOTT 체크 해제 후 비밀번호 설정
    - ![o16](/images/posts/201905/o16.jpg)  

  - HR 체크 해제 후 비밀번호 설정
    - ![o17](/images/posts/201905/o17.jpg)  

  - cmd에서 관리자 권한으로 접속
    - sqlplus / as sysdba
    - ![o18](/images/posts/201905/o18.jpg)  

  - 사용자 생성
    - sqlplus / as sysdba
    - ![o19](/images/posts/201905/o19.jpg)  

  - 사용자에게 권한 부여
    - grant connect, resource, dba to 아이디;
    - ![o20](/images/posts/201905/o20.jpg)

  - 사용자로 접속
    - sqlplus 아이디/패스워드
    - ![o21](/images/posts/201905/o21.jpg)

  - 사용자 접속 확인
    - show users;

  - 패스워드 변경 방법
    - 관리자 권한으로 접속 : sqlplus / as sysdba
    - alter user lbi5320 identified by 비밀번호
