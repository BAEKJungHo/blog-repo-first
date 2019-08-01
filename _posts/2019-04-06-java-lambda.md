---
title: "Lambda Expression"
layout: post
category: Java
tags: [Java]
excerpt: "자바의 람다식에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Lambda Expression

  자바8부터 람다식을 지원합니다.

  람다식을 배우기전에 객체지향과 함수적 언어에 대해 알아보겠습니다.

  1. 객체지향 : 객체명.메소드명
  2. 함수적언어 : 익명구현객체(익명함수, Anonymouse Function 생성)

  함수적 프로그래밍의 장점은 `병렬처리와 이벤트 지향 프로그래밍`방식에 적합합니다.
  또한 `코드의 라인 수 절약`이 가능하며 `메소드로 행동방식을 전달`할 수 있고, `의도가 명확`해집니다.

  람다식은 `함수 지향 언어`에 가까우며, `익명구현객체`방식을 사용하며, 인터페이스의 익명구현객체를
  생성합니다. 그리고 람다식은 `대입되는 인터페이스(target type)`의 종류에 따라 작성방법이 조금씩 다릅니다.

  - 람다식 표현 방법

  ```java
  Runnable runnable = () -> { ..... };
  ```

  위 처럼 람다식은 __(매개변수) -> {실행코드};__ 로 작성됩니다.

  -----------------------------------------------------------------------------

