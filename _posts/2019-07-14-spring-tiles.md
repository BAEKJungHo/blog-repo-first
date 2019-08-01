---
title: "Tiles Framework"
layout: post
category: Spring
tags: [Spring]
excerpt: "Tiles Framework에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Tiles Framework에 대해 배워 봅시다.

## Tiles Framework

  > [Tiles Framework !!](https://tiles.apache.org/)
  >
  > Tiles는 웹페이지의 상단메뉴나 좌측메뉴, 공통 파일 include 등의 반복적인 부분을 한 곳에서 깔끔하게 관리할 수 있게 도와주는 템플릿 프레임워크 입니다.

## 최소 요구 사항

  - JSTL 필요
    - STS를 사용하고 있다면 기본적으로 탑재되어 있고, 그렇지 않다면 JSTL Dependency를 Maven에 추가해줘야 함.
    - JSTL이 없으면 아래와 같은 에러 발생
    - HTTP Status 500 - Handler processing failed; nested exception is java.lang.NoClassDefFoundError: javax/servlet/jsp/jstl/core/Config

  - JDK 1.7 이상
  - Servlet 2.5 이상(2.4도 가능은 함)
  - JSP 2.1 이상(2.0도 가능은 함)
  - Spring 3.2 이상

## XML 파일로 설정

### Dependency 추가

  pom.xml에 Dependency를 추가해야 합니다.

  ```xml
  <dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-extras</artifactId>
    <version>3.0.8</version>
  </dependency>
<dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-servlet</artifactId>
    <version>3.0.8</version>
</dependency>
<dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-jsp</artifactId>
    <version>3.0.8</version>
</dependency>
  ```

### servlet-context.xml 파일 설정

  servlet-context.xml에서 Tiles를 사용하기 위해 추가해야하는 클래스는 `TilesConfigurer`와 `UrlBasedViewResolver`입니다.

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:beans="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->

    <!-- Enables the Spring MVC @Controller programming model -->
    <annotation-driven />

    <!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
    <resources mapping="/resources/**" location="/resources/" />

    <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <beans:property name="prefix" value="/WEB-INF/views/" />
        <beans:property name="suffix" value=".jsp" />
        <beans:property name="order" value="2" />
    </beans:bean>    

    <context:component-scan base-package="com.my.test" />

    <!-- Tiles -->
    <beans:bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
        <beans:property name="definitions">
            <beans:list>
                <beans:value>/WEB-INF/tiles/tiles.xml</beans:value>
            </beans:list>
        </beans:property>
    </beans:bean>        
    <beans:bean id="tilesViewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
        <beans:property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView" />
        <beans:property name="order" value="1" />
    </beans:bean>    

</beans:beans>
  ```

  위 처럼 스프링 설정파일에 여러가지의 ViewResolver가 존재하는 경우 `<beans:property name="order" value="1" />`
  다음과 같이 우선순위를 줄 수 있습니다. 우선순위를 주지않아 InternalResourceViewResolver가 먼저 실행되는 경우
  prefix와 return값 suffix로 View를 찾는데, View를 찾지못해 Null값을 반환하는경우, 다른 RESOLVER를 타지 못합니다.

  > InternalResourceViewResolver는 스프링 생성시 기본으로 설정되는 ViewResolver입니다.

  tilesViewResolver의 프로퍼티에 order를 주어, tilesViewResolver가 우선적으로 사용되도록 합니다.

  `<beans:value>/WEB-INF/tiles/tiles.xml</beans:value>`이 부분은 tiles.xml이 있는 경로를 지정해야 합니다.

### tiles.xml 생성

  - EXAMPLE 1.

  ```xml
  <!DOCTYPE tiles-definitions PUBLIC
  "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
  "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">

<tiles-definitions>

    <!-- 메뉴 표시 -->
    <definition name="base" template="/WEB-INF/tiles/template.jsp">
        <put-attribute name="left"   value="/WEB-INF/tiles/left.jsp" />
        <put-attribute name="header" value="/WEB-INF/tiles/header.jsp" />
        <put-attribute name="footer" value="/WEB-INF/tiles/footer.jsp" />
    </definition>

    <definition name="*.page" extends="base">
        <put-attribute name="body" value="/WEB-INF/views/{1}.jsp" />
    </definition>

     <definition name="*/*.page" extends="base">
         <put-attribute name="body" value="/WEB-INF/views/{1}/{2}.jsp" />
     </definition>

    <definition name="*/*/*.page" extends="base">
        <put-attribute name="body" value="/WEB-INF/views/{1}/{2}/{3}.jsp" />
    </definition>

    <!-- 메뉴 미표시 -->
    <definition name="baseEmpty" template="/WEB-INF/tiles/templateEmpty.jsp">
    </definition>

    <definition name="*.part" extends="baseEmpty">
        <put-attribute name="body" value="/WEB-INF/views/{1}.jsp" />
    </definition>

     <definition name="*/*.part" extends="baseEmpty">
         <put-attribute name="body" value="/WEB-INF/views/{1}/{2}.jsp" />
     </definition>

    <definition name="*/*/*.part" extends="baseEmpty">
        <put-attribute name="body" value="/WEB-INF/views/{1}/{2}/{3}.jsp" />
    </definition>        

</tiles-definitions>
  ```

  - EXAMPLE 2.

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
  <tiles-definitions>
    <!-- base-definition -->
    <definition name="base-definition" template="/WEB-INF/views/tiles/layouts/defaultLayout.jsp">
       <put-attribute name="title" value="" />
       <put-attribute name="header" value="/WEB-INF/views/tiles/template/defaultHeader.jsp" />
       <put-attribute name="menu" value="/WEB-INF/views/tiles/template/defaultMenu.jsp" />
       <put-attribute name="body" value="" />
       <put-attribute name="footer" value="/WEB-INF/views/tiles/template/defaultFooter.jsp" />
    </definition>
    <!-- Home Page -->
    <definition name="home" extends="base-definition">
      <put-attribute name="title" value="Welcome" />
      <put-attribute name="body" value="/WEB-INF/views/home.jsp" />
    </definition>
    <!-- BoarList Page -->
    <definition name="boardList" extends="base-definition">
      <put-attribute name="title" value="List" />
      <put-attribute name="body" value="/WEB-INF/views/boardList.jsp" />
    </definition>
    <!-- BoardView Page -->
    <definition name="boardView" extends="base-definition">
      <put-attribute name="title" value="View" />
      <put-attribute name="body" value="/WEB-INF/views/boardView.jsp" />
    </definition>
  </tiles-definitions>
  ```

  `base-definition` 부분은 페이지 레이아웃을 지정하는 곳입니다. 각각의 공통 조각들을 지정합니다.

   그 아래에는 공통이 아닌 부분을 지정하는 곳입니다. `extends="base-definition"`에서 알 수 있듯이 위의 기본 설정을 확장하여 바뀌는 부분을 지정합니다.

### 레이아웃 작성(template.jsp)

  - /WEB-INF/tiles/template.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>제목</title>
    <style>
        #header{            
            width:100%;
            height:50px;
            text-align: center;
            background-color: aqua;
        }
        #left{
            float:left;
             width:15%;
            background-color: gray;
        }
        #main{
            float:left;
             width:85%;
            background-color: lime;
        }
        #footer{
            width: 100%;
            height: 50px;            
            text-align: center;
            background-color: orange;
            clear:both;
        }
         #left, #main{
               min-height: 600px;
         }
    </style>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
