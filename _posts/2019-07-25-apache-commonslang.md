---
title: "Apache Commons Lang"
layout: post
category: Apache
tags: [Apache, Java]
excerpt: "Apache Commons Lang에서 지원하는 클래스에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Apache Commons Lang에서 지원하는 클래스에 대해서 배워 봅시다.

## DateUtils

  `Date 객체명 = DateUtils.parseDate(날짜, 포맷)` 을 이용하면 해당 날짜를 지정한 포맷으로 Date 객체로 반환합니다.
  그리고 `Date객체명.before(비교할 Date 객체)`를 이용하면, 괄호 안의 날짜가 Date 객체명 보다 나중 날짜일 경우 true를 반환하며
  괄호 안의 날짜가 더 빠르면 false를 반환합니다.

  이 개념을 이용해서 설문조사나 게시판에서 DatePicker를 사용하여 시작일과 종료일을 선택해야 하는경우 `DateUtils`를 이용하면 시작일과
  종료일을 비교할 수 있습니다.

  - 시작일과 종료일을 비교하는 함수

  ```java
  public static boolean validateBeforeDate (String startDate, String endDate, String dateFormat) {
    try {
        Date parsedStartDate = DateUtils.parseDate(startDate, dateFormat);
        Date parsedEndDate = DateUtils.parseDate(endDate, dateFormat);
        if (parsedStartDate != null &&  parsedEndDate != null) {
            return parsedStartDate.before(parsedEndDate);
        }
     } catch (ParseException e) {
          log.error("Can't parse date ... ");
      }
      return false;
    }
  ```

  문조사나, 게시판에서 날짜를 DatePicker를 사용하여 입력해야 할 때, 날짜 입력란에 한글이나 숫자등이 들어가게 되면
  게시글 등록시 에러가 발생할 수 있습니다. 이를 막기 위해서는 서버(Controller)쪽에서 함수를 사용하여 막고, 자바스크립트에서도
  추가적으로 막아줘야 합니다. 서버에서 막는 이유는, URL로 타고 들어오는 사람이 있을 수도 있기 때문 입니다.

  따라서 Controller쪽에서는 위 `validateBeforeDate` 함수를 호출하여 막을 수 있습니다.

## StringUtils

### MyBatis에서 isNotBlank 사용

  Mapper.xml에서 검색 조건을 동적 쿼리로 만들 경우, searchKeyword가 null값인지 공백인지 체크를 해야 합니다. 그럴 때 아래와 같은 방법으로 사용하면 됩니다.

  - `@org.apache.commons.lang3.StringUtils@isNotBlank`

  ```xml
  <if test="@org.apache.commons.lang3.StringUtils@isNotBlank(searchKeyword)">
      AND   M.user_name LIKE CONCAT('%',#{searchKeyword},'%')
  </if>
  ```

  __isNotBlank는 null과, 공백, 유니코드 공백까지 체크합니다. trim()은 유니코드 공백은 체크하지 못합니다.__

### 자바에서 isNotBlank 사용

  자바에서도 isNotBlank 메서드를 사용하여, 필수 값에 대해서 null, 공백, 유니코드 공백을 체크할 수 있습니다.

  ```java
if(StringUtils.isNotBlank(questVo.getContent()) || StringUtils.isNotBlank(questVo.getType())) {
  questRepository.createSurveyQuest(questVo);
}
  ```

### EXAMPLE

  ```java
  /* StringUtilsTest.java */

package com.chocolleto.board.user;

import org.apache.commons.lang.StringUtils;

public class StringUtilsTest {

public static void main(String[] args) {

String str;
String str1;
Boolean bool;

str = "hello java.";
// str이 java를 포함하고 있으면 true 반환.
bool = StringUtils.contains(str, "java");
System.out.println("contains : " + bool);

// str이 null이면 "", 아니면 str 반환.
str1 = StringUtils.defaultString(str);
System.out.println("defaultString : " + str1);

str = "h e l l o j a v a .";
// 문자열 중 공백 문자가 있으면 모두 제거.
str1 = StringUtils.deleteWhitespace(str);
System.out.println("deleteWhitespace : " + str1);

str = "chocolleto";
str1 = "chocolleto";
// str과 str1이 동일한지 유무 반환.
bool = StringUtils.equals(str, str1);
System.out.println("equals : " + bool);

str = "JAVA";
str1 = "java";
// 대소문자 무시하고 str과 str1 비교.
bool = StringUtils.equalsIgnoreCase(str, str1);
System.out.println("equalsIgnoreCase : " + bool);

str = "chocolleto chocolleto";
// str에서 첫 번째 co의 인덱스를 반환. (인덱스는 0부터 시작)
int i = StringUtils.indexOf(str, "co");
System.out.println("indexOf : " + i);

// str에서 마지막 to의 인덱스 반환.
i = StringUtils.lastIndexOf(str, "to");
System.out.println("lastIndexOf : " + i);

// str이 null이거나 길이가 0이면 true 반환.
bool = StringUtils.isEmpty(str);
System.out.println("isEmpty : " + bool);

// str이 null이 아니거나 길이가 0이 아니면 true 반환.
bool = StringUtils.isNotEmpty(str);
System.out.println("isNotEmpty : " + bool);

String[] str3 = {"java", "javascript", "jQuery", "json"};
str = " | ";
// array에서 문자열을 읽어와 ' | '를 구분자로 연결.
str1 = StringUtils.join(str3, str);
System.out.println("join : " + str1);

str = "CHOCOLLETO";
// str을 소문자로 변환.
str1 = StringUtils.lowerCase(str);
System.out.println("lowerCase : " + str1);

str = "chocolleto";
//str을 대문자로 변환.
str1 = StringUtils.upperCase(str);
System.out.println("upperCase : " + str1);

str = "HELLO java";
// 대문자는 소문자로, 소문자는 대문자로 변환.
str1 = StringUtils.swapCase(str);
System.out.println("swapCase : " + str1);

//문자열의 앞뒤 순서를 바꿈.
str1 = StringUtils.reverse(str);
System.out.println("reverse : " + str1);

str = "c++, java, c#, javascript, jQuery";
// ','를 구분자로 사용하여 분리.
String[] str2 = StringUtils.split(str, ',');
for(int j=0 ; j < str2.length ; j++) {
System.out.println("split str2[" + j + "] : " + str2[j]);
}

str = " java ";
// 문자열 좌우에 있는 공백 문자를 제거.(trim()과 동일.)
str1 = StringUtils.strip(str);
System.out.println("strip : " + str1);

// 문자열 좌우 공백 문자 제거.
str1 = StringUtils.trim(str);
System.out.println("trim : " + str1);

}
  ```
