---
title: "MyBatis Config. XML, JAVA"
layout: post
category: DataBase
tags: [MySQL, MyBatis]
excerpt: "MyBatis Config 파일을 XML과 자바로 설정하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis Config 파일을 XML과 자바로 설정하는 방법에 대해서 배워 봅시다.

## Mybatis_Config.xml

  - mybatis
  	- config
  	- mapper

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
    <settings>
        <!-- 자동으로 카멜케이스 규칙으로 변환 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="jdbcTypeForNull" value="NULL" />
    </settings>
</configuration>
  ```

## MybatisConfig.java

```java
@Configuration
@MapperScan(basePackages = "com.xxx.*.*.*.repository")
public class MybatisConfig {

}
```
