---
title: "[DBeaver] .ini 파일 설정"
layout: post
category: DataBase
tags: [MySQL, MyBatis]
excerpt: "DBeaver INI 파일 설정 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## INI 파일 설정 방법

  DBeaver를 실행하다 보면 `A Java Runtime Environment (JRE) or Java Development Kit (JDK) must be available in order to run Dbeaver. No Java virtual machine` 다음과 같은 오류 메세지가 나올 때가 있습니다.
  이것은 JDK 경로를 찾지못해서 발생하는 일입니다. 이 오류를 해결하기 위해서는 DBeaver ini 파일에 아래와 같은 코드를 맨 윗줄에 추가해주면 해결됩니다.

  ```
-vm
copy & paste your java path here (up to bin)

eg:
-vm
/home/admin/jdk1.8.0_131/bin
  ```
