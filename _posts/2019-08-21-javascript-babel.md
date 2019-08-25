---
title: "Babel"
layout: post
category: Javascript
tags: [Javascript]
excerpt: "Babel에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Babel에 대해서 배워 봅시다.

## Babel

  자바스크립트의 최신문법인 ES6와 ES7을 사용하여 브라우저에 인식시키기 위해서는 `babel`이라는 자바스크립트 컴파일러가 필요합니다.
  입력은 자바스크립트 코드이며, 출력도 자바스크립트 코드 입니다.

  즉, `최신 버전의 자바스크립트 문법(ES6, ES7)`은 브라우저가 이해하지 못하기 때문에 babel이 브라우저가 이해할 수 있는 문법으로 변환해줍니다. ES6, ES7 등의 최신 문법을 사용해서 코딩을 할 수 있기 때문에 생산성이 향상된다.

  Babel은 아시다시피 ES6/ES7 코드를 ECMAScript5 코드로 transpiling 하기 위한 도구입니다. Babel은 다양한 작은 모듈들로 구성되어 있습니다. Babel 다양한 모듈을 담는 일종의 상자 역할을 하며 코드를 컴파일 하기 위해 작은 모듈들(ex. presets)을 사용합니다.
