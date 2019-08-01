---
title: "CDATA"
layout: post
category: DataBase
tags: [MySQL, MyBatis]
excerpt: "MyBatis의 Mapper.xml에서 사용하는 CDATA에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis의 Mapper.xml에서 사용하는 CDATA에 대해서 배워 봅시다.

## CDATA

  CDATA를 사용하는 이유는 Mybatis 사용시 쿼리문에 문자열 비교연산자나 부등호를 처리할 때가 있습니다. 그러면 < 와 같은 기호를 사용할 때 괄호인지 비교연산자 인지 확인이 되지 않습니다. 또한 특수문자 사용하는데 제한이 있습니다.

  __단, CDATA를 사용한다 하더라도 ;(세미콜론)만은 예외입니다.__ 즉, Mapper.xml에서는 세미콜론을 적지 않습니다.
  또한 하나의 쿼리태그당 하나의 쿼리만 실행합니다.

  ```sql
  <select id="countBoardList" resultType="Integer">
  <![CDATA[
      SELECT count(*)
      FROM b_board
      WHERE del_chk = 'N'
  ]]>
  ```

  사용방법은 `<![CDATA[ ~쿼리~ ]]>` 방식으로 사용합니다.

  이렇게 사용하면 SQL안에 특수문자가 들어가도 문자열로 인식하기 때문에 문제를 해결할 수 있습니다.

  즉, del_chk = 'Y'와 같이 부등호 혹은 문자열을 표현하고 싶을 때에는 CDATA를 사용해야 합니다.
