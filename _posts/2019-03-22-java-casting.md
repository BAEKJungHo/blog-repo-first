---
title: "Casting & Promotion"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 타입변환인 자동타입변환(Promotion)과 강제 타입변환(Casting)에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## 타입변환

  [타입변환](https://baekjungho.github.io/java-operator/)을 Java Basic 1에서 배웠습니다.

  타입변환은 종류는 `묵시적타입변환(자동, Promotion)`, `명시적타입변환(강제, Casting)` 2가지가 있습니다.

  타입변환은 기본타입에서도 적용이 되지만, 객체에서도 타입변환이 가능합니다.

  __단! 상속관계에있는 SuperClass와 SubClass 사이에서 가능한 일입니다.__

  > 기본타입일 경우 int --> long 을 `Promotion`
  >
  > 기본타입일 경우 long --> int 를 `Casting`

  참조타입인 객체에서도 기본타입을 타입변환하는 개념이 똑같이 적용됩니다.

  > 자식타입 --> 부모타입 `Promotion`
  >
  > 부모타입 --> 자식타입 `Casting`

  __타입변환을 잘 사용하면 다형성(Pholymorphism)을 구현할 수 있습니다.__

## Promotion

  자동타입변환(Promotion)은 자식타입이 부모타입으로 변한 됩니다.

  - Architecture

  > SubClass subVar = new SubClass();
  >
  > SuperClass supVar = subVar; OR SuperClass supVar = new SubClass();

  `자동타입변환된 supVar(부모클래스 변수)는 메모리에서 자식객체를 참조하지만
  supVar변수로 접근가능한 멤버는 부모클래스.`

  왜냐하면, 자식은 부모로 부터 상속을 받았기 때문에 Heap영역에서도 자식객체는
  부모객체를 가리키게됩니다.

  - [Exception(Dynamic Binding)](https://baekjungho.github.io/java-binding/)

  __단, 자식클래스에서 메소드 오버라이딩을 구현했으면, supVar변수는 메소드 오버라이딩된
  자식클래스의 메소드를 호출하게 됩니다.__

  - 사용 예제

  ArrayList라는것은 List를 상속받는데 사용방법이 아래와 같습니다.

  > List list = new ArrayList();

  ```java
  public class Phone {
    Screen screen = new Screen();


    void run() { ..... }
  }
  //////////////////////////////////////////////////////////////////////////
  Phone phone = new Phone();
  phone.screen = new IphoneScreen();
  ```

## Casting

  강제타입변환(Casting)은 부모타입이 자식타입으로 변환 되는 것입니다.

  __단! Casting을 하기 위해서는 자식타입 --> 부모타입 --> 자식타입으로 진행됩니다.__

  만약에 부모타입이 원래 태초 부모타입(즉, 자식타입이 부모타입으로 변환한 것이 아닐 경우)일 경우에는
  Casting을 할 수 없습니다.

  - instanceOf(객체 타입 확인)

  Casting을 할때는 항상 같이 딸려오는 녀석입니다.

  객체가 태초 부모였는지, 자식이 부모가됬는지 확인해주는 키워드 입니다.

  > booelan result = `Object(객체)` instanceOf `Type(타입)`

  즉, 왼쪽 객체가 오른쪽 타입인지 확인하고 참이면 true, 거짓이면 false를 반환합니다.

  - 사용 예제

  ```java
  public void casting(Parent parent){
    if(parent instanceOf Child) {
      Child child = (Child) parent; // Casting
    }
  }
  ```

  만약 if(parent instanceOf Child) 구문이 없으면 ClassCastException이 일어날 수 있습니다.
