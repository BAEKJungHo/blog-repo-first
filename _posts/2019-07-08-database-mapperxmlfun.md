---
title: "SQL Query로 날짜 자르기"
layout: post
category: DataBase
tags: [MySQL, MyBatis]
excerpt: "SQL Query로 날짜 자르는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis에서 사용하는 #과 $의 차이에 대해서 배워 봅시다.

## SQL Query로 날짜 자르기

  ```sql
  select b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id
  from b_board as b
  inner join b_users as u
  where b.id = u.id and del_chk = 'N'
  order by num desc
  limit 0, 10;   
  ```

  Mapper.xml안에 위 Query가 있을 경우, 테이블의 컬럼명들은 JSP에서 EL로 출력할 때, 분명히
  컬럼명과, VO의 필드명이 같아야 합니다.

  datetime 타입인 date 컬럼을 `년-월-일`만 표시 하기 위해서 date_format 함수를 사용하는 경우
  `별칭(alias)`으로 VO의 필드명과 동일하게 만들지 않으면, Error가 발생합니다.

  따라서 별칭 as를 사용하여 원래 컬럼명인 date로 지정해야 합니다.
