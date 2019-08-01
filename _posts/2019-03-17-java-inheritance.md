---
title: "캡슐화, 상속, 다형성"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 가장 중요한 특징 3가지인 캡슐화, 상속, 다형성에 대해 배워 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Encapsulation

  캡슐화는 연관된 필드와 메소드를 하나로 묶어 구현 내용을 `은닉(private)`하는 것을 말합니다.

  예를들어, 사람 속성중 주민등록번호와 같이 공개하고 싶지 않은 속성을 감추는 것을 캡슐화 라고 합니다.

  즉, 외부 객체가 내부 객체의 내부구조를 알 수 없게하고 공개된 속성과 메소드만 이용
  가능하게 만드는 것.

  캡슐화를 하는 이유는 외부객체로 부터의 잘못된 사용으로 인해 객체가 손상되지 않도록
  하는 것입니다.

  자바에서 캡슐화 하는 방식은 `접근제한자(Access Modifier)`를 사용하는 것입니다.

  > 캡슐화(Encapsulation) 방법
  >
  > 접근제한자 private, protected 사용
  >
  > 필드나, 메소드 이름 앞에 `_언더바`를 붙여서 사용합니다.
  >
  > 언더바를 붙인 메소드나, 필드에 접근하지 말라는 암묵적인 약속 입니다.

## Access Modifier

  접근 제한자의 종류는 4가지 입니다. `public, private, protected, default`

  ![access](/images/posts/201903/access.jpg)

  public : 모든 접근을 허용합니다.

  protected : 상속관계가 없는 다른 패키지 클래스를 차단합니다.

  > protected는 클래스앞에 사용이 불가능 합니다.
  >
  > 또한, 다른 패키지에 있는 생성자, 메소드, 필드는 호출하지 못하지만 상속관계에 있는 클래스라면 예외로 호출이 가능합니다.
  >
  > 즉, 다른 패키지에 있는 클래스가 protected가 있는 해당 클래스의 자식이면 사용 가능


  default(friendly) : 다른 패키지 클래스를 전부 차단합니다.

  > default는 앞에 생략되어 있습니다. 즉, 앞에 접근 제한자를 적지 않으면 default입니다.
  >
  > 다른 패키지와의 클래스 선언과 생성자/메소드/필드의 호출이 불가능해집니다
  >
  > 즉, 동일 패키지 내에서만 사용이 가능합니다.

  private : 모든 외부에 있는 클래스의 접근을 차단합니다.

  > private는 자기 클래스 내부에서만 사용이 가능하고, 동일패키지내 다른 클래스들은 사용이 불가능 합니다.

  위 접근 제한자를 적절히 사용해서 캡슐화(Encapsulation)를 구현 하는 것입니다.

  -----------------------------------------------------------------------------

  __클래스에서만 사용가능한 접근 제한자__

  > public, default

  __필드, 메소드, 생성자에서 사용가능한 접근 제한자__

  > public, default, private, protected

