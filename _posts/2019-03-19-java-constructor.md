---
title: "클래스와 생성자"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 중요한 클래스와 생성자에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## 클래스와 생성자

  - 클래스 구성 요소

  ![class](/images/posts/201903/class.jpg)

  클래스의 구성요소는 `필드(멤버변수), 생성자, 메소드(멤버함수)`로 구성됩니다.

  위에서 배운 오버로딩을 통해 매개변수가 다른 생성자를 여러개 가질 수 있으며,

  객체를 생성해서 `객체명과 .(도트)연산자`를 통해 필드와 메소드에 접근이 가능합니다.

## Eclipse Tool Generate

  이클립스에서 generate를 소개 시켜 드리겠습니다.

  generate 기능을 이용하면, 생성자를 생성할 수 있고, Getter, Setter메소드를 만들 수 있으며,
  toString() 메소드 등을 만들 수 있습니다.

  > 사용방법
  >
  > Eclipse 코드 입력창 빈 곳을 오른쪽 마우스 클릭
  >
  > source - Generate Getter and Setter, Generate Constructor using Fields.. 클릭
  >
  > Generate ~ 라고 제공되는 기능들을 클릭하면 생성자구현 및 각종 기능을 사용할 수 있습니다.

  아래는 generate toString() 기능을 사용하였습니다.

  ```java
  @Override
  public String toString() {
    return "StudentScore [name=" + name + ", math=" + math + ", english=" + english + ", science=" + science
        + ", avg=" + avg + "]";
   }
  ```

## Method Signature

  메소드 시그니처란?

  > public void main(String[] args) { 메소드 실행 블록 }
  >
  > 위에서 public void main(String[] args)까지가 메소드 시그니처 입니다.

## Getter()와 Setter()

  위 Generate Tool을 통해서 getter()와 setter()메소드 만드는 방법을 배웠습니다.

  이번에는 왜 getter()와 setter()를 사용해야하는지 작성은 어떻게 하는지 배우겠습니다.

  캡슐화를 하기위해서 우리는 `private`로 객체필드를 접근제한 할 것입니다.

  > private String name;

  private 접근제한한 객체필드는 외부 클래스에서 사용할 수 없고 초기화 할 수도 없습니다.

  이럴때 getter()와 setter()를 사용하면 외부에서 이 메소드를 사용하여 안전하게
  필드값을 변경하여 사용할 수 있습니다.

  가급적 클래스 필드들을 private로 선언하여 외부로 부터 보호하고, Getter()와 Setter()를
  사용하여 값을 변경하는게 좋습니다.

  - Getter()와 Setter()의 뜻  

  > Getter() 메소드 : 속성에 저장된 값을 외부 클래스에서 얻을 수 있다.
  >
  > Setter() 메소드 : private로 지정한 객체필드에 값을 초기화/변경 할 수 있다.

  - 주의할 점

  > Getter()와 Setter()는 외부 클래스에서 사용해야하므로 private로 지정하면 안된다.

  -----------------------------------------------------------------------------

  - Getter()와 Setter() 생성 방법

  ```java
  public class Person {
    private String name;
    private String city;
    private boolean sleep;

    // Getter
    public String getName(){
      return name;
    }
    // Setter
    public void setName(String name) {
      this.name = name;
    }
    // Getter
    public boolean isSleep() {
      return sleep;
    }
    // Setter
    public void setSleep(boolean sleep) {
      this.sleep = sleep;
    }
  }
  ```

  -----------------------------------------------------------------------------

  - 특징

  > Getter() 메소드 내부에는 return 값만 있습니다.
  >
  > Getter() 메소드명은 접두어(get) + 필드명(Name)
  >
  > Setter() 메소드명은 접두어(set) + 필드명(Name) + 매개변수타입 + 매개변수이름
  >
  > Setter() 메소드는 매개변수가 존재하며 값을 초기화 하기위해 this를 사용합니다.
  >
  > this.필드명 = 매개변수이름
  >
  > 필드 타입이 boolean일 경우 Getter() 메소드의 접두어는 is로 시작하는것이 관례

  - 순서

  > 외부클래스에서 Setter() 메소드를 사용해 필드 값을 초기화/변경
  >
  > 외부클래스에서 Getter() 메소드를 사용해 필드 값을 출력

  자신이 손으로 직접 Getter()와 Setter()를 작성해도 되지만 이클립스에서 제공하는
  Generate Tool을 이용하면 손쉽게 생성이 가능합니다.
