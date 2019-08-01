---
title: "Map과 List의 병합"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 Map과 List를 병합하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## Map과 List의 병합

  Collection List와 Map을 병합하는 방법에 대해 배워보겠습니다.

  1. List : addAll() 메소드 사용
  2. Map : putAll() 메소드 사용

  List의 병합뿐만 아니라 [addAll() 메소드](https://baekjungho.github.io/java-maplist-merge/)는 복제 기능도 있습니다. 단, copyList에 아무런 값이 들어가 있으면 안됩니다. 또한 addAll()을 사용하면 원본과 복사본이 서로에게 영향을 끼치지 않습니다.

  ```java
  public class ListMerge {
    public static void main(String[] args) {
        List<String> list1 = new ArrayList<String>();
        list1.add("Java");
        list1.add("Python");
        list1.add("Jsp");

        List<String> list2 = new ArrayList<String>();
        list2.add("Servlet");
        list2.add("DataBase");
        list2.add("Html");

        list1.addAll(list2);

        System.out.println(list1); // [Java, Python, Jsp, Servlet, Database, Html]
    }
  }
  ```

  다음은 Map을 병합하는 과정입니다. 단 Map의 다이아몬드 연산자에 지정한 클래스타입이 서로 다를경우 에러가 발생합니다.

  ```java
  public class MapMerge {
    public static void main(String[] args) {
        Map<String, String> map1 = new HashMap<String, String>();
        map1.put("Java", "J");
        map1.put("Python", "P");
        map1.put("Jsp", "J");

        Map<String, String> map2 = new HashMap<String, String>();
        map2.put("Servlet", "S");
        map2.put("Database", "D");
        map2.put("Html", "H");

        map1.putAll(map2); // 맵에 추가

        System.out.println(map1); // {Java=J, Python=P, Jsp=J, Servlet=S, Database=D, Html=H}
    }
  }
  ```
