---
title: "Apache Commons Lang"
layout: post
category: Apache
tags: [Apache, Java]
excerpt: "Apache Commons Lang에서 DateUtils에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Apache Commons Lang에서 DateUtils에 대해서 배워 봅시다. 그리고 DateUtils를 사용해서, 날짜 비교 함수를 만들어 보겠습니다.

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
