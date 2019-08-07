---
title: "ServletContext"
layout: post
category: Servlet
tags: [Servlet]
excerpt: "ServletContext 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## ServletContext

  > [ServletContext Interface API](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletContext.html)

  서블릿이 서블릿 컨테이너(Servlet Container)와 통신하는 데 사용하는 메소드 세트 (예 : 파일의 MIME 유형 가져 오기, 요청 디스패치 또는 로그 파일에 쓰기)를 정의합니다.
  `Java Virtual Machine`마다 "웹 응용 프로그램"당 하나의 컨텍스트가 있습니다. "웹 애플리케이션"은 서버 URL 네임 스페이스의 특정 서브 세트 (예 : 파일을 /catalog 통해 설치 가능) 아래에 설치된 서블릿 및 컨텐츠의 모음입니다

  ServletConfig는 해당 서블릿에서만 사용할 수 있지만, `Web App 내에서 공통적인 내용을 가져다 사용하려면 ServletContext를 사용할 수 있습니다.` ServletContext는 ServletConfig와 마찬가지로 web.xml을 사용하며, 따라서 바로 사용하려면 String만 사용할 수 있습니다. 하지만, `ServletContextListener`를 이용하면 객체 역시 Web App 전역에서 사용할 수 있습니다.

### 서버 메모리에 저장된 값 꺼내 사용하기

```java
// servletContext를 이용하면 서버 메모리에 저장된 값을 꺼내 쓸 수있다.
// MANAGER_ENV는 키값을 의미합니다.
ServletContext servletContext = request.getServletContext();
List<XXXVO> xxxList =  servletContext.getAttribute("MANAGER_ENV");
```
