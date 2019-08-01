---
title: "CamelCase(카멜 표기법)"
layout: post
category: Spring
tags: [Spring, MyBatis]
excerpt: "mybatis-config.xml 파일에서 동작 방식을 결정할 때 사용하는 카멜 표기법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > CamelCase(카멜 표기법)에 대해서 배워 봅시다.

## CamelCase

  CamelCase는 스프링에서 MyBatis를 사용하여, `mybatis-config.xml` (MyBatis의 작동 규칙을 정의하는 파일)에서 설정할 수 있습니다.

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- MyBatis 작동 규칙 정의 -->
	<settings>
		<setting name="cacheEnabled" value="false" />
	<!-- 카멜 표기법 -->
	  <setting name="mapUnderscoreToCamelCase" value="true" />
	</settings>

	<typeAliases>
		<typeAlias alias="boardDto" type="com.mayeye.board.dto.BoardDTO"/>
		<typeAlias alias="usersDto" type="com.mayeye.board.dto.UsersDTO"/>
		<typeAlias alias="fileDetail" type="com.mayeye.board.dto.FileDetail"/>
		<typeAlias alias="fileMaster" type="com.mayeye.board.dto.FileMaster"/>
	</typeAliases>

</configuration>
  ```

  즉 카멜 표기법은 아래와 같이 추가하면 됩니다.

  ```xml
  <settings>
    <setting name="mapUnderscoreToCamelCase" value="true" />
  </settings>
  ```

  실무에서는 카멜 표기법을 사용하는데 그 이유는 setter나 getter를 만드는 등 자바에서는 언더바가 없는 편이 가독성이 좋고 편하기 때문입니다.
  __카멜 표기법을 사용하는 경우 xml을 포함한 VO, DAO, Service, Controller, View까지 전부다 카멜 표기법으로 바꿔줘야 합니다.__

  예를들어 DB 컬럼명으로 atch_file_id라는 컬럼을 생성하였으면, 카멜 표기법을 사용할 경우 자바의 VO에서는 atchFileId라고 생성해야
  정상적으로 바인딩이 됩니다. 만약 카멜표기법을 사용함에도 불구하고 Mapper.xml에서 쿼리문을 날릴때 아래와 같이 할 경우, 뭐가 문제인지 오류를 발견하기 힘듭니다. 또한 쿼리 결과에 따라 VO로 바인딩이 되어야 하는데, 바인딩이 일어나지 않습니다.

  ```sql
  INSERT INTO b_board (title, contents, id, atch_file_id)
  VALUES (#{title},#{contents},#{id},#{atch_file_id})
  ```

### 작동 방식 설정

  ```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
    <!-- mybatis cache 사용여부 -->
	 	<setting name="cacheEnabled" value="true"/>
    <!-- 지연로딩 사용여부 -->
	 	<setting name="lazyLoadingEnabled" value="true"/>
    <!-- 한 개의 구문에서 여러 개의 ResultSet을 허용할지 여부 -->
	  <setting name="multipleResultSetsEnabled" value="true"/>
    <!-- 컬럼명 대신 컬럼 라벨을 사용 -->
	 	<setting name="useColumnLabel" value="true"/>
    <!-- 생성키에 대한 JDBC 지원 허용 여부 -->
	 	<setting name="useGeneratedKeys" value="false"/>		
    <!-- mybatis가 컬럼을 필드/프로퍼티에 자동으로 매핑할지와 방법에 대한 명시(PARTIAL은 중첩되지 않은 것들을 매핑 -->
	 	<setting name="autoMappingBehavior" value="PARTIAL"/>
    <!-- 디폴트 Executor 설정(SIMPLE은 특별히 동작하는 것은 업음) -->
	  <setting name="defaultExecutorType" value="SIMPLE"/>
    <!-- DB 응답 타임아웃 설정 -->
	 	<setting name="defaultStatementTimeout" value="10"/>
    <!-- 중첩구문내 RowBound 사용 허용여부 -->
	  <setting name="safeRowBoundsEnabled" value="false"/>
    <!-- 전통적 DB 컴럼명을 JAVA의 Camel표기법으로 자동 매핑 설정 -->
	 	<setting name="mapUnderscoreToCamelCase" value="true"/>
    <!-- 로컬캐시 사용여부(SESSION: 세션을 사용해서 모든쿼리를 캐시) -->
	  <setting name="localCacheScope" value="SESSION"/>
    <!-- mybatis로 넘어오는 parameter가 null인 경우, jdbcType을 Setting -->
  	<setting name="jdbcTypeForNull" value="NULL"/>
    <!-- 지연로딩을 야기하는 객체의 메소드를 명시 -->
	 	<setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
    <!-- 가져온 값이 null일때 setter나 맵의 put 메소드를 호출할지를 명시 (false일경우, null인 field는 제거되어 나타남 : default는 false -->
	  <setting name="callSettersOnNulls" value="true"/>
	</settings>
</configuration>
  ```

## 참조

  > [http://www.mybatis.org/mybatis-3/ko/configuration.html](http://www.mybatis.org/mybatis-3/ko/configuration.html)
  >
  > [https://m.blog.naver.com/userspace/220835393572](https://m.blog.naver.com/userspace/220835393572)
