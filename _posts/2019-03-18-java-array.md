---
title: "Array"
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

## 배열(Array)

  자바에서 배열은 객체로 취급하며, [Array](https://baekjungho.github.io/java-array/)에서 배운 것 처럼, 참조타입에 속합니다.

  배열은 한 번 생성하면 크기 변경이 불가능 합니다.

  또한 __불특정 다수의 객체를 저장하기 어렵습니다.__ 이런 문제점을 개선한 것이 [List 컬렉션](https://baekjungho.github.io/java-collection/)입니다.

### 배열 생성 방법

  - 값을 지정하여 생성

  > int[] number = {1, 2, 3, 4, 5};
  >
  > String [] names = {"Java", "HTML"};

  배열은 `인덱스+데이터`로 구성되어있습니다. 생성한 값들에는 각자 인덱스(index)가
  부여되는데, 위에서 첫 번째 데이터인 1과 Java는 0번째 인덱스를 가지고 2와,
  HTML은 1번째 인덱스가 부여됩니다.

  number[0]의 값은 1, number[1]의 값은 2가 됩니다.

  names[0]의 값은 Java, names[1]의 값은 HTML이 됩니다.

  __즉, 인덱스의 시작은 0 부터 시작됩니다.__

  - new 연산자로 생성

  > int[] number = new int[10];
  >
  > 위 처럼 값을 넣지않고 배열의 크기만 지정하여 생성할 경우
  >
  > 데이터 타입이 정수형 이므로 기본 값이 0으로 초기화 됩니다.
  >
  > 실수형 일 경우 0.0으로 초기화 되고, 논리형일 경우 false
  >
  > 참조타입일 경우는 null값으로 초기화 됩니다.

  - 배열의 값을 나중에 지정하는 방법(new 연산자)

  > int[] number;
  >
  > number = new int[] {value1, value2 ...}

  위와 같은 방식으로 지정해주면 됩니다. 단, 반드시 new 연산자를 사용 해야합니다.

  - length 필드

  배열의 크기(길이)를 알 수 있게 해주고, 반복문 내에서 많이 사용 됩니다.

  > ArrayName.length;

  ```java
  import java.util.Arrays;

  int[] number;
  number = new int[] {1, 2, 3, 4, 5};

  int sum = 0;
  for(int i=0; i<number.length; i++){
    sum += number[i];
  }
  System.out.println(sum);
  System.out.println(Arrays.toString(number)); // 1, 2, 3, 4, 5 출력

  // Enhanced For Loop
  for(int score : scores){
    sum += score;
  }
  ```

  배열과 반복문을 사용할때 [Enhanced For Loop](https://baekjungho.github.io/java-operator/)을 사용하면 편합니다.

### 메소드 배열 리턴                 

  메소드를 배열로 만들어 리턴 받는 방법을 예제를 통해 알아 봅시다.

  ```java
  char[] array() {
    char[] temp = new char[5];
    return temp;
  }

  public static void main(String[] args) {
      char[] arrayChar = array();
  }    
  ```

### 매개변수가 배열을 받는 메소드

  예를들어 합을 구하는 메소드를 만들고 그 메소드 매개변수가 배열을 받고 리턴하는 역할이 있다 생각하고
  프로그램을 만들어 보겠습니다.

  ```java
  int[] scores;
  int sum = add(new int[] {90, 100});
  System.out.println("합 : " + sum);

  public static int add(int[] scores){
    int sum = 0;
    for(int score : scores){
      sum += score;
    }
    return sum;
  }
  ```

### 배열 초기화 방법

  위에서 보았듯이, { }를 이용해서 값을 지정하면 초기화가 가능합니다.

  ```java

  // 배열을 이용한 버블정렬
  // 1~10까지의 숫자를 랜덤으로 입력받고, 오름차순으로 출력
  import java.util.Scanner;
  public class BubbleSort {
    public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int i, j, temp;

      int[] array = new array[10]; // 10크기의 배열 선언 및 생성
      for(int i=0; i<array.length; i++) {
        System.out.println((i+1) + "번째 값 : ");
        array[i] = sc.nextInt();
      }

      for(int i=0; i<array.length-1; i++) {
        for(int j=0; j<array.length-1-i; j++) {
          if(array[j] > array[j+1]) {
            temp = array[j];
            array[j] = array[j+1];
            array[j+1] = temp;
          }
        }
      }

      for(int i=0; i<array.length; i++) {
        System.out.println(array[i] + " ");
      }
    }
  }
  ```

### 배열 복사 방법

  전에 사용하던 배열의 크기가 작을때 배열의 크기를 늘리고, 새로운 데이터를 담고 싶을때 사용합니다.

  배열을 복사하는 방법은 2가지가 있습니다.

  방법 : `for문`, `System.arraycopy()`, `Arrays.copyOf()`

  - for문을 사용하여 배열 복사

  ```java
  int[] firstArray = { 90, 100, 120 };
  int[] secondArray = new int[8];

  // Array copy
  for(int i=0; i<firstArray.length; i++){
    secondArray[i] = firstArray[i];
  }
  ```

  - System.arraycopy() 메소드 사용

  > System.arraycopy(전 배열이름, 전 배열 시작 인덱스, 새 배열 이름, 새 배열에 붙여 넣을 시작 인덱스, 복사할 개수);

  - Arrays.copyOf() 메소드 사용

  > 배열타입 새 배열이름 = Arrays.copyOf(전 배열이름, 새 배열 크기)

  ```java
  int[] firstArray = { 90, 100, 120 };
  int[] secondArray = new int[8];

  // Array copy

  // 첫 번째 배열을 두 번째로 복사
  // 나머지 빈공간 5칸은 기본값 0으로 초기화됨
  System.arraycopy(firstArray, 0, secondArray, 0, 3);

  // 두 번째 배열을 세 번째로 복사
  // 나머지 빈공간 2칸은 기본값 0으로 초기화됨
  int[] thirdArray = Arrays.copyOf(secondArray, 10);

  // 배열 출력
  System.out.println(Arrays.toString(secondArray));
  System.out.println(Arrays.toString(thirdArray));

  // 출력 결과
  [90, 100, 120, 0, 0, 0, 0, 0]
  [90, 100, 120, 0, 0, 0, 0, 0, 0, 0]
  ```
