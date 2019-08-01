---
title: "Comparable vs Comparator"
layout: post
category: Java
tags: [Java]
excerpt: "Comparable와 Comparator의 차이점에 대해서 알아봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Comparable<T> vs Comparator<T>

  Comparable과 [Comparator](https://baekjungho.github.io/java-api)는 `배열과 리스트의 정렬`을 돕기위해 존재합니다.

  둘다 interface 타입이며, java.lang 패키지에 속합니다.

## Comparable<T> Interface

  Comparable<T> 인터페이스에는 객체나, 객체 내부의 필드를 비교할 수 있는 int compareTo() 메소드가 있습니다.

  > 메소드 : int compareTo(T o) --> T는 클래스(객체)를 나타냅니다.

  Comparable<T> interface를 구현한 클래스는 기본적으로 `natural ordering(오름차순)` 정렬이 됩니다.

  자바에서 같은 타입의 객체(instance)를 비교해야하는 클래스들은 전부 `Comparable`인터페이스를 구현 해야합니다.

  __즉, Comparable Interface는 객체를 비교해야하는 클래스에 구현되어지며, 오름차순으로 정렬합니다.__

  Comparable Interface로 `내림차순`을 구현할 수 있는데 방법은 기존 필드 값과 매개값으로 받은 o를 비교할때
  return값 순서를 뒤집어 주면 됩니다.

  Comparable은 반환값이 0과 음수일때는 자리바꿈을 진행하지 않으며, `반환값이 양수`인 경우에만 자리바꿈을 진행 합니다.

  - Architecture

  ```java
  public interface Comparable<T> {
    int compareTo(T o);
  }
  ```

  - __오름차순 리턴 방식__

    1. 필드명 > o.필드명 return 1 : 필드명이 o.필드명보다 클 경우 양수 리턴(o1이 o2뒤에 위치)
    2. 필드명 == o.필드명 return 0 : 필드명이랑 o.필드명이 같을 경우 Zero 리턴
    3. 필드명 < o.필드명 return -1 : 필드명이 o.필드명보다 작을 경우 음수 리턴(o1이 o2앞에 위치)

  - __내림차순 리턴 방식__

    1. 필드명 > o.필드명 return -1 : 필드명이 o.필드명보다 클 경우 음수 리턴(o1이 o2앞에 위치)
    2. 필드명 == o.필드명 return 0 : 필드명이랑 o.필드명이 같을 경우 Zero 리턴
    3. 필드명 < o.필드명 return 1 : 필드명이 o.필드명보다 작을 경우 양수 리턴(o1이 o2뒤에 위치)

  - TreeSet에서 Comparable을 적용한 예제

  ```java
  public class Student implements Comparable<Student1> {

	public String stuNum;
	public int score;

	public Student(String stuNum, int score) {
		this.stuNum = stuNum;
		this.score = score;
	}

  // 내림 차순으로 정렬
	@Override
	public int compareTo(Student1 o) {
		if(score>o.score) return -1;
		else if(score == o.score) return 0;
		else return 1;
	 }
  }

  import java.util.TreeSet;
  public class TreeSetExample {
	public static void main(String[] args) {
		TreeSet<Student1> treeSet = new TreeSet<Student1>();
		treeSet.add(new Student("201363240", 100));
		treeSet.add(new Student("201575642", 88));
		treeSet.add(new Student("201912975", 98));
    // treeSet.add(new Student("201858942", 88)); 점수가 같은 사람 존재

		Student student = treeSet.last();
		System.out.println("최고점수 : " + student.score);
		System.out.println("학번 : " + student.id);
	 }
  }
  ```

  위 코드에서 만약 점수가 같은 사람이 존재하면 compareTo()의 else if문에서 막힙니다.
  따라서 else if문 안에 또다른 조건을 줘서 정렬해줘야 합니다.
  만약 점수가 같은사람이 존재할 경우 학번 오름차순으로 정렬한다고 하면 다음과 같이 코딩하면 됩니다.

  ```java
  @Override
  public int compareTo(Student1 o) {
    if(score>o.score) return -1;
    else if(score == o.score) {
      // 점수가 같을 경우 학번 오름차순으로 정렬
      if(stuNum.compareTo(o.stuNum) > 0) return 1;
      else return -1;
    }
    else return 1;
   }
  ```

  else if문의 코드를 더 간결하게 작성하면 다음과 같습니다. else if문에서 사용한 compareTo()메소드는
  [문자열 비교](https://baekjungho.github.io/java-basic1)메소드 입니다.

  ```java
  else if(score == o.score) return stuNum.compareTo(o.stunum);
  ```

  Comparable 인터페이스를 구현하는 클래스들은 당연히 `int compareTo(T o)`메소드를 오버라이딩 하여야합니다.

  - 오버라이딩 사용 예제

  [문자열 크기 비교](https://baekjungho.github.io/java-basic1)

  ```java
  class Member implements Comparable<Member> {
    private String id;
    private String name;

    public Member(String id, String name){
      this.id = id;
      this.name = name;
    }

    public String getIdentify() {
      return id + ", " + name;
    }

    @Override
    public int compareTo(Member o){
      // 문자열 크기비교 String클래스의 메소드 a.compareTo(b) > 0 사용
      if(this.id.compareTo(o.id) > 0) return 1;
      else if(this.id.equals(o.id)) return 0;
      else return -1;
     }
    }
  }

  public class ComparableExample {
  public static void main(String[] args) {
    Member m1 = new Member("kkk", "John");
    Member m2 = new Member("GGG", "Mike");

    System.out.println(m1.compareTo(m2)); // 출력 : 1
   }
  }
  ```

  자바 API에 있는 [Arrays 클래스](https://baekjungho.github.io/java-api)의
  sort()메소드에 클래스타입을 넣고 싶을 경우는 그 클래스가 `Comparable Interface`를
  구현하고 있어야 하며, 메인메소드에서 `객체`를 생성하고 다시 `객체 배열`을 만들어 배열안에 생성한 `객체변수`를
  넣고 `sort(객체배열변수)`이런식으로 Sorting 해줘야 합니다.

  > 객체배열 : 배열 타입이 클래스(객체)타입이면서 `클래스명[] 변수 = { 객체값1, 객체값2, ..}` 형태를 지니는 것
  >
  > 즉, 객체에 대한 래퍼런스를 배열 원소로 가지는 것을 말합니다.

  - EXAMPLE

  ```java
  public class Member implements Comparable<Member> {
	   String name;

	public Member(String name) {
		this.name = name;
	 }

	@Override
	public int compareTo(Member o) {
		return name.compareTo(o.name); // 문자열 크기비교 compareTo() 메소드 사용
	 }
  }

  import java.util.Arrays;
  public class SearchExample {
	public static void main(String[] args) {
		int[] scores = { 99, 97, 98 };
		Arrays.sort(scores);
		int index = Arrays.binarySearch(scores, 99);
		System.out.println(index);

		String[] names = { "홍", "손", "사", "저" };
		Arrays.sort(names);

		index = Arrays.binarySearch(names, "홍길동");
		System.out.println(index);
    System.out.println(Arrays.toString(names)); // 출력

    // Comparable Interface를 구현한 Member객체 생성
		Member m1 = new Member("홍길동");
		Member m2 = new Member("손오공");
		Member m3 = new Member("사오정");
    Member m4 = new Member("저팔계");

    // 객체 배열을 생성하여 생성한 객체변수(m1, m2, m3)를 배열 값으로 저장
		Member[] members = { m1, m2, m3, m4 };

    // Arrays.sort()는 배열을 정렬하므로 객체배열변수 넣어주기
		Arrays.sort(members);

		index = Arrays.binarySearch(members, m1);
		System.out.println(index);
    System.out.println(Arrays.toString(members)); // 출력
	 }
  }
  ```

## Comparator<T> Interface

  Comparator<T> 인터페이스는 Comparable 인터페이스와 달리 내림차순 or 다른 기준으로 정렬하고 싶을때 사용됩니다.
  사용자가 정의한 기준으로 정렬할 수 있습니다.

  > ex) 필드명에 이름이나, id가 있을시 이름순, id순으로 정렬 가능

  즉, Comparable 인터페이스는 오름차순으로만 정렬하지만 Comparator 인터페이스는 그 외의 정렬도 가능합니다.

  Comparator<T> 내부에도 객체나 객체의 필드를 비교할 수 있는 int compare(T o1, T o2)가 있습니다.

  따라서, Comparator<T> 인터페이스를 구현 하는 클래스는 메소드 오버라이딩하여 재정의 해야 합니다.

  > compare() 메소드를 재정의하여 정렬 규칙을 정하는 것입니다.
  >
  > Interface 구현 클래스에서 import java.util.Comparator;를 적어줘야 합니다.

  일반적으로 Comparator를 만들 때 주로 `익명구현객체`를 만들어서 사용합니다.

  왜냐하면 Comparator를 사용하는것 자체가 `그 때 그 때마다 정렬 기준이 바뀔 수 있기 때문` 입니다.

  - Architecture

  ```java
  public interface Comparator<T> {
    int compare(T a, T b);
  }
  ```

  Comparator도 Comparable과 같이 매개변수의 위치나 리턴값의 순서를 변경시켜주면 오름차순, 내림차순을 구현 할 수 있습니다.

  - __오름차순 리턴 방식__

    1. o1 > o2 : 1(양수 리턴)
    2. o1 == o2 : 0(Zero 리턴)
    3. o1 < o2 : -1(음수 리턴)

  - __내림차순 리턴 방식__

    1. o1 > o2 : -1(음수 리턴)
    2. o1 == o2 : 0(Zero 리턴)
    3. o1 < o2 : 1(양수 리턴)

  - Comparator Interface 특징

  Comparator Interface를 사용하기 위해서는 클래스가 `최소 2개`가 존재합니다.

  1. Comparator<T>의 T에 들어갈 속성과, 생성자, 추가적으로 메소드까지 가진 클래스
  2. Comparator Interface를 구현한 클래스 or 익명구현객체
  3. 실행 클래스

  - 사용 예제

  ```java
  // Comparator<T>의 T에 들어갈 속성과, 생성자, 추가적으로 메소드까지 가진 클래스
  class Member {
    String id; // Field

    public Member(String id){ // Constructor
      this.id = id;
    }
  }

  // Comparator Interface를 구현한 클래스
  import java.util.Comparator;
  public class MemberComparator implements Comparator<Member> {
	// 이름 순으로 오름차순 정렬
    @Override
    public int compare(Member o1, Member o2){
    	return o2.id.compareTo(o1.id);
    }
  }

  // 실행 클래스
  import java.util.Arrays;
  public class ComparatorExample {
    public static void main(String[] args) {
	  Member John = new Member("John");
	  Member Mike = new Member("Mike");
	  Member Ellis = new Member("Ellis");

	  Member[] members = { John, Mike, Ellis };

	  // Arrays.sort(객체배열변수, Comparator 규칙 객체)
	  Arrays.sort(members, new MemberComparator());

	  /*
	  for(int i=0; i<members.length; i++) {
		  System.out.println(members[i].id);
	  }
	  */

	  for(Member names : members) {
		  System.out.println(names.id); // 객체.필드
	  }

	  // 정렬 후 John의 Index는 1
	  int index = Arrays.binarySearch(members, John, new MemberComparator());
	  System.out.println(index);
    }
  }
  ```

  __짚고 넘어가기 !__

  1. 배열정렬 : Arrays.sort(객체배열변수, Comparator 규칙 객체)
  2. 리스트정렬 : Collections.sort(리스트변수, Comparator 규칙 객체)

  > 사용예시 : Arrays.sort(members, new MemberComparator());

  - __EXAMPLE__

    시험성적순으로 정렬하고 싶고 만약 점수가 같을경우 학번 오름차순으로 정렬하고 점수가 다른 경우 점수 내림차순으로 정렬 하고 싶은 경우 다음과 같이
    코드로 나타낼 수 있습니다.

    ```java
    Arrays.sort(student, new Comparator<Student>() {
      @Override
      public int compare(Student s1, Student s2) {
        if(s1.score == s2.score) { // 점수가 같은 경우
          return Integer.compare(s1.id, s2.id); // 학번 오름차순
        } // 매개변수의 위치를 다르게 지정하여 내림차순으로 설정
          return Integer.compare(s2.score, s1.score); // 점수 내림차순
      }
    });
    ```

    [Arrays.sort()](https://baekjungho.github.io/java-api)에서 괄호 안에 들어간 타입이 클래스 이므로 `Comparable<T>` 구현

  - TreeSet에서 Comparator을 적용한 예제

  동물 클래스가 있고 필드로 이름과 나이가 있으며, 나이를 비교하여 오름차순 정렬하고
  나이가 같은 동물이 있을 경우 이름순으로 오름차순 정렬하는 문제

  ```java
  public class Animal {

	public String name;
	public int age;

	public Fruit(String name, int age) {
		this.name = name;
		this.age = age;
	 }
  }

  import java.util.Comparator;
  import java.util.TreeSet;

  public class ComparatorExample {
  public static void main(String[] args) {
  // 익명구현객체로 Comparator 구현   
	TreeSet<Animal> treeSet = new TreeSet<Animal>(new Comparator<Animal>() {
		@Override
		public int compare(Animal a1, Animal a2) {
			if(a1.age > a2.age) return 1;
			else if(a1.age < a2.age) return -1;
			else return a1.name.compareTo(a2.name);
		}
	});

	treeSet.add(new Animal("강아지", 8));
	treeSet.add(new Animal("고양이", 2));
	treeSet.add(new Animal("사자", 15));
	treeSet.add(new Animal("치타", 8));
	treeSet.add(new Animal("코뿔소", 10));

	for(Animal animal : treeSet) {
		System.out.println(animal.name + " : " + animal.age);
	}
  }
 }  
 ```

## Question

  [Comparable Quesiton1](https://baekjungho.github.io/question-solving2)

  [Comparable Quesiton2](https://baekjungho.github.io/question-solving2)