## Inheritance

  상속은 SuperClass(부모클래스)의 속성과 기능(메소드)를 SubClass(자식클래스)가 물려 받아서 SuperClass의
  속성(필드)과 기능(메소드)을 재사용 할 수 있게 되는 것을 말합니다.

  자바에서 부모클래스로부터 상속을 받으면 자식 클래스는 상속받은 필드와, 메소드를
  사용할수 있고, 자신만의 필드와, 메소드를 추가할 수 있습니다.

  예를 들어, 사람은 동물에 속하고 동물중에서도 포유류에 속합니다.

  포유류의 공통된 특징을 상속받아서 사람클래스에서 사용할 수 있고, 사람의 특징을
  따로 필드, 메소드를 추가하여 정의할 수 있습니다.

  유지보수성이 뛰어난 프로그램을 만들기 위해서는 `인터페이스 와 합성`, `상속과 폴리모피즘` 개념을 잘 숙지
  하고 적절히 혼합해서 사용 해야 합니다.

  -----------------------------------------------------------------------------

  - 상속을 사용해야 하는 경우

  > __IS-A(SubClass is SuperClass : 하위클래스는 부모클래스이다)__

  관계가 성립 할 때만 상속을 사용합니다. 상속을 사용하게 되면 `"Familiy" 및 "영구적"` 이라는 개념이 확립됩니다.

  이 개념을 외워둬야 `합성` 이라는 개념과 서로 혼동되지 않고 잘 적용해서 코딩 할 수 있습니다.

  -----------------------------------------------------------------------------

  - 상속 가능한 멤버

  > private로 접근제한한 필드와 메소드를 제외한 나머지
  >
  > 부모와 자식이 패키지가 다를때 default로 접근제한한 필드와 메소드를 제외한 나머지
  >
  > protected는 다른 패키지여도 상속한 경우에는 사용할 수 있습니다.

  - 상속의 장점

  상속을 하게 되면 코드의 중복을 줄여주고, 공통된 속성을 수정할때 최소화 시킬 수 있습니다.
  A클래스를 상속받은 B는, A의 코드만 수정하면 자신은 수정할 필요가 없습니다.

  - 상속의 특징

  > 다중상속은 지원하지 않습니다.
  >
  > interface는 다중으로 상속받아 구현 할 수 있습니다.
  >
  > 자바의 모든 클래스는 Object 클래스를 자동 상속 받습니다.

  - Inheritance Architecture

  > class SubClass `extends` SuperClass { ..... }

  - 상속 예제

  > 사람 <= 포유류 <= 동물

  ```java
  public class Animal {

      private String specise; // private 선언 - 상속 불가능
      String gender;
      int age;
      boolean sleep;

      public Animal(String gender, int age) {
        this.gender = gender;
        this.age = age;
      }

      public void sleep(boolean sleep) {
        this.sleep = sleep;
      }
  }

  public class Mammalia extends Animal{
  int hasLeg;

  public Mammalia(String gender, int age, int hasLeg) {
    super(gender, age); // 부모 생성자 호출
    this.hasLeg = hasLeg;
   }
  }

  public class People extends Mammalia {
  private String ssn;

  public People(String gender, int age, int hasLeg) {
    super(gender, age, hasLeg); // 부모 생성자 호출
    this.ssn = ssn;
  }

  public String getSsn() {
    return ssn;
  }

  public void setSsn(String ssn) {
    this.ssn = ssn;
  }

  @Override
  public String toString() {
    return "People [ssn=" + ssn + ", hasLeg=" + hasLeg + ",
    gender=" + gender + ", age=" + age + ", sleep=" + sleep + "]";
   }
  }

  public class SpeciseTest {
  public static void main(String[] args) {
    People people = new People("Male", 26, 2);
    System.out.println(people.toString());

    people.sleep(true);
    people.setSsn("940502");
    // people.specise = "사람"; // ERROR
    System.out.println(people.sleep);
    System.out.println(people.getSsn());
    }
  }
  ```

  위 Animal에서 private로 지정한 필드는 자식클래스로 상속이 불가능하여 사용할 수 없습니다.

  주의 깊게 볼 것이 `super()`변수인데 자세한건 아래에 설명하겠습니다.

  부모한테 상속받은 필드와 메소드들은 객체를 생성하여 `객체.필드명`, `객체.메소드명`으로
  접근 가능합니다.

