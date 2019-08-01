---
title: "소수점 반올림"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 소수점 반올림을 하는 방법에 대해서 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## 소수점 반올림

  자바에서 소수점을 반올림하여 n번째 자리까지 나타낼때 사용되는 방법 2가지는 다음과 같습니다.

  1. [Math.round()](https://baekjungho.github.io/java-math/)
  2. [String.format](https://baekjungho.github.io/java-api)

  - __Math.round()__

    Math.round()는 소수점 첫번째 자리를 반올림하여 정수로 리턴시켜줍니다.
    하지만 아래처럼 소수점을 표현할 수도 있는데 곱셈과, 나눗셈을 이용하면 됩니다.

    ```java
    double pi = 3.141592;
    System.out.println(Math.round(pi)); //결과 : 3
    System.out.println(Math.round(pi*100)/100.0); //결과 : 3.14
    System.out.println(Math.round(pi*1000)/1000.0); //결과 : 3.142
    ```

  - __String.format__

    %.숫자f를 사용하여 숫자만큼 소수점 자리를 표시할 수 도 있습니다.

    ```java
    double pi = 3.141592;
    System.out.println(String.format("%.2f", pi)); //결과 : 3.14
    System.out.println(String.format("%.3f", pi)); //결과 : 3.141
    System.out.println(String.format("%,.4f", pi)); //결과 : 3.1415
    ```

  Math.round()와 String.format의 가장 큰 차이점은 Math.round()함수는 소수점아래가 0일경우 절삭하지만
  String.format은 절삭하지 않고 그대로 리턴합니다.

  > Reference : [Coding Factory](https://coding-factory.tistory.com/251)
