---
title: "Spring Assert Statements"
layout: post
category: Spring
tags: [Spring]
excerpt: "Spring Assert에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring Assert에 대해서 배워 봅시다.

## Assert

  Assert는 단순히 if문을 줄이는 역할만 하는 것은 아닙니다. 프로젝트 규칙을 적용하고 공통을 재사용한다는 것에 큰 의미가 있습니다.

  복잡한 기술이 필요한 것도 아니고 누구나 쉽게 사용할 수 있는 것이기에 초기 공통 개발 시 반드시 고려해야할 항목 중 하나입니다.

  Spring Assert 클래스는 인수를 검증하는 데 도움이됩니다. Assert 클래스의 메서드를 사용하여 , 우리는 우리가 기대하는 가정을 작성할 수 있습니다. 그리고 이들이 충족되지 않으면 `런타임 예외`가 발생합니다.

  각 Assert 의 메소드는 Java assert 문과 비교 될 수 있습니다 . Java assert 문 은 조건이 실패하면 런타임에 Error 를 throw 합니다. 흥미로운 사실은 이러한 주장이 사용 중지 될 수 있다는 것입니다.

### 특징

  Spring Assert 의 메서드의 몇 가지 특성은 다음과 같습니다.

  - Assert 의 메소드는 정적입니다.
  - IllegalArgumentException 또는 IllegalStateException를 throw 합니다.
  - 첫 번째 매개 변수는 일반적으로 `유효성 검사를 위한 인수이거나 검사 할 논리 조건`입니다
  - 마지막 매개 변수는 일반적으로 유효성 검사에 실패 할 경우(조건이 거짓일 경우) 표시되는 `예외 메시지(Exception Message)`입니다.
  - 메시지는 String 매개변수 또는 Supplier <String>  매개변수로 전달할 수 있습니다.

  > 유사한 이름에도 불구하고 Spring Assert는 JUnit의 Assert와 다릅니다. 스프링 어썰트는 테스트를위한 것이 아니라 디버깅을 위한 것입니다.

### 사용 목적

  `Spring Assert`는 `인수를 검증`하고 조건에 맞지 않는 경우 `IllegalArgumentException` 또는 `IllegalStateException`를 발생시킵니다. 이 둘의 예외는 RuntimeException의 하위 예외들입니다. RuntimeException은 `Unchecked Exception`이므로 예외 처리를 하지 않아도 됩니다.

  - 예외 구조(Exception Structure)

    - java.lang.Object
      - java.lang.Throwable
        - java.lang.Exception
          - java.lang.RuntimeException
            - IllegalStateException, IllegalArgumentException

  이 부분은 `조건문을 단순화`하고 `반복적인 코드를 줄이는 역할`을 합니다.

  > Spring Assert는 인수를 검증하며, 조건에 맞지 않는 경우 예외를 발생 시키고, 조건문을 단순화 하며, 반복적인 코드를 줄이는 역할을 한다.

### EXAMPLE

#### EXAMPLE 1.

  다음 코드를 보겠습니다.

  ```java
  if(user == null) {
    throw new IllegalArgumentException("사용자 정보가 존재하지 않습니다.");
  }
  ```

  위 코드는 아래와 같이 줄일 수 있습니다.

  ```java
  Assert.notEmpty(user, "사용자 정보가 존재하지 않습니다.");
  ```

#### EXAMPLE 2.

  public 메서드인 drive()를 사용하여 Car 클래스를 정의 해보겠습니다.

  ```java
public class Car {
  private String state = "stop";

  public void drive(int speed) {
      Assert.isTrue(speed > 0, "speed must be positive");
      this.state = "drive";
      // ...
  }
}
  ```

  위에서 Assert.isTrue() 코드를 if문으로 변경하면 아래와 같습니다.

  ```java
  if (!(speed > 0)) {
    throw new IllegalArgumentException("speed must be positive");
  }
  ```

  음수 인수로 drive () 메서드 를 호출하려고 하면 아래와 같이 IllegalArgumentException 예외가 발생합니다.

  ```java
  Exception in thread "main" java.lang.IllegalArgumentException: speed must be positive
  ```

### Assert의 확장

  개발자들은 값을 검증하는 방법을 여러 형태로 만들어낼 수 있습니다. 또 exception도 다양하게 사용할 수 있습니다. 그런데 프로젝트를 하다 보면 값을 검증하는 방법을 통합하거나 예외의 사용을 제한할 필요가 있습니다. 에러 페이지나 API에서 사용하기 위해 Exception을 정의해야 합니다. 더불어 공통 로직을 만들어 관리해야 합니다.

