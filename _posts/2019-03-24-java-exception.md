---
title: "Exception"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 예외발생과 예외처리에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Exception

  예외는 잘못된 값의 입력, 잘못된 코딩 등으로 인해 발생하는 프로그램 오류 입니다.

  예외가 발생되면 프로그램은 곧바로 종료합니다. 예외에는 일반 예외(Exception)와 실행 예외(Runtime Exception)이 있습니다.
  이 둘의 차이는 `컴파일 과정에서 예외처리코드를 검사하는가 안하는가의 차이`입니다.

  예외의 최상위 클래스는 `java.lang.Exception`입니다.  

  -----------------------------------------------------------------------------

### RuntimeException

  실행 예외가 자주 발생하는 몇가지 종류에 대해서 배워 보겠습니다.

  - NullPointerException

  `NullPointerException`은 참조타입들이 null값을 가질때 접근하여 생기는 예외입니다.

  ```java
  String name = null;
  System.out.println(name.toString());
  ```

  위 처럼 null값을 가지는 참조변수에 접근하게 될때 `NullPointerException`이 발생합니다.

  - ArrayIndexOutOfBoundsException

  `ArrayIndexOutOfBoundsException`은 배열의 인덱스 범위를 초과하거나 매개값을 주지 않았을때 발생합니다.

  ```java
  public class ArrayIndexOutOfBoundsExceptionExample {
    public static void main(String[] args) {
      String[] names = new String[] {"John", "Mike"};
      for(int i=0; i<=name.length; i++) {
        System.out.println(names[i]);
      }
    }
  }
  ```

  - NumberFormatException

  `NumberFormatException`은 문자열을 숫자로 변환하는 과정에서 숫자로 변환할 수 없는
  잘못된 문자열 값이 입력되었을때 발생합니다.

  예를들어 `int a = Integer.parseInt(scan.nextLine());`을 통해서 값을 입력 받았는데
  문자열 123과 같이 숫자로 변환가능한 것은 상관없지만 1a2b3c와 같이 숫자로 변환할 수 없는
  문자열이 들어올때 발생합니다.

  - ClassCastException

  `ClassCastException`은 [강제타입변환(casting)](https://baekjungho.github.io/java-basic3)간에 발생하는 예외인데 Casting을 할때
  `instanceof` 연산자를 사용하지 않으면 발생할 수 있습니다.

  따라서 Casting을 할때에는 instanceof연산자를 통해서 ClassCastException을 예방해야 합니다.

  -----------------------------------------------------------------------------

### try-catch-finally

  예외가 발생하면 프로그램이 종료되는데 이를 막기 위해서 `try-catch-finally`구문을 사용합니다.

  1. try { 예외 발생 가능 코드 작성}
  2. catch(예외클래스 e) { 예외 처리 }
  3. finally { 예외 발생과 상관 없이 항상 실행하는 코드 }

  > finally는 생략이 가능합니다.

  ```java
  import java.util.Scanner;
  public class ArithmeticExceptionExample {
    public static void main(String[] args) {
      int div = 0;
      Scanner scan = new Scanner(System.in);
      try{
        System.out.print("숫자 2개를 입력하세요 : ");
        int num1 = Integer.parseInt(scan.nextLine());
        int num2 = Integer.parseInt(scan.nextLine());
    } catch(NumberFormatException e){
      System.out.println("숫자로 변환가능한 문자열을 입력하세요!");
    }

    try {
      if(num1 > num2) {
        div = num1 / num2;
      } else if(num2 > num1) {
        div = num2 / num1;
      }
    } catch(ArithmeticException e){
      System.out.println("0으로 나눌 수 없습니다.")
    }
   }
  }
  ```

### 다중 catch문

  다중 catch문을 이용하면 try-catch구문을 줄여서 사용할 수 있습니다.

  바로 위에서 사용한 ArithmeticException예제를 가져와서 다중 catch문으로 바꿔보겠습니다.

  ```java
  import java.util.Scanner;
  public class ArithmeticExceptionExample {
    public static void main(String[] args) {
      int div = 0;
      Scanner scan = new Scanner(System.in);
      try{
        System.out.print("숫자 2개를 입력하세요 : ");
        int num1 = Integer.parseInt(scan.nextLine());
        int num2 = Integer.parseInt(scan.nextLine());

        if(num1 > num2) {
          div = num1 / num2;
        } else if(num2 > num1) {
          div = num2 / num1;
        }
    } catch(NumberFormatException e) {
      System.out.println("숫자로 변환가능한 문자열을 입력하세요!");
    } catch(ArithmeticException e) {
      System.out.println("0으로 나누면 안됩니다.");
    }
   }
  }
  ```

  코드가 위에서 아래로 내려오면서 차례대로 실행되고 예외 발생시 catch문도 위에서 아래로
  차례로 예외처리를 해줍니다. 단, 주의할 점은 `상위예외클래스(Exception)`가 먼저 작성되는
  경우에는 `하위예외클래스(ArithmeticException)`는 무시되고 상위예외클래스만 실행됩니다.

  - catch문 내에 예외처리클래스 여러개 작성하는 방법

  멀티 catch라고도 하는데 사용방법은 `|`기호를 사용하여 예외 클래스 들을 연결해줍니다.

  > try { ArithmeticException or NumberFormatException 발생}
  >
  > catch(ArithmeticException | NumberFormatException e) { 예외 처리 }

  ```java
  import java.util.Scanner;
  public class MultiCatchExample {
  public static void main(String[] args) {
    int div = 0;
    Scanner scan = new Scanner(System.in);
    try{
      System.out.print("숫자 2개를 입력하세요 : ");
      int num1 = Integer.parseInt(scan.nextLine());
      int num2 = Integer.parseInt(scan.nextLine());

      if(num1 > num2) {
        div = num1 / num2;
      } else if(num2 > num1) {
        div = num2 / num1;
      }
    } catch(ArithmeticException | NumberFormatException e) {
      System.out.println("0으로 나눌 수 없습니다 / 숫자로 변환가능한 문자열을 입력하세요!");
    } catch(Exception e) {
    	 System.out.println("ERROR");
    }
    }
  }
  ```

### try-with-resources

  자바7에서 새롭게 추가된 기능인데 `자동 리소스 닫기` 기능입니다.

  예를들어 Scanner를 사용하면 close()를 사용해서 닫아야 하는데 `try-with-resources`는 자동으로 닫아줍니다.

  > 조건 : 리소스 객체는 java.lang.AutoCloseable 인터페이스를 구현 하고 있어야 합니다.
  >
  > public class Resources implements AutoCloseable { ..... }

### throw와 throws

  throw는 강제적으로 예외를 발생시키는 키워드 입니다.

  throws는 메소드나 생성자에서 발생한 예외를 떠넘기는 키워드로 자신을 호출한 곳에서 예외처리를 떠넘기는
  역할을 합니다.

  ```java
  public class ThrowExample {

    public static void division() throws ArithmeticException {
      int x = 1;
      int y = 0;
      int div = 0;

      div = x / y;
    }

    public static void main(String[] args) {
      try {
        division();
      } catch(ArithmeticException e){
        System.out.println("0으로 나누면 안됩니다.");
      }
    }
  }
  ```

  division()메소드에서는 계산을 수행하기만 하고 main()메소드 내부에 try-catch구문을
  사용하여 try안에 division()메소드를 호출하면서 자신을 호출한 곳(main메소드)에서 예외처리를
  맡기는 것입니다.

  아래 예제는 `throw`키워드를 통해 강제로 예외를 발생시키는 것입니다.

  > 예외 발생 : throw new Exception();  

  ```java
  public class ThrowExample {

    public static void division() throws ArithmeticException {
      int x = 1;
      int y = 1;
      int div = 0;

      div = x / y;
    }

    public static void main(String[] args) {
      try {
        division();
        throw new ArithmeticException();
      } catch(ArithmeticException e){
        System.out.println("0으로 나누면 안됩니다.");
      }
    }
  }
  ```

  __throws Exception__ 을 사용하면 모든 예외를 떠넘길 수 있습니다.

  > throws 키워드가 붙어있는 메소드는 반드시 try 블록 내에서 호출되어야 합니다.

  메인 메소드에서 throws키워드를 붙이면 JVM이 최종적으로 예외처리를 하게되서 좋지 않습니다.
  따라서 메인 메소드 내부에 try-catch구문을 사용하여 처리하는것이 좋습니다.

## Application Exception

  Application Exception은 `사용자 정의 예외`라고도 하는데, 자바 표준 API에서 제공해주지 않은
  예외들을 사용자가 직접 만들어 사용합니다.

  1. 일반 예외로 만들기
  2. 실행 예외로 만들기

  > public class NameException extends Exception | RuntimeException

  Application Exception은 이름을 `~Exception`으로 끝나게 만들어야 좋습니다.

  사용자 정의 예외클래스 내부에도 필드, 메소드, 생성자를 가질 수 있지만 대부분은 `기본생성자`와
  `예외 메시지를 전달할 매개변수를 갖는 생성자` 2개를 가집니다.

  - Architecture

  ```java
  public class CheckNumberException extends Exception {
    public CheckNumberException() { }
    public CheckNumberException(String message){
      super(message);
    }
  }
  ```

## 예외 정보

  예외 정보를 얻기위해 사용되는 메소드는 `getMessage()`와 `printStackTrace()`입니다.

  `throw new DivisionByZero("예외 처리 메시지");`

  위 처럼 일부로 예외발생이 있을만한 코드에 조건을 걸어서 예외 발생 조건에 맞으면
  위 코드를 실행하여 예외를 발생시켜서 괄호()안의 메시지를 출력할 수 있습니다.

  > printStackTrace()는 예외 발생 코드를 추적해서 모두 콘솔에 출력합니다.
  >
  > 프로그램을 테스트하면서 오류를 찾을때 활용 됩니다.

  ```java
  public class ExceptionExample {

    public static void division(int num1, int num2) throws DivisionByZero {
      int x = num1;
      int y = num2;
      int div = 0;

      if((num1 == 0) || (num2 == 0))
        throw new DivisionByZero("0으로 나누면안됨");
      else
        div = x / y;
    }

    public static void main(String[] args) {
      try {
        division(10, 0);
      } catch(Exception e){
        String message = e.getMessage();
        System.out.println(message);
        System.out.println();
        e.printStackTrace();
      }
    }
  }

  // Application Exception
  public class DivisionByZero extends Exception {
    public DivisionByZero() { }
    public DivisionByZero(String message){
      super(message);
    }
  }
  ```
