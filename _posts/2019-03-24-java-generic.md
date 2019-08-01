---
title: "Generic"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Generic에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Generic

  제네릭(Generic)은 컬렉션, 람다식, 스트림, NIO등에서 많이 사용하고 중요한 부분입니다.
  제네릭은 클래스와 인터페이스, 메소드를 정의할 때 `Type Parameter(타입 매개변수)`로 사용합니다.

  `제네릭 타입(Generic Type)`은 타입을 Parameter로 가지는 클래스와 인터페이스를 말합니다.

  > class<T>, interface<T>을 제네릭 타입 이라고 합니다.
  >
  > < > 안에 있는 T는 Type Parameter 입니다.
  >
  > class<Type Parameter>, interface<Type Parameter>

  Type Parameter는 일반적으로 `대문자 알파벳 한 글자`로 나타냅니다.

  - 제네릭 사용 장점

    1. 컴파일 시 강한 타입체크 가능
    2. Casting 제거

  - EXAMPLE

  ```java
  public class Bucket<T> {
    private T t;
    public T getBucket() { return t; }
    public void setBucket(T t) { this.t = t; }
  }

  public class BucketList {
    public static void main(String[] args) {
      Bucket<String> bucket = new Bucket<String>();
      bucket.setBucket("번지점프");
      String str = bucket.getBucket();
    }
  }
  ```

  메인 메소드에서 객체를 생성할때 `Type Parameter`를 String으로 지정했기 때문에
  사실상 코드는 T자리에 String이 대입되어 다음과 같이 변하게 됩니다.

  ```java
  public class Bucket<String> {
    private String t;
    public String getBucket() { return t; }
    public void setBucket(String t) { this.t = t; }
  }
  ```

  만약 위에서 제네릭 타입 대신에 필드랑 메소드 생성시 `Object(최상위 클래스)`로
  생성하게 되면 T자리에 Object가 들어가게 되고 setter()메소드로 값을 저장 한 후에
  `String str = (String)bucket.getBucket();`처럼 앞에 자식클래스인(String)을 붙여서
  Casting(강제 형변환)을 해줘야 합니다. 따라서 제네릭 타입을 사용하면 다음과 같은
  Casting을 제거할 수 있습니다.

  -----------------------------------------------------------------------------

### Multi Type Parameter

  제네릭 타입은 두 개 이상의 `멀티 타입 파라미터(Multi Type Parameter)`를 사용할 수 있습니다.

  Travel<T, M> 방식처럼 콤마(,)로 Type Parameter를 구분 짓습니다.

  제네릭 타입도 상속 받아서 사용할 수 있으며, 제네릭 인터페이스도 만들 수 있습니다.

  만약 자식 제네릭 타입이 부모를 상속받았으면, 추가적으로 Type Parameter를 가질 수 있습니다.

  보통 제네릭 타입 클래스들은 아래와 같이 `필드, Getter(), Setter(), 생성자`를 가집니다.

  - EXAMPLE

  ```java
  public class Travel<T, M> {
    private T kind;
    private M region;

    public T getKind() { return this.kind; }
    public M getRegion() { return this.region; }

    public void setKind(T kind) { this.kind = kind; }
    public void setRegion(T region) { this.region = region; }
  }

  public class TravelExample {
    public static void main(String[] args) {
      // 해외여행
      Travle<Foreign, String> foreignCountry = new Travel<Foreign, String>();
      foreignCountry .setKind(new Foreign());
      Foreign foreign = foreignCountry.getKind();
      String foreignRegion = foreignCountry.getRegion();

      // 국내여행
      Travle<Domestic, String> interiorCountry = new Travel<Domestic, String>();
      interiorCountry.setKind(new Domestic());
      Domestic domestic = interiorCountry.getKind();
      String domesticRegion = interiorCountry.getRegion();
    }
  }
  ```

  - Inheritance

  ```java
  public class foreignCountry<T, M, D> extends Travel<T, M> {
    private D day;
    public D getDay() { return this.day; }
    public void setDay(D day) { this.day = day; }
  }
  ```

  -----------------------------------------------------------------------------

