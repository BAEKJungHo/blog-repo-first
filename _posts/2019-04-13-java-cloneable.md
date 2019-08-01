---
title: "Cloneable"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Shallow clone과 Deep Clone에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## cloneable

  자바의 복제방법에는 얕은 복제(Shallow copying), 깊은 복제(Deep copying)가 있습니다.

  자바의 Cloneable 인터페이스는 인스턴스가 복제 가능하도록 하기 위해서 구현해야 하는 인터페이스입니다.
  최상위 클래스인 Object 클래스는 객체의 클로닝을 위한 `Object.clone()` 메소드를 제공하고 있습니다.
  clone()을 사용해 객체 복사를 하여 객체 생성을 할 경우 객체의 생성자와 메소드를 실행하지 않고 객체 상태값들을 그대로 복사하게 됩니다. __원본 객체에는 아무런 영향이 없이 복제가 가능합니다.__ .

  > protected Object clone() throws CloneNotSupportedException

  clone()은 Cloneable인터페이스의 추상 메소드이며 Cloneable 인터페이스를 구현한 클래스만
  clone()메소드를 `public으로 오버라이딩` 하여 사용할 수 있습니다.

  ```java
  // Cloneable 인터페이스를 구현, clone()메소드를 오버라이딩
  // 객체가 힙영역에 가지고 있는 값이 복사되지만 main에서 형변환을 해줘야한다
  @Override
  public Object clone() throws CloneNotSupportedException {
    return super.clone();
  }
  ```

  clone()메소드가 지원되지 않을 수도 있기 때문에 CloneNotSupportedException 예외처리가 필요합니다.
  clone()메소드는 `깊은 복제(Deep copy)`를 이용한 방법으로서 주소값이아닌 힙영역에 저장된 객체의 값을 복사하여
  각자에게 영향을끼치지 않습니다.

