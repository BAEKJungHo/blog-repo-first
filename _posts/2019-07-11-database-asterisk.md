---
title: "Asterisk(*)"
layout: post
category: DataBase
tags: [MySQL, Oracle]
excerpt: "실무관련 Asterisk에 관한 팁을 배워 보겠습니다."
author: BAEKJungHo
---

* content
{:toc}

## Asterisk는 가급적 사용 배제

  쿼리(Query)문을 작성할 때,  `Asterisk(*)`는 사용하지말고, 결과를 얻고자 하는 컬럼명만 적어주는게 좋습니다.
  그 이유는 실무에서는 검색속도 즉, `성능`을 신경써서 개발해야하는데, 만약에 Asterisk로 불필요한 컬럼까지
  가져오게되면 속도가 느려지기 때문입니다. 
