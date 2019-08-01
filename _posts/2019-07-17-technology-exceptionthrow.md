---
title: "보안성에 위배되지 않게 예외처리하기"
layout: post
category: Technology
tags: [Java]
excerpt: "보안성에 위배되지 않게 예외처리하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 보안성에 위배되지 않게 예외처리하는 방법에 대해서 배워 봅시다.

## try-catch

  try-catch문을 사용할 때, 일반적으로 catch문에서 e.printStackTrace(e)와 같은 방식으로 처리했습니다.
  하지만 이렇게할 경우, 배포를 하기전에 보안성 검사를 받는데, 적합하지 않다고 판정이납니다.
  따라서 보안성에 위배되지 않게 예외처리를 하는방법에 대해서 배워 봅시다.

  적용 개념은 다음과 같습니다. 예를들어 파일처리를 하게되면 IOException 처리를 해야하는데, 이 예외는
  `CheckedException`입니다. 따라서 예외 처리가 필수 입니다. 하지만, catch문에서 `UncheckedException`인 RuntimeException을 발생시킨다면
  예외가 RuntimeException으로 넘어가서 안정적으로 변합니다.

  ```java
public void getDirFileList(String root) {
  File dir = new File(root);

  try{
    .....
  }catch(IOException e){
    throw new RuntimeException(e); // UncheckedException
  }
}
  ```
