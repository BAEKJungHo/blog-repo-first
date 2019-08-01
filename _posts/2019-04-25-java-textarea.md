---
title: "텍스트 파일에서 br 문자 제거"
layout: post
category: Java
tags: [Java]
excerpt: "텍스트 파일에서 textarea로 줄바꿈 할때 찍히는 <br>을 제거하는 방법을 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 줄바꿈 제거

  textarea에서 내용을 입력하고 Enter를 누르면 줄바꿈(< br >)이 되는데. 파일로 출력할때
  이 문자가 찍힙니다. 이 문자를 없애기 위해서는 다음과 같이 입력해주면 됩니다.

  ```java
  if(line.contains("<br>")) {
    line = line.replaceAll("<br>", "\r\n");
  }
  ```

  `\r\n`은 줄바꿈 처리를 하며 문자를 찍지 않습니다.

## 개행 문자 치환

  줄바꿈(개행) 문자를 치환하는 방법은 다음과 같습니다.

  ```java
  for (BbsMember bm : bmList) {
    String line = bm.getId() + "," + bm.getTitle() + ","
        + bm.getName() + "," + bm.getDate() + "," + bm.getContent()+"\r\n";
    if(line.contains(System.getProperty("line.separator"))) {
      line = line.replaceAll(System.getProperty("line.separator"), " ");
   }
  }
  ```

  [contains](https://baekjungho.github.io/java-api/#string-%EB%A9%94%EC%86%8C%EB%93%9C)는 해당문자가 포함되어 있을 경우
  `true`를 리턴합니다.
