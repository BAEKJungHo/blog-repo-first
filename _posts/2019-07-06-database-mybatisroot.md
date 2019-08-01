---
title: "MyBatis 사용 방식(Java, XML)"
layout: post
category: DataBase
tags: [DataBase, MySQL, MyBatis]
excerpt: "MyBatis를 사용하는 방식에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis를 사용하는 방식에 대해서 배워 봅시다.

## MyBatis 방식

  MyBatis를 쓰는 방식은 크게 2가지가 있습니다. 여기서는 MyBatis를 쓰기위해 설정하는 방법에 대해서는
  다루지 않겠습니다.

### 첫 번째 방식

  > Mapper.xml작성 -> DAO Interface 만들기 -> 구현체(실체클래스, DAOImpl) 만들기 -> Service Interface 만들기
  -> 구현체(실체클래스, ServiceImpl) 만들기 -> Controller 만들기

  위 방식대로 구현을 하면됩니다.

  DB Query가 실행되기 까지의 과정은 위 과정을 거꾸로 뒤집으면됩니다.

  첫 번째 방식의 특징은 Mapper.xml에서 `쿼리태그속성명(즉, id 값)`과 `추상메소드명`을 일치시키지 않아도 됩니다.
  일치 시켜야 할 부분은 쿼리태그속성명과 DAOImpl에서 sqlSessionTemplate.내장메소드("namespace.쿼리태그속성명")으로 일치 시켜야 합니다.

  코드로 보면 다음과 같습니다.

  - Mapper.xml

  ```xml
  <mapper namespace="boardDAO">
	<!-- 게시판 목록 -->
	<select id="list" resultType="boardDTO">
		select b.num, b.title, u.name, b.date, b.count, b.id
		from b_board as b
		inner join b_users as u
		where b.id = u.id
		order by num desc
	</select>
  </mapper>
  ```

  위 처럼 Mapper.xml을 설정했을때 select 태그의 id 속성명이 `list`로 되어있습니다.
  다음 DAOImpl 클래스를 보겠습니다.

  > TIP !!
  >
  > Mapper.xml 파일이 1개일 경우 namespace.쿼리태그속성명 으로 접근하지 않고 namepace를 생략하여 접근할 수 있습니다.
  단, Mapper.xml 파일이 여러개 인 경우 namsapce.쿼리태그속성명으로만 접근 가능합니다.

  - BoardDAOImpl.java

  ```java
  @Override
  public List<BoardDTO> list() {
    return sqlSessionTemplate.selectList("boardDAO.list");
  }
  ```

  즉 위처럼 namespace.쿼리태그속성명으로 접근해야 합니다.

### 두 번째 방식

  > Mapper.xml 작성 -> DAO Interface작성 -> Service Interface 작성 -> ServiceImpl 작성

  DB Query가 실행되는 순서는 반대입니다.

  __즉, 구현체를 만들지 않는다는 특징이 있습니다.__

  단, 이 경우는 `mybatis-config.xml(마이바티스 설정파일)`을 `자바(Java)`로 구현한다는 점이 특징입니다.

  그리고 이 경우는 `쿼리태그속성명`과 `추상메소드명`을 일치 시켜야 합니다.

  - MyBatis 설정파일(Java)

  ```java
  @Configuration
  @MapperScan(basePackages = "net.mayeye.*.*.*.repository")
  public class MybatisConfig { }
  ```

  위 코드 만으로 MyBatis 설정파일이 됩니다. @Configuration 어노테이션은 스프링 설정 파일임을 명시해주는 어노테이션으로,
  위 클래스가 스프링 설정 파일임을 명시하고, @Configuration 라고 불리는 스프링의 자바설정을 사용한다면 <mybatis:scan/>보다는 @MapperScan를 사용하길 선호할것입니다. 이 두번째 방식은 Spring Boot에서 자주 사용하는 방식입니다.

  @MapperScan 어노테이션을 통해서 해당 경로에 있는 `DAO Interface`를 통해 구현체를 구현하지 않아도 Mapper.xml과 통신할 수 있게 해줍니다.
  즉, `DAO Interface를 구현체로 인식` 하게 되는 것입니다.

  마이바티스 설정파일을 자바로 설정하는 이유는 자바로하면 디버깅을 사용할 수 있기 때문입니다.

## 참조

  > [http://www.mybatis.org/spring/ko/mappers.html](http://www.mybatis.org/spring/ko/mappers.html)
