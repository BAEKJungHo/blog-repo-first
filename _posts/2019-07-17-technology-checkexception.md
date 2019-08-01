---
title: "CheckedException & UnCheckedException"
layout: post
category: Technology
tags: [Java]
excerpt: "CheckedException & UnCheckedException에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > CheckedException & UnCheckedException에 대해서 배워 봅시다.

## CheckedException & UnCheckedException

  예외에는 CheckedException과 UnCheckedException이 있습니다. 영어 그대로, CheckedException은
  예외처리가 필수이며, UnCheckedException은 예외처리를 하지 않아도 됩니다. 체크예외가 발생할 수 있는 메소드를 사용할 경우 개발자의 의도와는 상관없이 예상치 못한 곳에서 예외가 발생할 경우입니다. 반드시 예외처리를 하는 코드를 작성해야 하며, 예외처리를 하지 않을 경우 컴파일 에러가 발생합니다.

  ![e1](/images/posts/201907/e1.jpg)

  UnCheckedException은 `RuntimeException`이 있습니다. 즉, RuntimeException을 상속받은 Exception들은
  예외처리를 하지 않아도 됩니다. NullPointerException, IllegalArgumentException 등이 포함됩니다. 이러한 예외는 코드에서 미리 예방할 수 있습니다.
  개발자가 부주의해서 발생하는 예외가 RuntimeException 입니다. 따라서 런타임 예외는 예상치 못한 상황에서 발생하는 것이 아니기 때문에, 굳이 catch, throws를 이용해서 처리하지 않아도 되도록 만들었습니다.
