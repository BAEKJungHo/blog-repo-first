---
title: "LEFT OUTER JOIN"
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

### 참고

  > [ON 절과 WHERE 절의 차이 공부하기](https://baekjungho.github.io/database-onwhere/)

## 언제 LEFT JOIN을 사용할까?

  LEFT JOIN을 써야 하는 경우는, `게시판-댓글`이나 `메인 페이지나-서브 페이지` 등 `서로 연관된 테이블 or 페이지` 일때, 댓글이 없다고 게시판 페이지를 에러페이지로 떨구면 안되며("데이터가 없습니다" 와 같은 문구가 나와야함), 서브 페이지의 데이터가 없다고 메인 페이지까지 안나오면 안되는 경우에 LEFT JOIN을 씁니다.

  ```xml
      <select id="countSurveyParticipant" parameterType="surveyStatistic" resultType="int">
            SELECT DISTINCT
            S.TOTAL_COUNT
          FROM
            MEC_C_SURVEY AS S
          LEFT JOIN SURVEY_ANSWER AS A
          ON (S.SEQ = A.SUR_SEQ)
          LEFT JOIN PRIVACY_INFO AS M
          ON (
              A.PRIVACY_SEQ = M.SEQ
                <if test="@org.apache.commons.lang3.StringUtils@isNotBlank(searchKeyword2)">
                    AND   M.user_name LIKE CONCAT('%',#{searchKeyword2},'%')
                </if>
              )
          WHERE
                S.SEQ = #{surSeq}
    </select>
  ```
