---
title: "사이트 코드(Site Code)"
layout: post
category: ETC
tags: [ETC]
excerpt: "사이트 코드에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 사이트 코드(Site Code)

  사이트 코드(Site Code)란 스프링의 `contextPath`와 비슷한 개념입니다.

  예를들어 `localhost8080:/seoul/~` 처럼 되어있을때 seoul을 사이트 코드라고 합니다.
  seoul, busan등 여러 값이 존재 할 수 있습니다.

  반면 contextPath도 seoul과 같은 자리에 위치하고 있는데 이 둘의 차이점은 다음과 같습니다.

  > Site Code vs contextPath !!
  >
  > 사이트 코드는 한 서버에 여러개가 존재할 수 있으며, contextPath는 한 서버에 한 개만 존재합니다.
