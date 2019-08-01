---
title: "contextPath"
layout: post
category: Spring
tags: [Spring, JSP, Servlet]
excerpt: "contextPath의 개념에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## contextPath

  > contextPath
  >
  > WAS(Web Application Server)에서 웹 어플리케이션을 구분하기 위한 path입니다.
  이클립스에서 프로젝트를 생성하면 , 자동으로 server.xml에 추가됩니다.

  즉, Spring을 사용하면  `src/main/webapp/WEB-INF/spring/appServlet/` 다음 경로 안에
  `servlet-context.xml`파일이 있습니다. 이 파일 안에서 다음 코드가 바로 `contextPath` 입니다.

  ```html
  <!-- contextPath : com.example.spring01 -->
  <context:component-scan base-package="com.example.spring01"/>
  ```

  > pageContext.request.contextPath
  >
  > pageContext.request.contextPath 방식을 사용하는 이유는 contextPath가 개발을 완료한 이후에 변경이 된다면 페이지마다 링크를 모두 바꿔야하는 불상사가 생기게 됩니다. 따라서 contextPath가 변경되어도 소스 수정없이 사용하기 위해 사용하며, 유지보수가 용이 해집니다.
