---
title: "MIME와 HTTP Multipart"
layout: post
category: Spring
tags: [Spring]
excerpt: "MIME와 HTTP Multipart에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MIME와 HTTP Multipart에 대해서 배워 봅시다.

## Multipart

  Multipart는 HTTP를 통해 File을 SERVER로 전송하기 위해 사용되는 `Content-Type` 입니다.

  HTTP(request와 response 둘 다)는 간단하게 4가지로 나눌 수 있습니다.

  1. Request Line
  2. HTTP Header
  3. Empty Line
  4. Message Body

  여기서 Message Body에 들어가는 데이터 타입을 HTTP Header에게 명시해 줄 수 있는데, 이 역할을 하는
  필드가 `Content-Type` 그리고 이 Content-Type 필드에 `MIME` 타입을 기술해 줄 수 있는데, 여러가지 MIME 타입 중 하나가
  바로 `MultiPart` 입니다.

## Multipart 사용방법

  스프링에서 Multipart를 사용하기 위해서 아래와 같이 하면 됩니다. MultiPart는 파일을 서버로 전송하기 위해 사용되므로
  파일 업로드, 다운로드를 구현하는데에 있어서 필수 입니다.

### Dependency 설정

  ```xml
<!-- 파일 업로드 -->
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
     <groupId>commons-fileupload</groupId>
     <artifactId>commons-fileupload</artifactId>
     <version>1.3.3</version>
</dependency>
 <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.4</version>
</dependency>
  ```

  commons-fileupload를 의존성 설정할 때에 버전을 1.3.3이상으로 하는게 좋습니다.
  1.3.3미만은 GitHub에 소스를 올릴때 Vulnerable(취약)하다고 나옵니다.

### servlet-context.xml

  스프링 설정 파일(IOC 컨테이너)에도 파일 업로드, 다운로드 구현을 위해 `MultiPartResolver`와 업로드 파일 경로가 담긴 `uploadPath`라는 이름으로 빈을 하나 등록해줘야 합니다.

  > Multipart 지원 기능을 사용하려면 먼저 MultipartResolver를 스프링 설정 파일에 등록. 스프링에서 기본으로 제공하는 MultipartResolver는 `CommonsMultipartResolver`입니다. CommonsMultipartResolver를 MultipartResolver로 사용하려면 다음과 같이 빈 이름으로 `multipartResolver`를 사용해서 등록하면 됩니다.

  ```xml
  <!-- 파일업로드에 필요한 Bean -->
  <beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 파일 업로드 용량 -->
      <beans:property name="maxUploadSize" value="10485760" />
  </beans:bean>

  <beans:bean id="uploadPath" class="java.lang.String">
    <beans:constructor-arg value="서버에 업로드된 파일을 저장할 경로"></beans:constructor-arg>
  </beans:bean>
  ```

### CommonsMultipartResolver 클래스의 프로퍼티

  ![m1](/images/posts/201907/m1.jpg)

### MultipartFile 인터페이스

  ![m3](/images/posts/201907/m3.jpg)

  MultipartFile 인터페이스는 업로드한 파일 정보 및 파일 데이터를 표현하기 위한 용도로 사용하며 스프링에서
  `단일 업로드`를 구현할 때 사용합니다.

### MultipartHttpServletRequest​ 인터페이스

  ![m2](/images/posts/201907/m2.jpg)

  MultipartHttpServletRequest​ 인터페이스는 __Multipart 요청이 들어올때__ 내부적으로 원본 HttptServletRequest 대신 사용되는 인터페이스 입니다.
  MultipartHttpServletRequest 인터페이스는 HttpServletRequest 인터페이스와 MultipartRequest인터페이스를 상속받고있습니다.

  즉 웹 요청 정보를 구하기 위한 getParameter()와 같은 메서드와 Multipart관련 메서드를 모두 사용가능합니다.

  MultipartHttpServletRequest를 이용하면 `다중 업로드`를 구현할 수 있습니다.

### 업로드 구현(View 설정)

  MultipartFile이나 MultipartHttpServletRequest를 이용하여 업로드를 구현할 때에, View에서는 파일 전송을 위해 form에서 다음과 같이 형식을 지정해줘야 합니다.

  ```html
  <!-- form 커스텀 태그 사용 -->
  <form:form commandName="boardDTO" mehtod="post" enctype="multipart/form-data">
  ```

  즉, `enctype="multipart/form-data"`를 작성해야 File 업로드를 구현할 수 있습니다.
  그리고 Input 태그는 아래와 같이 설정해주면 됩니다.

  ```html
  <!-- 단일 업로드 방식 -->
  <td><input type="file" name="file" /></td>

  <!-- 다중 업로드 방식 : multiple 속성 추가-->
  <input multiple="multiple" type="file" name="file" />
  ```

