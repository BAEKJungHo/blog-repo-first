---
title: "웹 개발시 검색조건이 3개 이상인 경우"
layout: post
category: JSP
tags: [JSP]
excerpt: "웹 개발시 검색조건이 3개 이상인 경우에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 웹 개발시 검색조건이 3개 이상인 경우에 대해서 배워 봅시다.

## 검색조건이 3개 이상인 경우

  검색조건이 3개이상이면서 검색키워드(실제로 문자열을 입력하여 검색하는 input text)가 있는 경우 필요한 컬럼은 다음과 같습니다.

  - 학번(SELECT OPTION)-학과(SELECT OPTION)-과목(SELECT OPTION)-검색(INPUT TEXT)

  위 처럼 구성이 되어있을 경우 검색조건(SearchCondition)과 검색키워드(SearchKeyword)는 다음과 같습니다.

  - 검색조건(SearchCondition)
    - searchCondition1 = 학번
    - searchCondition2 = 학과
    - searchCondition3 = 과목

  - 검색키워드(SearchKeyword)
    - searchKeyword = 검색

  - JSP

  ```html
  <form action="<c:url value="/ws/dept/"/>" id="searchForm" name="searchForm">
      <label for="cont_select1" class="hidden">학번</label>
      <select name="searchCondition" id="cont_select1">
          <option value="">전체</option>
          <c:forEach items="${stdNum}" var="stdNum" varStatus="loop">
              <option value="${stdNum.data}" <c:if test="${searchVo.searchCondition eq stdNum.data}">selected="selected"</c:if>>${minwonField.title}</option>
          </c:forEach>
      </select>
  </form>
  ```

  jsp에서는 forEach문을 사용하여 select option을 출력합니다.

  mysql에서는 searchCondition이 `""`값이 아니면 되므로 StringUtils의 isNotBlank를 이용하고, 아래와 같이
  `AND FIELD = #{searchCondition}` searchContdition에 저장된 값을 비교하면 됩니다. 왜냐하면, DB에 들어가있는 값이 searchCondition에 들어있는 값이기 때문입니다.

  - MyBatis.xml

  ```xml
  <sql id="minwonSearch">
      <choose>
          <when test="@org.apache.commons.lang3.StringUtils@isNotBlank(searchCondition)">
              AND FIELD = #{searchCondition}
          </when>
      </choose>
      <choose>
          <when test="@org.apache.commons.lang3.StringUtils@isNotBlank(searchCondition2)">
              AND   F_PROCESS_DEPT #{searchCondition2}
          </when>
      </choose>
      <choose>
          <when test="searchCondition3 == 1">
              AND   TITLE LIKE CONCAT('%',#{searchKeyword},'%')
          </when>
          <when test="searchCondition3 == 2">
              AND REQUEST_METHOD LIKE CONCAT('%', #{searchKeyword}, '%')
          </when>
          <otherwise>
              AND (
              TITLE LIKE CONCAT('%',#{searchKeyword},'%')
              OR REQUEST_METHOD LIKE CONCAT('%',#{searchKeyword},'%')
              )
          </otherwise>
      </choose>
  </sql>
  ```
