---
title: "Classpath(클래스패스)"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 Classpath(클래스패스)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Java의 Classpath(클래스패스)에 대해서 배워 봅시다.

## Classpath(클래스패스)

  클래스패스란 말 그대로 클래스를 찾기위한 경로입니다. 자바에서 클래스패스의 의미도 똑같습니다.

  __즉, JVM이 프로그램을 실행할 때, 클래스파일을 찾는 데 기준이 되는 파일 경로를 말하는 것입니다.__

  소스 코드(.java로 끝나는 파일)를 컴파일하면 소스 코드가 “바이트 코드”(바이너리 형태의 .class 파일)로 변환됩니다. java runtime(java 또는 jre)으로 이 .class 파일에 포함된 명령을 실행하려면, 먼저 이 파일을 찾을 수 있어야 합니다. 이때 .class 파일을 찾을 때 classpath에 지정된 경로를 사용합니다. __classpath는 .class 파일이 포함된 디렉토리와 파일을 콜론으로 구분한 목록입니다.__ java runtime은 이 classpath에 지정된 경로를 모두 검색해서 특정 클래스에 대한 코드가 포함된 .class 파일을 찾습니다. 찾으려는 클래스 코드가 포함된 .class 파일을 찾으면 첫 번째로 찾은 파일을 사용합니다.

  classpath를 지정할 수 있는 두 가지 방법이 있습니다. 하나는 환경 변수 CLASSPATH를 사용하는 방법이고, 또 하나는 java runtime에 -classpath 플래그를 사용하는 방법입니다. (-classpath 플래그 사용에 대한 자세한 설명은 java 메뉴얼 페이지를 참조)

## 참조

  > [https://effectivesquid.tistory.com/entry/%EC%9E%90%EB%B0%94-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%8C%A8%EC%8A%A4classpath%EB%9E%80](https://effectivesquid.tistory.com/entry/%EC%9E%90%EB%B0%94-%ED%81%B4%EB%9E%98%EC%8A%A4%ED%8C%A8%EC%8A%A4classpath%EB%9E%80)
