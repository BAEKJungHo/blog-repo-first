---
title: "const 사용 이유"
layout: post
category: Javascript
tags: [Javascript]
excerpt: "자바스크립트에서 상수를 선언할 때 const를 사용하는 이유에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## const

  ```javascript
  // A.js
  function hello () {

  }

  function hi () {
  	...
  }
  //

  // B.js
  function hello () {
  	...
  }
   ...
  //


  const A = {
  	hello () {
  		...
  	}
  }

  <script src="A.js"></script>
  <script src="B.js"></script>
  ```

  자바스크립트 A에 있는 HELLO 함수와 자바스크립트 B에있는 HELLO 함수가 각각 선언 되었다 하더라도 함수명이 같으면 덮어씌워집니다.

  따라서 아래와 같이 const로 선언해주면 겹칠일이 없습니다.

  ```javascript
  const A = {
  	hello () {
  		...
  	}
  }
  ```
