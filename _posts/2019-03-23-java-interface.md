---
title: "Interface"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Interface의 구현방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Interface

  자바에 있어서 위에서 배운 `abstract과 interface`는 너무 중요합니다.

  **From. 디자인패턴과 리팩토링**  

  |자바언어로 유연하고 확장성있는 프로그램을 만들려면 일반 클래스(class)가 아닌
  순수 추상 클래스(pure abstract class) 또는 자바 인터페이스(interface) 위주로 프로그래밍 하라!|

  -----------------------------------------------------------------------------

  인터페이스를 배우기전에 `인터페이스`의 사전적 의미를 알고 갑시다.

  - [인터페이스의 사전적 의미](https://terms.naver.com/entry.nhn?docId=2837557&cid=40942&categoryId=32828)

  사물과 사물 사이 또는 사물과 인간 사이의 경계에서, 상호 간의 소통을 위해 만들어진 물리적 매개체나 프로토콜을 말한다.
  예를들어 파워포인트의 각종도구들, 저장버튼(save()), 인쇄버튼(print())들이 인터페이스 입니다.

  save()와 print()들은 메소드이며 Method Signature와 각 이름에 맞게 기능(동작)을 담당하는 내부 코드가 있습니다.

  즉, 정리하면 아래와 같습니다.

  -----------------------------------------------------------------------------

  __사전적 의미의 인터페이스__ : Method Signature(자바 인터페이스에서의 추상메소드)

  __사전적 의미의 인터페이스 구현(implements)__ : 기능(동작)을 담당하는 메소드 내부코드

  - __실체 메소드__

  구현하고자 하는 객체의 클래스에서 추상메소드를 오버라이딩하여 사전적 의미의 인터페이스 구현을
  하는 메소드를 `실체 메소드`라고 합니다.

  - __구현 클래스__

  구현하고자 하는 객체의 클래스, 즉, 실체 메소드를 작성하는 클래스를 말합니다.

  >  구현클래스 = 객체를 구현한다, 실체 메소드를 작성한다.

  - __실행 클래스__

  메인 메소드가 있는 클래스

  ```java
  public void run() { 내부코드 }
  ```

  -----------------------------------------------------------------------------

  자바에서의 인터페이스는 내부에 `상수`와 `추상메소드`(즉, 사전적 의미의 인터페이스, ex)저장버튼)만 구현되어있고
  `추상메소드`의 이름에 맞게 동작을 구현하려면 `구현하고자하는 객체`에서 메소드 오버라이딩을 통해 구현하는 것입니다.

  > 자바 인터페이스 { 사전적의미의 인터페이스1, 사전적의미의 인터페이스2, ..... }
  >
  > 객체 클래스(구현하고자하는 객체)에 자바 인터페이스 장착
  >
  > 객체 클래스 내부 { 사전적의미의 인터페이스1 구현, 사전적의미의 인터페이스2 구현 .....}
  >
  > 실행 클래스(메인메소드가 있는 클래스) { 인터페이스 변수 = new 객체클래스명 }

  -----------------------------------------------------------------------------

  예를들어 자바에서 리모컨(RemoteControl)이라는 인터페이스를 만들고 내부에는 끄기(turnOff), 켜기(turnOn), 볼륨업(volumeUp),
  볼륨다운(volumeDown), 다음(next), 이전(prev) 이런 `추상메소드(사전적 의미의 인터페이스)`가 있습니다.

  이제 `인터페이스 구현(implements)`을 통해 각 객체에 맞게 동작하게 해야합니다.

  인터페이스의 구현은 `구현 클래스`에서 한다고 했습니다.

  각 객체에서 메소드 오버라이딩을 통해 구현한 메소드들은 `실행클래스안에 있는 메인메소드`에서 호출됩니다.

  우리는 TV객체와 Audio객체를 만들것입니다. 공통적인 기능이 담겨있는 인터페이스를 장착(사용)해서 __각 객체의 특성에 맞게
  기능(동작)을 다르게 구현(implements)__ 해줘야 할 것입니다.

  > 구현하고자하는 객체 : TV, Audio
  >
  > 메인메소드 : turnOn(), turnOff(), volumeUp(), volumeDown() 메소드 호출
  >
  > 인터페이스명 변수이름  = null;
  >
  > 변수이름 = new 클래스명(=객체);  

  자바 코드로 나타내면 다음과 같습니다.

  ```java
  public class TelevisionExample {
    public static void main(String[] args){
    RemoteControl rc = null; // 인터페이스 변수 선언
    rc = new Television(); // 텔레비전 객체 생성
    // 이 rc는 Television의 객체변수 입니다.
    // 즉, rc를 통해 Television클래스에서 메소드 오버라이딩을 통해 기능을 구현한
    // 메소드들을 사용할 수 있습니다.

    rc.turnOn(); // Television클래스에서 구현(implements)한 메소드 사용
    rc.volumeUp(); // Television클래스에서 구현(implements)한 메소드 사용

    rc = new Audio(); // Audio객체 생성, 이제 rc는 Audio객체변수 입니다.
    rc.turnOn(); // Audio클래스에서 구현(implements)한 메소드 사용
    rc.volumeDown(); // Audio클래스에서 구현(implements)한 메소드 사용
   }
  }
  ```

  - 자바에 인터페이스의의미

  자바의 인터페이스(interface)는 `객체에 장착할 사전적 의미의 인터페이스 집합체`입니다.

  - 이클립스에서 인터페이스 생성 방법

  > New - interface 클릭

  - 인터페이스 장착(사용) 방법

  > 클래스명 implements 인터페이스명 { ..... }

  - 실체 메소드 생성 방법

  인터페이스 내부에 있는 추상메소드를 구현하기위해서 실체 메소드를 작성해야하는데
  마우스클릭 몇번으로 간단하게 생성할 수 있습니다.

  > 인터페이스 장착(implements 키워드 사용)
  >
  > 화면 빈 곳 우클릭 - Source - Override/Implement Methods.. 선택

  - 특징

  > 인터페이스는 상수와 메소드만 가질 수 있습니다. (생성자, 필드 X)
  >
  > 인터페이스에 선언된 상수와 메소드들은 public static final과  public abstract를 생략해도 됩니다.
  >
  > 왜냐하면 컴파일시에 자동으로 생기기때문입니다.
  >
  > 상속과 달리 구현 클래스에 여러개의 인터페이스를 장착할 수 있습니다.
  >
  > 인터페이스는 다중 상속을 지원합니다
  >
  > 인터페이스를 장착 하는 클래스들의 이름을 마지막에 통상 ~Impl로 정합니다.
  >
  > ex) CarImpl

  - Architecture

  > 인터페이스나 추상클래스들은 대게 접근 제한자를 public으로 설정
  >
  > 인터페이스를 사용하는 클래스나 추상클래스를 상속받는 클래스의 필드는 대게 private로 설정
  >
  > 인터페이스의 기본 접근 제한자는 public

  __! 짚고 넘어가기__

  > 객체선언 : Car myCar;
  >
  > 객체생성 : myCar = new Benz();

  인터페이스는 생성자를 가지지 못하므로 `객체화가 불가능`합니다.

  하지만 실행 클래스에서 `InterfaceName 변수;` 이런식의 코드가 많이 보이는데 헷갈리시면 안되는게
  이것은 Inteface타입의 변수를 선언한 것이지 객체를 선언한 것이 아닙니다.

  > InterfaceName 변수; = 인터페이스 참조 변수

  이렇게 변수를 선언하고 나서 `변수 = new 실체클래스()` 방식으로 변수를 객체로 바꾸는 것입니다.

  즉, 저 변수는 실체클래스의 메소드와, 필드들을 이용할 수 있습니다.

  -----------------------------------------------------------------------------

