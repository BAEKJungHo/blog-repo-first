---
title: "[자바스크립트] 입력과 출력"
layout: post
category: Javascript
tags: [Javascript]
excerpt: "자바스크립트에서 입력과 출력에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 자바스크립트에서 입력과 출력을 하는 방법에 대해서 배워 봅시다.

## 스크립트 태그

  HTML5 표준 형식은 `<script></sciript>` 태그를 사용합니다. HTML5에서는 script 태그에 `type="text/javascript"` type 속성을 적지 않는게 원칙 입니다.

  ```html
  <!DOCTYPE html>
  <html>
  <head>
    <title></title>
    <script></script>
  </head>
  <body>
    <script></script>
  </body>
  </html>
  ```

  HTML 태그는 웹 브라우저에 의해 순차적으로 실행됩니다. head 태그를 먼저 읽은 후 head 안의 script 태그를 읽고
	body를 읽으면서 body안에 있는 script 태그를 읽습니다.

## 출력

  - 가장 기본적인 출력 방법 : alert()

```javascript
<script>
// Hello World라는 경고 창이 나온다.
  alert("Hello World");
</script>
```

  - 자료형 확인

```javascript
<script>
  alert(typeof('String')); // string
  alert(typeof(273)); // number
</script>
```

  - 기본 입력 : prompt()

```javascript
<script>
  var input = prompt('Message', '메세지를 입력하세요');
  alert(input);
</script>
```

  - 기본 입력 : confirm()

```javascript
<script>
  var input = confirm('수락하시겠습니까?');
  alert(input);
</script>
```

  사용자가 확인을 누르면 true 리턴, 취소를 누르면 false 리턴

  - undefined 자료형

```javascript
<script>
  alert(typeof (variable))
</script>
```

  자바스크립트에는 존재하지 않는 것을 표현하는 자료형이 있습니다. undefined 자료형인데, 변수를 선언했지만 초기화 하지 않아도, undefined 자료형을 가집니다.

## 개발자 도구 화면에 출력

  개발자 도구(F12)에 내용을 출력하고 싶을 경우 `console`을 사용하면 됩니다.
  console에 지정할 수 있는 여러 속성이 있는데, 대표적으로 `log와 dir` 입니다.

  - console.log를 사용한 경우

  ```javascript
  console.log(document.querySelector('.inner'));
  ```

  console.log를 사용한 경우 개발자 도구에서는 아래와 같이 태그 형식으로 결과가 출력됩니다.

  ```html
  <table class="table">
      <thead>
            <tr>
                <th>수정 날짜</th>
                <th>복구</th>
            </tr>
        </thead>
    </table>
  ```

  - console.dir을 사용한 경우

  ```javascript
  console.dir(document.querySelector('.inner'));
  ```

  console.dir을 사용한 경우는 아래와 같이 객체 형식으로 출력됩니다.

  ```
  accessKey: ""
  align: ""
  assignedSlot: null
  attributeStyleMap: StylePropertyMap {size: 0}
  attributes: NamedNodeMap {0: class, class: class, length: 1}
  ```

  이외에도 에러를 찍기위한 `console.error`가 있습니다.

  console.log를 사용할지, console.dir을 사용할지에 대한 기준은 일반 값을 찍을 때는 `console.log` 객체를 찍을 때는 `console.dir` 에러를 찍을때는 `console.error`를 사용하면 됩니다.
