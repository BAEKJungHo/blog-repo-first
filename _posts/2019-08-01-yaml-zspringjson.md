---
title: "Spring application.yml 설정 파일"
layout: post
category: YAML
tags: [YAML, Spring]
excerpt: "Spring application.yml 설정 파일에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Spring application.yml 설정 파일에 대해서 배워 봅시다.

## application.yml

```
spring:
datasource:
  url: jdbc:log4jdbc:mysql://XXX.XXX.XX.XX:XXXX/DB명?autoReconnect=true&useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
  username: DB명
  password: pw
  driver-class-name: net.sf.log4jdbc.DriverSpy
  tomcat:
    max-active: 400
    max-wait: 10000
    max-idle: 20
    test-on-borrow: true
    validation-query: select 1
http:
  multipart: // multipart 파일 사이즈
    max-file-size: 200MB
    max-request-size: 200MB
mvc:
  view:
    prefix: /WEB-INF/jsp/
    suffix: .jsp
messages:
  basename: messages/message
mybatis:
type-aliases-package: xxx.xxx.xxx.module // @Alias 어노테이션을 사용한 클래스들이 담겨있는 패키지
mapper-locations: classpath:mybatis/mapper/**/*_MySQL.xml
config-location: classpath:mybatis/config/Mybatis_Config.xml

server:
address: xxx.0.0.1

contents:
file-encoding: UTF-8
```
