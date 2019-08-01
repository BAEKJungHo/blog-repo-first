---
title: "JUnit"
layout: post
category: Java
tags: [Java]
excerpt: "단위테스트를 자동화 시켜주는 JUnit Tool에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## JUnit

### 설치

  JUnit을 사용 하기 위해서 Eclipse에서 설치하는 방법을 배워 봅시다.

  - 프로젝트 우클릭 - Build Path - Configure Build Path

    - ![junit1](/images/posts/201905/junit1.jpg)

  - Add Library... - JUnit 선택

    - ![junit2](/images/posts/201905/junit2.jpg)

  - 원하는 JUnit 버전 선택 후 Apply and Close

    - ![junit3](/images/posts/201905/junit3.jpg)

### 사용방법

  테스트 대상이 있는 패키지와 동일한 선상에 `패키지이름Test` 라는 패키지를 만들고
  그 안에 테스트 대상인 클래스 이름과 비슷하게 `클래스이름Test` 라는 클래스를 만듭니다.

  예를들어 calc라는 패키지 안에 Calculator라는 클래스 안에 add(), sub(), mul() 메소드가 있고
  이를 테스트 하기 위해서는 calcTest 패지지를 만들고 calcTest 패키지 안에 CalculatorTest클래스를 만들면 됩니다.

  ![junit5](/images/posts/201905/junit5.jpg)

## EXAMPLE 1

  ```java
  package member;

  public class Calculator {
  	public int add(int x, int y) {
  		return x+y;
  	}

  	public int sub(int x, int y) {
  		return x-y;
  	}

  	public int mul(int x, int y) {
  		return x*y;
  	}

  	public double div(double x, double y) {
  		if(y > 1e-30) return x/y;
  		else return 0.0;
  	}
  }
  ```

  member 패키지 안에 Calculator 클래스 생성 하고 테스트 하기위해 memberTest 패키지와
  CalculatorTest 클래스 생성

  ```java
  package memberTest;

  import static org.junit.Assert.assertEquals;

  import org.junit.After;
  import org.junit.Before;
  import org.junit.Test;

  import member.Calculator;

  public class CalculatorTest {
	private Calculator calc = null;

	@Before
	public void beforeTest() {
		calc = new Calculator();
		System.out.println("beforeTest()");
	}

	@Test
	public void addTest() {
		assertEquals(30, calc.add(10, 20));
		System.out.println("addTest()");
	}

	@Test
	public void subTest() {
		assertEquals(-10, calc.sub(10, 20));
		System.out.println("subTest()");
	}

	@Test
	public void mulTest() {
		assertEquals(200, calc.mul(10, 20));
		System.out.println("mulTest()");
	}

	@Test
	public void divTest() {
		assertEquals(0.5, calc.div(10.0, 20.0), 0.00001);
		// 실수는 argument를 하나 더 줘야 한다(오차범위)
	}

	@After
	public void afterTest() {
		System.out.println("afterTest()");
	 }
  }
  ```

  테스트를 하기 위해서는 `@Test`라는 어노테이션을 사용해야 합니다.

  ![assert](/images/posts/201905/assert.jpg)

  예를들어 위 프로그램을 돌리면 다음과 같은 결과가 나옵니다.

  ```
  beforeTest()
  subTest()
  afterTest()
  beforeTest()
  addTest()
  afterTest()
  beforeTest()
  mulTest()
  afterTest()
  beforeTest()
  afterTest()
  ```

  - 실행결과

    - ![junit4](/images/posts/201905/junit4.jpg)

  assertEquals(a, b, c)는 a와 b의 객체가 동일한지 판단하며, c는 실수 값을 판단할때
  오차범위를 설정해주는 역할을 합니다.

## EXAMPLE 2

  - BankAccount

  ```java
  package util;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  import state.AdminName;

  public class BankAccount {
	private static final Logger LOG = LoggerFactory.getLogger(BankAccount.class);
	// 계좌번호 얻기
	public String getAccount(String name) {
		AdminName admin = AdminName.valueOf(name);
		LOG.debug(String.valueOf(admin));
		switch(admin) {
		case ezen :
			return "352-1142-3666-63";
		case JH쇼핑몰 :
			return "110-1958-1241-68";
		case SW쇼핑몰 :
			return "220-2258-5461-90";
		case GJ쇼핑몰 :
			return "151-7654-1289-81";
		case 무신사 :
			return "430-1158-3498-68";
		case 와구와구 :
			return "111-1113-1234-13";
		case 하이마트 :
			return "112-1234-1233-10";
		case 언더아머 :
			return "113-1312-1657-65";
		case 이케아 :
			return "110-1951-4651-43";
		case 경기물류 :
			return "110-1952-6761-68";
		case 중부물류 :
			return "110-1953-1433-98";
		case 영남물류 :
			return "110-1954-1109-37";
		case 서부물류 :
			return "110-1955-9839-83";
		default : return null;
		}
	 }
  }
  ```

  - BankAccountTest

  ```java
  package utilTest;

  import static org.junit.Assert.assertEquals;

  import java.util.ArrayList;
  import java.util.List;

  import org.junit.Before;
  import org.junit.Test;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  import util.BankAccount;

  public class BankAccountTest {
	BankAccount ba = null;
	private static final Logger LOG = LoggerFactory.getLogger(BankAccountTest.class);

	@Before
	public void beforeTest() {
		ba = new BankAccount();
		LOG.debug("beforeTest()");
	}

	@Test
	public void getAccountTest() {
		List<String> result = new ArrayList<String>();
		result.add("352-1142-3666-63"); assertEquals(result.get(0), ba.getAccount("ezen"));
		result.add("110-1958-1241-68"); assertEquals(result.get(1), ba.getAccount("JH쇼핑몰"));
		result.add("220-2258-5461-90"); assertEquals(result.get(2), ba.getAccount("SW쇼핑몰"));
		result.add("151-7654-1289-81"); assertEquals(result.get(3), ba.getAccount("GJ쇼핑몰"));
		result.add("430-1158-3498-68"); assertEquals(result.get(4), ba.getAccount("무신사"));
		result.add("111-1113-1234-13"); assertEquals(result.get(5), ba.getAccount("와구와구"));
		result.add("112-1234-1233-10"); assertEquals(result.get(6), ba.getAccount("하이마트"));
		result.add("113-1312-1657-65"); assertEquals(result.get(7), ba.getAccount("언더아머"));
		result.add("110-1951-4651-43"); assertEquals(result.get(8), ba.getAccount("이케아"));
		result.add("110-1952-6761-68"); assertEquals(result.get(9), ba.getAccount("경기물류"));
		result.add("110-1953-1433-98"); assertEquals(result.get(10), ba.getAccount("중부물류"));
		result.add("110-1954-1109-37"); assertEquals(result.get(11), ba.getAccount("영남물류"));
		result.add("110-1955-9839-83"); assertEquals(result.get(12), ba.getAccount("서부물류"));
	 }
  }
  ```

## 참고

  http://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev:tst:test_case

  https://junit.org/junit5/