</head>
<body>
    <div style="width:100%; height:100%;">
    <div id="header"><tiles:insertAttribute name="header" /></div>
    <div id="left"><tiles:insertAttribute name="left" /></div>
    <div id="main"><tiles:insertAttribute name="body" /></div>    
    <div id="footer"><tiles:insertAttribute name="footer" /></div>
    </div>

    <script type="text/javascript">
        $(function() {

        });    
    </script>    
</body>
</html>
  ```

### 레이아웃 미적용(templateEmpty.jsp)

  - /WEB-INF/tiles/templateEmpty.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>제목</title>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
</head>
<body>
    <div id="main"><tiles:insertAttribute name="body" /></div>

    <script type="text/javascript">
        $(function() {

        });    
    </script>    
</body>
</html>
  ```

### header, left, footer.jsp 작성

  - /WEB-INF/tiles/header.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<h1>Top 메뉴</h1>

<script type="text/javascript">
    $(function() {

    });    
</script>
  ```

  - /WEB-INF/tiles/left.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<h1>Left 메뉴</h1>

<script type="text/javascript">
    $(function() {

    });    
</script>
  ```

  - /WEB-INF/tiles/footer.jsp

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<h1>Footer</h1>

<script type="text/javascript">
    $(function() {

    });    
</script>
  ```

### 테스트   

  - 테스트를 위한 View 파일(/WEB-INF/views/test.jsp)

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<h1>테스트</h1>

<script type="text/javascript">
    $(function() {
        alert('body');
    });    
</script>
  ```

