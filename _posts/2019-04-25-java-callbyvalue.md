---
title: "Call by value vs Call by reference"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Call by value vs Call by reference에 대해 배워 봅시다. "
author: BAEKJungHo
---

* content
{:toc}

## CBV & CBR

  자바는 `Call by value` (값에 의한 호출) 방식입니다.
  Call by Value는 메소드 호출하는측의 원본은 변화가 없습니다.
  자바에선 일반적인 primitive 변수를 매개변수로 주고 받을때 Call by value 가 발생합니다.

  Wikipedia에는 다음과 같이 설명이 나와 있습니다.

  > Call-by-value
    In call-by-value, the argument expression is evaluated, and the resulting value is bound to the corresponding variable in the function (frequently by copying the value into a new memory region). If the function or procedure is able to assign values to its parameters, only its local copy is assigned — that is, anything passed into a function call is unchanged in the caller's scope when the function returns.

  `Call by reference`는 (참조에 의한 호출) 이며 메소드 호출하는측의 원본에 영향을 받습니다. (원본값의 변화 발생)
  자바에선 일반적인 reference 변수를 주고 받을때 Call by value 가 발생합니다.
  자바의 대표적인 reference 변수 2가지 : `배열, 클래스`

  > Call-by-reference
    In call-by-reference evaluation(also referred to as pass-by-reference), a function receives an implicit reference to a variable used as argument, rather than a copy of its value. This typically means that the function can modify (i.e. assign to) the variable used as argument—something that will be seen by its caller.

  call-by-value 매커니즘은 함수로 인자를 전달할 때 전달 될 argument expression의 결과(actual-parameter)를 대응되는 함수의 변수(formal-parameter)로 복사하며, 복사된 값은 함수내에서 지역적으로 사용되는 `local value`라는 특징을 가지고 있습니다. 그리고 caller는 인자값을 복사 방식으로 넘겨 주었으므로, callee 내에서 어떤 작업을 하더라도 caller는 영향을 받지 않습니다.

  다음 예제를 통해 확인해 봅시다.

  - __Person.java__

  ```java
  public class Person {

	private String name;
	String str;

	public Person(String name, String test) {
		this.name = name;
		str = test;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getStr() {
		return str;
	}

	public void setStr(String test) {
		str = test;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", str=" + str + "]";
	 }
  }
  ```

  - __Main__

  ```java
  public class CallbyValue {
  	public static void main(String[] args) {
  		// Person 객체
  		// getter()와 setter()를 통한 Call by reference 흉내내기
  		Person name = new Person("John", "Test");
  		System.out.println(name.getName()); // John
  		name.setName("Mike");
  		System.out.println(name.getName()); // Mike

  		// Call by value
  		name.setStr("RED");
  		System.out.println(name.getStr());

  		person(name);
  		System.out.println(name.toString());

  		// String 객체
  		String ellis = new String("Ellis");
  		String nick = new String("Nick");
  		System.out.println(ellis + nick); // EllisNick

  		// swap 호출
  		swap(ellis, nick); // NickEllis
  		System.out.println(ellis + nick); // EllisNick

  		// String에 문자열 넣기
  		String rochelle = "Rochelle";
  		String coach = "Coach";
  		System.out.println(rochelle + coach); // RochelleCoach

  		// swap 호출
  		swap(rochelle, coach); // CoachRochelle
  		System.out.println(rochelle + coach); // RochelleCoach
  	}

  	static void swap(String a, String b) {
  		String temp = a;
  		a = b;
  		b = temp;
  		System.out.println(a+b); // 메소드 내부에서는 값이 바뀌지만 메소드를 벗어나면 소용 없다.
  	}

  	static void person(Person p) {
  		p = new Person("Bob", "Kiz"); // Bob, Kiz라는 새로운 객체가 생성
  		System.out.println(p.toString()); // 메소드 내부에서는 값이 변경
  	}
  }
  ```

  다음 프로그램을 보면 String 객체를 생성해서 swap() 메소드 인자값에 값을 넣으면
  인자는 그 객체에 대한 주소에 대한 값을 복사하여 인자에 넘겨주고 메소드 내부에서는
  그 값을 이용해서 swap을 하는 것입니다.(call by value)

  > 복사된 값은 함수내에서 지역적으로 사용되는 local value 라는 특징을 지닙니다.

  그래서 메소드 내부에서는 swap이 이루어지지만 __메소드 밖을 나가게 되면 사실상 주소가
  바뀐 것이 아니므로__ 똑같이 출력되는 것입니다.

  Person에서도 객체를 만들어 person()메소드를 호출하여 인자값으로 넘겨주어도 person() 메소드 내부에서만
  값 변경이 이루어지고 main()에 있는 Person객체는 값이 바뀌지 않습니다.

  대신 call by reference를 흉내내는 방법이 있는데 [getter()와 setter()](https://baekjungho.github.io/java-constructor/#getter%EC%99%80-setter)를 이용하면 값을 바꿀 수 있습니다.

  > 즉 메소드에서 변수의 주소를 받았다 하더라도 실제 메소드 안에서 실행될때는 지역변수일 뿐입니다. 즉, 메소드를 벗어나게 되면 영향력이 없어지는 것입니다. 그래서 영향력이 있을때인 메소드 안에서 직접 그 주소의 값을 변경하면 영향을 받지만, 그런것이 아닌 단순한 주소의 복사는, 그 메소드 안에있는 지역변수의 변화일 뿐 실제 값에는 영향이 없습니다.

  다음 배열 정렬에 대한 예를 보면서 더 이해해 봅시다.

  ```java
  public interface MySort {
  public abstract String[] sort(String[] strArray);
  public abstract String[] sort(String[] strArray, boolean descOrder);
  }

  public class MySortImpl implements MySort {

  private String[] sort = null; // 옮겨 담을 배열
  private String temp; // 임시 저장소

  @Override
  public String[] sort(String[] strArray) {
    // 배열 크기 설정
    sort = new String[strArray.length];

    // 배열 복사
    for(int i=0; i<strArray.length; i++) {
      sort[i] = strArray[i];
    }

    // 오름차순 정렬
        for (int i=0; i<sort.length-1; i++) {
            for (int k=0; k<sort.length-i-1; k++) {
                if ((sort[k].compareTo(sort[k+1])) > 0) {
                    temp = this.sort[k];
                    sort[k] = sort[k+1];
                    sort[k+1] = temp;
                }
            }
        }   
        return this.sort;

  }

  @Override
  public String[] sort(String[] strArray, boolean descOrder) {
    // 배열 크기 설정
    sort = new String[strArray.length];

    // 배열 복사
    for(int i=0; i<strArray.length; i++) {
      sort[i] = strArray[i];
    }

    // 내림차순 정렬
    if(descOrder) {
          for (int i=0; i<sort.length-1; i++) {
              for (int k=0; k<sort.length-i-1; k++) {
                  if ((sort[k].compareTo(sort[k+1])) < 0) {
                      temp = this.sort[k];
                      sort[k] = sort[k+1];
                      sort[k+1] = temp;
                  }
              }
          }   
          return this.sort;
      // 오름차순 정렬
    } else if(!descOrder) {
          for (int i=0; i<strArray.length-1; i++) {
              for (int k=0; k<strArray.length-i-1; k++) {
                  if ((strArray[k].compareTo(strArray[k+1])) > 0) {
                      strArray[k] = this.sort[i];
                      strArray[k] = strArray[k+1];
                      this.sort[i] = strArray[k+1];
                  }
              }
          }   
          return this.sort;
    }
    return null;
   }
  }

  public class MySortApp {
  public static void main(String[] args) {
    MySort mysort = null;
    mysort = new MySortImpl();

    String[] array = new String[] {"김길동", "박길동", "이길동", "홍길동", "정길동"};

    for(String osc : mysort.sort(array)) {
      System.out.print(osc + " ");
    }

    System.out.println();

    for(String desc : mysort.sort(array, true)) {
      System.out.print(desc + " ");
    }
   }
  }
  ```

  위 배열에서 배열 복사를 하지 않고 매개변수로 바로 정렬을 실행할 경우
  원본 배열도 같이 바뀌게 됩니다. 그 이유는 배열의 주소가 바뀌게 되기 때문입니다.
  배열과 클래스로 인자값으로 넘겨서 값 변경을 할때 `call-by-reference`가 일어나게 됩니다.

 __즉, 자바에서 메소드 호출시 기본형(primitive type)을 넘겨주면 Call by Value 이며,
  reference 변수를 메소드로 주고 받을때는 Call by Reference가 발생합니다.__
