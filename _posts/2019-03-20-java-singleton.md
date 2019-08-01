---
title: "Singleton"
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

## Singleton

  싱글톤은 전체 프로그램에서 단 하나의 객체만을 만들어야할때, 생성된 `단 하나의 객체`를 말합니다.

  따라서 객체가 한 개만 존재하기 위해서는 외부클래스에서 new연산자로 객체를 생성 하지못하게
  해야하는데 그러기 위해서는 `private` 접근제한자로 생성자를 만들면 됩니다.

  > private ConstructorName() { ..... }

  그리고 외부에서는 `getInstance()` 메소드를 통해 객체를 리턴 받습니다.

  > 자바의 Calendar 클래스가 Singleton 입니다.

  -----------------------------------------------------------------------------

  - Architecture

  > 객체 - 생성자 - getInstance()

  ```java
  public class ClassName {
    // static Instance
    private static ClassName singleton = new ClassName();

    // private Constructor
    private ClassName() {
      .....
    }

    static ClassName getInstance() {
      return singleton;
    }
  }


  public class SingletonTest {
    public static void main(String[] args){
      // getInstance() 메소드가 static 메소드(클래스 메소드) 이므로
      // 클래스명.메소드이름 으로 접근해야 합니다.
      ClassName variable = ClassName.getInstance();

      // variable1, 2, 3, 4, 5를 만들어도 다 서로 같은 싱글톤 객체 입니다.
    }
  }
  ```
