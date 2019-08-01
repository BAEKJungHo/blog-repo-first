---
title: "Abstract"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 추상클래스와 추상메소드에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## abstract class

  추상 클래스(abstarct class)는 부모 클래스이며, 자식 클래스들의 공통적인 속성(필드)과 기능(메소드)을
  정의한 클래스이며, `abstarct` 키워드를 사용하여 정의합니다.

  추상 클래스도 `class`이므로 `필드, 생성자, 메소드`를 가질 수 있습니다.

  추상 클래스는 추상 메소드를 가지지 않아도 상관없습니다.

  다만, 추상 메소드가 하나라도 존재하는 클래스는 추상 클래스가 되어야 합니다.

  `abstract`로 선언한 추상 클래스는 __new 연산자를 통한 객체생성이 불가능__ 합니다.

  > abstract class는 상속(extends)을 통해서만 사용이 가능하다.

  따라서, 자식 클래스의 생성자에서 `super()`변수를 사용해서 추상 클래스의 생성자를 부르고 초기화 시킵니다.

  > abstarct class는 생성자(Constructor)가 필수적이다.

  - Architecture

  ```java
  public abstract class AbstractClass {
    // Field
    private String name;
    private String id;

    // Constructor 필수
    public AbstractClass(String name, String id){
      this.name = name;
      this.id = id;
    }

    // Method
    public method() { }
  }

  public class RealClass extends AbstractClass {
    private String number;

    public RealClass(String name, String id, String number) {
      super(name, id); // super()로 추상클래스(부모) 생성자 호출, 초기화
      this.number = number;
    }
  }
  ```

  추상 클래스는 공통적인 속성과 기능을 정의한 클래스 이므로 접근 제한자를 `private`로 지정해서는 안됩니다.

  추상 클래스를 상속받은 자식 클래스를 `실체 클래스`라고 부릅니다.

  실체 클래스(자식 클래스)는 공통적인 속성과, 기능을 물려받고(상속) 자신만의 속성과 기능을 추가할 수 있습니다.

  - 사용 목적

  > 공통된 필드와 메소드 이름 통일
  >
  > 코드 작성 시간 절약

  - abstract class 정리

  > new 연산자를 통한 객체생성 불가능, 상속(extends)을 통해서 사용 가능
  >
  > 추상 클래스 = 부모 클래스
  >
  > 실체 클래스 = 자식 클래스
  >
  > private 사용 금지

  -----------------------------------------------------------------------------

  - 사용예제

  ```java
  public abstract class Person {
    public abstract String getGender();
  }

  public class Man extends Person {
    public String getGender() {
    return "M";
    }
  }

  public class Woman extends Person {
    public String getGender() {
    return "F";
    }
  }

  Person steve = new Man(); // 객체생성
  ```

## abstarct method

  추상 메소드도 추상 클래스와 사용방법이 비슷합니다.

  추상 클래스 내부에는 실체 클래스들이 공통적으로 가질 속성(필드)과 기능(메소드)를 정의해 놓는데,
  `실체 클래스에서 동작을 다르게 구현`해줘야 하면 추상 클래스 내부에 메소드를 abstract으로 정의합니다.

  __즉, 모든 실체클래스들의 기능(동작, 메소드)이 동일하다면 abstract 사용하지않고 메소드 정의__

  __반면, 기능(동작, 메소드)이 서로 상이하다면 abstract을 사용해서 추상 메소드로 정의__

  추상 메소드도 상속 되서 사용 되어지는 것이므로 private 접근 제한자 사용 불가능합니다.

  추상 메소드는 `중괄호 { }`를 사용하지 않습니다.

  - 사용방법

  > public abstract returnType MethodName();

  -----------------------------------------------------------------------------

  - 사용 예제

  ```java
  public abstarct class Phone {
    public String name;
    public String serial;

    public Phone(String name, String serial){
      this.name = name;
      this.serial = serial;
    }

    public abstract void store(); // abstarct method

    public void call() {
      System.out.println("전화를 겁니다.");
    }
  }

  public class Iphone extends Phone {
    public Iphone(String name, String serial){
      super(name, serial);
    }

    @Override
    public void store() {
      System.out.println("앱스토어");
    }
  }

  public class Android extends Phone {
    public Android(String name, String serial){
      super(name, serial);
    }

    @Override
    public void store(){
      System.out.println("플레이스토어");
    }
  }

  public class PhoneExample {
    public static void main(String[] args){
      Iphone iphone = new Iphone("아이폰X", "12345");
      Android android = new android("갤러시10", "54321");

      iphone.store();
      android.store();
    }
  }
  ```
