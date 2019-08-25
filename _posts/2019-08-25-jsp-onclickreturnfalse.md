---
title: "태그내 return false 사용 이유"
layout: post
category: JSP
tags: [JSP]
excerpt: "태그내 return false 사용 이유에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 태그내 return false 사용 이유에 대해서 배워 봅시다.

## return false

a 태그에서 href="#" 를 사용 할시 페이지 상단으로 위치가 이동한다.

하지만
function aa() {
   return false;
}

`<a href="#" onclick = "aa(); return false;" />`

onclick 에서 return false; 를 사용하면 기본 속성을 무시하고 페이지가 이동하지 않는다.

1. `<a href="#" onclick = "aa(); return false;" />`
2. `<a href="#" onclick = "return aa();"  />`

1번과 2번 소스 2개다 페이지가 이동하지 않는다.