### final클래스, final메소드

  [final](https://baekjungho.github.io/java-static/)로 선언한 클래스와 메소드는
  상속도 불가능하고 메소드 오버라이딩도 불가능합니다.

  왜냐하면 final은  `최종`이라는 의미를 지니고 있기 때문입니다.

### super

  우리가 배운 [this](https://baekjungho.github.io/java-static/)는 `자기자신`을 나타냅니다.

  반면에 이번에 배울 `super`변수는 `부모`를 나타냅니다.

  - 부모 생성자 호출

  super 변수는 상속받은 자식클래스의 생성자에서 생성자 첫 줄(First Line)에 적어줍니다.

  super(매개변수1, 매개변수2) 형식으로 부모클래스의 생성자 매개변수에 맞게 적어 사용합니다.

  만약에 자식 클래스에서 생성자가 default일 경우 메인메소드에서 객체생성시 자식클래스의
  생성자 안에는 `super()`가 자동으로 생깁니다.

  __왜냐하면 메인메소드에서 자식객체를 생성할때, 사실은 부모객체가 먼저생성되고 자식객체가 생성됩니다.__

  따라서 메인메소드에서 자식객체를 생성하면, 자식객체는 초기화됬지만, 부모객체는 아직 초기화가 안됬으므로
  초기화를 시켜주기 위하여 `super`변수를 사용하여 부모생성자를 불러와서 초기화를 시켜주는 것입니다.

  - 부모 메소드 호출

  super 변수의 역할은 부모 메소드를 호출 할 수도 있습니다.

  자식이 부모로부터 메소드오버라이딩하여 재정의 하면, 메소드 호출시 재정의된 자식의 메소드만
  호출되는데 만약 재정의 되지않은 부모의 메소드를 호출하고 싶을때는 `super.메소드명()`으로 호출합니다.

  부모 메소드 호출시에는 생성자 호출과 다르게 굳이 First Line에 적을 필요가 없습니다.

  -----------------------------------------------------------------------------

  > super변수의 의미는 부모 생성자와 부모 메소드를 호출 하는 역할 입니다.
  >
  > 부모생성자의 매개변수에 맞게 super()변수안에 알맞은 매개변수를 넣어야합니다.
  >
  > 즉, 자식생성자의 `First Line`에 있는 super()변수 안에는 부모생성자의 매개변수 개수, 타입이 같아야합니다.

  - super Architecture

  ```java
  public SuperClass {
    String fieldName1;
    String fieldName2;

    public SuperClass(String fieldName1, String fieldName2){
      this.fieldName1 = fieldName1;
      this.fieldName2 = fieldName2;
    }
  }

  public SubClass extends SuperClass {
    String subFieldName1;

    public SubClass(String fieldName1, String fieldName2, String subFieldName1){
      super(fieldName1, fieldName2);
      this.subFieldName1 = subFieldName1;
    }
  }

  public class InheritanceExample {
    SubClass subclass = new SubClass("mike", "john", "ellis");
    System.out.println(subclass.fieldName1 + "+" + subclass.fieldName2);
    System.out.println(subclass.subFieldName1);
  }
  ```

## Polymorphism

  다형성은 동일한 타입의 여러 객체에게 동일한 명령을 내렸을때 각자 다르게 반응하는 현상을 말한다.

  위에서 강아지, 고양이 예시를 다시 가져오겠습니다.

  강아지와 고양이의 동작(메소드)는 짖다가 있는데 각자 짖는 소리가 다릅니다.

  강아지는 멍멍, 고양이는 야옹 이렇게 서로 다르게 반응하는것이 다형성 입니다.

  - 종류

  → 추상클래스로 폴리모피즘 구현

  → 인터페이스로 폴리모피즘 구현

  → [메소드 오버로딩](http://tcpschool.com/java/java_inheritance_overriding)

  → [메소드 오버라이딩](https://ko.wikipedia.org/wiki/%EB%A9%94%EC%86%8C%EB%93%9C_%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%94%A9)

  -----------------------------------------------------------------------------

### Method Overloding

  메소드 오버로딩은 부모로 부터 상속받은 메소드를 매개변수와, 매개변수의 타입을
  다르게하여, 사용하는 것.

  메소드와, 생성자 둘다 가능 합니다.

  > 오버로딩과 오버라이딩을 구분하는 방법
  >
  > 매개변수의 개수, 타입, 리턴타입이 다르면 오버로딩 같으면 오버라이딩

  ```java
  public class Person {
    String name;
    int age;

    // Constructor Overloding
    Person() { } // Defalut Constructor
    Person(String name){
      this.name = name;
    }
    Person(String name, int age){
      this.name = name;
      this.age = age;
    }

    // Method Overloding
    public int nextAge(){
      return ++age;
    }

    public int nextAge(int age){
      this.age = age;
      return ++age;
    }
  }
  ```

### Method Overriding

  [Overloding](https://baekjungho.github.io/java-inheritance)은 두가지가 있었습니다.

  Constructor Overloding(생성자 오버로딩), Method Overloding(메소드 오버로딩)

  즉, 메소드 오버로딩은 상속받지 않아도 사용이 가능했습니다.

  하지만 오버라이딩은 자식클래스에서 부모로부터 `상속`받은 메소드를 `메소드 재정의` 할때만 사용합니다.
  [메소드 오버라이딩](https://baekjungho.github.io/java-inheritance)은 부모로 부터 상속받은 메소드를 같은 이름의 메소드로
  내부의 내용물을 따로 추가하거나 조정해서 사용하는 것입니다.

  즉 메소드이름과 매개변수의 개수, 리턴타입이 같습니다.

  > Method Overriding = 메소드 재정의
  >
  > [@Override](https://baekjungho.github.io/java/java-basic2#28)라는 어노테이션을 붙여야 합니다.
  >
  > 메소드가 제대로 재정의 되었는지(Method Override)되었는지 확인하는 어노테이션입니다.

  보통 부모클래스에서 메소드를 제대로 정의해서 자식클래스에서도 재정의가 필요 없게 사용되면
  좋지만, 그렇지 않은경우가 있기때문에 메소드 오버라이딩이 존재 하는 것입니다.

  -----------------------------------------------------------------------------

  - 메소드 오버라이딩 특징

  > 자식클래스에서 부모로부터 상속받은 메소드를 재정의
  >
  > 메소드 위에 @Override 어노테이션을 적어준다.
  >
  > `Method Signature`가 부모와 자식이 동일해야한다.
  >
  > 즉, 메소드명, 매개변수 개수, 타입, 반환값이 같아야 한다는 의미입니다.
  >
  > 부모클래스의 Access Modifier(접근제한자)보다 더 협소한 접근제한자를 사용 할 수 없습니다.

  즉, 부모 접근제한자가 Public일 경우 자식은 default나 private로 접근제한 할 수 없습니다.

  -----------------------------------------------------------------------------

  - Eclipse Tool Method Overriding

  > 자식클래스에서 메소드 오버라이딩 할 위치로 커서 이동
  >
  > 오른쪽 마우스 클릭 - Source - Override/Implement Methods 클릭
  >
  > 부모클래스에서 오버라이딩될 메소드 선택

  -----------------------------------------------------------------------------

### 매개변수로 다형성 구현

  매개변수에 타입을 클래스로 두고, 객체를 받으면 다형성을 구현할 수 있습니다.

  ```java
  public class Phone {
    public void run(){
      System.out.println("핸드폰이 동작합니다");
    }
  }

  // 매개변수 타입을 클래스(Phone)로 지정하고
  // 매개변수를 객체로 받는 메소드를 생성
  // iphone 객체를 받으면 iphone.run()을 통해서 IPhone 클래스의 run()메소드 실행
  public class On {
    public void onSwitch(Phone phone){
      phone.run();
    }
  }

  public class IPhone extends Phone {
    @Override
    public void run() {
      System.out.println("아이폰이 동작합니다");
    }
  }

  public class Android extends Phone {
    @Override
    public void run() {
      System.out.println("안드로이드가 동작합니다");
    }
  }

  public class PhoneExample {
    public static void main(String[] args) {
      On on = new On();
      IPhone iphone = new IPhone();
      Android android = new Android();

      on.onSwitch(android);
      on.onSwitch(iphone);
    }
  }
  ```

  > Reference : [https://hunit.tistory.com/162](https://hunit.tistory.com/162)
