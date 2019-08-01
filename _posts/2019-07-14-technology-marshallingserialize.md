---
title: "Marshalling vs Serialization"
layout: post
category: Technology
tags: [Java]
excerpt: "Marshalling(마샬링) vs Serialization(직렬화)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Marshalling(마샬링) vs Serialize(시리얼 라이즈)에 대해서 배워 봅시다.

## Serialization

  Serialization은 객체의 상태를 저장하기 위해서 `객체를 Byte Stream 으로 변환`하는 것을 의미합니다. 그 반대는 deserialization 이라고 합니다.

  Object(객체)를 하나의 프로그래밍에서만 사용한다면 큰 어려움이 없으나 Object를 binary 파일로 저장한다던가 통신 등을 통해 다른 프로세스로 전달을 해야할 경우 이를 Stream으로 전달하여야 하나 이는 byte와 같은 Primitive 변수만 전달될 수 있다는 단점이 있습니다. 이를 해결한 방법으로서 __Object를 Serialization(직렬화)하여 Stream으로 Object의 값을 전달하거나 저장__ 할 수 있게 된 것입니다.

  ![s1](/images/posts/201907/s1.jpg)

  Serialization을 하는 이유는 자신이 가지고 있는 Object가 상대에게는 어떤 방식으로 표현되어 있는지 알 수 없기 때문에 이를 전달하기 전에 Object가 가지고 있는 값을 직렬화시켜 Object의 값을 받고자 하는 쪽에서 분별할 수 있게 만들어줍니다. 직렬화 된 값이 상대에게 도착한 후에는 이를 다시 역직렬화(Deserialization)를 거쳐 자신이 갖고 있었던 Object값으로 다시 만들어줍니다.

  > 직렬화는 네트워크 의존성이 높은 소프트웨어일 경우, 전체 성능을 좌우할 수도 있는 중요한 기술 입니다.

### 적용 분야

  - 파일 저장소(File Storage)
    - 프로그램 실행 중에 생성된 데이터를 영구 저장소(파일 시스템)등에 저장한 후, 이후에 프로그램이 다시 실행 되었을 때, 저장된 데이터를
    메모리상에 객체 형태로 복구해서 사용합니다.

  - 네트워크 통신(Network Communication)
    - 네트워크 상에 떨어져 있는 프로그램 간에 데이터를 주고 받기 위해 데이터를 직렬화 한 후 패킷(Packet)에 담아 전송합니다.

  - 데이터베이스(DataBase)
    - 복잡한 형태의 객체를 DB에 저장할 때 직렬화한 문자열 형태로 테이블의 컬럼에 저장하기도 합니다.

  - 웹 환경(Web Enviornment)
    - 웹 서버에서 브라우저(클라이언트)로 구조화된 데이터를 전송할 때 직렬화 한 후 JSON 형식 등
    전달하는 방식이 점차 많이 사용되고 있습니다.

