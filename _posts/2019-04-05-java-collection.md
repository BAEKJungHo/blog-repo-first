---
title: "Collection"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 컬렉션 List, Set, Map에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Collection

  __컬렉션(Collection)__ 은 사전적 의미로 요소를 수집해서 저장하는 것을 말합니다.

  자바 컬렉션은 `객체를 수집해서 저장`하는 역할을 합니다.

  > FrameWork : 사용방법을 미리 정의해 놓은 라이브러리

  즉, 자바의 컬렉션 프레임워크는 배열에서 추가, 검색, 삭제시 배열의 인덱스가 비어있는
  문제점을 개선하기 위해서 객체를 효율적으로 추가, 검색, 삭제를 할 수 있게 만들어 주는
  java.util에 포함되어있는 컬렉션 라이브러리 입니다.

  ![collection](/images/posts/201904/collection.jpg)

  위 그림은 각 인터페이스의 분류를 나타낸 것입니다.

  > Collection Interface : List와 Set의 공통적인 메소드를 정의한 인터페이스

  - 특징

    - List : 순서를 유지하며, 중복 저장 가능
    - Set : 순서를 유지하지 않으며, 중복 저장 불가능 (중복제거 효과, 넣을때 꺼낼때의 순서가 다름)
    - Map : 키와 값으로 저장하며, 키는 중복 저장 불가능

  제네릭타입을 사용하는 컬렉션들은 `Diamond Operator`를 사용합니다.

