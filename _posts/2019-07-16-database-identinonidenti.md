---
title: "식별관계와 비식별관계"
layout: post
category: DataBase
tags: [DataBase]
excerpt: "식별관계와 비식별관계에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 식별관계와 비식별관계

  A, B, C 테이블이 있다고 가정하겠습니다. A테이블의 기본키는 aPrimary이고 B테이블의 기본키는 bPrimary
  C테이블의 기본키는 cPrimary라고 하겠습니다.

  B, C테이블은 A테이블을 참조합니다. 즉, 기본키 외래키 관계가 성립하는데 만약, B테이블의 기본키인 bPrimary가
  FK의 역할도 한다면, A와 B는 `식별관계`라고 합니다.

  하지만, C테이블의 cPrimary키는 기본키역할만 하고, 다른 컬럼인 c1Column이 FK역할을 담당한다면,
  이 둘은 `비식별관계`라고 합니다.

  __식별관계는 참조하는 테이블에서 기본키가 외래키의 역할까지 하는경우, 비식별관계는 참조하는 테이블에서
  기본키는 기본키의 역할만하고, 다른 컬럼이 외래키의 역할을 담당하는 경우를 말합니다.__