### 직렬화 데이터 형식

  - Binary
    - 메모리에 저장된 데이터를 최소한의 가공 또는 가공 없이 바이트의 연속된 형태로 저장하는 방식.

  - JSON(Javascript Object Notation)
    - 텍스트 형식이므로 사람과 기계가 모두 읽기 가능. 다양한 프로그래밍 언어에서 읽고 쓸 수 있기때문에 널리사용.

  - XML(eXtensible Markup Language)
    - 텍스트 형식이며, JSON에 비해 복잡, JSON에 대해 가지는 장점은 스키마(Schema)를 적용할 수 있고 무결성 검사가 가능.

  - YAML(YAML Ain't Markup Language)
    - XML에 비해 사람이 읽고 쓰기 쉽도록 고안된 Markup 언어. 문법이 상대적으로 단순하고, 가독성이 높게 설계되어 있다.

### 구현 기법에 따른 성능 차

  - Native memory copying using C operations
    - 객체가 할당되어 있는 메모리 자체를 C 함수를 이용해 복사

  - Unsafe operations
    - 본래 자바 코어 개발자들이 Low-Level 프로그래밍 하기 위해 만든 API. JNI(Java Native Interface)와 비슷하거나 유사한 성능을 가짐.

  - Ignore Object Introspection
    - Introspection은 Reflection과 유사한 기술이다. 다만, Introspection은 자바의 instanceof 연산자 처럼 객체의 타입 정보만 조회.

  - Direct object-object copying
    - 자바 코드로 객체 내의 모든 멤버 변수를 복사하는 로직을 구현하는 것.

  > 성능이 중요한 이유 !! CPU 비용, 메모리 비용, 네트워크 비용

## Serialization FrameWork

### JDK's Serializable

  - JDK의 Serializable 인터페이스
    - 프로그래밍 하기 가장 쉽고, Serializable 인터페이스를 통해 별도의 라이브러리 없이 즉시 사용 가능.
    - 클래스를 Release 한 후에는 구현을 변경하기 어려워 유연성(Flexibility)을 감소 시킨다.
    - C++, Python등 다른 언어로 구현된 프로그램과 데이터를 교환(Exchange)할 수 없다.
    - 기본 연산자의 취약점(Hole)으로 인해 불변 값이 손상되거나, 비정상적인 접근이 발생할 수 있다.
      - Invariant Corruption and Illegal Access
    - 커스터마이징(Customization)이 불가능하고, 소스 코드를 수정할 수 있어야 한다.

  - 직렬화 조건 : `java.io.Serializable` 인터페이스를 상속받은 객체

  ```java
public class Member implements Serializable {
  private String name;
  private String email;
  private int age;

  public Member(String name, String email, int age) {
      this.name = name;
      this.email = email;
      this.age = age;
  }
  @Override
  public String toString() {
      return String.format("Member{name='%s', email='%s', age='%s'}", name, email, age);
  }
}
  ```

  - 직렬화 : `java.io.ObjectOutputStream`을 사용하여 구현

  ```java
public static void main(String[] args){
  Member member = new Member("BAEK", "BAEKJH@naver.com", 26);
  byte[] serializedMember;
  try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
      try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
          oos.writeObject(member);
          // serializedMember -> 직렬화된 member 객체
          serializedMember = baos.toByteArray();
      }
  }
  // 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
  System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
  ```

  ByteArrayInputStream과 ByteArrayOutputStream은 메모리, 즉 `바이트 배열에 데이터를 입출력하는데 사용되는 스트림`입니다. 주로 다른곳에 입출력하기 전에 데이터를 임시로 바이트 배열에 담아서 변환등의 작업을 하는데 사용됩니다.

  > ByteArrayInputStream과 같이 메모리를 사용하는 스트림과 System.in, System.out과 같은 표준입출력 스트림은 닫아주지 않아도 됩니다.

  ObjectOutputStream, ObjectInputStream은 자바에서 객체 안에 저장되어 있는 내용을 파일로 저장하거나 네트워크를 통하여 다른 곳으로 전송하려면 객체를 바이트 형태로 일일이 분해해야 하는데 이를 위하여 `객체를 직접 입출력 할 수 있도록 해주는 객체 스트림`입니다.

  - 역직렬화 조건
    - 직렬화 대상이 된 객체의 클래스가 클래스 패스에 존재해야 하며 import 되어 있어야 합니다.
    - 중요한 점은 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있다는 것을 반드시 고려해야 합니다.
    - 자바 직렬화 대상 객체는 동일한 `serialVersionUID` 를 가지고 있어야 합니다.
    - private static final long serialVersionUID = 2224577561626809324L;

  - 역직렬화 : `java.io.ObjectInputStream`을 사용하여 구현

  ```java
public static void main(String[] args){
  // 직렬화 예제에서 생성된 base64 데이터
  String base64Member = "...생략";
  byte[] serializedMember = Base64.getDecoder().decode(base64Member);
  try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)) {
      try (ObjectInputStream ois = new ObjectInputStream(bais)) {
          // 역직렬화된 Member 객체를 읽어온다.
          Object objectMember = ois.readObject();
          Member member = (Member) objectMember;
          System.out.println(member);
      }
  }
}
  ```

#### 자바 직렬화 단점

  자바 직렬화 단점으로는 `역직렬화시 클래스 구조 변경 문제`가 있습니다.

  위에서 직렬화에 사용했던 객체를 직렬화한 DATA는 다음과 같습니다.

  ```
  rO0ABXNyABp3b293YWhhbi5ibG9nLmV4YW0xLk1lbWJlcgAAAAAAAAABAgAESQADYWdlSQAEYWdlMkwABWVtYWlsdAASTGphdmEvbGFuZy9TdHJpbmc7TAAEbmFtZXEAfgABeHAAAAAZAAAAAHQAFmRlbGl2ZXJ5a2ltQGJhZW1pbi5jb210AAnquYDrsLDrr7w=
  ```

  위 객체에서 속성을 추가해 보겠습니다.

  ```java
 public class Member implements Serializable {
    private String name;
    private String email;
    private int age;
    // phone 속성을 추가
    private String phone;
  }
  ```

  - 직렬화한 Data를 역직렬화하면 어떤 결과가 나올까요? > 결과는 `java.io.InvalidClassException`이 발생합니다.
  - 위에서 언급했던 것처럼 직렬화하는 시스템과 역직렬화하는 시스템이 다른 경우에 발생하는 문제입니다.
  - 각 시스템에서 사용하고 있는 Model의 Version 차이가 발생했을 경우에 생기는 문제입니다.

  - String -> StringBuilder, int -> long으로 변경해도 역직렬화에서 Exception이 발생합니다.
  - 자바 직렬화는 상당히 타입의 엄격하다는 것을 알 수 있습니다.
  - 멤버 변수가 빠지게 된다면 Exception 대신 null값이 들어가는 것을 확인할 수 있습니다.
  - 일반적인 메모리기반의 Cache에서는 Data를 저장할 수 있는 용량의 한계가 있기 때문에 Json 형태와 같은 경량화된 형태로 직렬화하는 것도 좋은 방법입니다.

#### 해결책

  - Model의 Version간의 호환성을 유지하기 위해서는 `SUID(serialVersionUID)`를 정의해야 합니다.
  - Default는 클래스의 기본 해쉬값을 사용합니다.

#### 정리

  - 외부 저장소로 저장되는 데이터는 짧은 만료시간의 데이터를 제외하고 자바 직렬화를 사용을 지양합니다.
  - 역직렬화시 반드시 예외가 생긴다는 것을 생각하고 개발합니다.
  - 자주 변경되는 비즈니스적인 데이터를 자바 직렬화을 사용하지 않습니다.
  - 긴 만료 시간을 가지는 데이터는 JSON 등 다른 포맷을 사용하여 저장합니다.

### Java Externalization

  - 직렬화 코드를 직접 구현
    - 객체를 저장(Persist)및 복구(Restore)하는 Externalizable 인터페이스를 구현해 직접 직렬화를 구현.
    - 객체의 컨텐츠를 저장하고 복구하는 역할을 수행하는 클래스를 구현해야 한다.
    - 클래스의 구조가 변경 될 때 마다, 읽고 쓰는 코드를 수정해야 한다.

  ```java
  public void writeExternal(ObjectOutput out) throws IOException {
    out.writeInt(getId());
    out.writeObject(getName());
    out.writeDouble(getPrice());
  }

  public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
    setId(in.readInt());
    setName((String)in.readObject());
    setPrice(in.readDouble());
  }
  ```

### Google GSON

  - Google GSON
    - 자바 객체를 JSON으로 변환하거나 반대의 작업을 수행하는 자바 라이브러리.
    - 직렬화된 객체의 소스를 필요로 하지 않는다.
    - 커스텀 표현(Custom Representatives)을 지원.

  ```java
  class Item {
    @Since(2.0)
    private String currentName;
    @Since(1.0)
    private String newName;
    private String name;

    public static void main(String[] args) {
      Item versionItem = new Item();
      Gson gson = new GsonBuilder().serVersion(1.0).create();
      String json = gson.toJson(versionItem);
    }
  }
  ```

### Jackson JSON

  - Jackson JSON
    - 고성능, 인간공학적 JSON 프로세서 자바 라이브러리.
    - 광범위한 커스터마이징 툴 제공.
    - 혼합 어노테이션(Mix-in Annotation)
    - 실체화된 인터페이스(Materialized Interfaces)
    - 다양한 데이터 포맷(JSON, CSV, Smile(binary JSON), XML, YAML)

### BSON for Jackson

  - BSON for Jackson
    - 바이너리 인코딩된 JSON(Binary encoded JSON)
    - 몽고 DB의 주된 데이터 교환 포맷(Main data exchange format for MongoDB)
    - 확장 프로그램 작성 가능(Allows writing custom extensions)

### Protocol Buffers

  - Protocol Buffers
    - 구조적인 데이터를 확장 가능하며 효율적인 포맷을 변환하는 방법을 제공
    - 구글 내부에서 대부분의 내부 RPC 프로토콜과 파일 포맷에 Protocol Buffers를 사용 중
    - Java, C++, Python 사용

### Kyro

  - Kyro
    - 빠르고 효율적인 객체 그래프 직렬화 자바 프레임워크
    - 구글 코드 상의 오픈소스 프로젝트
    - 자동화된 깊고 얕은 복제(Automatic deep and shallow copying/cloning)
    - 소스 클래스에 대한 코드 작성 요건이 거의 없음(Doesn't put requirements on the source classes in most cases)

## Marshalling

  직렬화는 자료구조 혹은 객체 상태를 저장(파일,메모리 버퍼, 네트워크로 전송 등에 사용될)될 수 있는 포맷으로 변환하는 것을 말합니. 그리고 다른 컴퓨터 환경에서 같은 것으로 재조립이 가능하다. 객체지향객체의 직렬화는 객체와 관계된 메소드를 포함하지 않습니다. 이것을 마샬링이라고도 부릅니다. 마샬링은 객체의 메모리 표현을 저장공간 또는 전송에 적합한 데이터 포맷으로 변환하는 과정이다. 이것은 데이터가 프로그램에서 다른 프로그램으로 이동할 때 전형적으로 사용됩니다.

  이런 의미에서 직렬화는 마샬링을 수행하기 위한 한 수단으로 볼 수 있습니다.

  마샬링에서는 오브젝트에 있는 `멤버데이터 + 코드베이스`가 전해집니다. So Serialization is part of Marshalling. 따라서 `직렬화는 마샬링의 일부`입니다.

  __코드베이스는 오브젝트 코드의 위치를 담고 있습니다. 그래서 코드가 로컬에 없어서 수행할 수 없을 시 코드베이스에 담겨진 위치로 가서 코드를 로컬에 다운을 받아서 수행할 수 있습니다.__

## 차이점

  Marshalling과 Serialization의 차이점은 다음과 같습니다.

  Marshalling 과 Serialization 은 `원격 프로시저 호출(remote procedure call)` 측면에서는 유사하지만 Marshalling은 이곳에서 저곳으로 Parameter를 전달하는 방식이며, Serialization은 구조화된 데이터 를 Byte Stream 과 같은 Primitive 형식 혹은 그 반대로 복사를 하는 것을 의미힌다. 이러한 의미에서 Serialization은 Marshalling 의 pass-by-value 를 구현하는 것의 일종으로 볼 수 있다.

  > Marshalling 은 추가적인 메타 데이터(코드 베이스)를 가질 수 있다는 것에서 Serialization 과 구별됩니다.

## 참조

  > [http://woowabros.github.io/experience/2017/10/17/java-serialize.html](http://woowabros.github.io/experience/2017/10/17/java-serialize.html)
  >
  > [https://nesoy.github.io/articles/2018-04/Java-Serialize](https://nesoy.github.io/articles/2018-04/Java-Serialize)
  >
  > [https://en.wikipedia.org/wiki/Serialization](https://en.wikipedia.org/wiki/Serialization)
  >
  > [https://stackoverflow.com/questions/770474/what-is-the-difference-between-serialization-and-marshaling](https://stackoverflow.com/questions/770474/what-is-the-difference-between-serialization-and-marshaling)
