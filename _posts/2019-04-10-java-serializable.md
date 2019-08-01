---
title: "Serializable"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 직렬화(Serializable)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Serializable

  자바에서는 `Serializable Interface`를 구현한 클래스만 직렬화가 가능합니다.

  > public class XXX implements Serializable { }

  객체를 직렬화하면 바이트로 변환되는 것은 `필드`입니다.(생성자 X, 메소드 X)
  따라서 역직렬화를 하면 `필드의 값만 복원`됩니다.

  __단, 필드명 앞에 static이나 transient 키워드가 붙어있으면 직렬화가 불가능 합니다.__

  > transient : Serialize하는 과정에 제외하고 싶은 경우 선언하는 키워드입니다.

  - transient 사용시 판단 해야 할 점

    1. 실제로 필요가 없는지에 대한 고려.
    2. Data를 제외하였을 경우에 서비스 장애에 이상이 없는지에 대한 고려.

  ```java
  public class XXX implements Serializable {
    public int field1; // Byte data
    protected int field2; // Byte data
    default int field3; // Byte data
    private int field4; // Byte data
    XXXclass filed5 = new XXXclass(); // Byte data
    public static int field6;
    private transient int field7;

    public XXX() { }
    public int getField1() {
      return this.field1;
    }
  }
  ```

  위에서 직렬화(Serializable)가 되서 `Byte data`가 된 필드들은 field1 ~ field5까지 입니다.
  생성자와 메소드 field6, field7은 직렬화가 불가능 합니다.

  - ClassA

    ```java
    import java.io.Serializable;

    public class ClassA implements Serializable {
    	public int field1;
    	ClassB field2 = new ClassB();
      ClassC field3 = new ClassC();
    	static int field4; // 직렬화 불가능
    	public transient int field5; // 직렬화 불가능
    }
    ```

  - ClassB

    ```java
    import java.io.Serializable;

    public class ClassB implements Serializable {
    	int field1; // 직렬화 대상
    }
    ```

  - ClassC

    ```java
    import java.io.Serializable;

    public class ClassC implements Serializable {
      int field1; // 직렬화 대상
    }
    ```

  - Serializable / DeSerializable

    ```java
    import java.io.FileOutputStream;
    import java.io.ObjectOutputStream;

    public class SerializableExample {
	  public static void main(String[] args) throws Exception {
		  FileOutputStream fos = new FileOutputStream("C:/temp/Serialize.dat");
		  ObjectOutputStream oos = new ObjectOutputStream(fos);

		  ClassA classA = new ClassA();
		  classA.field1 = 10;
		  classA.field2.field1 = 20;
		  classA.field3.field1 = 30;
		  classA.field4 = 40;
      classA.field5 = 50;

		  oos.writeObject(classA);
		  oos.flush(); oos.close(); fos.close();
	   }
    }

    import java.io.FileInputStream;
    import java.io.ObjectInputStream;

    public class DeSerializableExample {
    public static void main(String[] args) throws Exception {
      FileInputStream fis = new FileInputStream("C:/temp/Serialize.dat");
      ObjectInputStream ois = new ObjectInputStream(fis);

      ClassA v = (ClassA)ois.readObject();
      System.out.println(v.field1); // 10
      System.out.println(v.field2.field1); // 20
      System.out.println(v.field3.field1); // 30
      System.out.println(v.field4); // 0
      System.out.println(v.field5); // 0
      }
    }
    ```

    ---------------------------------------------------------------------------

### serialVersionUID

  serialVersionUID는 같은 클래스임을 알려주는 식별자 역할을 합니다.

  직렬화나 역직렬화나 둘 다 같은 클래스를 사용해야하며, 클래스의 내용이 변경되서는 안됩니다.
  만약 변경되거나 다른 클래스를 사용했을경우 serialVersionUID = -1523473240242이런식과 같은
  오류가 발생합니다.

  serialVersionUID는 컴파일 시에 자동적으로 정적 필드로 추가되는데 `재컴파일`을 하게 되면
  serialVersionUID가 변하게 됩니다. 예를들어서 직렬화때 사용했던 클래스를 재컴파일 한 후
  역직렬화를 실행하면 예외가 발생합니다.

  만약, serialVersionUID값을 고정시키고 싶으면 상수로 선언 해주면 됩니다.

  ```java
  public class XXX implements Serializable {
    static final long  serialVersionUID = "정수값";
  }
  ```

  정수값은 cmd실행 후 `serialver 클래스명`을 입력후 나오는 serialVersionUID값을 복사하여
  붙여 넣어주면 됩니다.

  가급적이면 클래스마다 다른 serialVersionUID값을 갖는게 좋고 자바에서 JDK\bin아래에
  serialver.exe(자동으로 UID값 생성)을 제공하고 있습니다.

  -----------------------------------------------------------------------------

### 상속관계의 직렬화

  만약 부모클래스는 Serializable Interface를 구현하고 있지 않고, 자식만 구현하고 있을 경우
  부모 필드는 직렬화 대상에서 제외됩니다. 이때 부모클래스 필드를 직렬화 하고 싶은경우
  자식클래스에서 `writeObject() 메소드와 readObject() 메소드`를 구현 해주면 됩니다.

  > writeObject() : 직렬화 될때 자동으로 호출
  >
  > readObject() : 역직렬화 될때 자동으로 호출

  ```java
  public class Parent {
    public String field1;
  }

  public class Child extends Parent implements Serializable {
    public String field2;

    private void writeObject(ObjectOutputStream out) throws IOException {
      out.writeUTF(field1);
      out.defaultWriteObject();
    }

    private void readObject(ObjectInputStream in) throws Exception {
      field1 = in.readUTF();
      in.defaultReadObject();
    }
  }
  ```