### 디폴트메소드, 정적메소드

  자바8부터 인터페이스에 지원하는 디폴트 메소드와 정적 메소드 입니다.

  인터페이스에서 추상메소드의 내부코드(사전적 의미의 구현)는 객체 클래스에서 작성합니다.

  반면, default method의 특징은 인터페이스 내부에 선언되면서, 내부코드도 가질 수 있습니다.

  defualt method는 `인터페이스명.메소드명`으로 접근이 불가능하고 인터페이스 객체를 선언하고
  객체변수에 new연산자를 통해 원하는 객체를 생성해서 접근해야 합니다.

  static method는 `인터페이스명.메소드명`으로 접근이 가능합니다.

  - default method

  디폴트 메소드는 인터페이스 내에 메소드시그니처와 메소드 내부코드가 구현되어있습니다.

  즉, 사전적 의미의 인터페이스와, 사전적 의미의 인터페이스 구현(implements)이 되어있습니다.

  자바 8부터 디폴트 메소드를 지원하는 이유는 `디폴트 메소드 내부에 새로운 공통적인 기능을 추가`함으로써
  코드작성반복을 줄여줍니다.

  따라서 인터페이스를 장착한 구현클래스에서 실체메소드를 만들 필요가 없고 인터페이스의 디폴트 메소드만
  수정해서 사용하면됩니다. 만약 추가하고싶은 내용이 있으면 실체 메소드에서 추가해서 사용하면 됩니다.

  > Defualt Method : 실행 블록을 가지고 있는 메소드
  >
  > 목적 : 기존 인터페이스 기능을 확장
  >
  > 특징 : default 키워드를 반드시 붙여야함

  ```java

  interface Printable {
    public abstarct void paper();
    // Default Method : 실행 내용까지 있음
    public default void setPrint(boolean color){
      if(color){
        System.out.println("컬러 출력");
      }
      else {
        System.out.println("흑백 출력");
      }
    }
  }
  ```

  나중에 구현클래스에서 각 객체들이 가지는 공통속성인 setPirnt()기능이 업그레이드 되었을때
  여러군데에서 고칠 필요 없이 interface 내부의 디폴트 메소드만 고치면 됩니다.

  -----------------------------------------------------------------------------

  - 디폴트 메소드가 있는 인터페이스 상속받기

  만약 하위 인터페이스에서 상위 인터페이스를 상속받으려고하는데 상위 인터페이스 내부에
  디폴트 메소드가 있는경우 하위 인터페이스에서 디폴트 메소드를 활용하는방법이 3가지 있습니다.

  1. 디폴트 메소드를 단순히 상속만 받는다.
  2. Override(재정의)한다.
  3. 디폴트 메소드를 추상 메소드로 재선언 한다.

  ```java
  public interface Printable {
    public void paper();
    public default void setPrint(boolean color){
      if(color){
        System.out.println("컬러 출력");
      }
      else {
        System.out.println("흑백 출력");
      }
    }
  }

  // 단순히 상속받는 경우
  public interface Printablechild1 extends Printable {
    public void battery();
  }

  // Override
  public interface Printablechild2 extends Printable {
    @Override
    public default void setPrint(boolean color){
      if(color){
        System.out.println("레드컬러 출력");
      }
      else {
        System.out.println("흑백 출력");
      }
    }
    public void lens();
  }

  // 추상메소드로 변경
  public interface Printablechild3 extends Printable {
    public void setPrint();
    public void setOutColor();
  }
  ```

  - static method

  정적 메소드도 디폴트 메소드처럼 실행블록을 가지고 있습니다.

  디폴트 메소드처럼 static 키워드를 꼭 붙여야합니다.

  ```java
  interface Printable {
    public abstarct void paper();

    // Default Method : 실행 내용까지 있음
    public default void setPrint(boolean color){
      if(color){
        System.out.println("컬러 출력");
      }
      else {
        System.out.println("흑백 출력");
      }
    }

    // Static Method : 실행 내용까지 있음
    public static void changeBattery() {
      System.out.println("배터리 교환");
    }
  }
  ```

  만약에 default나 static 키워드를 붙이지 않고 실행코드를 작성하는 것을 불가능합니다.
  왜냐하면 인터페이스에서 메소드의 기본 타입이 `public abstract` 때문입니다.
  따라서 추상메소드는 메소드의 시그니처만 구현되어있는 메소드라서 메소드 내부에
  실행코드를 작성하는것을 불가능합니다.