## MIME

  > MIME : [Multipurpose Internet Mail Extensions](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types) (Email을 위한 인터넷 표준 포맷)

  MIME 타입이란 클라이언트에게 전송된 문서의 다양성을 알려주기 위한 메커니즘입니다: 웹에서 파일의 확장자는 별  의미가 없습니다. 그러므로, 각 문서와 함께 올바른 MIME 타입을 전송하도록, 서버가 정확히 설정하는 것이 중요합니다. 브라우저들은 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지를 결정하기 위해 대게 MIME 타입을 사용합니다.

### 유래

  - 기본적으로 E-mail(전자우편) 전송 프로토콜인 SMTP는 7비트 ASCII 문자만을 지원합니다. 그러나 ASCII 문자는
  영어밖에 표현할 수 없기에 영어 이외의 언어로 쓰인 E-mail은 제대로 전송 될 수 없었습니다. 이를 위해 탄생한 것이
  MIME 입니다.

  - MIME는 ASCII가 아닌 문자 인코딩을 이용해 영어가 아닌 다른 언어로 된 E-mail을 보낼 수 있는 방식을 정의 합니다.
  또한 그림, 음악, 영화 같은 8비트 짜리 이진 파일을 전자 우편으로 보낼 수 있도록 합니다. __즉, E-mail을 위한 인터넷 표준 포맷 입니다.__

  - MIME는 본래 E-mail을 위해 정의된 것이지만 HTTP, SIP와 같은 인터넷 프로토콜에서 함께 사용하고 있습니다.

  - MIME는 메세지 종류는 나타내는 __Content-Type__ , 메세지 인코딩 방식을 나타내는 content-transfer-encoding과 같은 추가적인 E-mail 헤더를 정의합니다.

  - 메세지를 MIME 형식으로 변환하는 것은 E-mail 프로그램이나 서버상에서 자동으로 이루어 집니다.

  - MIME는 확장 가능합니다. MIME 표준은 새로운 Content-Type과 또 다른 MIME 속성 값을 등록할 수 있는 방법을 정의하고 있습니다.

### MIME의 구조

  - MIME 헤더
    - MIME Version
      - MIME-Version: 1.0
      - 해당 메세지가 MIME 형식임을 나타냅니다.

    - Content-Type
      - Content-Type:text/plain
      - HTTP Message Body에 들어가는 데이터 타입과 서브 타입을 나타냅니다.
      - Multipart 타입을 통해 MIME는 트리 구조의 메세지 형식을 정의할 수 있습니다.
        - 어떤 것이 첨부된 텍스트(multipart/mixed)
        - 텍스트와 HTML과 같이 다른 포맷을 함께 보낸 메세지(multipart/alternative) 등

    - Encoded-Word
      - RFC 2822 정의에 따르면, 메세지 헤더와 그 값은 항상 ASCII 문자를 사용해야 합니다. ASCII가 아닌 헤더 값은
      encoded-word 문법에 따라 인코딩 해야 합니다.

    - Mulipart 메세지
      - 서로 붙어있는 여러개의 메세지를 포함하여 하나의 복합 메세지로 보내집니다.
      - MIME multipart 메세지는 `Content-Type` 헤더에 `boundary` 파라미터를 포함합니다.
        - boundary는 메세지 파트를 구분하는 역할을 하며, 메세지의 시작과 끝 부분에도 나타납니다.
        - 첫 번째 boundary 전에 나오는 내용은 MIME를 지원하지 않는 클라이언트를 위해 제공됩니다.
        - boundary를 선택하는 것은 클라이언트의 몫입니다. 보통 무작위 문자를 선택해 메세지의 본문과의 충돌을 피합니다.

      - 멀티파트 폼 제출
        - HTTP form을 채워서 제출하면, 가변 길이 텍스트 필드와 업로드 될 객체는 각각 멀티파트 본문을 구성하는 하나의 파트가 되어 보내집니다.
        멀티파트 본문은 여러 다른 종류와 길이 값으로 채워진 form을 허용합니다.
        - multipart/form-data : 사용자가 양식을 작성한 결과 값의 집합을 번들로 만드는데 사용됩니다.

## 참조

  > [https://qssdev.tistory.com/47](https://qssdev.tistory.com/47)
  >
  > [https://lng1982.tistory.com/209](https://lng1982.tistory.com/209)
