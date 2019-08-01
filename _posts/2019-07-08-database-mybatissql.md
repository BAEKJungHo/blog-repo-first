---
title: "MyBatis 동적 SQL"
layout: post
category: DataBase
tags: [DataBase, MySQL, MyBatis]
excerpt: "스프링과 MyBatis에서 사용하는 동적 SQL에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 동적 SQL

  MyBatis의 가장 강력한 기능 중 하나는 동적 SQL을 처리하는 방법입니다.
  JDBC나 다른 유사한 프레임워크를 사용해본 경험이 있다면 동적으로 SQL 을 구성하는 것이 얼마나 힘든 작업인지 이해할 것입니다. 간혹 공백이나 콤마를 붙이는 것을 잊어본 적도 있을 것입니다. 동적 SQL 은 그만큼 어려운 것입니다.

  스프링 MVC 게시판에 적용한, 동적 SQL을 사용한 Mapper.xml 코드를 보겠습니다.

### choose, when, otherwise

  ```xml
  <!-- 검색 대한 게시글 수 -->
<select id="countArticle" resultType="int">
  SELECT count(*)
  FROM b_board as b
  INNER JOIN b_users as u
  ON (b.id = u.id)
  WHERE del_chk = 'N'
  <include refid="search" />
</select>

<!-- 검색 -->
<select id="searchList" resultType="boardDTO">
  SELECT b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id
  FROM b_board as b
  INNER JOIN b_users as u
  ON (b.id = u.id)
  WHERE del_chk = 'N'
  <include refid="search" />
  ORDER BY num DESC
  LIMIT #{pageStart}, #{perPageNum}
</select>

<!-- MyBatis 동적 sql -->
<sql id="search">
  <choose>
    <when test="searchType == 'all'">
      AND (u.name LIKE CONCAT('%', #{keyword}, '%')
      OR b.contents LIKE CONCAT('%', #{keyword}, '%')
      OR b.title LIKE CONCAT('%', #{keyword}, '%'))
    </when>
    <when test="searchType == 'writer'">
      AND u.name LIKE CONCAT('%', #{keyword}, '%')
    </when>
    <when test="searchType == 'content'">
      AND b.contents LIKE CONCAT('%', #{keyword}, '%')
    </when>
    <when test="searchType == 'title'">
      AND b.title LIKE CONCAT('%', #{keyword}, '%')
    </when>
  </choose>
</sql>
  ```

  위 코드에서 동적 SQL을 나타내는 곳은 `<sql id="search">` 이 부분입니다.

  동적 SQL에서 지원하는 `choose, when, otherwise` 구문을 사용한 것이며, 자바의 if-else와 비슷합니다.
  또한 JSTL의 choose 태그와 사용법이 비슷합니다.

## <when test="searchType == 'writer'">

  `<when test="searchType == 'writer'">`이 코드에서 searchType과 writer는 어떤 파일의 속성과 일치 시켜줘야 하는지 배워 보겠습니다.

  > 다음 코드는 [스프링 MVC 게시판](https://github.com/BAEKJungHo/SpringBoard/blob/master/Board/src/main/resources/mapper/boardMapper.xml)에서 가져온 코드입니다.

  스프링 MVC 게시판에서 검색 기능을 구현하기 위해 JSP 파일에서 select와 option태그를 사용했으며, 페이지 구현을 위한 Criteria 클래스와 검색을 구현하기 위한 Criteria를 상속받은 SearchCriteria 클래스를 만들고 SearchCriteria 클래스안에 searchType이라는 속성명이 있습니다.

  - View 파일

  ```html
  <form name="searchForm" action="<c:url value="/boardSearchList" />" method="post" >
  <select name="searchType">
    <option value="all" <c:out value="${map.searchType =='all'? 'selected':'' }"/>>제목+이름+내용</option>
    <option value="writer" <c:out value="${map.searchType =='writer'? 'selected':'' }"/>>이름</option>
    <option value="content" <c:out value="${map.searchType =='content'? 'selected':'' }"/>>내용</option>
    <option value="title" <c:out value="${map.searchType =='title'? 'selected':'' }"/>>제목</option>
  </select>
  <input type="text" name="keyword" value="${map.keyword}" placeholder="검색">
  <input id="submit" name="submit" type="submit" value="검색">
  <input id="submit" name="cancel" type="reset" value="취소" />
  </form>
  ```

  즉, 동적 SQL의 when 태그에서 `searchType`은 __View 파일의 select name명과 동일해야 하며, SearchCriteria 클래스의 속성명과 동일해야 합니다.__
  그리고 writer는 View 파일의 option value 값과 동일해야 합니다.

### 반복되는 Query를 묶어서 include 하기

  위 코드에서 주의 깊게 봐야하는 부분이 __중복되는 Query를 동적 SQL로 만들고 include 태그를 사용하여 검색과 검색에 대한
  게시글 수 쿼리문에 삽입하였습니다.__ 사용방법은 아래와 같습니다.

  ```xml
  <include refid="sql id 속성명" />
  ```

  반복되는 Query를 묶어서 include할 때에 하나의 Tip이 있습니다.

  동적 sql문에서 where절이 없는 것을 볼 수 있는데, where절을 각각의 쿼리문에 무조건 실행 되도록 삽입하는 것입니다.

  즉, 아래와 같이 where 절을 삽입할 경우, 무조건 실행되며 그 다음 동적 SQL문을 실행합니다.

  ```sql
  where 1=1
  ```

  즉 1과 1은 무조건 같으므로, 다음 코드를 안정적으로 실행할 수 있게 됩니다.

## if

  동적 SQL의 if 태그는, c:if 태그 사용법과 유사하며, 자바와도 유사합니다.

  ```xml
  <select id="findActiveBlogWithTitleLike" resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = 'ACTIVE'
  <if test="title != null">
    AND title like #{title}
  </if>
  </select>
  ```

  그 외에도 foreach등 여러가지 동적 sql문이 많습니다. 아래 참조에 있는 링크를 타면 MyBatis에 관한 동적 SQL에 대한
  정보를 얻을 수 있을 것입니다.

## 참조

  > [http://www.mybatis.org/mybatis-3/ko/dynamic-sql.html](http://www.mybatis.org/mybatis-3/ko/dynamic-sql.html)
