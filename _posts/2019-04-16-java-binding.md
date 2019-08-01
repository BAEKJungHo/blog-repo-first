---
title: "동적바인딩 vs 정적바인딩"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 동적바인딩과 정적바인딩에 대해서 배워 봅시다. "
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 동적바인딩 vs 정적바인딩

  `동적바인딩(Dynamic Binding)`은 다형성(Polymorphism)을 사용하여 메소드를 호출할때 발생하는 현상입니다.
  동적바인딩은 `실행시간(Runtime)` 즉, 파일이 실행하는 시점에서 성격이 결정되고 `정적바인딩(Static Binding)`은
  `컴파일(Compile)`시간에 성격이 결정됩니다.

  동적바인딩과 정적바인딩을 할때 알아둬야할 점은 [Object Promotion](https://baekjungho.github.io/java-casting/)에 관한 내용입니다.

  > 동적바인딩 : 실제 참조하는 객체는 서브클래스이니 서브클래스의 메소드를 호출한다.
  >
  > 정적바인딩 : 변수의 타입이 수퍼클래스이니 수퍼클래스의 메소드를 호출한다.

  - __Dynamic Binding vs Static Binding__

    아래 예제를 통해서 동적바인딩 동작과정에 대해 확인해 보겠습니다.

    ```java
    class Polymorphism {
      public static void main(String[] args) {
      SuperClass sup = new SubClass(); // Promotion : 자동 타입변환, Pholymorphism
      // 동적 바인딩
      sup.methodA(); // Runtime시에 결정된다. SubClass의 메소드 호출
      // 정적 바인딩
      sup.staticMethodA(); // static 메소드는 compile시에 결정, SuperClass의 메소드 호출
     }
    }

    class SuperClass {
      void methodA() { System.out.println("SuperClass"); }
      void staticMethodA() { System.out.println("SuperClass"); }
    }

    class SubClass extends SuperClass {
      void methodA() { System.out.println("SubClass"); }
      void staticMethodA() { System.out.println("SubClass"); }
    }
    ```

    methodA()는 SubClass에서 SuperClass를 상속받아 오버라이딩 한 것입니다.
    methodA()는 어떤 클래스의 메소드를 나타내는지는 `런타임시간`, 즉 클래스 파일이 실행하는 시점에서 결정이 된다는 말입니다.
    다시말해 동적바인딩은 런타임 시점에 객체 타입을 기준으로 실행될 함수를 호출하는 것을 의미합니다.

    위 코드에서는 sup객체참조변수로 접근가능한것은 부모 클래스 멤버이지만, 자식클래스에서 메소드를 오버라이딩 한 경우에는
    [Promotion Exception](https://baekjungho.github.io/java-casting/)에 의하여 자식클래스의 메소드를 호출하는 것입니다.

    > sup객체참조변수는 런타임시에 SubClass의 methodA() 호출
    >
    > sup객체참조변수는 컴파일시에 SuperClassd의 staticMethodA 호출

  __모든 인스턴스 메소드는 Runtime시에 결정되지만 클래스(static) 메소드와 인스턴스 변수는 Compile 시에 결정이 된다.__
  따라서 메소드가 instance인지 static인지에 따라서 달라집니다.
