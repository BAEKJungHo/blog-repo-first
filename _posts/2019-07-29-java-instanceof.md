---
title: "instanceof"
layout: post
category: Java
tags: [Java]
excerpt: "instanceof 연산자의 사용 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Java에서 지원하는 instanceof 연산자에 대해서 배워 봅시다.

## instanceof

  `instanceof` 연산자는 만들어진 객체가 특정 클래스의 인스턴스인지 물어보는 연산자입니다.
  특정 클래스의 객체(인스턴스)이면 `true`를 반환, 아닐 경우 `false`를 반환 합니다.

  ```java
  // 객체 참조 변수 instanceof 클래스명

  interface EatAble {

  }

  class Animal implements eatAble {

  }

  class Mammal extends Animal implements eatAble {

  }

  public class CheckInstanceOfAnimal {
    public static void main(String[] args) {
      Animal animal = new Animal();
      Mammal mammal1 = new Mammal();
      Animal mammal2 = new Mammal();

      EatAble mammal3 = new Mammal();

      if(mammal1 instanceof Animal) {
        System.out.println("포유류는 동물이 맞습니다.");
      }
      if(mammal2 instanceof Animal) {
        System.out.println("mm은 동물이 맞습니다.");
      }
      if(mammal3 instanceof EatAble) {
        System.out.println("포유류는 음식을 먹을 수 있습니다.");
      }
    }
  }
  ```

  위 프로그램 실행 결과 모두 true이기 때문에, if문이 실행 됩니다.

  __즉, instanceof 연산자는 객체 참조 변수의 타입이 아닌 실제 객체의 타입에 의해 처리합니다.__

  또한, 상속관계 뿐만 아니라 인터페이스의 관계까지도 표현 할 수 있습니다.
