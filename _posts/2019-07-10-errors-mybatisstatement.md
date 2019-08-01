---
title: "Mapped Statements collection does not ..."
layout: post
category: ERRORS
tags: [Spring]
excerpt: "Mapped Statements collection does not contain value ... 에러를 해결합시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Mapped Statements collection does not contain value ... 에러를 해결합시다.

## ERRORS

  __Mapped Statements collection does not contain value ...__ 해당 문구의 에러는 mapper에서 설정한 id값이 DAOImpl에서 namespace.id로 접근하는 id값이 다르기 때문이다.

  ```xml
  <mapper namespace="boardDAO">
    <insert id="insert" parameterType="boardDTO">
      INSERT INTO b_board (title, contents, id, atch_file_id)
      VALUES (#{title},#{contents},#{id},#{atch_file_id})
    </insert>
  </mapper>
  ```

  위 처럼 insert 태그의 id 속성명이 insert인데 아래와 같이 DAOImpl에서 다른 이름으로 접근할 경우 발생하는 오류 입니다.
  따라서 Mapper.xml과 DAOImpl을 잘 찾아보시면 오타가 있을 것입니다.

  ```java
public void insert(BoardDTO boardDTO) {
  sqlSessionTemplate.insert("boardDAO.insertDAO", boardDTO);
}
  ```
