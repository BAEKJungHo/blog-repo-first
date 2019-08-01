---
title: "파일 확장자명 얻는 방법"
layout: post
category: Java
tags: [Java, Spring]
excerpt: "파일에서 확장자명만 분리하는 방법을 배워 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

## 파일 확장자명 얻는 방법

  Apache Commons IO에 보시면 `FilenameUtils`라는 클래스가 제공됩니다. `getExtension()` 메서드를 쓰시면 확장자를 얻을 수 있습니다.

  ```java
  destinationFileNameExtention = FilenameUtils.getExtension(origin).toLowerCase();
  ```