## List

  리스트 컬렉션은 객체를 일렬로 늘어 놓은 구조를 가지고 있습니다.
  __객체를 인덱스(index)로 관리__ 하며 __객체 저장시 자동으로 인덱스가 부여__ 됩니다.
  중복된 객체를 저장할 수 있습니다.(동일한 메모리 번지 참조)

  > 리스트는 빈 엘리먼트를 허용하지 않습니다.

  ![list1](/images/posts/201904/list1.jpg)

  - __객체 추가 메소드__

  |메소드명   |특징|
  |-----------|----|
  |boolean add(E e)|  객체를 맨 끝에 추가|
  |void add(int index, E element)|  해당 인덱스에 객체를 추가|
  |E set(int index, E element)|  해당 인덱스에 저장된 객체를 값으로 주어진 객체로 변경|

  - __객체 검색 메소드__

  |메소드명   |특징|
  |-----------|----|
  |boolean contains(Object o)|  해당 객체가 저장되어 있는지 여부|
  |E get(int index)|  주어진 인덱스에 저장된 객체를 리턴|
  |boolean isEmpty()|  컬렉션이 비어 있는지 조사|
  |int size()|  저장된 객체의 개수를 리턴|

  - __객체 삭제 메소드__

  |메소드명   |특징|
  |-----------|----|
  |void clear()|  저장된 모든 객체를 삭제|
  |E remove(int index)|  해당 인덱스에 있는 객체를 삭제|
  |boolean remove(Object o)|  주어진 객체를 삭제|

  - EXAMPLE

    ```java
    List<String> list = new ArrayList<>(); // 컬렉션 생성
    list.add("John") // 맨끝에 해당 객체 추가
    list.add(1, "Mike") // 해당 인덱스에 객체 삽입
    String name = list.get(1) // 해당 인덱스에 저장된 객체 찾기
    list.remove(1); // 해당 인덱스에 저장된 객체 삭제

    for(int i=0; i<list.size(); i++) {
      names[i] = list.get(i); // get()으로 저장된 객체 가져오기
    }
    ```

  for문을 이용해서 변수에 인덱스에 저장된 값을 얻어오고 싶을 경우 `list참조변수.get(인덱스번호)`를 사용하면 됩니다.
  반면에, 인덱스 번호가 필요 없을경우 [Enhanced For-Loop](https://baekjungho.github.io/java-operator/#enhanced-for-loop)을 사용하면 됩니다.

  -----------------------------------------------------------------------------

### ArrayList

  ArrayList<E>는 `List 인터페이스의 구현 클래스`입니다.

  배열과 비슷하지만 가장 큰 차이점은 배열과 달리 `capacity(용량)`이 자동으로 늘어납니다.
  즉, 배열은 크기가 고정되어있는 저장공간에 값을 넣고, 값을 빼도 저장공간의 크기는 그대로 입니다.
  반면에, `ArrayList`는 값을 넣으면 저장공간이 자동으로 늘어나고 값을 삭제하면 저장공간의 크기가 줄어듭니다.

  - __배열과 ArrayList의 차이점__

    ex) 크기 20의 배열과, ArrayList가 있다고 가정.

    - 배열 인덱스8에 저장된 값 삭제

      배열 인덱스5 자리의 값은 비어있지만, 배열의 크기는 그대로이며 원래 가지고있던 인덱스 번호를 그대로 가지게 된다.

    - ArrayList 인덱스8에 저장된 값 삭제

      인덱스8에 저장된 객체가 사라지며, 바로 뒤의 인덱스(9~19)까지의 인덱스 번호가 한 칸씩 앞으로 땡겨진다.
      즉, 인덱스 9가 인덱스 8의 번호를 가지게 되고 인덱스 10이 인덱스 9의 번호를 가지게 된다.

  - __사용방법__

    ```java
    List<String> list = new ArrayList<>(10); // 크기 10의 객체를 저장할 수 있습니다.
    // 크기 초과시 capacity가 자동으로 늘어납니다.
    ```

  - __ArrayList vs LinkedList__

    |종류|ArrayList|LinkedList|
    |----|---------|----------|
    |중간에 추가/삭제| 느림|  빠름|
    |순차적 추가/삭제| 빠름|  느림|
    |인덱스 조회| 빠름|  느림|

  [ArrayList를 사용한 예제](https://baekjungho.github.io/question-solving2)

  -----------------------------------------------------------------------------

### Vector

  Vector는 ArrayList와 동일한 내부구조를 가지고 있습니다. 또한 `synchronized method`로 되어있어서
  스레드가 동시에 접근이 불가능합니다.

  __따라서, 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제 할 수 있습니다.(Thread Safe)__

  - __사용방법__

    ```java
    List<E> list = new Vector<E>();
    ```

  -----------------------------------------------------------------------------

### LinkedList

  LinkedList는 ArrayList와 내부 구조가 완전히 다릅니다.

  - __Single LinkedList__

    ![link1](/images/posts/201904/link1.jpg)

  LinkedList에서 가장 중요한 것은 `노드의 구현`입니다.
  `노드는 실제로 데이터가 저장되는 그릇`과 같은 것이기 때문에 이것부터 구현을 시작합시다.
  HEAD는 첫 번째 노드를 가리키는 참조값이고 SIZE는 노드의 크기, TAIL은 마지막을 지정합니다.

  - __노드의 구성__

    1. 데이터(data)
    2. 다음 노드 참조값(next)

    > Node = data + next

  - __Double LinkedList__

    ![link2](/images/posts/201904/link2.jpg)

  Double LinkedList는 데이터 삭제시 ArrayList 처럼 인덱스가 앞으로 한 칸씩 당겨 지는 것이 아니라
  삭제할 객체의 링크를 끊고 삭제된 객체의 앞 링크와 삭제된 객체의 뒤 링크를 서로 이어주는 것입니다.
  LinkedList는 처음 생성시에 링크는 비어있는 상태 입니다.

  - __사용방법__

    ```java
    List<E> list = new LinkedList<E>();
    ```

## Set

  Set은 `집합`이라고 생각하면 됩니다. `순서 없이 저장하고, 중복 저장이 불가능`합니다.
  null값도 하나만 저장 가능합니다. __즉, 집합 효과 때문에 중복제거 효과가 있습니다.__

  treeSet을 제외한 Set 컬렉션들은 저장할때와 빼낼때(찾을때)의 순서가 다를 수 있습니다.
  그 이유는 `순서 없이 저장`하기 때문입니다. 하지만 treeSet은 이진트리의 효과때문에 오름차순정렬 기능이 있어서
  정렬된 상태로 출력됩니다.

  따라서 List와 Set중에서 중복저장을 허용하지 않겠다(중복을 없애겠다)라고 하면 Set을 이용하면 됩니다.

  - __객체 추가 메소드__

  |메소드명   |특징|
  |-----------|----|
  |boolean add(E e)|  객체를 저장하고 저장완료시 true, 아닐 시 false 리턴|

  - __객체 검색 메소드__

  |메소드명   |특징|
  |-----------|----|
  |boolean contains(Object o)|  해당 객체가 저장되어 있는지 여부|
  |Iterator<E> iterator()|  저장된 객체를 한 번씩 가져오는 반복자 리턴|
  |boolean isEmpty()|  컬렉션이 비어 있는지 조사|
  |int size()|  저장된 객체의 개수를 리턴|

  - __객체 삭제 메소드__

  |메소드명   |특징|
  |-----------|----|
  |void clear()|  저장된 모든 객체를 삭제|
  |boolean remove(Object o)|  주어진 객체를 삭제|

  - __Iterator 인터페이스의 메소드__

  |메소드명   |특징|
  |-----------|----|
  |hasNext()|  가져올 객체가 있으면 true, 없으면 false 리턴 (return type : boolean)|
  |next()|  컬렉션에서 하나의 객체를 가져온다. (return type : E)|
  |remove()|  객체를 제거한다|

  Iterator를 사용하여 제거를 할 때에는 항상 가져올 객체가 있는지 확인하는게 좋습니다.

  ```java
  while(iterator.hasNext()) { // 가져올 객체가 있는지 확인하는 반복문, 객체 수 만큼 반복
    String name = iterator.next(); // name에 객체 하나씩 저장 후 비교
    if(name.equals("John")) {
      iterator.remove(); // Set 컬렉션의 객체 삭제
    }
  }
  ```

  -----------------------------------------------------------------------------

### HashSet

  HashSet은 Set 인터페이스의 구현 클래스 입니다. 사용방법은 ArrayList와 비슷합니다.

  - __사용방법__

    ```java
    Set<E> set = new HashSet<E>();
    ```

  - __동등객체판단 : hashCode(), equals() 오버라이딩__

    HashSet에 객체를 저장할때는 Set의 특성상 순서없이 저장, 중복된 값 저장 안함 이므로
    [hashCode()](https://baekjungho.github.io/java-api)를 오버라이딩하여 해시코드 값을 비교하고, 같을경우 equals()로 비교하여 같은 객체 인지
    확인합니다.

    > 동등객체 : hasCode()와 equals()의 값이 모두 같은 객체

    `문자열(String Class)`은 내부적으로 [hashCode()](https://baekjungho.github.io/java-api)와 equals()가 재정의 되어있어서 HashSet에 저장할때
    같은 문자열은 같은 객체, 다른 문자열은 다른 객체로 판단합니다.

    만약, 제네릭 타입에 클래스를 넣고 비교할 경우에는 [hashCode()](https://baekjungho.github.io/java-api)와 equals()를 재정의 해야 합니다.

    ```java
    @Override
    public boolean equals(Object o) {
      if(o instanceof ClassName) {
        ClassName classname = (ClassName)o;
        // name과 id가 같을경우 true 리턴
        return classname.name.equals(name) && (classname.id == id);
      } else {
        return false;
      }
    }

    // name과 id가 같을경우 동일한 hashCode() 리턴
    @Override
    public int hashCode() {
      return name.hasCode() + id;
    }
    ```

  - __EXAMPLE1__

    Member클래스가 있고 속성으로는 이름, 성별, 전화번호가 있습니다.
    이름과 성별의 hashCode()와 equals()가 같을 경우 동등객체로 판단하는 프로그램이 있습니다.

    ```java
    public class Member {

    	public String name;
    	public String sex;
    	public String phone;

    	public Member(String name, String sex, String phone) {
    		this.name = name;
    		this.sex = sex;
    		this.phone = phone;
    	}

    	@Override
    	public boolean equals(Object o) {
    		if(o instanceof Member) {
    			Member member = (Member)o;
    			return member.name.equals(name) && (member.sex.equals(sex));
    		} else {
    			return false;
    		}
    	}

    	@Override
    	public int hashCode() {
    		return name.hashCode() + sex.hashCode();
    	 }
      }

      import java.util.HashSet;
      import java.util.Set;

      public class HashCodeExample {
      	public static void main(String[] args) {
      		Set<Member> set = new HashSet<Member>();

      		// 동명이인
      		set.add(new Member("홍길동", "M", "010-1234-5678"));
      		set.add(new Member("홍길동", "M", "010-5678-1234"));

      		System.out.println("총 객체 수 : " + set.size()); // 결과 1
      	}
    }
    ```

    위 코드를 보면 알 수 있듯이 어떻게 hashCode()와 equals()메소드를 오버라이딩 하느냐에 따라
    결과가 다르게 나오는 걸 알 수 있습니다.

    문제가 이름과, 성별이 같으면 동등객체로 판단하라고 했는데 hashCode()에서 성별이 같냐를 판단하고
    equals()에서 이름이 같냐라는 판단을 해서 코드를 작성할 수도 있습니다.

  - __EXAMPLE2__

    ```java
    @Override
    public boolean equals(Object o) {
      if(o instanceof Member) {
        Member member = (Member)o;
        return member.name.equals(name);
      } else {
        return false;
      }
    }

    @Override
    public int hashCode() {
      return sex.hashCode();
     }
    }
  ```

  - __Iterator 사용__

    ```java
    Set<String> set = new HashSet<String>();
    set.add("John");
    set.add("Mike");
    Iterator<String> iterator = set.iterator(); // Iterator(반복자) 얻기
    while(iterator.hasNext()) {
      String name = iterator.next();
      System.out.println(name);
    }
    ```

## Map

  Map 컬렉션은 `Key + Value`로 이루어져 있습니다. __키는 중복 저장이 불가능하지만 값은 중복 저장이 가능__ 합니다.
  Key와 Value는 모두 객체(Instance)로 구성되어있습니다. key와 value에는 null값도 허용됩니다.

  [Map과 List의 병합](https://baekjungho.github.io/java-maplist-merge/)

  예를 들어 환경설정에 있는 PATH가 key이며, PATH안에 설정되어있는 C:\JAVA_HOME이 Value입니다.

  > Map.Entry : Key + Value

  - __객체 추가 메소드__

  |메소드명   |특징|
  |-----------|----|
  |V put(K key, V value)|  해당 키로 값을 저장, 새로운 키일 경우 null값 리턴, 동일한 키가 있을 경우 값을 대체하고 이전값 리턴|

  - __객체 검색 메소드__

  |메소드명   |특징|
  |-----------|----|
  |boolean containsKey(Object key)|  해당 키가 있는지 여부|
  |boolean containsKey(Object value)|  해당 값이 있는지 여부|
  |boolean isEmpty()|  컬렉션이 비어 있는지 조사|
  |int size()|  저장된 키의 개수를 리턴|
  |V get(Object key)|  주어진 키가 이는 값을 리턴|
  |Set<K> keySet()|  모든 키를 Set 객체에 담아서 리턴|
  |Set<Map.Entry<K,V>>entrySet()|  키와 값으로 구성된 모든 Map.Entry 객체를 Set에 담아서 리턴|
  |Collection<V> values()|  저장된 모든 값을 Collection에 담아서 리턴|

  - __객체 삭제 메소드__

  |메소드명   |특징|
  |-----------|----|
  |void clear()|  모든 Map.Entry(Key, Value) 삭제|
  |V remove(Object key)|  해당 키와 일치하는 Map.Entry를 삭제하고 값을 리턴|

  -----------------------------------------------------------------------------

### HashMap

  HashMap도 HashSet처럼 hashCode()와 equals()를 오버라이딩 해야 합니다.

  - __사용방법__

    ```java
    Map<K, V> map = new HashMap<K, V>();
    ```

    예를들어 K는 Integer, Value는 String을 가지고 싶으면 다음과 같이 작성하면 됩니다.

    ```java
    Map<Integer, String> map = new HashMap<Integer, String>();
    ```

  - __Iterator Architecture__

    ```java
    Map<K, V> map = new HashMap<K, V>();
    Set<K> keySet = map.keySet();
    Iterator<K> keyIterator = keySet.iterator();
    while(keyIterator.hasNext()) {
      K key = keyIterator.next();
      V value = map.get(key);
    }
    ```

  - __Enhanced For Loop Architecture__

    ```java
    Map<K, V> map = new HashMap<K, V>();
    Set<K> keySet = map.keySet();
    for(K key : keySet) {
      K keyField = key;
      V value = map.get(key);
    }
    ```

  - __EXAMPLE__

    ```java
    import java.util.*;
    public class HashMapExample {
	  public static void main(String[] args) {
		Map<String, Integer> map = new HashMap<String, Integer>();

    // put으로 키와 값을 넣기
		map.put("John", 10);
		map.put("Mike", 20);

    // get으로 키에 해당하는 값 찾기
		System.out.println(map.get("홍길동"));

		Set<String> keySet = map.keySet(); // keySet 얻기
		Iterator<String> keyIterator = keySet.iterator(); // 키에 해당하는 반복자 얻기
		while(keyIterator.hasNext()) {
			String key = keyIterator.next();
			Integer value = map.get(key);
			System.out.println(key + " & " + value);
		}

		System.out.println();

    // Map.Entry 얻기
		Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
    // Map.Entry의 반복자 얻기
		Iterator<Map.Entry<String, Integer>> entryIterator = entrySet.iterator();

		while(entryIterator.hasNext()) {
			Map.Entry<String, Integer> entry = entryIterator.next();
			String key = entry.getKey();
			Integer value = entry.getValue();
			System.out.println(key + " & " + value);
		  }
	   }
   }
   ```

   위에서 while문대신 Enhanced For Loop을 사용할 수 도 있습니다.

   > for(Type Parameter Variable : 제네릭타입변수){ 실행코드 }

   ```java
   for(String key : keySet) {
     System.out.println(key + " & " + map.get(key));
   }

   for(Map.Entry<String, Integer> entry : entrySet) {
     System.out.println(entry.getKey() + " & " + entry.getValue());
   }
   ```

  -----------------------------------------------------------------------------

### Hashtable

  Hashtable은 HashMap과 구조가 동일 합니다. Hashtable은 Vector처럼 synchronized method로 되어있어서
  멀티 스레드 프로그램에서 안전하게 객체를 추가, 삭제 할 수 있습니다.

  - __사용방법__

    ```java
    Map<K, V> map = new Hashtable<K, V>();
    ```

    예를들어 K는 Integer, Value는 String을 가지고 싶으면 다음과 같이 작성하면 됩니다.

    ```java
    Map<Integer, String> map = new HashMap<Integer, String>();
    ```

  -----------------------------------------------------------------------------

### Properties

  Properties는 Hashtable의 하위 클래스입니다.(특징이 똑같습니다.) 차이점은 Properties의
  Key와 Value는 `String`만 가능합니다.

  > Properties File은 키와 값이 = 기호로 연결되어있는 ISO 8859-1 문자셋으로 저장 됩니다.
  >
  > ~.properties, 한글은 유니코드로 변환됩니다.

  Properties File은 클래스 파일(~.class)와 함께 저장됩니다.

## TreeSet

  TreeSet은 이진트리(binary tree)를 기반으로 한 Set 컬렉션 입니다.

  ![tree](/images/posts/201904/tree.jpg)

  이진트리는 첫번째로 저장되는 값은 루트 노드가 되며 부모노드와 자식노드로 구성됩니다.
  부모노드의 값과 저장할 값을 비교해서 저장값이 작을경우 왼쪽노드에 저장, 클 경우 오른쪽 노드에 저장합니다.

  __이진트리의 특성상 같은값은 저장이 안되므로, 중복 제거효과가 있습니다.__

  - __구성__

    |left|value|right|

    left는 왼쪽 자식 노드 객체를 가리키고 right는 오른쪽 자식 노드 객체를 가리킵니다.

    TreeSet에 객체를 저장하면 `자동으로 오름차순 정렬`합니다. 부모 값과 비교해서 작은 값은 왼쪽 노드에 높은 것은 오른쪽 자식 노드에 저장합니다.

  - __사용방법__

    ```java
    TreeSet<E> treeSet = new TreeSet<E>();
    ```

  - __EXAMPLE__

    ```java
    List<String> list = new ArrayList<>();
    TreeSet treeSet = new TreeSet();

    // 리스트에 문자열 넣고, treeSet에 List넣기
    // treeSet 특성상 중복을 제거하고 자동으로 오름차순 정렬
    for(int i=0; i<countToken; i++) {
      list.add(st.nextToken());
      treeSet.addAll(list);
    }
    ```

  - [TreeSet을 이용한 Enhanced For Loop](https://baekjungho.github.io/question-solving2)

    ```java
    TreeSet<Employee> treeSet = new TreeSet<Employee>(new Comparator<Employee>() {
    @Override
    public int compare(Employee e1, Employee e2) {
      if(e1.position.compareTo(e2.position) > 0) return 1;
      else if(e1.position.compareTo(e2.position) < 0) return -1;
      else {
        return e1.date.compareTo(e2.date);
      }
     }
    });

    treeSet.add(new Employee(1, "백부장", Position.부장, LocalDate.of(2018, 01, 10)));
    // Enhanced For Loop
    for(Employee employee : treeSet) {
      System.out.println(employee.name + " - " + employee.position + " - " + employee.date);
    }
    ```

  - __TreeSet 관련 메소드__

  |메소드명   |특징|
  |-----------|----|
  |first()|  제일 낮은 객체를 리턴|
  |last()|  제일 높은 객체를 리턴|
  |lower(E e)|  주어진 객체보다 바로 아래 객체 리턴|
  |higher(E e)|  주어진 객체보다 바로 위 객체 리턴|
  |floor(E e)|  주어진 객체와 동등한 객체가 있으면 리턴, 없으면 바로 아래 객체 리턴|
  |ceiling(E e)|  주어진 객체와 동등한 객체가 있으면 리턴, 없으면 바로 위 객체 리턴|
  |pollFirst()|  제일 낮은 객체를 꺼내오고 컬렉션에서 제거|
  |pollLast()|  제일 높은 객체를 꺼내오고 컬렉션에서 제거|
  |descendingIterator()|  내림차순으로 정렬된 Iterator를 리턴|
  |descendingSet()|  내림차순으로 정렬된 NavigableSet을 리턴|
  |headSet(E toElement, boolean inclusive)|  주어진 객체보다 낮은 객체들을 NavigableSet으로 리턴, 주어진 객체 포함 여부는 두 번째 매개값에 따라 달라짐|
  |tailSet(E fromElement, boolean inclusive)|  주어진 객체보다 높은 객체들을 NavigableSet으로 리턴, 주어진 객체 포함 여부는 두 번째 매개값에 따라 달라짐|
  |subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)|  시작과 끝으로 주어진 객체 사이의 객체들을 NavigableSet으로 리턴, 시작과 끝 객체 포함 여부는 2, 4번째 매개값에 따라 달라짐|

  [TreeSet에서 Comparable을 구현한 예제](https://baekjungho.github.io/java-comparable)

  [TreeSet에서 Comparator를 구현한 예제](https://baekjungho.github.io/java-comparable)

  [TreeSet과 Comparator LocalDate Enum을 사용한 예제](https://baekjungho.github.io/question-solving2)

## TreeMap

  TreeMap은 이진 트리를 기반으로 한 Map 컬렉션 입니다. TreeSet은 left, value, right로 구성됬지만,
  TreeMap은 left, MapEntry(key, value), right로 구성됩니다.

  TreeMap도 TreeSet과 같이 자동으로 정렬하는데 키 값이 작은 것은 왼쪽 노드에 큰 것은 오른쪽 노드에 저장됩니다.

  __TreeSet과 TreeMap은 정렬을 위해 Comparable Interface 구현을 요구합니다.__

  만약 Comparable Interface를 구현하지 않으면(즉, compareTo()메소드를 오버라이딩 하지 않으면)
  `ClassCastException`이 발생합니다.

  - __사용방법__

    ```java
    TreeMap<K, V> treeMap = new TreeMap<K, V>();
    ```

  - __TreeMap 관련 메소드__

  |메소드명   |특징|
  |-----------|----|
  |firstEntry()|  제일 낮은 Map.Entry 리턴|
  |lastEntry()|  제일 높은 Map.Entry 리턴|
  |lowerEntry(K key)|  주어진 키보다 바로 아래의 Map.Entry 리턴|
  |higherEntry(K key)|  주어진 키보다 바로 위의 Map.Entry 리턴|
  |floorEntry(K key)|  주어진 키와 동등한 키가 있으면 Map.Entry 리턴, 없으면 바로 아래 Map.Entry 리턴|
  |ceilingEntry(K key)|  주어진 키와 동등한 키가 있으면 Map.Entry 리턴, 없으면 바로 위 Map.Entry 리턴|
  |pollFirstEntry()|  제일 낮은 Map.Entry를 꺼내오고 컬렉션에서 제거|
  |pollLastEntry()|  제일 높은  Map.Entry를 꺼내오고 컬렉션에서 제거|
  |descendingKeySet()|  내림차순으로 정렬된 키의 NavigableSet을 리턴 |
  |descendingMap()|  내림차순으로 정렬된 Map.Entry의 NavigableMap을 리턴|
  |headMap(K toKey, boolean inclusive)|  주어진 객체보다 낮은 Map.Entry들을 NavigableMap으로 리턴, 주어진 객체 포함 여부는 두 번째 매개값에 따라 달라짐|
  |tailMap(K fromKey, boolean inclusive)|  주어진 객체보다 높은 Map.Entry들을 NavigableMap으로 리턴, 주어진 객체 포함 여부는 두 번째 매개값에 따라 달라짐|
  |subMap(K fromKey, boolean fromInclusive, E toKey, boolean toInclusive)|  시작과 끝으로 주어진 키 사이의 Map.Entry들을 NavigableMap으로 리턴, 시작과 끝 객체 포함 여부는 2, 4번째 매개값에 따라 달라짐|

## Stack

  스택(Stack)은 LIFO(Last In First Out)으로 마지막으로 들어간 것이 맨 처음으로 나오는걸 의미합니다.
  push()와 pop()으로 넣고 빼는 과정을 진행하고 peek()는 스택의 맨 위 객체를 가져옵니다.

  |메소드명   |특징|
  |-----------|----|
  |push(E item)|  주어진 객체를 스택에 넣는다|
  |pop()|  스택의 맨 위 객체를 가져오고, 객체를 스택에서 제거한다|
  |peek()|  스택의 맨 위 객체를 가져오고, 객체를 스택에서 제거하지 않는다|

  - __사용방법__

    ```java
    Stack<E> stack = new Stack<E>();
    ```

## Queue

  큐(Queue)는 FIFO(First In First Out)으로 맨 처음 들어간 것이 맨 처음으로 나옵니다.
  `선입선출` 알바하면서 한 번쯤은 들어본 말 일것입니다. Queue의 대표적인 예는
  LinkedList입니다.

  |메소드명   |특징|
  |-----------|----|
  |offer(E e)|  주어진 객체를 큐에 넣는다|
  |poll()|  객체를 하나 가져오고, 객체를 큐에서 제거한다|
  |peek()|   객체를 가져오고, 객체를 큐에서 제거하지 않는다|

  - __사용방법__

    ```java
    Queue<E> queue = new LinkedList<E>();
    ```

## Synchronized Collection

  Vector와 Hashtable은 synchronized method로 되어있어서 멀티스레드 환경에서 안전합니다.
  반면 나머지 컬렉션들은 그렇지 않습니다. 하지만 멀티스레드 환경에서 처리해야할 경우도 생기는데
  이를 위해 `synchronizedXXX()`메소드를 제공하고 있습니다.

  |메소드명   |특징|
  |-----------|----|
  |synchronizedList(List<T> list)|  List를 동기화된 List로 리턴|
  |synchronizedMap(Map<K,V>, m)|  Map을 동기화된 Map으로 리턴|
  |synchronizedSet(Set<T> s)|   Set을 동기화된 Set으로 리턴|

  - __사용방법__

    ```java
    List<T> list = Collections.synchronizedList(new ArrayList<T>());
    Set<E> set = Collections.synchronizedSet(new HashSet<E>());
    Map<K, V> map = Collections.synchronized(new HashMap<K, V>());
    ```
