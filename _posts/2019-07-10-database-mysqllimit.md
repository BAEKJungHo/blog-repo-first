---
title: "MySQL limit"
layout: post
category: DataBase
tags: [MySQL]
excerpt: "MySQL의 limit 사용법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MySQL의 limit 사용법에 대해서 배워 봅시다.

## limit

  limit을 사용하면, 게시판 페이징 처리 등을 쉽게 구현할 수 있습니다.

  ```sql
  SELECT b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id
  FROM b_board as b
  INNER join b_users as u
  ON (b.id = u.id)
  WHERE del_chk = 'N'
  ORDER BY num DESC
  LIMIT 1, 10;
  ```

  limit a, b는 a번째부터 b개의 값을 가져오라는 의미를 가집니다.

  그냥 limit 8과 같이 적으면 8개의 데이터를 가져오라는 의미를 가집니다.

  limit a, b말고도 limit a offset b와 같이 사용하는 방법도 있습니다.

## OFFSET

  - limit a, b         a부터 b만큼
  - limit b offset a   a부터 b만큼
  - limit a offset b   b부터 a만큼

  즉, 콤마(,)와 OFFSET은 정반대로 동작합니다.
