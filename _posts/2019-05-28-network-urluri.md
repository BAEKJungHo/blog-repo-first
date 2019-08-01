---
title: "URI vs URL"
layout: post
category: Network
tags: [Network]
excerpt: "URI vs URL vs URN의 차이점에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## URI vs URL vs URN

  - URI : Uniform Resource Identifier
    - 정보 리소스를 고유하게 식별하고 위치를 지정

  - URL : Uniform Resource Locator
    - 리소스의 구체적인 위치 정보를 지정

  - URN : Uniform Resource Name
    - 리소스 위치와 상관 없이 리소스의 이름 값을 이용하는 접근 방식

  URL과 URI, URN의 관계도는 다음과 같습니다.

  > URI는 URL과 URN을 포함하고 있으며, URL과, URN은 URI의 부분 집합 입니다.

  즉, URI는 URL이라고 할 수 없지만, URL은 URI라고 할 수 있습니다.

  ```java
  http://www.naver.com/page.html?name=맛집&food=삼겹살
  ```

  위와 같은 주소가 있다고 할때 일반적으로 `http://www.naver.com/`을 URL이라고 하며
  `page.html`을 URN이라고 하고 `?name=맛집&food=삼겹살`를 URI라고 합니다.

  위와 같은 형식의 주소는 URI지만 URL은 아닌 형식입니다.

  그러면 URI이면서, URL인 형식은 다음과 같습니다.

  ```java
  https://baekjungho.github.io/category
  ```

  따라서 URI가 URL을 포함하는 더 큰 개념이라는 것을 알고 둘의 차이점을 알고 있으면 됩니다.