### 프로젝트 검증 예제

  - 숫자 검증 : 단순히 숫자여야만 한다.

  - 실수 검증 : 숫자, '-', '+', 소수점 허용

  - 전화번호 검증

  - 사업자 번호 검증

  - 날짜 검증 : 프로젝트에서 정의한 포맷의 날짜 확인

  - 비밀번호 검증 : 비밀번호는 영문, 숫자, 특수문자 혼용으로 8자 이상이어야 한다.

  - 사용자 아이디 검증 : 영문, 숫자 혼용 4자 이상 10자 이내 등등

  이런 여러 규칙을 일관성 있게 적용하고 재사용하기 위해 Assert를 확장해서 사용할 필요가 있습니다.

## Logical Assertions

### isTrue()

  - Assert.isTrue(조건, 메시지);

  ```java
  Assert.isTrue(speed > 0, "speed must be positive");
  ```

  해당 조건이 false일 경우 `IllegalArgumentException`을 던집니다.

### state()

  > The state() method has the same signature as isTrue() but throws the IllegalStateException.

  이름에서 알 수 있듯이 객체의 잘못된 상태로 인해 메소드를 계속 진행하지 않아야하는 경우 사용해야합니다.

  자동차가 작동중인 경우 fuel () 메소드를 호출 할 수 없다고 상상해보십시오 . 이 경우에 state () assertion을 사용합시다.

  ```java
  public void fuel() {
    Assert.state(this.state.equals("stop"), "car must be stopped");
    // ...
  }
  ```

  Of course, we can validate everything using logical assertions. But for better readability, we can use additional assertions which make our code more expressive.

## Object and Type Assertions

### notNull()

  notNull() 메서드를 사용하여  객체가 null이 아닌 것으로 가정 할 수 있습니다.

  ```java
  public void сhangeOil(String oil) {
    Assert.notNull(oil, "oil mustn't be null");
    // ...
  }
  ```

### isNull()

  반면에 isNull() 메서드를 사용하여  객체가 null인지 검사 할 수 있습니다.

  ```java
  public void replaceBattery(CarBattery carBattery) {
    Assert.isNull(
      carBattery.getCharge(),
      "to replace battery the charge must be null");
    // ...
  }
  ```

### isInstanceOf()

  객체가 특정 유형의 다른 객체의 인스턴스인지 확인하려면 isInstanceOf()  메서드를 사용할 수 있습니다.

  ```java
  public void сhangeEngine(Engine engine) {
    Assert.isInstanceOf(ToyotaEngine.class, engine);
    // ...
  }
  ```

  이 예에서는 `ToyotaEngine`이 `Engine`의 하위 클래스 이므로 검사가 성공적으로 통과 합니다.

### isAssignable()

  유형을 확인하기 위해 `Assert.isAssignable(상위클래스, 하위클래스)` 을 사용할 수 있습니다 .

  ```java  
  public void repairEngine(Engine engine) {
    Assert.isAssignable(Engine.class, ToyotaEngine.class);
    // ...
  }
  ```

  두 객체는 `is-a kind of(상속)` 관계를 나타냅니다 .

## Text assertions

### hasLength()

  hasLength()를 사용하여 문자열이 비어있지 않은 경우를 체크 할 수 있습니다.

  ```java
  public void startWithHasLength(String key) {
    Assert.hasLength(key, "key must not be null and must not the empty");
    // ...
  }
  ```

### hasText()

  hasText() 메서드를 사용하여 조건을 강화하고 문자열에 공백이 아닌 문자가 하나 이상 들어 있는지 확인할 수 있습니다.

  ```java
  public void startWithHasText(String key) {
    Assert.hasText(
      key,
      "key must not be null and must contain at least one non-whitespace  character");
    // ...
  }
  ```

### doesNotContain()

  doesNotContain () 메서드를 사용하여 문자열 인수에 특정 하위 문자열이 포함되어 있지 않은지 확인할 수 있습니다.

  ```java
  public void startWithNotContain(String key) {
    Assert.doesNotContain(key, "123", "key mustn't contain 123");
    // 즉, 123을 포함하면 Exception Message를 던집니다.
  }
  ```

## Collection and Map Assertions

