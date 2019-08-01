---
title: "MySQL과 Oracle의 차이"
layout: post
category: DataBase
tags: [DataBase, MySQL, Oracle]
excerpt: "MySQL과 Oracle의 차이에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## MySQL과 Oracle의 차이

### 자료형의 차이

  - MySQL의 자료형
    - int
    - char
    - varchar

  - Oracle의 자료형
    - number
    - char
    - varchar2

  > char은 고정형 문자열 자료형이며, varchar은 가변형 문자열 자료형인데 varchar가 메모리 관리 측면에서
  효율이 더 좋음에도 불구하고 char를 사용하는 이유는, 주민등록번호와 같이 고정된 문자열을 담을 때 사용하며
  varchar보다 검색 속도가 빠르기 때문에 사용합니다.

## 실무에서 많이 쓰이는 것은?

  실무에서 많이 쓰이는 것은 `Oracle` 입니다.

  php를 사용하면 1000명으로 접근하면 1000개의 프로세스를 사용하기 때문에 mysql을 사용하면 예전의 학사 프로그램같은 경우 서버가 다운될 수도 있습니다.
  따라서 oracle을 회사에서 더 많이 사용합니다. jsp는 스레드 개념으로 처리합니다.
