---
title: "static, this, final"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Array(배열)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## static

  static은 정적이라는 뜻으로 static이 붙은 메소드나 필드는 `클래스 명`으로 접근해서 사용 가능합니다.

  > 클래스명.필드(멤버변수);
  >
  > 클래스명.메소드(멤버함수);

  우리가 위에서 객체를 생성해서 `객체명.메소드`, `객체명.필드`로 접근해서 사용한 것은
  외부클래스에서 객체가 있는 클래스에 접근하기 위해 사용되는 방식입니다.

  따라서 객체명으로 접근하는방식은 `외부 호출`입니다.

  `내부 호출`방식은, 자기 내부클래스에서 메소드를 호출하기 위해서는 그냥 메소드 이름만
  작성해주시면됩니다.

  > static 키워드가 붙은 필드나 메소드는 `클래스 멤버`라고 부릅니다.

  즉, static 필드나, static 메소드는 객체멤버가 아니라 클래스에 속한 멤버입니다.

  static이 붙은 필드나 메소드는 객체이름으로 접근하는것이아니라 클래스 이름으로 접근합니다.

  따라서 __static method를 클래스 메소드__ 라고 부르기도 합니다.

  -----------------------------------------------------------------------------

### 언제 static을 사용할까 ?

  static 키워드는 __클래스의 모든 객체들이 같은 값을 가질때 사용__ 하는것이 정석입니다. 그 외의 경우에도 사용할 수 있지만, 정당한 이유가 있어야 합니다.

  클래스 멤버로 둘 건지 객체 멤버로 둘건지의 판단 여부는 아래와 같습니다.

  > 각 객체마다 특징이 달라서 속성(필드)과, 메소드를 다르게 구현해야하면 객체멤버
  >
  > 공통된 속성(필드)과 메소드라고 판단되면 static을 붙여서 클래스 멤버로 구현

  - 특징

  > static으로 선언한 필드들은 `공통된 속성`이므로 초기값을 지정해주는 편이 좋습니다.
  >
  > 하지만 계산이 필요한 초기화 작업이 있을 경우 `static block`을 사용합니다.
  >
  > 클래스 멤버들은 클래스가 메모리에 로딩이 끝날때 바로 사용 가능합니다.

  -----------------------------------------------------------------------------

  - 예제

  ```java
  public class StaticInstance {
    String name; // 객체 필드
    String gender; // 객체 필드
    static String racial = "백인"; // 클래스 필드, 초기값 지정

    static void eat() { ..... } // 클래스 메소드 = 정적 메소드
    void sleep() { ..... } // 객체 메소드
   }
  ```

### main() 메서드가 static 인이유

  T 메모리가 초기화된 순간 객체는 하나도 존재하지 않기 때문에 객체 멤버 메서드를 바로 실행할 수 없습니다. 따라서 main() 메서드를 정적 메서드로 두는 것입니다.