### notEmpty() for collections

  이름에서 알 수 있듯이 notEmpty() 메서드는 컬렉션이 비어 있지 않은 것으로 주장하고 컬렉션이 null이 아니며 적어도 하나 이상의 요소를 포함하고 있음을 나타냅니다.

  ```java
  public void repair(Collection<String> repairParts) {
    Assert.notEmpty(
      repairParts,
      "collection of repairParts mustn't be empty");
    // ...
  }
  ```

### notEmpty() for maps

  동일한 메소드가 맵에 오버로드되어 맵이 비어 있지 않고 적어도 하나 이상의 항목이 있는지 확인할 수 있습니다.

  ```java
  public void repair(Map<String, String> repairParts) {
    Assert.notEmpty(
      repairParts,
      "map of repairParts mustn't be empty");
    // ...
  }
  ```

## Array Assertions

### notEmpty() for arrays

  notEmpty() 메서드를 사용하여 배열이 비어 있지 않고 하나 이상의 요소가 포함되어 있는지 확인할 수 있습니다.

  ```java
  public void repair(String[] repairParts) {
    Assert.notEmpty(
      repairParts,
      "array of repairParts mustn't be empty");
    // ...
  }
  ```

### noNullElements()

  noNullElements() 메서드를 사용하여 배열에 null 요소가 없는지 확인할 수 있습니다.

  ```java
  public void repairWithNoNull(String[] repairParts) {
    Assert.noNullElements(
      repairParts,
      "array of repairParts mustn't contain null elements");
    // ...
  }
  ```

  null 요소가 없는 한 이 검사는 배열이 비어있는 경우에도 계속 수행 됩니다.