### @FunctionalInterface

  함수적 인터페이스(@FunctionalInterface)의 특징은 다음과 같습니다.

  > 람다식의 Target Type이 되는 인터페이스는 하나의 추상 메소드만 선언 되어있어야 합니다.
  >
  > 2개이상의 추상 메소드를 가지는 인터페이스는 Target Type이 될 수 없습니다.

  인터페이스 선언시 @FunctionalInterface 어노테이션을 작성하면 두 개 이상의 추상메소드가
  선언 되었는지 체크해주는 기능을 가지고 있습니다.

  ```java
  public interface RamdaFunctionalInterface {
    public void method(int x, int y);
  }
  ```

  - 람다식 실행 방법

  람다식을 실행할때에는 인터페이스에 선언된 메소드를 호출하여야 합니다. 호출 할때에는
  `인터페이스 참조 변수명.메소드명` 으로 호출합니다.

  ```java
  public class RamdaFunctionalInterfaceExample {
    public static void main(String[] args) {
      RamdaFunctionalInterface fi; // 인터페이스 참조 변수

      fi = (x * y) -> { return x * y };
      fi.method(8, 8); // 람다식 실행
    }
  }
  ```

  람다식은 실행문이 하나만 있는 경우 중괄호({})를 생략 가능하며, 실행코드에
  return문만 있는 경우 return도 생략 가능합니다.

  ```java
  fi = (x * y) -> return x * y;
  fi = (x * y) -> x * y; // expression
  ```

  즉, 람다식에서는 return문이 있는 경우 return문을 생략하고 `수식(expression)`으로
  위 처럼 표현할 수있습니다.

  매개 변수가 1개일 경우도 다음과 같이 생략할 수 있습니다.

  ```java
  fi = x -> System.out.println(x+5);
  ```

  - 표준 API의 함수적 인터페이스(FunctionalInterface)

    단 한개의 추상 메소드(abstract method)를 가지는 인터페이스들은 람다식으로 표현 가능합니다.

    예륻들어 Thread의 [Runnable Interface](https://baekjungho.github.io/java-basic4)는 run() 추상메소드 하나만
    구현되어있으므로 람다식으로 표현 가능합니다.

    ```java
    @FunctionalInterface
    public interface Runnable {
      public abstract void run();
    }
    ```

  -----------------------------------------------------------------------------

### 메소드 내부에서 작성

  람다식은 주로 메소드 내부에서 작성되며, 메소드 시그니처의 매개변수나, 로컬변수를 사용할때는
  위 두 변수는 [final 고정값 특징](https://baekjungho.github.io/java-static/)을 지니기 때문에 수정이 불가능 합니다.

  ```java
  public class WritingLambdaInMethodExample {
    public static void main(String[] args){
      .....
    }
    public void testMethod(int arg) {
      int localVariable = 80;

      // arg = 10; - 수정 불가능(final 특징)
      // localVariable = 100; - 수정 불가능(final 특징)
    }
  }
  ```

  -----------------------------------------------------------------------------

### java.util.function

  java.util.function 패키지에서는 다음과 같은 함수적 인터페이스가 있습니다.
  Consumer, Supplier, Function, Operator, Predicate

  - __Consumer__

    특징 : 매개값 O, 리턴값 X / Setter() 메소드와 비슷한 역할을 하는 리턴값이 없는 `accept()` 추상메소드를 가지고 있습니다.

    - 인터페이스 종류(XXXConsumer)

      |인터페이스명   |추상 메소드|
      |---------------|-----------|
      |Consumer<T>|  void accept(T t)|
      |BiConsumer<T,U>|  void accept<T t, U u>|
      |DoubleConsumer| void accept(double value)|
      |IntConsumer| void accept(int value)|
      |LongConsumer| void accept(long value)|
      |ObjIntConsumer<T>|  void accept(T t, int value)|
      |ObjLongConsumer<T>|  void accept(T t, long value)|
      |ObjDoubleConsumer<T>|  void accept(T t, double value)|

    - 람다식 작성 방법

      ```java
      Consumer<String> consumer = t -> { 실행코드 };
      BiConsumer<String, String> biConsumer = (t, u) -> { 실행코드 };
      ObjIntConsumer<String> objConsumer = (t, i) -> { 실행코드 };
      ```

  - __Supplier__

    특징 : 매개값 X, 리턴값 O / getter() 메소드와 비슷한 `getXXX()`메소드를 가지고 있습니다.

    - 인터페이스 종류

      |인터페이스명   |추상 메소드|
      |---------------|-----------|
      |Supplier<T>|  T get()|
      |BooleanSupplier|  boolean getAsBoolean()|
      |DoubleSupplier| double getAsDouble()|
      |IntSupplier| int getAsInt()|
      |LongSupplier| long getAsLong()|

    - 람다식 작성 방법

      ```java
      Supplier<String> supplier = () -> { 실행코드; return 문자열; };
      IntSupplier intSupplier = () -> { 실행코드; return int값 };
      ```

  - __Function__

    특징 : 매개값 O, 리턴값 O / `applyXXX()`를 가지고 있습니다. 매개값을 리턴값으로 타입변환(매핑)하는 역할을 담당합니다.

    - 인터페이스 종류

      |인터페이스명   |추상 메소드|
      |---------------|-----------|
      |Function<T,R>|  R apply(T t)|
      |BiFunction<T,U,R>|  R apply(T t, U u)|
      |DoubleFunction<R>| R apply(double value)|
      |IntFunction<R>| R apply(int value)|
      |IntToDoubleFunction| double applyAsDouble(int value)|
      |IntToLongFunction| long applyAsLong(int value)|
      |LongToDoubleFunction| double applyAsDouble(long value)|
      |LongToIntFunction| int aplyAsInt(long value)|
      |ToDoubleBiFunction<T,U>| double applyAsDouble(T t, U u)|

    - 람다식 작성 방법

      ```java
      Function<ClassName, String> function = t -> { return t.getName(); }
      Function<ClassName, String> function = t -> t.getName();
      LongToIntFunction<ClassName> = t -> t.getName();
      ```

  - __Operator__

    특징 : 매개값 O, 리턴값 O / `applyXXX()`를 가지고 있습니다. 매개값을 이용한 연산 후 리턴하는 역할을 담당합니다.

    - 인터페이스 종류

      |인터페이스명   |추상 메소드|
      |---------------|-----------|
      |BinaryOperator<T>|  T apply(T t, T t)|
      |UnaryOperator<T>|  T apply(T t)|
      |DoubleBinaryOperator| double applyAsDouble(double, double)|
      |DoubleUnaryOperator| double applyAsDouble(double)|
      |IntBinaryOperator| int applyAsInt(int, int)|
      |IntUnaryOperator| int applyAsInt(int)|
      |LongBinaryOperator| long applyAsLong(long, long)|
      |LongUnaryOperator| long applyAsLong(long)|

    - 람다식 작성 방법

      ```java
      IntBinaryOperator operator = (a, b) -> { 실행코드; return 값; }
      ```

  - __Predicate__

    특징 : 매개값 O, 리턴값 O / `testXXX()`를 가지고 있습니다. 매개값을 조사해서 true OR false를 리턴하는 역할을 합니다.

    - 인터페이스 종류

      |인터페이스명   |추상 메소드|
      |---------------|-----------|
      |Predicate<T>|  boolean test(T t)|
      |BiPredicate<T,U>|  boolean test(T t, U u)|
      |DoublePredicate| boolean test(double value)|
      |IntPredicate| boolean test(int value)|
      |LongPredicate| boolean test(long value)|

    - 람다식 작성 방법

      ```java
      Predicate<ClassName> predicate = t -> t.getXXX().equals("String");
      ```
