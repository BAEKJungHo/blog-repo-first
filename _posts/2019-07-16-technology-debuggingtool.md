---
title: "디버깅(Debugging)"
layout: post
category: Technology
tags: [Technology, Java]
excerpt: "자바에서 디버거를 사용하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 자바에서 디버거를 사용하는 방법에 대해서 배워 봅시다.

## 디버깅(Debugging)

  - BreakPoints : 해당 지점 부터 코드를 실행하며, 오류를 찾아내거나 결과를 보고싶은 경우 사용
  - Variables : 코드실행결과가 나타남(객체들의 상태를 알 수 있음)

  - Server Tab에서 Debugging 모드로 실행 방법

  ![d1](/images/posts/201907/d1.jpg)

  그림과 같이 벌레모양을 클릭하면 됩니다.

  - Open Perspective를 열어서 Debug 선택

  ![d2](/images/posts/201907/d2.jpg)

  오른쪽 상단에 Debug 탭이 없는경우 Open Perspective를 클릭하고 Debug를 클릭하면 됩니다.

  - Breakpoints 설정

  ![d3](/images/posts/201907/d3.jpg)

  코드 라인이 적혀있는 곳에 자기가 원하는 지점을 클릭하여 BreakPoints를 설정합니다.

  - 디버깅 후 Variables Tab에서 객체들의 상태를 보면서 오류 체크

  ![d4](/images/posts/201907/d4.jpg)

## 단축키

  - F5 : 디버깅 한줄씩 실행 함수 내부로 들어감(Step Into)
  - F6 : 다음줄로 이동하면서 코드를 실행(Step Over)
  - F7 : 현재의 메소드에서 리턴한 직후에 다시 멈춤(Step Return) 
  - F8 : BreakPoints 전체 실행 후 View로 화면전환(즉, 코드실행 결과가 View에 나타남)
  - F11 : Debugging 모드로 전환
