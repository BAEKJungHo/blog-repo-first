---
title: "VO들의 부모 BaseVO"
layout: post
category: Spring
tags: [Spring]
excerpt: "VO들의 부모 BaseVO에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > VO들의 부모 BaseVO에 대해서 배워 봅시다.

## BaseVO

  실무에서 기획서를 보고 DB를 짜게되면 테이블이 여러개 있습니다. 여기서는 3개라 가정하고 설명하겠습니다.

  A B C 라는 각각의 테이블이 있고, 각 테이블이 가지는 컬럼은 다음과 같습니다.

  - A : a, b, c, d, e
  - B :  a, b, x, y, z
  - C : a, b, p, q, r

  여기서 각 테이블에서 공통으로 가지고 있는 속성은 a와 b입니다. 만약 A, B, C가 기본키를 SEQ라는 컬럼명으로 가지고있으면
  기본키명을 통일시켜서 공통 컬럼명으로도 뽑을 수 있습니다.

  DB를 설계하고 VO를 만들때 컬럼명과 동일하게 만드는데, 각 테이블에서 공통으로 사용하는 속성들이 있는 경우 `BaseVO`를 만들어서
  AVO, BVO, CVO에서 상속받아 사용할 수 있습니다.  그리고 BaseVO는 직렬화 하기 위해서 Serializable 인터페이스를 구현합니다.

  또한 __BaseVO는 페이징, 검색 기능을 구현하기 위한 보조 클래스로서 Criteria와 SearchCriteria가 가지는 필드를 가지고 있습니다.__ 따라서
  각 테이블에 매칭되는 VO를 생성할 때에는 페이징, 검색 혹은 중복되는 컬럼이 있는 경우 BaseVO를 상속받아 사용합니다.

  그렇게되면 `상위클래스를 상속받은 하위클래스의 커맨드 사용`에 의해서 상위 클래스의 필드까지 바인딩이 됩니다.

  즉, BaseVO에는 테이블에서 중복되는 공통속성을 가지고 있으며 페이지 Criteria 속성과 검색 SearchCriteria 속성을 가지고 있습니다. 그리고 페이지기능을 구현하기 위해 `initPaging()` 이라는 메서드를 가지고 페이지 초기화를 시켜줍니다.
