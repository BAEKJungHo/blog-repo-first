---
title: "replace vs replaceAll"
layout: post
category: Java
tags: [Java]
excerpt: "자바 String 클래스에서 제공하는 메소드인 replace와 replaceAll의 차이를 알아봅시다."
author: BAEKJungHo
---

* content
{:toc}

## replace vs replaceAll

  replace와 replaceAll의 공통점은 첫번째 매개값은 변경 대상인 문자열이며, 두 번째
  매개값은 변경할 문자열 입니다. 이 두 메소드는 다음과 같은 특징을 지닙니다.

  **replace(charSequence target, charSequence replacement)** 는 charSequence의 특성을 가지며
  **replaceAll(String target, String replacement)** 는 String의 특성을 지닙니다.

  String의 특징은 `Regular Expression`을 사용할 수 있습니다.

  예제를 통해서 두 메소드의 차이점을 확인해 봅시다.

  ```java
  String str = "abcdefghijklmnopqrstuvwxyz";
  String newReplaceAll = str.replaceAll("[abcde]", "G");
  ```

  위에서 newReplaceAll은 a, b, c, d, e를 각각 G로 변경한 문자열을 가지고 있습니다.
  아래 replace() 메소드로 replaceAll()과 같은 효과를 내기 위해서는 아래처럼 해야 합니다.

  ```java
  String str = "abcdefghijklmnopqrstuvwxyz";
  String newReplace = str.replace("a", "G");
  String newReplace = str.replace("b", "G");
  String newReplace = str.replace("c", "G");
  String newReplace = str.replace("d", "G");
  String newReplace = str.replace("e", "G");
  ```

  따라서 불특정 입력값을 변환할때에는 `Regular Expression`을 사용한 replaceAll()이 더 효과적입니다.
