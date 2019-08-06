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

  > Asynchronous JavaScript and XML(AJAX)에 대해서 배워 봅시다.

## HTTP 요청 방식

  HTTP 요청 방식은 GET과 POST로 나뉩니다.

  ![aj1](/images/posts/201907/aj1.jpg)

  GET 방식은 주소에 데이터(data)를 추가하여 전달하는 방식입니다. GET 방식의 HTTP 요청은 브라우저에 의해 캐시되어(cached) 저장됩니다. 또한, GET 방식은 보통 쿼리 문자열(query string)에 포함되어 전송되므로, 길이의 제한이 있습니다. 따라서 보안상 취약점이 존재하므로, 중요한 데이터는 POST 방식을 사용하여 요청하는 것이 좋습니다.

  POST 방식은 데이터(data)를 별도로 첨부하여 전달하는 방식입니다. POST 방식의 HTTP 요청은 브라우저에 의해 캐시되지 않으므로, 브라우저 히스토리에도 남지 않습니다. 또한, POST 방식의 HTTP 요청에 의한 데이터는 쿼리 문자열과는 별도로 전송됩니다. 따라서 데이터의 길이에 대한 제한도 없으며, GET 방식보다 보안성이 높습니다.

  > 참고 : [GET과 POST 방식은 RequestParameter 방식으로 HtttpRequestMessage에 담겨져 보내진다.](https://baekjungho.github.io/spring-webform/)

## AJAX

  > Ajax(Asynchronous JavaScript and XML) 란 서버와 통신하기 위해 XMLHttpRequest 객체를 사용하는 것을 의미합니다.
  >
  > [JQuery Ajax API Documents](https://api.jquery.com/jquery.ajax/)

### 특징

  - `Ajax 특징`
    - Web에서 화면을 갱신하지 않고 `Server(Java)`로부터 Data를 가져오는 방법
      - 즉, 페이지 새로고침 없이 서버에 요청
      - `XMLHttpRequest(XHR)`을 이용해서 서버에서 자료를 조회하고, 가져와서 작업을 수행

    - 동기방식과 비동기 방식이 있는데 둘 다 페이지의 전환이 없다.

    - 백그라운드 영역에서 서버와 데이터를 교환하여 웹 페이지에 표시해 준다.

    - Ajax를 이용하여 개발을 손쉽게 할 수 있도록 미리 여러 가지 기능을 포함해 놓은 개발 환경을 `Ajax 프레임워크`라고 한다.
      - `제이쿼리(jQuery)` : 현재 가장 널리 사용되고 있는 Ajax 프레임워크
        -  `$.ajax() 메서드`는 모든 제이쿼리 Ajax 메서드의 핵심이 되는 메서드

### 동기 vs 비동기

  - `동기(Synchronous)`
    - 상대방의 신호에 의해서 다음 동작이 이루어지는 방식
    - 먼저 실행된 루틴을 완전히 끝내고 제어를 반납하는 방식
    - 한 요청이 완전히 끝날 때 까지, 다른 함수를 호출 할 수 없다.

  - `비동기(Asynchronous)`
    - 함수 호출을 하고, 바로 다음 것을 수행하다가 처리완료 이벤트가 오면 다시 처리를 하는 방식
    - 요청한 동작이 완료되지 않아도 일단 제어권을 반납한 후, 자기할 일을 계속 하는 방식
    - 비동기 메서드는 메서드만 호출할 뿐, 결과를 기다리지 않는다.

### 동작 원리

  ![j1](/images/posts/201908/j1.jpg)

   Ajax의 동작원리는 Browser에서 서버로 보낼 Data를 `Ajax Engine`을 통해 Server로 전송합니다. 이 때 Ajax Engine에서는 `JavaScript를 통해 DOM을 사용하여 XMLHttpRequest(XHR) 객체로 Data를 전달`합니다. 이 XHR을 이용해서 Server에서 비동기 방식으로 자료를 조회해 올 수 있습니다. Server에서 Data를 전달 할 때 화면전체의 HTML을 전달하지 않고 Text 또는 Xml형식으로 Browser에 전달합니다.

### XMLHttpRequest

  서버와 상호작용하기 위해 `XMLHttpRequest(XHR)` 객체를 사용합니다. __전체 페이지의 새로고침없이도 URL로부터 데이터를 받아올 수 있습니다.__ 이는 웹 페이지가 사용자가 하고 있는 것을 방해하지 않으면서 페이지의 일부를 업데이트할 수 있도록 해줍니다. XMLHttpRequest 는 AJAX 프로그래밍에 주로 사용됩니다.

  ![j2](/images/posts/201908/j2.jpg)

  XMLHttpRequest는 그 이름으로봐서 XML만 받아올 수 있을 것 같아 보이지만, `모든 종류의 데이터를 받아오는데 사용할 수 있습니다.` 또한 HTTP 이외의 프로토콜도 지원합니다(file 과 ftp 포함).

#### 생성자

  - `XMLHttpRequest()`
    - 생성자는 XMLHttpRequest 를 초기화합니다. 다른 모든 메소드 호출이전에 호출되어야 합니다.

#### 속성

  - `XMLHttpRequest.onreadystatechange`
    - readyState 어트리뷰트가 변경될때마다 호출되는 EventHandler 입니다.

  - `XMLHttpRequest.readyState : Read only`
    - 요청의 상태를 unsigned short 로 반환합니다.

  - `XMLHttpRequest.response : Read only`
    - 응답 엔티티 바디를 갖는하는 XMLHttpRequest.responseType의 값에 따라 ArrayBuffer, Blob, Document, JavaScript 객체, 또는 DOMString 을 반환합니다.

  - `XMLHttpRequest.responseText : Read only`
    - 요청에 대한 응답을 텍스트로 갖는 DOMString을 반환합니다. 요청이 성공하지 못했거나 아직 전송되지 않았을 경우 null을 반환합니다.

  - `XMLHttpRequest.responseType`
    - 응답 타입을 정의하는 열거형 값입니다.

  - `XMLHttpRequest.responseURL : Read only`
    - 응답의 연속된 URL 을 반환합니다. URL이 null 인 경우 빈 문자열을 반환합니다.

  - `XMLHttpRequest.responseXML : Read only`
    - 요청에 대한 응답을 갖는 Document 를 반환합니다. 요청이 성공하지 못했거나, 아직 전송되지 않았거나, XML 또는 HTML 로 파싱할 수 없는 경우 null 을 반환합니다. workers 에서는 사용이 불가합니다.

  - `XMLHttpRequest.status : Read only`
    - 요청의 응답 상태를 갖는 unsigned short 를 반환합니다.

  - `XMLHttpRequest.statusText : Read only`
    - HTTP 서버에 의해 반환된 응답 문자열을 갖는 DOMString 을 반환합니다. XMLHTTPRequest.status 와는 다르게, 응답 메시지의 전체 텍스트를 갖습니다(예, "200 OK").

  > 노트: HTTP/2 명세(8.1.2.4 Response Pseudo-Header Fields)에 따르면, HTTP/2 는 HTTP/1.1 상태 라인에 포함된 버전이나 원인 문구를 전달하는 방법을 정의하지 않습니다.

  - `XMLHttpRequest.timeout`
    - 요청이 자동으로 종료될때까지 걸린 시간을 밀리초 단위로 나타내는 unsigned long 입니다.

  - `XMLHttpRequestEventTarget.ontimeout`
    - 요청 시간 초과때마다 호출되는 EventHandler 입니다.

  - `XMLHttpRequest.upload : Read only`
    - 업로드 과정을 나타내는 XMLHttpRequestUpload 입니다.

  - `XMLHttpRequest.withCredentials`
    - 사이트 간 Access-Control 요청이 쿠키나 인증 헤더와 같은 자격 증명을 사용해야하는지 여부를 나타내는 Boolean 입니다.

#### 비표준 속성

  - `XMLHttpRequest.channel : Read only`
    - nsIChannel 입니다. 요청을 수행할 때 객체에 의해 사용된 채널입니다.

  - `XMLHttpRequest.mozAnon : Read only`
    - Boolean 입니다. true 일 경우, 요청이 쿠키나 인증 헤더 없이 전송됩니다.

  - `XMLHttpRequest.mozSystem :Read only`
    - Boolean 입니다. true 일 경우, 요청에대해 동일 출처 정책(same origin policy)이 강제되지 않습니다.

  - `XMLHttpRequest.mozBackgroundRequest`
    - Boolean 입니다. 객체가 백그라운드 서비스 요청을 나타내는지 여부를 표시합니다.

#### 메서드

  - `XMLHttpRequest.abort()`
    - 이미 전송된 요청을 중지합니다.

  - `XMLHttpRequest.getAllResponseHeaders()`
    - 모든 응답 헤더를 CRLF 로 구분한 문자열로 반환합니다. 응답을 받지 않은 경우 null 입니다.

  - `XMLHttpRequest.getResponseHeader()`
    - 지정한 헤더의 텍스트를 갖는 문자열을 반환합니다. 응답을 아직 받지 못했거나 응답에 헤더가 존재하지 않을 경우 null 입니다.

  - `XMLHttpRequest.open()`
    - 요청을 초기화합니다. 이 메소드는 네이티브 코드로부터의 요청을 초기화하기 위해 JavaScript 코드에 의해 사용됩니다. 대신 openRequest() 를 사용하세요.

  - `XMLHttpRequest.overrideMimeType()`
    - 서버에의해 반환된 MIME 타입을 오버라이드합니다.

  - `XMLHttpRequest.send()`
    - 요청을 보냅니다. 요청이 비동기인 경우(기본값), 이 메소드는 요청이 보내진 즉시 반환합니다.

  - `XMLHttpRequest.setRequestHeader()`
    - HTTP 요청 헤더의 값을 설정합니다. open() 후, send() 전에 setRequestHeader() 를 호출해야합니다.

#### 비표준 메서드

  - `XMLHttpRequest.init()`
    - C++ 코드에서 사용할 객체를 초기화합니다.
    - 주의: 이 메소드는 JavaScript 에서 호출되면 안됩니다.

  - `XMLHttpRequest.openRequest()`
    - 요청을 초기화합니다. 이 메소드는 JavaScript 코부로부터의 요청을 초기화하기위해 네이티브 코드에서 사용됩니다. 대신 open() 을 사용하세요. open() 에 대한 문서를 확인하세요.

#### Events

  - `abort`
    - 예를 들어 프로그램이 XMLHttpRequest.abort()를 호출해서 요청이 중단되면 발생한다.onabort 속성을 통해서도 가능하다.

  - `error`
    - 요청에 에러가 생기면 발생한다.onerror 속성을 통해서도 가능하다.

  - `load`
    - XMLHttpRequest 처리 과정이 성공적으로 완료되면 발생한다.onload 속성을 통해서도 가능하다.

  - `loadend`
    - 요청이 성공이든 (load 다음) 실패든 (abort 또는 error 다음) 완료되면 발생한다.
    - onloadend 속성을 통해서도 가능하다.

  - `loadstart`
    - 요청이 데이터를 받기 시작하면 발생한다.
    - onloadstart 속성을 통해서도 가능하다.

  - `progress`
    - 요청이 데이터를 받는 동안 주기적으로 발생한다.
    -onprogress 속성을 통해서도 가능하다.

#### 참고

  > [MOZILLA XMLHttpRequest, AJAX](https://developer.mozilla.org/ko/docs/Web/Guide/AJAX/Getting_Started)

### $.ajax()

  $.ajax() 메소드의 원형은 다음과 같습니다.

  ```javascript
  $.ajax([옵션])
  ```

  - `BASIC AJAX STRUCTURE EXMAPLE`

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

#### 메서드

  - `$.ajax()`
  	- 비동기식 Ajax를 이용하여 HTTP 요청을 전송함.

  - `$.get()`
    - 전달받은 주소로 GET 방식의 HTTP 요청을 전송함.

  - `$.post()`
  	 - 전달받은 주소로 POST 방식의 HTTP 요청을 전송함.

  - `$.getScript()`
  	 - 웹 페이지에 스크립트를 추가함.

  - `$.getJSON()`
    - 전달받은 주소로 GET 방식의 HTTP 요청을 전송하여, 응답으로 JSON 파일을 전송받음.

  - `.load()`
  	- 서버에서 데이터를 읽은 후, 읽어 들인 HTML 코드를 선택한 요소에 배치함.

#### EXAMPLE

```javascript
function surveyPrivacy(cnt){
let sur_seq = $("#seq").val();
if(sur_seq == undefined){
    sur_seq = 0;
}
if(cnt == '1'){
    $.ajax({
        type : 'GET' // HTTP 요청 방식
        ,async : false // async의 default 값은 true(비동기 방식), false(동기 방식)
        ,url : `${SURVEY_BASE_URL}/privacy?surSeq=${sur_seq}` // 해당 URI의 컨트롤러로 이동
        ,error  : function(xhr) {
            alert("error");
        },success : function(result){
            $("#surveyPrivacy").html(result); // Controller에서 View를 가져와서 랜더링
            CKEDITOR.replace('privacyVo.privacyContent', {
                height: 150,
                width: 1400,
                padding : 10,
            });
        }
    })
}else{
    $("#surveyPrivacy").html(''); // Controller에서 View를 가져와서 랜더링
}
}
```

  위 예제 처럼 `$("#surveyPrivacy").html(result);` 이런 방식으로 사용한다면, 컨트롤러의 HandlerMethod에서 `return "XXX/survey/list"` 와 같은 방식으로 View를 리턴해서 가져올 수 있습니다.
  따라서 해당 View가 result에 들어가서 랜더링(Rendering)됩니다.

  만약, JSON과 같은 데이터를 비동기로 처리할 때에는 `ResponseEntity나 @ResponseBody, @RestController`를 사용하여 데이터를 직접 넘겨서 처리할 수 있습니다.

## 참조

  > [http://tcpschool.com/ajax/ajax_jquery_ajax](http://tcpschool.com/ajax/ajax_jquery_ajax)