## 사용자 정의 Assert EXAMPLE

  아래는 Spring Assert를 상속 받아서 사용자가 정의한 예외를 사용하게 오버로딩한 부분과 숫자 체크, 패턴 체크 등을 추가해서 만들어봤습니다. 프로젝트 초기에 규칙을 정의하고 필요한 기능을 추가한 후 개발자와 공유하면 개발도 훨씬 쉽고 코드 품질도 향상하게 됩니다.

  ```java
  public class EbloAssert extends Assert {

    private static final String NUMBER_ALPHABET_PATTERN = "^[a-zA-Z\\d]+$";
    private static final String NUMERIC_PATTERN = "^[\\+\\-]{0,1}[\\d]+$";
    private static final String FLOAT_PATTERN = "^[\\+\\-]{0,1}[\\d]+[\\.][0-9]+$";

    /**
     * Assert a boolean expression, TRUE가 아닌 경우 사용자가 정의한 예외를 던진다.
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.state(id == null, "The id property must not already be initialized", EbloInvalidRequestException.class);</pre>
     * @param expression a boolean expression
     * @param message the exception message to use if the assertion fails
     * @param exceptionClass
     * @throws IllegalStateException if {@code expression} is {@code false}
     */
    public static void state(boolean expression, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (!expression) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * Assert a boolean expression, TRUE가 아닌 경우 사용자가 정의한 예외를 던진다.
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.isTrue(user != null, "사용자 정보가 존재하지 않습니다.", EbloNotFoundException.class);</pre>
     * @param expression a boolean expression
     * @param message the exception message to use if the assertion fails
     * @param exceptionClass
     * @throws IllegalStateException if {@code expression} is {@code false}
     */
    public static void isTrue(boolean expression, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (!expression) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * Assert that an object is {@code null}. 객체가 null이 아닌 경우 사용자가 정의한 예외를 던진다.
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.isNull(user, "기존 사용자 정보가 존재합니다.", EbloInvalidRequestException.class);</pre>
     * @param expression a boolean expression
     * @param message the exception message to use if the assertion fails
     * @param exceptionClass
     * @throws IllegalStateException if {@code expression} is {@code false}
     */
    public static void isNull(@Nullable Object object, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (object != null) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * null인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.notNull(user, "사용자 정보가 존재하지 않습니다.", EbloNotFoundException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void notNull(@Nullable Object object, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (object == null) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 전달받은 값이 null이 거나 빈값인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.hasLength(value, "전달 받은 값이 빈값입니다.", EbloInvalidRequestException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void hasLength(@Nullable String text, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (!StringUtils.hasLength(text)) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 전달받은 값이 null이 거나 빈값인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.hasText(value, "전달 받은 값이 빈값입니다.", EbloInvalidRequestException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void hasText(@Nullable String text, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (!StringUtils.hasText(text)) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 전달받은 값이 null이 거나 빈값인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.notEmpty(array, "전달 받은 값이 빈값입니다.", EbloInvalidRequestException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void notEmpty(@Nullable Object[] array, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (ObjectUtils.isEmpty(array)) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 전달받은 값이 null이 거나 빈값인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.notEmpty(collection, "전달 받은 값이 빈값입니다.", EbloInvalidRequestException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void notEmpty(@Nullable Collection<?> collection, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (CollectionUtils.isEmpty(collection)) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 전달받은 값이 null이 거나 빈값인 경우 사용자 정의 예외 발생
     * 사용자 정의 예외는 AbstractEbloBaseException.class를 상속 받아 구현해야 한다.
     * <pre class="code">Assert.notEmpty(map, "전달 받은 값이 빈값입니다.", EbloInvalidRequestException.class);</pre>
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void notEmpty(@Nullable Map<?, ?> map, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if (CollectionUtils.isEmpty(map)) {
            throwException(message, exceptionClass);
        }
    }

    private static void throwException(String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        try {
            throw exceptionClass.getDeclaredConstructor( String.class ).newInstance( message );
        } catch (Exception e) {
            e.printStackTrace();
            throw new EbloSystemException("예외 처리 중 오류가 발생했습니다. "+e.getMessage());
        }
    }

    /**
     * 값이 영문 알파벳, 숫자가 아닌 경우 예외 발생
     * @param object
     * @param message
     */
    public static void isAlphaNumber(String object, String message) {
        isMatched(object, NUMBER_ALPHABET_PATTERN, message, EbloInvalidRequestException.class);
    }

    /**
     * 값이 영문 알파벳, 숫자가 아닌 경우 사용자 정의 예외 발생
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void isAlphaNumber(String object, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        isMatched(object, NUMBER_ALPHABET_PATTERN, message, exceptionClass);
    }

    /**
     * 값이 숫자가 아닌 경우 예외 발생
     * @param object
     * @param message
     */
    public static void isNumeric(String object, String message) {
        isMatched(object, NUMERIC_PATTERN, message, EbloInvalidRequestException.class);
    }

    /**
     * 값이 숫자가 아닌 경우 사용자 정의 예외 발생
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void isNumeric(String object, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        isMatched(object, NUMERIC_PATTERN, message, exceptionClass);
    }

    /**
     * 값이 float, double이 아닌 경우 예외 발생
     * @param object
     * @param message
     */
    public static void isFloat(String object, String message) {
        isMatched(object, FLOAT_PATTERN, message, EbloInvalidRequestException.class);
    }

    /**
     * 값이 float, double이 아닌 경우 사용자 정의 예외 발생
     * @param object
     * @param message
     * @param exceptionClass
     */
    public static void isFloat(String object, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        isMatched(object, FLOAT_PATTERN, message, exceptionClass);
    }

    /**
     * 패턴 매치, 해당 패턴과 일치 하지 않는 경우 사용자 정의 예외 발생
     * @param object
     * @param pattern
     * @param message
     * @param exceptionClass
     */
    public static void isMatched(String object, String pattern, String message, final Class<? extends AbstractEbloBaseException> exceptionClass) {
        if(object == null || "".equalsIgnoreCase(object)) return;

        if(!object.matches(pattern)) {
            throwException(message, exceptionClass);
        }
    }

    /**
     * 패턴 매치, 해당 패턴과 일치 하지 않는 경우 예외 발생
     * @param object
     * @param pattern
     * @param message
     */
    public static void isMatched(String object, String pattern, String message) {
        if(object == null || "".equalsIgnoreCase(object)) return;

        if(!object.matches(pattern)) {
            throwException(message, EbloInvalidRequestException.class);
        }
    }

}
  ```

#### 테스트

  ```java
@Test
public void test() {
    EbloAssert.isTrue(1>0, "test", EbloInvalidRequestException.class);
    EbloAssert.isNumeric("1234567", "숫자만");
    EbloAssert.isFloat("-12345.67", "FLOAT DOUBLE 형태");
    EbloAssert.isFloat("-12345.0", "FLOAT DOUBLE 형태");
    EbloAssert.isMatched("-12345.0", "^[\\+\\-]{0,1}[\\d]+[\\.][0-9]+$", "FLOAT DOUBLE 형태");
}
  ```

## 참조

  > [https://eblo.tistory.com/63](https://eblo.tistory.com/63)
  >
  > [https://www.baeldung.com/spring-assert](https://www.baeldung.com/spring-assert)
