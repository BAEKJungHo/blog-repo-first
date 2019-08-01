---
title: "jdbcTypeForNull"
layout: post
category: Spring
tags: [Spring, MyBatis]
excerpt: "mybatis-config.xml 파일에서 동작 방식을 결정할 때 사용하는 jdbcTypeForNull에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > jdbcTypeForNull에 대해서 배워 봅시다.

## jdbcTypeForNull

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!-- 자동으로 카멜케이스 규칙으로 변환 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="jdbcTypeForNull" value="NULL" />
    </settings>
</configuration>
  ```

  실무에서는 주로 상황에 따라 다르지만 `카멜케이스와 jdbcTypeForNull`을 주로 사용합니다. `jdbcTypeForNull` 이부분을 Null로 설정해주면 MyBatis에서 쿼리에 매핑되는 파라미터 값이 Null인경우 에러가 발생합니다.
