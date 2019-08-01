---
title: "Static Block, Instance Block"
layout: post
category: Java
tags: [Java]
excerpt: "Static Block, Instance Block에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Static Block과 Instance Block에 대해서 배워 봅시다.

## Static Block

  객체에 생성자가 있다면, 클래스에는 static 블록이 있습니다.

  객체의 생성자를 이용하는 경우는, new 연산자를 통해서 객체를 생성하면서 초기화를 해줍니다.

  클래스 생성자는 존재하지 않지만, 클래스가 static 영역에 배치 될 때 실행되는 코드 블록이 있습니다. 이것이 바로 static 블록 입니다.

```java
public class Animal {
	static {
	   System.out.println("동물 클래스 Ready On");
	}
}
```

```java
public class TestAnimal {
	public static void main(String[] args) {
		System.out.println("main 메서드 시작");
	}
}
```

  위 코드를 실행하면 `main 메서드 시작` 문구만 나옵니다. 왜냐하면 Animal 클래스를 사용하는 코드가 없기 때문에
	Animal 클래스의 static 블록을 실행하지 않습니다. 그리고 static 블록에서 사용할 수 있는 속성과 메서드는 static 멤버 뿐입니다.

	static 블록을 실행시키기 위해서는 다음과 같이 해야합니다.

```java
public class Animal {
  static {
    System.out.println("동물 클래스 Ready On");
  }
}
```

```java
public class TestAnimal {
  public static void main(String[] args) {
    System.out.println("main 메서드 시작");
    Animal puppy = new Animal();
  }
}
```

  출력 결과는 main 메서드, static 블록 순서로 출력이 됩니다.

  main 메서드에 동물 클래스를 여러개 만들어도 static 블록은 단 한번만 실행 됩니다.

  클래스의 객체를 만들지 않고도 static 블록을 실행 시킬 수 있는 방법은 static 속성이나 static 메서드를 호출하는 경우 입니다.

```java
public class Animal {
	static int age = 0;

	static {
		System.out.println("동물 클래스 Ready On");
	}
	static void animalMethod() {}
}
```

```java
public class TestAnimal {
	public static void main(String[] args) {
		System.out.println("main 메서드 시작");
		Animal puppy = new Animal(); // static 블록 실행
		System.out.println(Animal.age); // static 블록 실행
		Animal.animalMethod(); // static 블록 실행
	}
}
```

  즉, 클래스 정보는해당 클래스가 코드에서 맨 처음 사용될 때 T메모리의 스태틱 영역에 로딩되며, 이때 단 한번 해당 클래스의 static 블록이 실행됩니다. 여기서 클래스가 제일 처음 사용되는 경우는 다음 3가지중 하나 입니다.

  1. 클래스의 static 속성을 사용할 때
  2. 클래스의 static 메서드를 사용할 때
  3. 클래스의 객체를 최초로 생성할 때


  프로그램이 실행될 때 바로 클래스 정보를 T메모리의 static영역에 배치하지 않고, 클래스가 처음 사용될 때 로딩하는 이유는 static 영역도 메모리이기 때문입니다.
  __메모리는 최대한 늦게 사용을 시작하고, 최대한 빨리 반환하는 것이 정석입니다.__ 자바에서는 static 영역에 한번 올라가면 프로그램이 종료되기 전까지는 해당 메모리를 반환할 수 없습니다.

  클래스는 여러 개의 정적 초기화 블록을 가질 수 있으며 클래스 본문의 어느 위치 에나 나타날 수 있습니다. 런타임 시스템은 정적 초기화 블록이 소스 코드에 나타나는 순서대로 호출되도록 보장합니다.
  정적 블록에 대한 대안이 있습니다. private static method를 작성할 수 있습니다.


## 인스턴스 블록

  static 블록과 유사하게 클래스의 객체를 위한 인스턴스 블록도 존재합니다. 아무런 표시 없이 `{ }` 블록을 사용하면, 객체가 생성 될 때마다 { } 블록이 실행됩니다. { } 블록은 객체 생성자가 실행되기 전에 먼저 실행됩니다.

  Initializing Instance Members

  http://docs.oracle.com/javase/tutorial/java/javaOO/initial.html

  일반적으로 생성자에 인스턴스 변수를 초기화하는 코드를 넣습니다. 인스턴스 변수를 초기화하기 위해 생성자를 사용하는 방법에는 초기화 프로그램 블록과 최종 메소드의 두 가지 방법이 있습니다.

  인스턴스 변수의 초기화 블록은 정적 초기화 프로그램 블록처럼 보이지만 static키워드는 없습니다.

  ```java
  {
    	// 초기화에 필요한 모든 코드가 여기에 옵니다.
  }
  ```

  Java 컴파일러는 이니셜 라이저 블록을 모든 생성자에 복사합니다. 따라서이 방법을 사용하여 여러 생성자간에 코드 블록을 공유 할 수 있습니다.
