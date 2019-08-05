---
title: "ON절과 WHERE절의 차이"
layout: post
category: DataBase
tags: [MySQL, Oracle]
excerpt: "ON절과 WHERE절의 차이에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > ON절과 WHERE절의 차이에 대해서 배워 봅시다.

## ON절과 WHERE절의 차이

  > ON절과 WHERE절의 차이점은 JOIN 하는 범위와 제약조건 위치가 다릅니다.

### 제약조건 위치 차이

  ```xml
      <select id="countSurveyParticipant" parameterType="surveyStatistic" resultType="int">
            SELECT DISTINCT
            S.TOTAL_COUNT
          FROM
            SURVEY AS S
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

  위 예제를 보면 LEFT JOIN이 2번 사용된 것을 볼 수 있습니다.

  - 첫 번째

  ```SQL
  LEFT JOIN SURVEY_ANSWER AS A
  ON (S.SEQ = A.SUR_SEQ)
  ```

  - 두 번째

  ```SQL
  LEFT JOIN PRIVACY_INFO AS M
  ON (
      A.PRIVACY_SEQ = M.SEQ
        <if test="@org.apache.commons.lang3.StringUtils@isNotBlank(searchKeyword2)">
            AND   M.user_name LIKE CONCAT('%',#{searchKeyword2},'%')
        </if>
  )
  ```

  - 마지막 WHERE절

  ```SQL
  WHERE
        S.SEQ = #{surSeq}
  ```

  위 코드를 자세히 보면 LEFT JOIN을 사용할 때 중요한 특징을 발견할 수 있습니다.

  > 규칙 !!
  >
  > LEFT OUTER JOIN 시 ON 절에서는 우측(널값으로 채워지는 쪽)의 추가 제약조건을 넣고, 좌측의 추가 제약조건은 WHERE절에 넣어야 한다.

  즉, 첫 번째에서 좌측 제약조건을 걸어야 하는 테이블은 `FROM 절에 있는 SURVEY`이며, 우측 제약조건을 걸어야 하는 테이블은 `JOIN을 사용한 SURVEY_ANSWER`입니다.
  따라서 마지막 WHERE절을 보면 알 수 있듯이, 규칙에 따라 FROM절에 있는 SURVEY에 대한 조건이 추가되었습니다.

  두 번째에서 좌측 제약조건을 걸어야 하는 테이블은 `SURVEY_ANSWER` 테이블이며, 우측 제약조건을 걸어야 하는 테이블은 `PRIVACY_INFO` 테이블 입니다.
  규칙에 따라, ON절에는 우측 제약 조건 테이블인 `PRIVACY_INFO`에 대한 제약조건이 들어가 있습니다.

### 범위 차이

  위 예제를 기준으로 설명하겠습니다.

  위 예제의 ON절을 해석하면 __searchKeyword2가 NULL이나 빈 값이 아닐경우, PRIVACY_INFO의 userName이 searchKeyword2인 값들에 대해서 조인__ 을 한다는 의미입니다.
  만약, ON절 조건이 WHERE로 가게 되면, 선 조인 후, PRIVACY_INFO 테이블의 userName이 searchKeyword2인 애들을 검색합니다.

  이 둘의 차이는, ON절에 조건을 줘서 조인을 하게되면, PRIVACY_INFO 테이블이 NULL값이더라도, 결과에 출력이 됩니다.

  하지만, WHERE절로 가게되면, NULL값이기 때문에 결과에 출력이 안됩니다.
