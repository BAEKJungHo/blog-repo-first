---
title: "@RequestBody @ResponseBody"
layout: post
category: Spring
tags: [Spring]
excerpt: "@RequestBody @ResponseBody어노테이션의 사용 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @RequestBody @ResponseBody 어노테이션의 사용 방법에 대해서 배워 봅시다.

## @ResponseBody

  메서드에 `@ResponseBody`로 어노테이션이 되어 있다면 메소드에서 리턴되는 값은 View를 통해서 출력되지 않고 `HTTP Response Body`에 직접 쓰여지게 됩니다. 이때 쓰여지기 전에 리턴되는 데이터 타입에 따라 `MessageConverter`에서 변환이 이뤄진 후 쓰여지게 됩니다.

  즉, @ResponseBody 어노테이션은 `비동기 처리`를 할 때 많이 사용됩니다.

  비동기처리를 하기 위해서는 `@ResponseBody` 어노테이션을 사용하며(@RestController 어노테이션 사용시 생략 가능), @ResponseBody 어노테이션이 View를 리턴하는게 아니라 `응답 본문(HTTP Response Body)`으로 리턴을합니다.

  > @Controller와는 다르게 `@RestController`는 리턴값에 자동으로 @ResponseBody를 붙게되어 HTTP 응답데이터(body)에 자바 객체가 매핑되어 전달 된다고 합니다.( ※ @Controller인 경우에는 @ResponseBody를 적어줘야 합니다. )

  ContentsNegotiationViewResolver -> accept-header 어떤 응답을 원하는지 분석

    - text/html
      - InternalResourceViewResolver, Tiles...

    - application/json, xml 등
      - ViewResolver가 없으므로 알맞은 `MessageConverter`를 사용
      - Jackson, JAXb2 ..
      - MessageConverter로 응답 변환해서 응답 본문으로 쏴준다.

  `ContentsNegotiationViewResolver`가 text/html 혹은 application/json 등 InternalResourceViewResolver, TilesViewResolver 등을 탈지 MessageConverter를 사용할지 정합니다.

  > ContentsNegotiationViewResolver는 스프링에서는 따로 web.xml에 설정을 해야하는데, 스프링 부트에서는 자동으로 설정을 해줍니다.

### MessageConverter의 종류

  - StringHttpMessageConverter

  - FormHttpMessageConverter

  - ByteArrayMessageConverter

  - MarshallingHttpMessageConverter

  - MappingJacksonHttpMessageConverter
    - Jackson's ObjectMappter를 사용하여 request, response를 JSON 으로 변환할때 사용되는 MessageConverter 입니다.

  - applicaton/json 을 지원합니다.

  - SourceHttpMessageConverter

  - BufferedImagedHttpMessageConverter

## @RequestBody

  @RequestBody는 @ResponseBody와는 반대로, HTTP 요청 몸체를 자바 객체로 전달받음, HTTP 요청의 Body 내용을 자바 객체로 매핑하는 역할을 합니다.

  ```java
  @RestController
  public class LoginWebController {
    // HTTP 요청의 내용을 객체에 매핑하기위해 @RequestBody 애너테이션을 설정한다.
    @RequestMapping(value="test/test", method = RequestMethod.POST)

    public testDto login(@RequestBody Test testVO) {
      Test test = test.login(testVO);
      return test;
    }
  }
  ```

  위의 예제 소스에서는 HTTP 요청의 Body안에 Test 데이터를 파라미터로 받기위해, @RequestBody를 사용하였습니다.

## @RequestBody vs @ModelAttribute

  @ModelAttribute는 생략이 가능 : formData or QueryParameter 자동 바인딩

  ```java
  @ModelAttribute FileManageDto.UpdateRequest updateRequest // formData or QueryParameter 자동바인딩
  ```

  @RequestBody는 요청 본문(JSON)을 해당 객체의 타입으로 받아야 해서, 파라미터 타입 앞에 선언해줍니다. (생략 불가능) 생략하게되면 @ModelAttribute랑 구분을 할 수가 없습니다.(요청 본문을 읽어올 때 자동바인딩)

  ```java
  @Valid @RequestBody FileManageDto.UpdateRequest updateRequest
  ```