### 다중 상속

  인터페이스는 다른 인터페이스들을 상속 받을 수 있습니다.

  - Architecture

  > public interface 하위인터페이스 extends 상위인터페이스1, 상위인터페이스2 .....

  상속받은 하위 인터페이스는 상위 인터페이스의 메소드 들을 모두 사용할 수 있습니다.

### 구현 클래스에서 메소드 오버라이딩시 주의점

  구현 클래스에서 실체 메소드를 작성할 때(추상클래스를 메소드 오버라이딩하여 재작성) 주의할 점은
  인터페이스의 모든 메소드 들은 기본이 `public 접근 제한자`이기 때문에 이것보다 더 낮은
  접근 제한자로 설정 할 수 없습니다.

  만약 더 낮은 접근제한자로 설정시 아래와 같은 오류가 발생합니다.

  > Cannot reduce the visibility of the Inherited method  

### 익명구현객체

  익명구현객체(Anonymouse class)는 구현클래스와 실행클래스를 합쳐서 하나의 클래스 안에서 사용하는 것입니다.

  자바8에서 지원하는 람다식에서 많이 사용합니다.

  익명구현객체는 이름이 없는 객체로서 일회성이며, 재사용할 필요가 없을때 사용합니다.

  - Architecture

  > 인터페이스 변수 = new 인터페이스() { 실체 메소드 선언 };

  - 특징

  인터페이스 내에 선언된 모든 `추상메소드`(디폴트, 정적메소드 제외)를 오버라이딩 하여
  실체 메소드를 작성해야합니다. 하나라도 빠질시 오류가 납니다.

  또한 추가적으로 필드와 메소드 선언이 가능하지만 익명구현객체 안에서만 사용가능합니다.

  ```java
  public class PrintExamle {
    public static void main(String[] args){
      Printable print = new Printable() {
        public void paper() {
          System.out.println("용지 부족");
        }
      };
      print.paper();
    }
  }
  ```

