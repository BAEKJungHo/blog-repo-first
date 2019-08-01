---
title: ".jar .war .ear"
layout: post
category: Network
tags: [Network]
excerpt: ".jar .war .ear의 차이점에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > .jar .war .ear의 차이점에 대해서 배워 봅시다.

## .jar .war .ear

   - jar, war, ear은 모두 압축파일의 종류입니다.
   - 각 파일이 담고있는 규모를 따지면 `ear > war > jar > class` 순입니다.
   - 우선 jar, war, ear 모두 어플리케이션 소스들을 배포할 시에 path 등의 설정에서의 에로점을 제거하기위해 탄생한 압축방식입니다.
   - 이 압축방식 들은 압축의 해제없이 JDK에서 각 파일들을 접근하여 사용할 수 있도록 설계되어있습니다.

### jar(Java Archive)

  - jar 압축은 하나의 application 기능이 가능하도록 `java파일 등을 압축하고 지원`해줍니다. 앞서 알려드린 대로 path 등의 경로를 유지하기 때문에 배포된 jar 파일을 사용하는 사용자들은 각 파일들에 대한 path 문제에서 벗어날 수 있습니다.

  - EX) ojdbc14.jar, servlet-api.jar 등을 들 수 있습니다.

### war(Web Archive)

  - war는 jar와 달리 `웹 어플리케이션을 지원하기 위한 압축`방식입니다.
  - 웹 어플리케이션을 지원하기 위해서 war 압축방식은 `jsp, servlet, gif, html, jar` 등을 압축하고 지원해주며
  - 이는 jar와 같은 맥락으로 servlet context 접근을 위해 관련된 모든 파일들을 패키지화하여 준다는 말입니다.

### ear(Enterprise Archive)

  - 하나의 웹어플리케이션 단위를 넘어 `실제 서버에서 배포하기 위한 단위`를 말합니다.
  - 이를 위해서 jar와 war를 묶어서 각각의 기능을 지원하여 줍니다.
  - jar는 어플리케이션 레벨(business layer)
  - war는 웹어플리케이션 레벨(web layer)
  - jar와 war를 지원하도록 하는 것입니다.


## 참조

  > [https://creator1022.tistory.com/114](https://creator1022.tistory.com/114)