### static block

  정적 블록은 클래스가 메모리로 로딩될때 자동으로 실행됩니다.[(Load time dynamic loading)](https://baekjungho.github.io/jvm-run/)

  ```java
  public class Person {
    static String name = "홍길동";
    static String city = "대전";
    static String info;

    int age; // 객체 필드

    void work() { ..... } // 객체 메소드

    // static block
    static{
      age = 26; // ERROR 객체 필드를 정적 블록내에 사용할 수 없음
      work(); // ERROR 객체 메소드를 정적 블록 내에 사용할 수 없음

      // 만약 객체 멤버를 사용하고 싶으면
      // static block 내에 객체를 생성하여 객체명.멤버로 접근
      Person mike = new Person();
      mike.age = 26;
      info = name + "-" city;
    }
  }
  ```

  우리는 이미 자바를 배우면서 Hello World를 출력할때 static 메소드를 접했습니다.

  바로 `main()메소드` 즉 실행을 담당하는 메소드 입니다.

  ```java
  public PersonExample {
    String name; // 객체 필드
    String city; // 객체 필드

    public static void main(String[] args){
      name = "mike"; // ERROR

      // CORRECT
      PersonExample user1 = new PersonExample();
      user1.name = "mike";
    }
  }
  ```

## this

  this 키워드는 객체 자신의 속성을 나타낼때 사용됩니다. 즉, 인스턴스 멤버에 접근할때 사용.

  또한, 같은 클래스에 오버로딩된 다른 생성자를 호출할 때도 사용됩니다.

  또한, 메소드를 호출할 때도 사용 됩니다.

  > this(arg1, arg2 ....) // 생성자 호출
  >
  > this.필드명 // 멤버변수 접근
  >
  > this.메소드명 // 메소드에 접근


  ```java
  public class Person {

	String name = "홍길동";
	String gender;
	String racial;

	// defualt Constructor
	Person() {

	}

	Person(String name){
		this.name = name;
	}

	Person(String name, String gender){
		this(name, gender, null);
	}

	public Person(String name, String gender, String racial) {
		this.name = name;
		this.gender = gender;
		this.racial = racial;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", gender=" + gender + ", racial=" + racial + "]";
	 }
  }
  ```

  ```java
  public class PersonExample {
	public static void main(String[] args) {
		Person p1 = new Person();
		System.out.println(p1.toString());
		System.out.println();

		Person p2 = new Person("mike");
		System.out.println(p2.toString());
		System.out.println();

		Person p3 = new Person("mike", "남자");
		System.out.println(p3.toString());
		System.out.println();

		Person p4 = new Person("john", "남자", "흑인");
		System.out.println(p4.toString());
		System.out.println();
	 }
  }
  ```

  위 예제에서 생성자 매개변수이름과 필드이름을 같게하여 this키워드를 사용하는 이유는
  이 방법이 자바의 Default입니다.

## final

  final로 선언된 필드는 값이 정해지면 절대 바꿀 수 없습니다. 즉, final은 마지막, 최종이라는 의미를 가지는 단어이며, final 키워드가 나타날 수 있는 곳은 클래스, 변수, 메서드 뿐입니다.

  우리가 PI와 같은 [상수](https://baekjungho.github.io/java-operator/)를 선언할 때 static final double PI = 3.14;와 같이 선언하는데
  이 값은 고정되어있는 값이며 바꿀수 없다라는 뜻입니다.

  주로 final은 값이 변하지 않는 `주민등록번호나 학번`과 같은 곳에 사용됩니다.

  ```java
  public class Student {
    final String jumin;
    final String hakbun;

    public student(String jumin, String hakbun){
      this.jumin = jumin;
      this.hakbun = hakbun;
    }
  }

  public class StudentTest {
    public static void main(String[] args){
      Student mike = new Student("940502-1111111", "201363240");
      mike.jumin = "940502-1212121" // ERROR
    }
  }
  ```

  - final 클래스 : 상속이 불가능함

	```java
	public final class cat {}
	```

	class 키워드 앞에 final을 붙이게 되면, extends 키워드를 사용하여 하위 클래스를 만들 수 없습니다. 만약 extends를 사용하면 다음과 같은 에러가 발생합니다.
	`The type SubClassName cannot subclass the final class SuperClassName`

	- final 변수 : 변경 불가능한 상수

	```java
	public class cat {

		final static int 정적상수 = 1;
		final static int 정적상수2;
		final int 객체상수1 = 2;
		final int 객체상수2;

		static {
			정적상수2 = 2;

			// 상수는 한 번 초기화 되면 값을 변경 할 수 없다.
			// 정적상수2 = 3;
		}

		cat {
			객체상수2 = 2;

			// 상수는 한 번 초기화 되면 값을 변경 할 수 없다.
			// 객체상수2 = 4;

			final int 지역상수 = 1;
		}

	}
	```

	- final 메서드 : 오버리이딩이 불가능함

	```java
	public class Animal {
		final void breath() {
			System.out.println("호흡 중");
		}
	}

	class Mammal extends Animal {
		// ERROR : Cannot override the final method from Animal
		void breath() {
			System.out.println("포유류가 호흡 중");
		}
	}
	```