### Generic Method

  제네릭 메소드(Generic Method)는 매개타입과 리턴 타입으로 `Type Parameter`를 갖습니다.

  - Architecture

  ```java
  public <Type Parameter> ReturnType MethodName(arg) { ..... }

  public <T> Bucket<T> bucketList(T t) { ..... }
  ```

  - 호출 방법  

  ```java
  1. ReturnType variable = <구체적타입> MethodName(arg); // 명시적으로 구체적 타입 지정
  2. ReturnType variable = MethodName(arg) // 매개값을 보고 구체적 타입을 추정

  Bucket<String> bucket = <String>bucketList("번지점프");

  Bucket<String> bucket = bucketList("번지점프");
  ```

  - EXAMPLE

  ```java
  public class Use {
	public static <K, V> V getValue(How<K, V> h, K k){
		if(h.getKey() == k) {
			return h.getValue();
		} else {
			return null;
		}
	 }
  }
  ```

  -----------------------------------------------------------------------------

### Restricted

  제한된 타입 파라미터(Restricted Type Parameter)는 구체적인 타입을 제한하는 것을 말합니다.

  예를들어 숫자 연산을 하는 `제네릭 메소드`는 매개값으로 Number타입이나 Integer, Long과
  같은 하위 클래스 타입만을 가져야 합니다.

  - Architecture

  ```java
  public <T extends 상위타입> ReturnType MethodName(arg) { ..... }
  ```

  즉, 위에서 __T extends 상위타입__ 이것의 의미는 T는 Type Parameter로서
  메소드 시그니처의 매개변수타입으로 쓸 수 있으며, extends뒤에 지정한 상위타입은
  다음과 같이 해석 할 수 있습니다.

  > 매개변수 값으로 상위타입 값이나 이것을 상속받은 하위 클래스 타입만을 가지겠다.

  - EXAMPLE

  ```java
  public class Use {
  public static <H extends How<K, V>, K, V> V getValue(H h, K k){
    if(h.getKey() == k) {
      return h.getValue();
    } else {
      return null;
    }
   }
  }
  ```
  위 프로그램을 해석 해 보겠습니다.

  <H extends How<K, V>, K, V>이것은 Type Parameter를 H로 선언하고 How<K, V> 제네릭
  타입 클래스와 그 것을 상속받은 하위 클래스 값만 H h의 h자리에 올 수 있고,
  리턴 값은 Value(V)이며, 두 번째 매개변수로는 Key값을 받겠다는 의미 입니다.
  if문에서는 How<K, V>의 getKey() `즉, Getter() 메소드`로 key값을 가져와서 두 번째
  매개값으로 받은 Key(k)와 비교하여 같으면 How<K, V>의 Value값을 리턴하는 것이고
  비교한 값이 다르다면, null을 리턴한다는 코드입니다.

  -----------------------------------------------------------------------------

### Wildcard Type

  와일드카드 타입(Wildcard Type)은 `?(물음표)`로 표시합니다.

  - 종류

    - 제네릭타입<?> : Unbounded Wildcards(제한 없음)

      타입 파라미터를 대치하는 구체적인 타입으로 모든 클래스나 인터페이스 타입이 올 수 있습니다.

    - 제네릭타입<? extends 상위타입> : Upper Bounded Wildcards(상위 클래스 제한)

      타입 파라미터를 대치하는 구체적인 타입으로 상위 타입이나 하위 타입만 올 수 있습니다.

    - 제네릭타입<? super 하위타입> : Lower Bounded Wildcards(하위 클래스 제한)

      타입 파라미터를 대치하는 구체적인 타입으로 하위 타입이나 상위 타입이 올 수 있습니다.

  -----------------------------------------------------------------------------

### Generic Array

  제네릭으로 배열을 만드는 방법은 다음과 같습니다.

  - 배열생성

  ```java
  public class Travle<T> {
    private String country;
    private T[] regions;

    public Travle(String country, int number) {
      this.country = country;
      regions = (T[]) (new Object[number]);
    }
  }
  ```

  T[] regions = new T[n]; 방식으로 배열을 생성할 수 없습니다.

  위 처럼 `(T[]) (new Object[number]);` 이 방식으로 배열을 생성해야 합니다.
