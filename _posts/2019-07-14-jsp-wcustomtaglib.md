---
title: "커스텀 태그 라이브러리"
layout: post
category: JSP
tags: [JSP]
excerpt: "커스텀 태그 라이브러리(Custom Tag Library)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 커스텀 태그 라이브러리(Custom Tag Library)에 대해서 배워 봅시다.

## Custom Tag Library

  > Custom Tag !!
  >
  > 커스텀 태그는 사용자가 필요한 경우에 정의해서 사용하는 태그를 말합니다. 우리는 앞에서 useBean, param, setProperty 등의 태그를 JSP 페이지에서 사용해 왔습니다. 그리고 이렇게 미리 정의되어있는 태그를 액션 태그라고 불렀습니다. 커스텀 태그는 액션 태그의 한계성을 극복하여 사용자가 특성에 맞게 스스로 태그를 설계하는 것을 말하며, 이렇게 필요한 태그의 특성, 태그 해석의 방식, 그리고 태그들을 한군데다 모아서 라이브러리로 묶어 필요한 곳에 사용하게 됩니다. 이렇게 만들어진 것을 Tag Library라고 합니다.

  커스텀 태그는 빈과 유사한 성격을 가집니다. __즉, 재사용이 가능하고, 사용자는 태그 라이브러리를 컴포넌트처럼 필요한 곳에 선택적으로 사용할 수 있습니다.__ 하지만, 작성자와 사용자의 입장 그리고 그 내용면에서 몇 가지 차이점을 보입니다. 빈과 커스텀 태그의 특징을 비교해서 정리해 보았습니다.

  - Java Bean
    - JSP 페이지의 내용을 조작할 수 없음
    - 커스텀 태그에 비해 상대적으로 복잡한 처리과정
    - 하나의 서블릿 클래스에 정의되어 다른 서블릿이나 JSP에 의해 사용됨
    - JSP 1.0 스펙 이상에서 사용가능

  - Custom Tag
    - JSP 페이지의 내용을 조작 가능
    - 상대적으로 빈보다 다루기 쉬움
    - 많은 자체 작업을 정의할 수 있음
    - JSP1.1 스펙 이상에서 사용가능

이렇게 커스텀 태그는 자바 빈과 다른 면을 가지며, JSP1.1 스펙 이상을 구현할 수 있다면 어떤 서블릿 엔진에서도 동작합니다. 프로그래머가 태그 라이브러리를 잘 만들어두면, 사용자는 내부의 원리를 몰라도 그냥 가져다 태그를 사용할 수 있습니다. 즉, 프로그램을 하나도 모르는 디자이너도 태그 라이브러리를 사용해서 동적인 페이지를 만들 수 있는 것입니다.

### JSP에서 Beans 접근 방법

  - 스크립트릿 등 표현식 사용
    - 사용할 빈즈를 request에서 꺼내어 사용해야 함, 가독성 및 코드량(비지니스로직)이 증가

  - EL을 사용하여 접근하는 방법
    - Scope를 지정하면 해당하는 빈즈에 접근이 가능함
    - 비지니스 로직이 없고 가독성이 뛰어남

  - usebean 선언을 통해서 사용
    - 선언하는 방법은 비교적 간단하나, 사용해야할 usebean이 10개면 10번을 선언해 주어야 함
    - 사용할 경우에는 스크립트릿이나 표현식으로 사용하며 역시 가독성 및 코드량이 증가함

### 구성

  커스텀 태그 라이브러리를 작성하고 JSP 페이지에서 태그를 사용하기 위해서는 태그 핸들러 클래스, 태그 라이브러리 서술파일(Tag Library Descriptor) 그리고 태그를 사용하는 JSP 파일이 필요합니다. 먼저 그 구성 및 태그 구현의 원리를 그림으로 나타내면 다음과 같습니다.

  ![s5](/images/posts/201907/s5.jpg)

  - Tag Handler Class
    - 태그의 의미와 행동을 정의하는 클래스로, JSP 페이지에서 커스텀 태그를 사용할 때 어떤 일을 할 것인지 정의하고, 처리하게 됩니다.

  - Tag Library Descriptor TLD
    - TLD는 태그 라이브러리를 설명해주는 파일로, XML의 표준형식을 따르는 XML 문서로서, XML Element 이름과 태그의 구현을 연결시키는 역할을 합니다. 즉, JSP에서 태그가 호출되었을 때 그 태그를 처리하는 태그 핸들러로 연결시켜 주어 관련된 처리를 실행하게 해 줍니다.

