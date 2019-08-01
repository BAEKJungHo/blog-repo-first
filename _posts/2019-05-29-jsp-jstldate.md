---
title: "JSTL fmt"
layout: post
category: JSP
tags: [JSP]
excerpt: "Jsp의 JSTL FormatDate에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## formatDate

  formatDate를 사용하기 위해서는 맨위에 core를 사용할 때 처럼 `라이브러리`를 추가해야 합니다.

  > <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>

  - EXAMPLE

  ```html
  <pre>
  <c:set var="now" value="<%=new java.util.Date() %>"></c:set>
  \${now} : ${now}
  <fmt:formatDate value="${now}"></fmt:formatDate>
  date : <fmt:formatDate value="${now}" type="date"></fmt:formatDate>
  date : <fmt:formatDate value="${now}" type="time"></fmt:formatDate>
  both : <fmt:formatDate value="${now}" type="both"></fmt:formatDate>
  default : <fmt:formatDate value="${now}" type="both" dateStyle="default" timeStyle="default"></fmt:formatDate>
  short : <fmt:formatDate value="${now}" type="both" dateStyle="short" timeStyle="short"></fmt:formatDate>
  medium : <fmt:formatDate value="${now}" type="both" dateStyle="medium" timeStyle="medium"></fmt:formatDate>
  long : <fmt:formatDate value="${now}" type="both" dateStyle="long" timeStyle="long"></fmt:formatDate>
  full : <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"></fmt:formatDate>

  pattern="yyyy년 MM월 dd일 hh시 mm분 ss초" : <fmt:formatDate value="${now}" pattern="yyyy년 MM월 dd일 hh시 mm분 ss초"></fmt:formatDate>
  </pre>
  ```

  - 결과
    - ![time](/images/posts/201905/time.jpg)  

## timeZone

  timeZone을 사용하여 스위스와, 뉴욕시간을 출력할 수 있습니다.

  - EXAMPLE

  ```html
  <jsp:useBean id="now" class="java.util.Date"></jsp:useBean>
  <pre>
    default : <c:out value="${now}"></c:out>
    Korea KST : <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"></fmt:formatDate>
    <fmt:timeZone value="GMT">
    Swiss GMT : <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"></fmt:formatDate>
    </fmt:timeZone>
    <fmt:timeZone value="GMT-8">
    NewYork GMT-8 : <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"></fmt:formatDate>
    </fmt:timeZone>
  </pre>
  ```

  - 결과
    - ![time1](/images/posts/201905/time1.jpg)  

## Locale

  setLocale을 사용하는 이유는 나라마다 사용하는 화페의 종류나 날짜를 표현하는 방식이 다르기 때문에
  setLocale로 각 나라의 방식으로 표현할 수 있습니다.

  - EXAMPLE

  ```html
  <c:set var="now" value="<%=new java.util.Date()%>"></c:set>
  <pre>
    톰캣 서버의 기본 로케일 : <%=response.getLocale() %>
    <fmt:setLocale value="ko_kr"></fmt:setLocale>
    로케일을 한국어로 설정 후 로케일 확인 : <%=response.getLocale() %>
    통화(currency) : <fmt:formatNumber value="10000" type="currency"></fmt:formatNumber>
    날짜 : <fmt:formatDate value="${now}"></fmt:formatDate>

    <fmt:setLocale value="ja_JP"></fmt:setLocale>
    로케일을 일본어로 설정 후 로케일 확인 : <%=response.getLocale() %>
    통화(currency) : <fmt:formatNumber value="10000" type="currency"></fmt:formatNumber>
    날짜 : <fmt:formatDate value="${now}"></fmt:formatDate>

    <fmt:setLocale value="en_US"></fmt:setLocale>
    로케일을 영어로 설정 후 로케일 확인 : <%=response.getLocale() %>
    통화(currency) : <fmt:formatNumber value="10000" type="currency"></fmt:formatNumber>
    날짜 : <fmt:formatDate value="${now}"></fmt:formatDate>
  </pre>
  ```

  - 결과
    - ![time2](/images/posts/201905/time2.jpg) 