## Inheritance vs Interface

  상속을 쓸지 인터페이스를 쓸지 고민하는 것은 아래와 같은 선택지 입니다.

  1. 추상클래스로 만들어 자식들이 상속하여 쓸것인가?
  2. 인터페이스를 만들어 장착 시킬 것인가?

  - 예시

  ```java
  abstarct class Printer {
    public abstarct void print(String message);
  }

  interface Printable {
    public void print(String message);
  }
  ```

  둘 다 추상메소드로 구현되어 있고 프린트 기능이 있습니다.

  추상클래스인 Printer는 각종 "프린터" 들의 최상위 클래스 입니다.

  Printer를 상속받는 클래스들은 잉크젯 프린터, 레이저 프린터 등등 `패밀리` 개념의 클래스들 입니다.

  [상속](https://baekjungho.github.io/java-basic2)을 하는 이유에 대해서 배웠습니다.

  > IS-A 하위클래스는 부모클래스이다 - Family

  위 개념이 성립할때만 상속을 사용하는 것입니다.

  인터페이스 이름을 지을때 특징이 `~able`과 같이 `가능한`이라는 의미를 담는 이름이 많습니다.

  그 이유는 인터페이스가 `특정 객체들의 능력`을 상징합니다.

  Printable 인터페이스를 구현하는 실체 클래스들은 X-ray, BodyPrinting등
  프린트 가능한 객체들일 것입니다.

  어떤 것을 선택하여 쓸지는 개발자들의 몫입니다.

## Nested Class

  중첩클래스는 두 클래스가 서로 긴밀한 관계를 가질때 사용하며, 코드의 캡슐화를 증가시키고
  외부에서 내부클래스에 접근할 수 없으므로 코드의 복잡성을 줄여줍니다.
  또한, 외부클래스와 내부클래스간의 멤버 접근이 쉬워집니다.

  중첩 클래스는 클래스 내부에 또 다른 클래스를 선언하는 것을 말합니다.

  중첩 클래스(NestedClass)는 크게 `멤버 클래스`와 `로컬 클래스`, `익명 클래스`로 나뉩니다.

  또 멤버클래스는 `객체(인스턴스)멤버 클래스`와 `정적(static) 멤버 클래스`로 나뉩니다.

  [객체멤버와 정적멤버의 차이 공부하기](https://baekjungho.github.io/java-basic2)

### Inner Class를 사용하는 경우

  내부 클래스(inner class)란 하나의 클래스 내부에 선언된 또 다른 클래스를 의미합니다.

  이러한 내부 클래스는 외부 클래스(outer class)에 대해 두 개의 클래스가 서로 긴밀한 관계를 맺고 있을 때 선언할 수 있습니다.

  1. 내부 클래스에서 외부 클래스의 멤버에 손쉽게 접근할 수 있게 됩니다.

  2. 서로 관련 있는 클래스를 논리적으로 묶어서 표현함으로써, 코드의 캡슐화를 증가시킵니다.

  3. 외부에서는 내부 클래스에 접근할 수 없으므로, 코드의 복잡성을 줄일 수 있습니다.

  > DTO를 여러개 만들어야 할 경우, 하나의 DTO에서 내부 클래스로 만들어 사용할 수 있습니다.
  >
  > 내부 클래스를 사용할 경우 Mapper.xml의 resultType에서 com.xxx.xxx.className과 같이 지정할 수 없습니다.
  >
  > 내부 클래스는 주로 데이터를 가공하거나, MyBatis Mapper.xml과 직접적으로 연관이 없는 경우 사용합니다.

### Member Class

  - 객체(instance) 멤버 클래스

  ```java
  public class OuterClass {
    class IntanceClass {
      int a;
      // static int b; - 불가능
      public IntanceClass(int a) {
         this.a = a;
      }
      public void IntanceMethod(){
        // 내부 코드
      }
    } // 객체 멤버 클래스
  }
  ````

  객체 멤버 클래스는 위에서 `객체멤버와 정적멤버의 차이`에서 배운 것처럼
  객체를 생성해야 내부의 클래스에 접근할 수 있습니다.

  또한, `클래스 내부에 static이 붙은 클래스 필드와, 클래스 메소드를 선언할 수 없습니다.`

  __단, 인스턴스 멤버 클래스는 외부 클래스(OuterClass)의 모든 필드와 메소드에 접근 가능합니다.__

  객체 멤버 클래스의 객체 생성방법은 아래와 같습니다.

  먼저 외부클래스의 객체를 생성하고 그 다음 내부 클래스의 변수를 생성해야 합니다.

  > OuterClass 객체변수1 = new OuterClass();
  >
  > 바깥클래스.중첩클래스 객체변수2 = 바깥클래스.new 중첩클래스();
  >
  > OuterClass.InnerClass 객체변수2 = OuterClass.new InnerClass();

  그리고 객체변수2로 InnerClass의 필드와 메소드에 접근 할 수 있습니다.

  -----------------------------------------------------------------------------

  - 정적(static) 멤버 클래스

  ```java
  public class OuterClass {
    static class StaticClass {
      int a;
      static int b;
      public StaticClass(int a) {
         this.a = a;
      }
      public void StaticMethod(){
        // 내부 코드
      }
    } // 객체 멤버 클래스
  }
  ```

  정적 멤버 클래스는 객체 생성 없이 클래스명으로 바로 접근 가능 합니다.

  또한, 객체 멤버 클래스와 달리 `모든 종류의 필드와 메소드를 선언`할 수 있습니다.

  __단, 정적 멤버 클래스는 외부 클래스(OuterClass)의 정적필드와, 메소드에만 접근 가능합니다.__

  정적 멤버의 클래스 생성 방법은 아래와 같습니다.

  외부 클래스 객체를 생성할 필요없이 바로 내부클래스의 객체만 생성하면 됩니다.

  > 바깥클래스.중첩클래스 객체변수1 = new 바깥클래스.중첩클래스();
  >
  > OuterClass.InnerClass 객체변수1 = new OuterClass.InnerClass();

  그리고 객체변수1로 InnerClass의 필드와 메소드에 접근 할 수 있습니다.


  -----------------------------------------------------------------------------

### Local Class

  로컬 클래스는 메소드 내부에 선언된 클래스를 뜻합니다.

  주로 스레드 객체를 만들때 사용합니다.

  로컬 클래스는 [접근제한자(Access Modifier)](https://baekjungho.github.io/java-inheritance/#access-modifier)를 붙이지 않습니다.

  또한 `static`도 사용할 수 없습니다. 따라서 생성자, 객체필드, 객체 메소드만 사용가능 합니다.

  __단, 로컬 클래스는 외부 클래스(OuterClass)의 필드와 메소드를 마음껏 사용할 수 있습니다.__

  중요한 점은 로컬클래스 내에서 메소드 내의 지역변수와 매개변수를 사용할때인데 자바 8이전에는
  `final`을 붙여서 값이 수정되는 일을 막았습니다. 왜냐하면 컴파일 시에 매개변수와 지역변수가
  로컬클래스 내부에 복사되어 사용되어지기 때문입니다.

  __하지만 자바 8부터는 final을 붙이지 않아도 기본적으로 final특성을 가지고 있습니다.__

  단, final을 붙이고 안 붙이고의 차이는 `로컬클래스`내에서 지역변수와, 매개변수의 복사위치가
  달라진다는 점입니다.

  1. final을 붙였을때 : 로컬클래스에 있는 메소드 내부에 지역변수 형태로 복사됨
  2. fianl을 붙이지 않았을때 : 로컬클래스의 필드로 복사됨

  로컬 클래스는 `메소드가 실행 될때 메소드 내에서 객체를 생성하여 사용`합니다.

  ```java
  public class OuterClass {
    public void method() {
      class LocalClass {
        int loc;
        LocalClass() { ..... }
      }
      LocalClass lc = new LocalClass();
      lc.loc = 100;
    }
  }
  ```

  -----------------------------------------------------------------------------

### Architecture

  ```java
  public class OutterClass {

	String field1 = "dope";
	int field2;

	static int field3;

	public void method1(final int arg1, String arg2) {
		final int method1Field1 = 10;
		String methodField2 = null;

		// Local-Member-Class
		class LocalMemberClass {
			int sum = 0;
			// 컴파일시에 이 위치에 method1Field2이 복사됨
			// 컴파일시에 이 위치에 arg2가 복사됨
			String localField1;
			public void localMethod() {
				sum = arg1 + method1Field1;
				System.out.println(sum + "&" + arg2);
				// 컴파일시에 이 위치에 methodField1이 복사됨
				// 컴파일시에 이 위치에 arg1이 복사됨
			}
		}
		// 로컬멤버클래스 객체 생성
		LocalMemberClass lClass = new LocalMemberClass();
		lClass.localMethod();
		System.out.println(arg1 +","+ arg2);
	}

	static public void method2() { }

	// Instance-Member-Class
	class InstanceMemberClass {
		// Instance-Member-Class  내부에선 OuterClass의 모든 필드와 메소드에 접근 가능

		String instanceField1 = "pill";
		int intanceField2;
		// static int instanceField3; (X)
		// static public void instanceMethod() { } (X)
		public void instanceMethod1() { }
		public void instanceMethod2() {
			System.out.println(this.instanceField1); // 중첩객체필드참조
			this.instanceMethod1(); // 중첩객체메소드참조

			System.out.println(OutterClass.this.field1); // 바깥클래스필드참조
			OutterClass.this.method1(10, "Mike"); // 바깥클래스메소드 참조
		}
	}

	// Static-Member-Class
	static class StaticMemberClass {
		// Static-Member-Class 내부에서는 OuterClass의 static이 붙은 필드와 메소드만 접근가능

		static int staticField1;
		int staticField2;

		// field1은 클래스 필드가 아니므로 접근 불가능
		// int staticField3 = OutterClass.this.field1; (X)

		static public void staticMethod1() { }
		public void staticMethod2() { }
	 }
  }

  public class NestedClassTest {
	public static void main(String[] args) {
		OutterClass outter = new OutterClass();
		OutterClass.InstanceMemberClass iClass = outter.new InstanceMemberClass();

		OutterClass.StaticMemberClass sClass = new OutterClass.StaticMemberClass();

		outter.method1(8, "john");
		iClass.instanceMethod2();
	 }
  }
  ```

  -----------------------------------------------------------------------------

### 바깥클래스 참조

  바깥클래스(OutterClass)의 필드와 메소드를 내부클래스(NestedClass)에서 참조하는 방법은 아래와 같습니다.

  > OutterClassName.this.FieldName;
  >
  > OutterClassName.this.MethodName;

  참조할때 주의할 점은 static member class의 경우에는 바깥 클래스의 필드나 메소드 중에서
  static이 붙은 클래스 필드, 클래스 메소드에만 접근 가능하므로, 각 중첩클래스의 특징을 잘 기억하여
  접근권한이 어디까지 되는지 기억하여 사용하는게 중요합니다.

### 익명클래스

  익명클래스는 내부에 특정한 기능을 담당하는 메소드를 만들고 그 메소드에는 매개변수 타입과 이름이 있습니다.

  그 매개변수에 `new 클래스명()` 과 같이 [익명구현객체](https://baekjungho.github.io/java-interface/#%EC%9D%B5%EB%AA%85%EA%B5%AC%ED%98%84%EA%B0%9D%EC%B2%B4)를 넘기게 만드는것이 익명 클래스 입니다.

  주로 UI 프로그래밍에서 자주 사용됩니다.

  > 이것이 자바다(한빛미디어, 신용권) - 익명구현클래스 예제

  ```java
  public class Button {
	OnClickListener listener; // 인터페이스 변수 선언

	// 매개변수의 다형성
	// 매개변수로 객체를 받아 생성
	void setOnClickListener(OnClickListener listener) {
		this.listener = listener;
	}

	// 생성된 객체명으로 오버라이딩된 실체메소드 호출
	void touch() {
		listener.onClick();
	}

	// 인터페이스에 추상메소드 선언
	interface OnClickListener {
		void onClick();
	 }
  }

  public class Window {
    Button button1 = new Button();
    Button button2 = new Button();

    // 필드 초기값으로 대입
    Button.OnClickListener listener = new Button.OnClickListener() {
      @Override
      public void onClick() {
        System.out.println("전화를 겁니다");
      }
    };

    // Constructor
    Window() {
      // listener 객체 대입
      button1.setOnClickListener(listener);

      // 매개변수에 익명구현객체 대입
      button2.setOnClickListener(new Button.OnClickListener() {
        @Override
        public void onClick(){
          System.out.println("메시지를 보냅니다");
        }
      });
    }
  }

  public class Main {
    public static void main(String[] args) {
      Window w = new Window();
      w.button1.touch();
      w.button2.touch();
    }
  }
  ```

## Nested Interface

  중첩 인터페이스는 인터페이스를 클래스 내부에 선언하고 인터페이스 변수도 클래스 내부에 선언하는 것을 말합니다.
  인터페이스를 클래스 내부에 선언하면 클래스와 `긴밀한 관계`가 형성됩니다.

  클래스 내부에 선언된 인터페이스에 접근하는 방법은 `클래스명.인터페이스명`으로 접근합니다.

  > ClassName.InterfaceName 변수명;
  >
  > 변수명 = new 구현클래스명();

  주로 UI 프로그래밍에서 자주 사용됩니다.

  > 이것이 자바다(한빛미디어, 신용권) - 중첩 인터페이스 예제

  ```java
  public class Button {
	OnClickListener listener; // 인터페이스 변수 선언

	// 매개변수의 다형성
	// 매개변수로 객체를 받아 생성
	void setOnClickListener(OnClickListener listener) {
		this.listener = listener;
	}

	// 생성된 객체명으로 오버라이딩된 실체메소드 호출
	void touch() {
		listener.onClick();
	}

	// 인터페이스에 추상메소드 선언
	interface OnClickListener {
		void onClick();
	 }
  }

  public class CallListener implements Button.OnClickListener {
	@Override
	public void onClick() {
		System.out.println("전화를 겁니다");
	 }
  }

  public class MessageListener implements Button.OnClickListener {
	@Override
	public void onClick() {
		System.out.println("메시지를 보냅니다");
   }
  }

  public class ButtonExample {
	public static void main(String[] args) {
		Button btn = new Button();

		btn.setOnClickListener(new CallListener());
		btn.touch();
		btn.setOnClickListener(new MessageListener());
		btn.touch();
	 }
  }
  ```