### 과정

  1. 자바 클래스 파일 작성(ex) CusomTagLibrary.java) : 메서드는 `static` 으로 구현되어 있어야 함
  2. TLD(Tag Library Descriptor) 파일을 작성(XXX.tld) : WEB-INF하위에 등록하면 됨
  3. web.xml 설정 : `<jsp-config>` 속성 추가
  4. 자바 메소드를 호출하는 JSP 파일 작성 : Java 코드에서는 패키지 import후 `클래스명.메소드명()` 형태로 호출, EL에서는 접두어를 사용하여 `\${접두어:메소드명()}` 형태로 호출

### 공용 Util 패키지의 자바 클래스 파일 작성

  ```java
package test.framework.util;

public class CommonUtil {
    public static String getTest(String txt){
        return txt + "님";
    }
}
  ```

### 커스텀 태그 라이브러리 생성(.tld)

  .tld 파일은 XML형태이므로 파일을 생성할 때에 XML로 생성하면 됩니다.

  - Create XML file from a DTD file 클릭
  - Select XML Catalog entry를 체크
  - DTD JSP Tag Library 1.2//EN을 클릭
  - Next - Finish
  - 그리고 파일 확장자명을 .tld로 변환

  ```xml
<?xml version="1.0" encoding="UTF-8" ?>
<taglib xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
    version="2.0">

    <!-- <description> : 태그 라이브러리 파일 설명
    <display-name> : 태그라이브러리 파일 이름
    <uri> : JSP에서 선언될 URI
    <function> : 함수 시작
    <description> : 함수 설명
    <name> : 함수 이름
    <function-class> : 사용할 함수의 클래스
    <function-signature> : 함수 선언
    <function-signature>리턴타입 함수명(파라미터)</function-signature>
    <example> : 사용 방법 설명
    ${cutil:getTest(str)} : JSP에서 사용하는 방법
    -->
    <description>CommonUtil functions library</description>
    <display-name>CommonUtil functions</display-name>
    <tlib-version>1.1</tlib-version>
    <short-name>cutil</short-name>
    <uri>tld/CommonUtil.tld</uri>

    <function>
        <description>문자열에 님 붙이기</description>
        <name>getTest</name>
        <function-class>test.framework.util.CommonUtil</function-class>
        <function-signature>java.lang.String getTest(java.lang.String)</function-signature>
        <example>
        ${cutil:getTest(str)}
        </example>
    </function>
</taglib>
  ```

  계속 다른 함수를 추가하려면 `<function> ... </function>` 을 추가하면 됩니다.

### web.xml 설정

  ```xml
  <jsp-config>
    <taglib>
        <taglib-uri>
            /WEB-INF/tlds/el-functions.tld
        </taglib-uri>
        <taglib-location>
            /WEB-INF/tlds/el-functions.tld
        </taglib-location>    
    </tablib>
  </jsp-config>
  ```

  혹시 태그라이브러리를 인식하지 못하거나 파일의 경로를 바꾸고 싶다면 web.xml에 선언해주면 됩니다.

  taglib-uri와 taglib-location에 `/WEB-INF/tlds/파일명`을 적어주면 됩니다.

### JSP 파일에서 사용

  ```html
  <%@ taglib prefix="cutil" uri="tld/CommonUtil.tld"%>
  ${cutil:getTest('홍길동')}
  ```
  JSP 최 상단 선언부에 태그라이브러리를 사용하겠다고 선언하고, `prefix(접두어)`를 설정한 다음
  tld파일에서 지정한 방식으로 함수를 호출하여 사용하면 됩니다.

## 참조

  > [http://www.libqa.com/wiki/148](http://www.libqa.com/wiki/148)
  >
  > [https://cofs.tistory.com/259](https://cofs.tistory.com/259)