## 자바 파일로 타일즈 설정

  타일즈(Tiles)를 설정할 때, XML로 설정해도 되지만, MyBatis 설정파일처럼 Java로도 설정할 수 있습니다.

  ```java
  @Configuration
  public class TilesConfig {

      @Bean
      public UrlBasedViewResolver viewResolver() {
          UrlBasedViewResolver tilesViewResolver = new UrlBasedViewResolver();
          tilesViewResolver.setViewClass(TilesView.class);
          return tilesViewResolver;
      }

      @Bean
      public TilesConfigurer tilesConfigurer() {
          TilesConfigurer tiles = new TilesConfigurer();
          tiles.setDefinitions(new String[] {"/WEB-INF/config/XXX_*_Config.xml"});
          return tiles;
      }
  }
  ```

### 실무 방식

  실무에서는 위 EXAMPLE 1. 방식으로 tiles.xml 파일을 만듭니다. 즉 아래와 같은 방식입니다.

  ```xml
  <definition name="*/*.pop" extends="pop">
      <put-attribute name="body" value="/WEB-INF/jsp/core/{1}/{2}.jsp" />
  </definition>

  <definition name="*.core" extends="core">
      <put-attribute name="body" value="/WEB-INF/jsp/core/{1}.jsp" />
  </definition>
  ```

  타일즈에서 다음과 같이 설정할 때, Controller에서는 타일즈에서 설정한 `definition name`으로 View를 찾아서 접근합니다. tiles 설정파일에서는
  name 부분을 Asterisk로 설정하는데, 그 이유는 모든 이름을 받기 위해서 입니다.

  `/WEB-INF/jsp/core`가 기본경로이고 core안에 각 모듈에 대한 패키지(폴더)와 파일들이 존재하기 때문에 Asterisk 가장 앞 부분이 {1}로 바인딩되고, 그 다음부분이 {2}로 바인딩 됩니다.

  컨트롤러에서는 다음과 같이 사용하면 됩니다.


  ```java
/**
* 설문 게시글 전체 리스트에 대한 정보 가져오기
* @param searchVo
* @return 설문조사 게시글 리스트
*/
@GetMapping
public String list(@ModelAttribute("searchVo") XXXSurveyVo searchVo, Model model) {
  int count = xxxSurveyService.countSurvey(searchVo);
  XXXPagingHelper xxxPagingHelper = new xxxPagingHelper(searchVo, count);
  searchVo.initPaging(xxxSurveyServicePagingHelper);

  List<XXXSurveyVo> surveyList = xxxSurveyService.findSurveys(searchVo);
  model.addAttribute("pagingHelper", xxxPagingHelper);
  model.addAttribute("list", surveyList);

  return "survey/list.core";
}
  ```

  즉, `return "survey/list.core";` survey와 타일즈 설정파일에 있는 put-attribute Value값(name경로를 제외)이 더해집니다.
  `/WEB-INF/jsp/core/ + survey`그리고 `/list.core` 즉, 타일즈 설정파일의 name명으로 접근한 경로를 마지막에 붙여줍니다. `/WEB-INF/jsp/core/survey/list.jsp`가 최종 경로가 됩니다.

  실무에서는 자주 사용하는 URI를 상수로 빼두기도 합니다.

  ```java
  private final String VIEW_ROLE_REGIST = "role/regist.core";
  private final String REDIRECT_URL = "redirect:/XXX/roles";
  ```

## 참조

  > [https://its-easy.tistory.com/13](https://its-easy.tistory.com/13)
  >
  > [https://offbyone.tistory.com/9](https://offbyone.tistory.com/9)
