---
title: "Arrays.asList()"
layout: post
category: Java
tags: [Java]
excerpt: "Arrays.asList()에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Arrays.asList()에 대해서 배워 봅시다.

## Arrays.asList()

  ```java
  Arrays provides a static method Arrays.asList()
  to convert an array into a List<T>.
  ```

  사실 완벽히 변환(convert)한다기 보단, 배열을 List처럼 set() 등의 메소드를 이용해서 List처럼 편리하게 쓰도록 하겠다는 것입니다.

  `java.util.Arrays.ArrayList` 클래스는 `set(), get(), contains()` 매서드를 가지고 있지만 원소를 추가하는 메서드는 가지고 있지 않기 때문에 사이즈를 바꿀 수 없습니다. 또한 Arrays는 주로 읽기, 상수에 많이 쓰입니다.

  ```java
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;
public class TestArrayAsList {
   public static void main(String[] args) {
      String[] strs = {"alpha", "beta", "charlie"};
      System.out.println(Arrays.toString(strs));   // [alpha, beta, charlie]

      List<String> lst = Arrays.asList(strs); // new ArrayList<String>(); 대신에 사용
      System.out.println(lst);  // [alpha, beta, charlie]

      lst.add("ttt");  // error : asList()로 List를 생성하면 원소를 새롭게 추가할 수 없음

      // Changes in array or list write thru
      strs[0] += "88";
      lst.set(2, lst.get(2) + "99"); // 2번째 인덱스 원소에 charlie99 넣음
      System.out.println(Arrays.toString(strs)); // [alpha88, beta, charlie99]
      System.out.println(lst);  // [alpha88, beta, charlie99]

      // Initialize a list using an array
      List<Integer> lstInt = Arrays.asList(22, 44, 11, 33);
      System.out.println(lstInt);  // [22, 44, 11, 33]
   }
}
  ```

  asList() 사용해서 List 객체를 만들 때 새로운 배열 객체를 만드는 것이 아니라, `원본 배열의 주소값`을 가져오게 됩니다.

  따라서 asList()를 사용해서 내용을 수정하면 원본 배열도 함께 바뀌게 되고, 원본 배열을 수정하면 그 배열로 만들어두었던 asList()를 사용한 List 내용도 바뀌게 됩니다.

  이러한 이유 때문에 Arrays.asList()로 만든 List에 새로운 원소를 추가하거나 삭제는 할 수 없습니다.
