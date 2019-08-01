---
title: "DB 컬럼이 Y/N의 상태를 가지는 경우"
layout: post
category: JSP
tags: [JSP, MySQL]
excerpt: "DB 컬럼이 Y/N의 상태를 가지는 경우의 JSP 화면출력을 어떤 방식으로 하는지 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > DB 컬럼이 Y/N의 상태를 가지는 경우의 JSP 화면출력을 어떤 방식으로 하는지 배워 봅시다.

## JSP에서의 화면출력 방법

  DB컬럼명이 DEL_STS일 경우 이 컬럼은 Y/N 삭제, 미삭제의 컬럼값을 가집니다.

  이때 화면에 삭제, 미삭제 여부를 출력하기 위해서는 다음과 같이 하면 됩니다.

  ```html
  <c:if test="${result.delSts eq 'N'}" >미출력</c:if>
  <c:if test="${result.delSts eq 'Y'}" >출력</c:if>
  ```
