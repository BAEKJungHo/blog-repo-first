---
title: "LEFT(RIGHT) OUTER JOIN"
layout: post
category: DataBase
tags: [MySQL, Oracle]
excerpt: "LEFT OUTER JOIN에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > LEFT OUTER JOIN에 대해서 배워 봅시다.

## LEFT OUTER JOIN

  > TIP!!
  >
  > INNER JOIN은 INNER를 생략하고 쓸 수 있습니다. LEFT, OUTER JOIN은 OUTER를 생략하고 쓸 수 있습니다.

  ![join](/images/posts/201904/join.jpg)

  ```sql
SELECT DISTINCT
  B.NUM,
  B.TITLE,
  B.DATE,
  B.COUNT,
  B.CONTENTS,
  B.ID,
  B.ATCH_FILE_ID,
  F.atch_file_id,
  F.del_chk
FROM
  B_BOARD AS B
JOIN B_USERS AS U
ON (B.ID = U.ID)
LEFT JOIN B_FILEDETAIL F
ON (B.ATCH_FILE_ID = F.ATCH_FILE_ID)
WHERE B.NUM = #{NUM}
AND F.del_chk = 'N'
AND B.DEL_CHK = 'N'
  ```

  여기서 B_FILEDETAIL 테이블의 값이 없는경우 FROM절과 JOIN을 수행하고나서  WHERE절로 NUM(게시물 번호)이 5인애와 FILEDETAIL의 삭제상태가 'N'인 애들을 판단하기 때문에 정상적으로 나오지 않습니다.

  위 쿼리를 정상적으로 실행 시키기 위해서는 아래와 같이 하면 됩니다.

  ```sql
  SELECT DISTINCT
    B.NUM,
    B.TITLE,
    B.DATE,
    B.COUNT,
    B.CONTENTS,
    B.ID,
    B.ATCH_FILE_ID,
    F.atch_file_id,
    F.del_chk
  FROM
    B_BOARD AS B
  JOIN B_USERS AS U
  ON (B.ID = U.ID)
  LEFT JOIN B_FILEDETAIL F
  ON (B.ATCH_FILE_ID = F.ATCH_FILE_ID
    and F.del_chk = 'N')
  WHERE b.NUM = #{num}
  and b.DEL_CHK = 'N';
  ```

  ON 절안에 `F.del_chk = 'N'` 조건을 추가로 줘서 먼저 실행시키게 되면, B_FILEDETAIL의 값이 NULL이더라도
  정상적으로 쿼리 결과가 나옵니다.
