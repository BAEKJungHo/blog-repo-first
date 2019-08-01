---
title: "생명 주기(Life Cycle)"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 컨테이너와 빈(Bean)객체의 생명 주기(Life Cycle)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 컨테이너와 빈(Bean)객체의 생명 주기(Life Cycle)에 대해서 배워 봅시다.

## IOC 컨테이너, 빈(Bean) 객체 생명 주기

  - IOC 컨테이너 생명 주기

  ![c4](/images/posts/201906/c4.jpg)

  GenericXmlApplicationContext을 이용하여 IOC 컨테이너를 생성하면 그 안에 빈(Bean)들이
  존재하므로, 스프링 컨테이너 생성 시점과 빈(Bean) 객체 생성 시점은 거의 동일하다고 볼 수 있습니다.

  - 빈(Bean) 객체 생명 주기

  ![c5](/images/posts/201906/c5.jpg)

  destory()와 afterPropertiesSet()을 사용하려면 `InitializingBean, DisposableBean` 이 두개의 인터페이스를
  구현 해야 합니다.

## init-method, destory-method

  ![c6](/images/posts/201906/c6.jpg)

  그림과 같이 스프링 설정 파일에 `init-method와 destory-method 속성` 을 지정하면 `InitializingBean, DisposableBean` 인터페이스를 구현할 필요 없이 initMethod()와 destroyMethod()를 사용할 수 있습니다. 이 두 메소드는 빈(Bean) 객체가 생성 되는 시점에 호출되며, 빈(Bean) 객체가 소멸되는 시점에 호출됩니다.

  따라서 생명 주기를 이용하는 방법에는 `인터페이스`와 `스프링 설정 파일에서 속성을 지정`하는 방법 2가지가 있는데
  자신이 편한 방식대로 한 가지를 골라서 사용하시면 됩니다.
