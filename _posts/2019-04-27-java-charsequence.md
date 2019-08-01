---
title: "CharSequence vs String"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 CharSequence와 String의 차이점에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## String

  String은 하나의 `클래스`입니다.
  Java 프로그램의 모든 문자열 리터럴은 이 클래스의 객체로 구현된다. 중요한 점은 문자열 값은 작성된 후에 변경할 수 없다는 것이다. *String 객체에 보관하는 문자열은 유니코드로 변형되므로 HTML과 같은 마크업 문자를 입출력할 때 문제가 발생합니다.*  이와 같이 마크업 문자를 입력하여 사용할 수 없기 때문에 변경할 수 없는 문자열이라고 부릅니다.

## CharSequence

  CharSequence는 `인터페이스`입니다.
  이름을 보면 알 수 있듯이 연속된 문자라는 의미를 지닙니다. 그리고 이 인터페이스는 다양한 종류의 char 시퀀스에 대해 균일한 읽기 전용 접근 권한을 제공한다. CharSequence를 implements 하여 구현된 대표적인 클래스로는 String, SpannableStringBuilder, StringBuilder, StringBuffer 등이 있습니다.

  *중요한 점은 CharSequence 객체에 보관하는 문자열은 같은 유니코드라도 마크업 문자를 사용할 수 있습니다.* 이처럼 String 클래스와 반대로 변형과 가공을 할 수 있기 때문에 스타일 문자 또는 연속된 문자라고도 합니다.

## 차이점

  CharSequence를 구현한 StringBuilder와 String의 차이점은 String은 +(더하기) 연산자로
  문자열을 concatenation할 수 있습니다. 하지만 이렇게 붙인 문자열은 `새로운 문자열(객체)` 가 생성되는 것이므로
  메모리 낭비가 심합니다. 반면 StringBuilder는 append()라는 메소드로 문자열을 concatenation 하며 새로운 문자열
  즉, 객체를 생성하는 것이 아닌 `기존 문자열에 대한 변경`을 수행합니다.

  StringBuilder 혹은 StringBuffer로 문자열에 대한 변경을 이루고나서 출력하기 위해서 주로
  toString() 메소드를 사용하여 객체를 생성하여 넘기는데 이런 경우 보다는 CharSequence 매개변수 타입으로
  매개값을 받아서 처리하는 것이 String 객체로 만들지 않기 때문에 불필요한 객체 생성을 막아
  메모리 측면에서 효율을 높일 수 있습니다.

  다음 예제를 통해 확인해 봅시다.

  ```java
  public class CharSequenceExample {

	public static void check(CharSequence seq) {
		StringBuilder sb = new StringBuilder(seq);
		System.out.println(sb);
	}

	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder();
		sb.append("Charsequence");
		System.out.println(sb.toString()); // String 객체를 하나 생성
		check(sb); // String 객체 생성 없이 출력
	 }
  }
  ```

  [StringBuilder와 StringBuffer의 차이점](https://baekjungho.github.io/java-api/#stringbuffer-stringbuilder-class)은 StringBuffer는 멀티 스레드 환경에서 사용하며, StringBuilder는 단일 스레드 환경에서 사용합니다.

  - __생성자__

  |생성자 |특징|
  |-----------|----|
  |StringBuffer, StringBuilder(CharSequence seq)| CharSequence를 매개변수로 받아 seq값을 갖는 객체 생성|
  |StringBuffer, StringBuilder(String str)| String을 매개변수로 받아 str값을 갖는 객체 생성|

## 참고

  http://dudmy.net/android/2017/09/15/difference-char-string/
