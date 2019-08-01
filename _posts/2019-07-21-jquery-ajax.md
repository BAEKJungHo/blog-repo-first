---
title: "Asynchronous JavaScript and XML"
layout: post
category: JQurey
tags: [JQuery, Ajax]
excerpt: "Asynchronous JavaScript and XML(AJAX)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 코드로 AJAX가 어떤 방식으로 이루어 지는지 배워 봅시다.

## HTTP 요청 방식

  HTTP 요청 방식은 GET과 POST로 나뉩니다.

  ![aj1](/images/posts/201907/aj1.jpg)

  GET 방식은 주소에 데이터(data)를 추가하여 전달하는 방식입니다. GET 방식의 HTTP 요청은 브라우저에 의해 캐시되어(cached) 저장됩니다.

  또한, GET 방식은 보통 쿼리 문자열(query string)에 포함되어 전송되므로, 길이의 제한이 있습니다. 따라서 보안상 취약점이 존재하므로, 중요한 데이터는 POST 방식을 사용하여 요청하는 것이 좋습니다.

  POST 방식은 데이터(data)를 별도로 첨부하여 전달하는 방식입니다.

  POST 방식의 HTTP 요청은 브라우저에 의해 캐시되지 않으므로, 브라우저 히스토리에도 남지 않습니다. 또한, POST 방식의 HTTP 요청에 의한 데이터는 쿼리 문자열과는 별도로 전송됩니다. 따라서 데이터의 길이에 대한 제한도 없으며, GET 방식보다 보안성이 높습니다.

## AJAX

  Ajax란 `Asynchronous JavaScript and XML`을 의미합니다.

  Ajax는 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신할 수 있게 해줍니다. Ajax는 백그라운드 영역에서 서버와 데이터를 교환하여 웹 페이지에 표시해 줍니다.

  Ajax를 이용하여 개발을 손쉽게 할 수 있도록 미리 여러 가지 기능을 포함해 놓은 개발 환경을 Ajax 프레임워크라고 합니다.
  그중에서도 현재 가장 널리 사용되고 있는 Ajax 프레임워크는 바로 제이쿼리(jQuery)입니다.

  `$.ajax() 메소드` 제이쿼리는 Ajax와 관련된 다양하고도 편리한 메소드를 많이 제공하고 있습니다. 그중에서도 $.ajax() 메소드는 모든 제이쿼리 Ajax 메소드의 핵심이 되는 메소드입니다. __$.ajax() 메소드는 HTTP 요청을 만드는 강력하고도 직관적인 방법을 제공합니다.__

  $.ajax() 메소드의 원형은 다음과 같습니다.

  ```javascript
  $.ajax([옵션])
  ```

  URL 주소는 클라이언트가 HTTP 요청을 보낼 서버의 주소입니다. 옵션은 HTTP 요청을 구성하는 키와 값의 쌍으로 구성되는 헤더의 집합입니다.
  다음 예제는 $.ajax() 메소드에서 사용할 수 있는 대표적인 옵션을 설명하는 예제입니다.

  ```javascript
  $.ajax({
    url: "/examples/media/request_ajax.php", // 클라이언트가 요청을 보낼 서버의 URL 주소
    data: { name: "홍길동" },                // HTTP 요청과 함께 서버로 보낼 데이터
    type: "GET",                             // HTTP 요청 방식(GET, POST)
    dataType: "json"                         // 서버에서 보내줄 데이터의 타입
  })

  // HTTP 요청이 성공하면 요청한 데이터가 done() 메소드로 전달됨.
  .done(function(json) {
    $("<h1>").text(json.title).appendTo("body");
    $("<div class=\"content\">").html(json.html).appendTo("body");
  })

  // HTTP 요청이 실패하면 오류와 상태에 관한 정보가 fail() 메소드로 전달됨.
  .fail(function(xhr, status, errorThrown) {
    $("#text").html("오류가 발생했습니다.<br>")
      .append("오류명: " + errorThrown + "<br>")
      .append("상태: " + status);
  })

  // HTTP 요청이 성공하거나 실패하는 것에 상관없이 언제나 always() 메소드가 실행됨.
  .always(function(xhr, status) {
    $("#text").html("요청이 완료되었습니다!");
  });
  ```

  아래는 $.ajax() 메서드의 동작방식을 보여주는 예제 입니다.

  ```javascript
  $(function() {
    $("#requestBtn").on("click", function() {
      $.ajax("/examples/media/request_ajax.php")
        .done(function() {
          alert("요청 성공");
        })
        .fail(function() {
          alert("요청 실패");
        })
        .always(function() {
          alert("요청 완료");
      });
    });
  });
  ```

### $.get(), $.post()

  제이쿼리에서는 Ajax를 이용하여 GET 방식의 HTTP 요청을 구현한 $.get() 메소드를 제공합니다.

  이 메소드를 사용하면 서버에 GET 방식의 HTTP 요청을 보낼 수 있습니다.

  ```javascript
  $.post(URL주소[,데이터][,콜백함수]);
  ```

  URL 주소는 클라이언트가 HTTP 요청을 보낼 서버의 주소입니다. 데이터는 HTTP 요청과 함께 서버로 보낼 데이터를 전달합니다.

  콜백 함수는 HTTP 요청이 성공했을 때 실행할 함수를 정의합니다.

  ```JavaScript
  $(function() {
    $("#requestBtn").on("click", function() {
        // POST 방식으로 서버에 HTTP Request를 보냄.
        $.post("/examples/media/request_ajax.php",
            { name: "홍길동", grade: "A" },// 서버가 필요한 정보를 같이 보냄.
            function(data, status) {
              $("#text").html(data + "<br>" + status); // 전송받은 데이터와 전송 성공 여부를 보여줌.
            }
        );
    });
  });
  ```

### Ajax와 Form 요소

  Ajax에서는 서버와의 비동기식 통신을 위해 form 요소를 통해 입력받은 데이터를 `직렬화(serialization)`하여 전송합니다.

  이때 직렬화(serialization)란 입력받은 여러 데이터를 하나의 `쿼리 문자열`로 만드는 것을 의미합니다. 이렇게 함으로써 form 요소를 통해 입력받은 데이터를 한 번에 서버로 보낼 수 있게 됩니다.

  제이쿼리에서는 HTML form 요소를 통해 입력된 데이터의 직렬화 작업을 매우 간단하게 수행할 수 있습니다.  `.serialize()` 메서드와 `.serializeArray()` 메서드를 이용하여 손쉽게 데이터를 직렬화할 수 있습니다.

 .serialize() 메소드는 HTML form 요소를 통해 입력된 데이터를 쿼리 문자열로 변환합니다.

 .serializeArray() 메소드는 serialize() 메소드와는 달리 입력된 데이터를 문자열이 아닌 배열 객체로 변환합니다.

  ```javascript
  $(function() {
    $("form").on("submit", function(event) {  // <form>요소에 "submit" 이벤트가 발생할 때,
      event.preventDefault(); // 서버로 전송하지 않음.
      $("#text").html($(this).serialize()); // 입력받은 데이터를 직렬화하여 나타냄.
    });
  });
  ```
  
## 참조

  > [http://tcpschool.com/ajax/ajax_jquery_ajax](http://tcpschool.com/ajax/ajax_jquery_ajax)
