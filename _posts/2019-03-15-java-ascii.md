---
title: "ASCII CODE"
layout: post
category: Java
tags: [Java]
excerpt: "ASCII CODE와 예제입니다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권)_

## ASCII CODE

  ASCII CODE(American Standard Code for Information Interchange)는 문자형태입니다.
  아스키코드를 변환하고 변환 되는 과정을 배워 보겠습니다.

  아스키 코드(ASCII Table)는 0번부터 127번까지만 사용하고, 아스키 코드는 7비트만 사용합니다.
  그 이유는 1비트를 통신 에러 검출을 위해 사용하기 때문이다.

  [아스키 코드 사용 예제(HexaDump)](https://baekjungho.github.io/java-byte/)

  > 통신 에러 검출을 위한 비트 : Parity Bit
  >
  > ANSI 코드 : ASCII 코드를 8비트로 확장

  ![ascii](/images/posts/201904/ascii.jpg)

  ```java
   // 문자를 아스키 코드 (10진수) 로 변환 출력
   System.out.println((int) 'A'); // 65

   // 문자를 유니코드 코드 (10진수) 로 변환 출력
   System.out.println((int) '가'); // 44032

   // 문자를 아스키 코드 (16진수) 로 변환 출력
   System.out.format("0x%02X%n", (int) 'A'); // 0x41

   // 문자를 유니코드 코드 (16진수) 로 변환 출력
   System.out.format("0x%04X%n", (int) '가'); // 0xAC00

   // 코드 번호를 문자로 변환 출력
   System.out.println((char) 65); // A

   System.out.println((char) 0x41); // A (이것은 16진수로 'A'를 출력한 예제)

   // 코드 번호를 문자로 변환 출력 (한글)
   System.out.println((char) 0xAC00); // 가
   ```

   다음은 문자열을 입력받고 Hexa(16진수) 모양의 String으로 변환 시키는 메소드 입니다.

   ```java
   // 문자열을 헥사 스트링으로 변환하는 메서드
   public static String stringToHex(String s) {
     String result = new String("");
     for (int i = 0; i < s.length(); i++) {
       result += String.format("%02X ", (int) s.charAt(i));
     }
     return result;
   }

   // 접두사 0x 붙이는 Version
   public static String stringToHex0x(String s) {
   String result = "";
   for (int i = 0; i < s.length(); i++) {
     result += String.format("0x%02X ", (int) s.charAt(i));
   }
    return result;
   }
   ```

   > 출처 : [mwultong Blog ― 디카 / IT](http://mwultong.blogspot.com/2006/10/java-char-to-ascii-unicode.html)