### Shallow cloning

  Shallow cloning(얕은 복제)는 객체의 `reference(참조)` 즉, 주소값만 복사를 하여
  한쪽에서 값을 변경할경우 다른 한쪽에도 영향을 끼치는 복제를 말합니다.

  ![clone](/images/posts/201904/clone.jpg)

  메소드에서 매개변수로 [배열](https://baekjungho.github.io/question-solving1)을 받고 그 매개변수로 정렬(sort)을
  실행할 경우 매개변수는 원본 배열의 주소를 참조하고 있어서 원본 배열이 바뀌게 됩니다.
  따라서 원본배열은 그대로 유지 위해서는 배열을 복사하여 정렬해야 합니다.(Deep copying)

  문자열, 배열, List를 생성하여 값을 넣고 복제하는 예제를 만들어 확인해 보겠습니다.

  ```java
  package sec02;

  import java.util.ArrayList;
  import java.util.List;

  public class ShallowCopyExample {
  	public static void main(String[] args) {
  		String str1 = "Java";
  		String copyStr1 = str1;

  		System.out.println("-----------------------값 확인----------------------");

  		System.out.println("Original : " + str1 + "   -----   " + "Copy : " + copyStr1);

  		String str2 = new String("Maven");
  		String copyStr2 = str2;

  		System.out.println("Original : " + str2 + "   -----   " + "Copy : " + copyStr2);

  		int[] array1 = new int[] {1, 2, 3, 4, 5};
  		int[] copyArray1 = array1; // 배열 참조변수로 복사

  		int[] copyArray2 = new int[5];

  		System.out.print("Original : ");
  		for(int array : array1) {
  			System.out.print(array);
  		}

  		// copyArray2는 각각 원소로 복사
  		for(int i=0; i<array1.length; i++) {
  			copyArray2[i] = array1[i];
  		}


  		System.out.print("   -----   ");

  		System.out.print("Copy : ");
  		for(int array : copyArray1) {
  			System.out.print(array);
  		}

  		System.out.println();

  		List<String> list = new ArrayList<String>();
  		list.add("Java");
  		list.add("Python");

  		List<String> copyList1 = list; // 리스트 참조 변수로 복사
  		List<String> copyList2 = new ArrayList<String>();

  		// list 원소를 하나씩 대입해서 복사
  		for(int i=0; i<list.size(); i++) {
  			copyList2.add(list.get(i));
  		}


  		System.out.print("Original : ");
  		for(String str : list) {
  			System.out.print(str);
  		}

  		System.out.print("   -----   ");

  		System.out.print("Copy1 : ");
  		for(String str : copyList1) {
  			System.out.print(str);
  		}

  		System.out.print("   -----   ");

  		System.out.print("Copy2 : ");
  		for(String str : copyList2) {
  			System.out.print(str);
  		}

  		System.out.println(); System.out.println();
  		System.out.println("-----------------------메모리 번지 확인----------------------");
  		System.out.println("Str1 :" + str1.hashCode());
  		System.out.println("copyStr1 :" + copyStr1.hashCode());
  		System.out.println("Str2 :" + str2.hashCode());
  		System.out.println("copyStr2 :" + copyStr2.hashCode());
  		System.out.println("array :" + array1.hashCode());
  		System.out.println("copyArray :" + copyArray1.hashCode());
  		System.out.println("copyArray2 :" + copyArray2.hashCode());
  		System.out.println("list :" + list.hashCode());
  		System.out.println("copyList1 :" + copyList1.hashCode());
  		System.out.println("copyList2 :" + copyList2.hashCode());

  		System.out.println();

  		// 복제한 copyList에 문자열 추가
  		copyList1.add("Spring");
  		System.out.println("list :" + list);
  		System.out.println("copyList :" + copyList1);
  	}
  }
  ```

  ![result1](/images/posts/201904/result1.jpg)

  위 결과를 보면 복제 결과가 메모리번지가 같게 나온것을 보니 Shallow copying(얕은 복제)임을
  알 수 있습니다. 하지만 배열을 복제할 때 있어서 배열 참조 변수명으로 복제를 하면 주소가 같게 되지만, 반복문을 이용하여 배열원소를 대입하여 복제하면 `Deep copying`이 이루어 집니다. 반면 리스트는 copyList2.add(list.get(i)); 이 방식을 사용해도 주소 값이 같게 나옵니다. 마지막에 list에 Spring이라는 문자열을 추가하고 원본과 복제본을 비교해봤더니
  둘 다 값이 바뀌어있습니다. 따라서 얕은 복제는 같은 메모리번지를 공유하여 값을 참조한다는 것을
  알 수 있습니다.

  리스트에서 `addAll()`을 사용하면 깊은복제(Deep cloning)을 구현할 수 있습니다.

  ```java
  List<String> list = new ArrayList<String>();
  list.add("Java");
  list.add("Python");

  // Deep copying
  List<String> copyList = new ArrayList<String>();
  copyList.addAll(list);
  copyList.remove(1); // 원본에는 영향을 미치지 않음
  ```

  -----------------------------------------------------------------------------

### Deep cloning

  Deep cloning(깊은 복제)는 객체의 모든 값을 복사합니다. 즉, 서로 다른 주소를 가지게 되고
  각자에게 영향을 끼치지 않습니다.

  ![clone1](/images/posts/201904/clone1.jpg)

  - __EXAMPLE__

    ```java
    public class Member implements Cloneable {
    private int age;// Member객체의 age 속성
    private int id;// Member객체의 id 속성
    private String name;// Member객체의 name 속성
    private String address;// Member객체의 address 속성
    private String job;// Member객체의 job 속성

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone(); // 객체가 힙영역에 가지고 있는 값이 복사되지만 main에서 형변환을 해줘야한다
    }

    // Member 타입에 사용할 수 있는 copy()메소드 선언
    // 즉, main에서 형변환을 따로 해주지 않고 복제 할 수 있다.
    public Member copy() throws CloneNotSupportedException {
         Member member = (Member)clone();
        return member;
    }

    // getter와 setter대신에 생성자와 함수를 이용해서 값을 설정하고 출력
    public Member(int age, int id, String name, String address, String job) {
        super();
        this.age = age;
        this.name = name;
        this.id = id;
        this.address = address;
        this.job = job;
    }

    // 객체의 속성을 출력할 printCarAttribe 함수를 선언하고 정의
    public void printMemberAttribute() {
        System.out.println("나이 : "+age);
        System.out.println("ID : " +id);
        System.out.println("이름 : "+name);
        System.out.println("주소 : "+address);
        System.out.println("직업 : "+job);
     }
    }

    public class MemberApplication {
    public static void main(String[] args) throws CloneNotSupportedException {
        Member member = new Member(26, 10001, "John", "대전 서구", "Developer");

        // member객체의 힙 영역 값을 member2객체의 힙 영역에 저장
        Member member2 = (Member) member.clone(); // Shallow Copy

        Member member3 = member.copy(); // copy()메소드에서 형변환을 미리 해줘서 따로 필요 없다

        member.printMemberAttribute();
        System.out.println();
        member2.printMemberAttribute();
        System.out.println();
        member3.printMemberAttribute();

        System.out.println(member);//car 객체의 주소 출력
        System.out.println(member2);//car2 객체의 주소 출력
        System.out.println(member3);//car3 객체의 주소 출력
     }
    }
    ```

    ![result](/images/posts/201904/result.jpg)

    위 결과를 보면 객체 값이 완전히 복사되고, 주소도 다른 것을 볼 수 있습니다.

    즉, Cloneable를 구현하여 clone()메소드를 오버라이딩하여 객체를 복제하는 방법은
    Deep copying(깊은 복제) 방법입니다.

    얕은 복제(Shallow copying)와 깊은 복제(Deep copying)의 차이점은 주소 값을 복사하면 얕은 복제이며, 실제 값을 복제 하는 경우
    깊은 복제 입니다.

  - __List addAll()을 사용한 Deep copying__

  ```java
  import java.util.*;

  public class MarathonApplication {
  public static void main(String[] args) {
    List<String> participants = new ArrayList<String>(); // 참가자

    participants.add("홍길동"); participants.add("홍길동"); participants.add("홍길동");
    participants.add("손오공"); participants.add("저팔계"); participants.add("사오정");
    participants.add("손오공"); participants.add("신용재"); participants.add("신용재");
    participants.add("윤민수"); participants.add("류재현"); participants.add("양다일");

    System.out.print("참가자 : ");
    for(String str : participants) {
      System.out.print(" " + str);
    }
    System.out.println();

    complete(participants);

  }

  public static void complete(List<String> list) {
    Random random = new Random();
    int fail = random.nextInt(12)+1;
    List<String> completioners = new ArrayList<String>();
    // List 복사를 위해 addAll()사용
    completioners.addAll(list);
    System.out.println("미완주 : " + completioners.get(fail));
    completioners.remove(fail);

    System.out.print("완주 : ");
    for(String str : completioners) {
      System.out.print(" " + str);
    }
   }
  }
  ```
