---
title: "JSTL Install"
layout: post
category: JSP
tags: [JSP]
excerpt: "Jsp의 JSTL설치 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

JSP, Servlet 공부를 하면서 참고한 서적입니다.

_프로젝트로 배우는 자바 웹 프로그래밍_

## JSTL

  JSTL(JSP Standard Tag Library)는 커스텀 태그 라이브러리 기술을 이용해서 일반적으로
  필요한 기능들을 표준화 한 것 입니다.

  - __JSTL 설치 Version 1. Original__

    > 설치 표준 사이트 : [http://jakarta.apache.org](http://jakarta.apache.org)

    - Taglibs 클릭
      - ![jstl1](/images/posts/201905/jstl1.jpg)  

    - Apache Standard Taglib 클릭
      - ![jstl2](/images/posts/201905/jstl2.jpg)  

    - 원하는 Version 다운로드
      - ![jstl3](/images/posts/201905/jstl3.jpg)  

    - binaries/ 클릭
      - jakarta-taglibs-standrad1.1.2zip(일반적으로 이 버전을 주로 사용) 설치
      - ![jstl4](/images/posts/201905/jstl4.jpg)  

  - __JSTL 설치 Version 2.__

    - [https://tomcat.apache.org/download-taglibs.cgi](https://tomcat.apache.org/download-taglibs.cgi)에 방문
    - 맨위 링크  Apache Standard Taglib 클릭
    - 원하는 버전 설치
    - binary에서 jakarta-taglibs-standrad1.1.2zip (원하는 버전) 파일 압축 풀기
    - lib 폴더에서 jstl.jar, standard.jar파일을 복사
    - Eclipse - WEB-INF\lib 폴더로 복사

  - __JSTL 라이브러리별 URI 및 prefix__

    |라이브러리  |URI|prefix|
    |------------|---|------|
    |핵심|  http://java.sun.com/jsp/jstl/core |c|
    |XML|  http://java.sun.com/jsp/jstl/xml |x|
    |I18N|  http://java.sun.com/jsp/jstl/fmt |fmt|
    |데이터베이스|  http://java.sun.com/jsp/jstl/sql |sql|
    |함수|  http://java.sun.com/jsp/jstl/functions |fn|

  핵심 라이브러리를 사용하는 방법은 다음과 같습니다.

  > < %@ taglib prefix="c" uri=http://java.sun.com/jsp/jstl/core" % >
