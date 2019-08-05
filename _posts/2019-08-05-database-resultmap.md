---
title: "MyBatis resultMap"
layout: post
category: DataBase
tags: [MyBatis]
excerpt: "MyBatis resultMap에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis resultMap에 대해서 배워 봅시다.

## resultMap

  - `Query`

```xml
 <select id="findSurveyParticipantExcel" parameterType="surveyStatistic" resultMap="participant">
  SELECT DISTINCT
  M.USER_NAME,
  A.PRIVACY_SEQ,
  A.SUR_SEQ,
  M.SEQ,
  M.USER_NAME,
  M.PHONE,
  .....
 </select>
 ```

 - `resultMap Query`

 ```xml
 <!-- id : 키값 -->
 <!-- type : VO 클래스의 이름, CamelCase 적용 -->
 <resultMap id="participant" type="surveyStatistic">
 <!-- column : DB 컬럼명 -->
 <!-- property : type에 적힌 VO클래스에 있는 속성 명 -->
        <result property="userName" column="USER_NAME" />
        <result property="privacySeq" column="PRIVACY_SEQ" />
        <result property="surSeq" column="SUR_SEQ" />

 <!-- association은 1:1 관계 즉, VO 형식으로 받는 경우-->
 <!-- collection은 1:N 관계 즉, LIST 형식으로 받는 경우-->

 <!-- association의 javaType은 surveyStatistic 클래스 안에 명시된 privacyInfo 클래스명 -->
 <!-- assoication의 property는 surveyStatistic 클래스 안에 명시된 privacyInfo 객체 참조 변수명 -->
        <association property="privacy" javaType="privacyInfo">
            <id property="seq" column="PRIVACY_SEQ" />
            <result property="userName" column="user_name" />
            <result property="phone1" column="phone" />
            <result property="email1" column="email" />
            <result property="tel1" column="tel" />
        </association>
    </resultMap>
 ```

  resultMap에서 `id`에 들어가는 값은 select 쿼리태그의 `resultMap에 들어가는 키값`이라고 생각하면 됩니다. type은 VO의 이름이 들어간다고 생각하면됩니다.

  예를들어 VO의 클래스명이 SurveyStatisticVO일 경우에는 type에 surveyStatisticVO를 적으면 되고, `@Alias` 별칭을 준 경우 @Alias("별칭명")에 적은 별칭명을 `type`에 적어주면 됩니다.

  - `SurveyStatisticVO`

  ```java
  public class SurveyStatisticVO {

    /**
    * SurveyStatisticVO의 Original 속성
    */
    private String userName;
    private int privacySeq;
    private int surSeq;

    /**
    * PrivacyInfo의 속성
    * userName
    * phone1
    * email1
    * tel1
    */
    private PrivacyInfo privacy; // association 형식
    private List<PrivacyInfo> privacy; // collection 형식
    .....
  }
  ```

  위에서 select 동적 쿼리 실행 결과, 데이터를 가져오면, SurveyStatisticVO의 클래스에 자동으로 값이 매핑 됩니다.

  `userName, privacySeq, surSeq`값은 SurveyStatisticVO 클래스에 매핑이 되고, 그 외의 userName, phone1, email1, tel1의 값들은
  PrivacyInfo 클래스에 매핑이 됩니다. 따라서 SurveyStatisticVO 클래스에 PrivacyInfo 객체참조변수를 만들었으므로, `privacy 객체참조변수`로 PrivacyInfo 클래스의
  속성에 접근할 수 있습니다.
