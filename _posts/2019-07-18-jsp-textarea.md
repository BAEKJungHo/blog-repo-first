---
title: "TextArea를 문자열로 파싱"
layout: post
category: JSP
tags: [JSP]
excerpt: "TextArea를 문자열로 파싱할 때 일어나는 특징을 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > TextArea를 문자열로 파싱할 때 일어나는 특징을 배워봅시다.

## TextArea로 입력받고 문자열로 파싱

  TextArea로 입력받은 문자열은 \r\n 개행문자가 들어가 있어서 줄바꿈을 하지 않아도 됩니다.

  ```
  abcdefghijklmnopqrstuvwxyz \r\n
  abcdefghijklmnopqrstuv \r\n
  abcdefghijklmnopq \r\n
  ```

  위 처럼 받아지기 때문에, 줄바꿈 처리를 하지않아도 되므로, String으로 받아서 사용하면 됩니다.
