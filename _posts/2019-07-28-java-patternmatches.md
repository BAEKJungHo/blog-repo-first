---
title: "정규식(Regular Expression)"
layout: post
category: Java
tags: [Java]
excerpt: "정규식(Regular Expression)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 정규식(Regular Expression)에 대해서 배워 봅시다.

## 정규식(Regular Expression)

  > Wikipedia
  >
  > 정규 표현식(正規表現式, 영어: regular expression, 간단히 regexp[1] 또는 regex, rational expression) 또는 정규식(正規式)은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어이다.

### 정규 표현식을 만들기 위해 사용되는 표현식

  - `^` : 문자열의 시작
  - `$` : 문자열의 종료
  - `.` : 임의의 한 문자 (문자의 종류 가리지 않음)
    - 단, \ 는 넣을 수 없음
  - `*` : 앞 문자가 없을 수도 무한정 많을 수도 있음
  - `+` : 앞 문자가 하나 이상
  - `?` : 앞 문자가 없거나 하나있음
  - `[]` : 문자의 집합이나 범위를 나타내며 두 문자 사이는 - 기호로 범위를 나타낸다. []내에서 ^가 선행하여 존재하면 not 을 나타낸다.
  - `{}` : 횟수 또는 범위를 나타낸다.
  - `()` : 소괄호 안의 문자를 하나의 문자로 인식
  - `|` : 패턴 안에서 or 연산을 수행할 때 사용
  - `\s` : 공백 문자
  - `\S` : 공백 문자가 아닌 나머지 문자
  - `\w` : 알파벳이나 숫자
  - `\W` : 알파벳이나 숫자를 제외한 문자
  - `\d` : 숫자 [0-9]와 동일
  - `\D` : 숫자를 제외한 모든 문자
  - `\` : 정규표현식 역슬래시(\)는 확장 문자
    - 역슬래시 다음에 일반 문자가 오면 특수문자로 취급하고 역슬래시 다음에 특수문자가 오면 그 문자 자체를 의미
  - `(?i)` : 앞 부분에 (?i) 라는 옵션을 넣어주면 대소문자를 구분하지 않음

### Matcher 클래스와 Pattern 클래스

  자바에서 정규식을 사용하기 위해서 사용되는 클래스는 `Matcher, Pattern`이 있습니다. 문자열로 표현된 정규식(Regular Expression)은 사용되기 전에 반드시
  Pattern 클래스의 객체로 컴파일 되야 합니다.

  ```java
  import java.util.regex.Matcher;
  import java.util.regex.Pattern;
  ```

  - Matcher 클래스와 Pattern 클래스의 메서드

  ```java
  static Pattern compile(String regexp) // 문자열의 정규식을 Pattern 객체로 컴파일
  static boolean matches(String pattern, String input) // 정규식 pattern과 문자열 input을 비교하여 boolean으로 리턴
  static Matcher matcher(CharSequence input) // 입력 CharSequence에서 정규식 패턴을 찾는 Matcher 객체를 생성
  String pattern() // 컴파일된 정규식을 문자열로 반환
  String split(CharSequence input) // 주어진 CharSequence를 패턴에 따라 분리
  ```

  ```java
  Pattern p = Pattern.compile("(^[0-9]*$)");
  ```

  ```java
  Matcher m = p.matcher(inputVal);
  ```

  inputVal이 Pattern 객체인 p와 패턴이 일치하는지 체크합니다.

  ```java
  if(m.find()) {
    // Matcher의 find() 메서드
    // 패턴이 일치하는 경우 true, 일치하지 않는 경우 false를 리턴
  }

  if(Pattern.matches(pattern, input)) {
    // 첫 번째 인자로는 regexp가 들어감
    // 두 번째 인자로는 정규식과 비교할 문자열이 들어감
    // 일치하는 경우 true, 일치하지 않는 경우 false를 리턴
  }
  ```

### 자주 쓰이는 정규식

  ```
1) 숫자만 : ^[0-9]*$

2) 영문자만 : ^[a-zA-Z]*$

3) 한글만 : ^[가-힣]*$

4) 영어 & 숫자만 : ^[a-zA-Z0-9]*$

5) E-Mail : ^[a-zA-Z0-9]+@[a-zA-Z0-9]+$

6) 휴대폰 : ^01(?:0|1|[6-9]) - (?:\d{3}|\d{4}) - \d{4}$

7) 일반전화 : ^\d{2,3} - \d{3,4} - \d{4}$

8) 주민등록번호 : \d{6} \- [1-4]\d{6}

9) IP 주소 : ([0-9]{1,3}) \. ([0-9]{1,3}) \. ([0-9]{1,3}) \. ([0-9]{1,3})
  ```

## 참조

  > [https://highcode.tistory.com/6](https://highcode.tistory.com/6)
