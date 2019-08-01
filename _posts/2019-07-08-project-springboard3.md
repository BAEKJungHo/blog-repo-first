---
title: "[스프링 MVC 게시판] 3. 테이블 생성"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 테이블 생성

  - Board

  ```sql
CREATE TABLE b_board (
num int(6) unsigned NOT NULL AUTO_INCREMENT,
title varchar(50) NOT NULL,
count int(5) unsigned default 0,
date datetime NOT NULL DEFAULT current_timestamp,
contents varchar(250),
id varchar(20) not null default '',
del_chk varchar(1) not null default 'N',
PRIMARY KEY (num),
foreign key (id) references b_users(id)
) AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
  ```

  위에서 count는 조회수 증가를 나타내기 위한 컬럼인데 default를 0으로 설정해놔야 숫자가 증가합니다.
  null값일 경우 증가하지 않습니다.

  del_chk는 게시판에서 작성한 글을 삭제할 때 실제로 삭제하는 것이 아니라, 상태 값으로 N(미삭제)인 게시글만 조회해서
  보여주는 방식을 사용합니다.(실무 방식)

  - Users

  ```sql
  CREATE TABLE b_users (
	id varchar(20) not null default '',
	name varchar(10) not null default '',
	pwd varchar(20) not null default '',
	PRIMARY KEY (id)
  ) DEFAULT CHARSET=utf8;
  ```
