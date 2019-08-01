---
title: "어댑터 패턴(Adapter Pattern)"
layout: post
category: 디자인패턴
tags: [디자인패턴]
excerpt: "어댑터 패턴(Adapter Pattern)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 어댑터 패턴(Adapter Pattern)에 대해서 배워 봅시다.

## 어댑터 패턴(Adapter Pattern)

  어댑터를 변역하면 `변환기(Converter)`라고 할 수 있습니다. 변환기의 역할은
  서로 다른 두 인터페이스 사이에 통신이 가능하게 하는 것입니다.

  DB를 사용하여 프로그램을 해본 사람을 알 수 있는데, 다양한 데이터베이스 시스템을 공통의 인터페이스인 `ODBC, JDBC`를 이용해 조작할 수 있다는 사실을 알고 있을 것입니다.
  바로 ODBC/JDBC가 어댑터 패턴을 이용해 다양한 데이터베이스 시스템을 단일한 인터페이스로 조작할 수 있게 해주기 때문입니다.

  어댑터 패턴은 [합성(Compose)](https://baekjungho.github.io/java-compose/)을 이용합니다.

  아래 예제를 통해서 어댑터 패턴을 배워 봅시다.

  - `어댑터 패턴이 적용되지 않은 코드`

  ```java
  public class AService {
    void runA() {
      System.out.println("AService");
    }
  }
  ```
  ```java
  public class BService {
    void runB() {
      System.out.println("BService");
    }
  }
  ```
  ```java
  public class ClientWithNoAdapter {
    public static void main(String[] args) {
      AServcie as = new AService();
      BService bs = new BService();

      as.runA();
      bs.runB();
    }
  }
  ```

  run 메서드가 비슷한 역할을 하지만 이름이 다른것을 볼 수 있습니다. 이 코드를 Adapter 패턴을 적용하여 구현해 보겠습니다.

  - `어댑터 패턴이 적용된 코드`

  ```java
  public class AdapterServiceA {
    AService as = new AService();

    void runService() {
      as.runA();
    }
  }
  ```

  ```java
  public class AdapterServiceB {
    BService bs = new BService();

    void runService() {
      bs.runB();
    }
  }
  ```

  ```java
  public class ClientWithAdpater {
    public static void main(String[] args) {
      AdapterServiceA adapterServiceA = new AdapterServiceA();
      AdapterServiceB adapterServiceB = new AdapterServiceB();

      adapterServiceA.runService();
      adapterServiceB.runService();
    }
  }
  ```

  위 코드를 보면 알 수 있듯이, 변환기를 통해 runService()라는 동일한 메서드명으로 두 객체의 메서드를 호출하는 것을 볼 수 있습니다.

  __어댑터 패턴은 합성, 즉 객체를 속성으로 만들어서 참조하는 디자인 패턴입니다.__

  > Adapter Pattern : 호출당하는 쪽의 메서드를 호출하는 쪽의 코드에 대응하도록 중간에 변환기를 통해 호출하는 패턴
