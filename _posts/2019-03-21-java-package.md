---
title: "Package & Import"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Singleton에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Package와 import

  패키지는 클래스의 일부분 입니다. 클래스명이 같더라도 패키지가 다르면 서로 다른 클래스입니다.
  자바에서는 패키지를 디렉터리로 나눕니다.(패키지 = 디렉터리)

  > 상위패키지.하위패키지.클래스

  - 다른 패키지의 클래스 사용하기

  두 가지 방법이 있는데 하나는 패키지이름과 클래스이름을 한꺼 번에 써 주는것이고
  다음 방법은 import문을 사용하여 작성하는 것입니다.

  - 첫 번째 방법

  ```java
  package animal;

  public class Person {
    animal.Mammalia mammalia = new animal();
  }  
  ```

  - 두 번째 방법

  ```java
  package person;
  import animal.*;

  public class Person {
    animal.Mammalia mammalia = new animal();
  }
  ```

## 자바 프로그래밍의 기본구조

  클래스는 최소한 2개이상 존재

  ```java
  public class Architecture {
    private String fieldName1;
    private int fieldName2;

    public ClassName(String fieldName1, int fieldName2){
      this.fieldName1 = fieldName1;
      this.fieldName2 = fieldName2;
    }

    public String getFieldName1(){
      return fieldName1;
    }

    public int getFieldName2() {
      return fieldName2;
    }

    public void setFieldName1(String fieldName1) {
      this.fieldName1 = fieldName1;
    }

    public void setFieldName2(int fieldName2){
      this.fieldName2 = fieldName2;
    }
  }
  ```

  ```java
  public class ArchitectureTest {
    public static void main(String[] args){
      // 생성자를 통한 객체 초기화, 객체필드에 값 저장
      Architecture arc1 = new Architecture("Mike", 26);
      System.out.println(arc1.getFieldName1());
      System.out.println(arc1.getFieldName2());

      // Setter() 호출
      arc1.setFieldName1("홍길동"); // 객체필드 값 변경
      arc1.setFieldName2(28); // 객체필드 값 변경

      System.out.println(arc1.getFieldName1()); // Getter() 호출
      System.out.println(arc1.getFieldName2()); // Getter() 호출
    }
  }

  // 출력결과
  Mike
  26
  홍길동
  28
  ```
