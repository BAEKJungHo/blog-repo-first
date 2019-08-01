---
title: "문자열 찾기"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 문자열을 찾는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 문자열 찾기

  주어진 문자열에 대해서 찾고자 하는 문자열을 찾는 방법을 배워 봅시다.

  [String 클래스](https://baekjungho.github.io/java-api)에서 지원하는 다음 메소드 들을 사용하면 됩니다.

  1. contains :  문자열에 검색하고자 하는 문자가 있는지 확인(포함: true / 미포함: false)
  2. indexOf : 문자열에서 검색하는 문자의 인덱스를 반환(포함: 인덱스위치 / 미포함: -1)
  3. matches : 정규식을 이용하여 문자열을 검색한다, 특정 문자열을 검색할때 사용하기 보다는 한글, 숫자 등과 같이 해당 형태의 텍스트가 존재하는지 확인할때 사용하면 좋습니다(포함: true / 미포함: false)

  ```java
  public class FindString {
    public static void main(String[] args) {
      String str1 = "Java Programming" ;
      String str2 = "Java Python Django JSP Servlet" ;
      String str3 = "농구공 가격은 29,000원 입니다" ;

      // contains를 이용한 방법(true, false 반환)
      if(str1.contains("Java"))
          System.out.println("문자열 있음!");
      else
          System.out.println("문자열 없음!");


      // indexOf를 이용한 방법
      if(str2.indexOf("Java") > -1)
          System.out.println("문자열 있음!");
      else
          System.out.println("문자열 없음!");

      // matches를 이용하여 정규 표현식으로 문자열에 숫자가 있는지 확인
      if(str3.matches(".*[0-9].*"))
          System.out.println("숫자 있음!");
      else
          System.out.println("숫자 없음!");
   }
  }
  ```
