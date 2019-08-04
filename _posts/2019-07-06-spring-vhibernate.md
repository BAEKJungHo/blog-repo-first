---
title: "Bean Validation"
layout: post
category: Spring
tags: [Spring]
excerpt: "유효성 검증 방법인 Hibernate-Validator와 BindingResut에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 유효성 검증 방법인 Hibernate-Validator와 BindingResut에 대해서 배워 봅시다.

## Bean Validation

  Bean Validation은 JavaBean 유효성 검증을 위한 메타데이터 모델과 API에 대한 정의이며 여기서 언급하고 있는 JavaBean은 직렬화 가능하고 매개변수가 없는 생성자를 가지며, Getter 와 Setter Method를 사용하여 프로퍼티에 접근이 가능한 객체라합니다.

  > Wikipedia
  >
  > Bean Validation defines a metadata model and API for JavaBean validation — https://en.wikipedia.org/wiki/Bean_Validation JavaBeans are classes that encapsulate many objects into a single object (the bean). They are serializable, have a zero-argument constructor, and allow access to properties using getter and setter methods. The name “Bean” was given to encompass this standard, which aims to create reusable software components for Java. — https://en.wikipedia.org/wiki/JavaBeans

  데이터 검증은 애플리케이션의 여러 계층(Presentation Layer, Business Layer, Data Access Layer)에 전반에 걸쳐 발생하는 흔한 작업이다. 종종 동일한 데이터 검증 로직이 각 계층에 구현되는데, 이는 오류를 일이 키기 쉽고, 시간을 낭비하는 일이 된다.

  ![x1](/images/posts/201906/x1.jpg)

  개발자는 이런 것을 피하기 위해 도메인 클래스들 사이에 퍼져있는 유효성 검증 로직(실제로 클래스 자신의 메타데이터)을
  도메인 모델로 묶는다.

  ![x5](/images/posts/201906/x2.jpg)

  즉, 위의 내용을 종합하면 다음과 같다.

  - 애플리케이션을 개발할 때 데이터를 각 계층으로 전달하게 된다. 이러한 데이터는 JavaBean의 형태를 띠게 된다.
  - 데이터 유효성 검증을 위해 사용하게 되는 것이 데이터에 대한 데이터 즉 어떤 목적을 가지고 만들어진 데이터(Constructed data with a purpose) 바로 메타데이터이다.
  - Java에서는 메타데이터를 표현할 수 있는 대표적인 방법이 어노테이션Annotation이다.
  - 어노테이션(기본적으로는)을 이용하여 메타데이터를 정의하고 이를 통해 JavaBean의 유효성을 검증하는 것에 대한 명세가 Bean Validation이다.

  Bean Validation은 명세일 뿐 동작하는 코드가 아니다. 애플리케이션에 적용하기 위해서는 이를 구현한 코드가 필요하다.

  Bean Validation을 실제 동작하도록 구현Implementation한 것이 바로 Hibernate Validator이다.

## Hibernate-Validator

  Bean Validation 버전에 따른 Hibernate Validator 버전은 아래와 같습니다.

  ![x6](/images/posts/201906/x3.jpg)

  Hibernate-Validator는 자바에서 지원하는 `유효성 검증(Validation)기능` : JSR-303 자바 규약을 뜻합니다.
  이 것을 사용하기 위해서는 Maven에 Dependency를 추가해야 합니다.

  ```xml
  <!-- Spring 4.3.3 Release, JDK 1.8 기준 -->
  <!-- Hibernate-Validator -->
  <dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>4.3.1.Final</version>
  </dependency>
  ```

  Hibernate-Validator의 특징은 Null값 체크및 Validator에 있는 각종 어노테이션으로 지정한 형식에 따른 유효성 검증을
  할 수 있습니다.

  Hibernate-Validator를 사용할 때에는 매개변수타입 앞에 `@Valid` 어노테이션을 붙여야합니다.

  - 어노테이션

  ![x4](/images/posts/201906/x4.jpg)

  Hibernate-Validator를 사용할 때에는 JSP 파일에서 주로 `form:form 태그`를 사용합니다.

### BindingResult와 같이 사용하기(어노테이션에 의한 검증)

  Hibernate-Validator만 단독으로 사용하기에는 오류를 잡아주는 try-catch역할을 하는 것이 필요합니다.

  Validation Check의 Error를 받도록 두는 것과, 폼 값을 커맨트객체에 보관하고 에러코드를 전달받는 역할을 합니다.

  또한 BindingResut를 사용하면 예를들어 숫자값이 들어와야 하는 곳에 문자열이 들어왔다던지 이런 잘못된 입력에 대한
  에러 체크가 가능합니다.

  > [OKKY](https://okky.kr/article/341115)
  >
  > BindingResult 의 경우 ModelAttribute 을 이용해 매개변수를 Bean 에 binding 할 때 발생한 오류 정보를 받기 위해 선언해야 하는 애노테이션입니다. RequestParam 도 그렇지만, 매개변수 binding 이 실패하면 400 오류가 발생합니다. 이게 500.1 같은 오류가 발생해야 맞을 것 같지만, Spring 입장에선 요청 자체가 잘못된 것이 뭔가 설계가 잘못 되어서 혹은 잘못된 경로를 호출해서 발생한 걸로 판단하기 때문에 400 으로 오류를 발생시키는 것입니다. Controller method 을 실행할 수 없는 것이니까요. 하지만, 프로그램도 잘못된 요청에 대한 처리를 해야할 수 있기 때문에 BindingResult 을 통해 값을 전달받아 이후 처리를 할 수 있도록, 즉 흐름을 진행할 수 있도록 설계된 것입니다.

  - DTO에 어노테이션 적용

  ```java
  public class BoardDTO {

	private int num; // 게시물 번호
	@Length(min=2, max=20, message="제목은 2자 이상, 20자 이하 입력하세요.")
	private String title; // 제목
	private int count; // 조회수
	private String name; // 작성자
	private String date; // 날짜
	@NotEmpty(message="내용을 입력하세요.")
	private String contents; // 내용
	private String id;

  .....
  }
  ```

  위 처럼 위배된 Bean 유효성 검증 제약 조건에 대한 오류 메시지를 생성하는 것을 `메세지 보간법(Message Interpolation)`이라고 합니다.

  - Hibernate-Validator와 BindingResut를 같이 사용한 코드

  ```java
// 수정 검증 : BindingResut + Validator
@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.POST)
public String boardEdit(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
  if(bindingResult.hasErrors()) return "boardEdit";
  else {
    boardService.edit(boardDTO);
    return "redirect:/boardList";
  }
}
  ```

  bindingResult의 `hasErrors()` 메소드를 통해 에러 체크를 합니다.

### Errors와 같이 사용하기

  ```java
  @RequestMapping(value = "/boardInsert.do", method = RequestMethod.POST)
  public String boardInsert(@Valid BoardVO boardVO, Errors errors, Model model) throws Exception {
    if(errors.hasErrors()) {
        return "boardRegisterForm";
    }
    return "redirect:/boardRegisterForm.do";
  }
  ```

  - 사용자가 입력한 값을 boardVO 에 자동으로 맵핑됩니다. 여기에 @Valid 아노테이션을 지정하면 유효성 검사가 이루어 집니다.
  - Errors errors 는 검증한 결과를 가지고 있습니다. BindingResult 객체를 사용해도 됩니다.
  - errors.hasErrors() 가 참이면 검증에서 오류가 발생한 것입니다.

  BindingResult를 사용할 때 자바 Hibernate-Validator에서 지원하는 어노테이션을 적용하는 방법 외에 `Validator 클래스`를 상속받아서 `검증 로직(Validation Logic)`을 구현하는 방법도 있습니다.

  아래 `프로젝트 개발시, 정규식을 검증하는 방법`에서 다루도록 하겠습니다.

## 프로젝트 개발시, 정규식을 검증하는 방법

  프로젝트 개발시, JSP에서 input으로 `이메일, 전화번호, 나이` 등을 입력받고 form으로 전송해서 컨트롤러에서 처리해줘야하는 로직이 분명이 있을 수 있습니다.
  이때, 사용자가 잘못된 값을 입력하면 안되므로, `검증 로직(Validation Logic)`을 추가해야 합니다.

  이메일, 전화번호, 나이같은 경우는 정규식을 통해서 로직을 구현할 수 있으며, 항상 프로젝트에서 어떠한 `필수 값`에 대해서 검증을 해야할 경우
  `클라이언트와 서버`를 항상 같이 생각해야합니다.

  여기서 말하는 클라이언트는 JS(Javascript)에서 구현하는 로직을 말하며, 서버는 Java에서 구현하는 로직을 말합니다. 스크립트 만으로 막을 수 있다고 생각할 수 있겠지만,
  오류는 어디서든 터질 수 있으며, 사용자가 URL로 웹 페이지에 접근하게 되면, JS만으로는 막을 수 없습니다.

  __따라서 항상, 클라이언트와 서버 양쪽에 검증로직을 추가하는 습관을 들이는게 좋습니다.__  

  불필요하다고 느끼실 수도 있지만, 초보 개발자의 경우에, 정확한 판단을 내리기 어렵다면, 오류나는것보다 로직하나 추가해서 오류발생가능성을 막는게 훨씬 좋다고 생각합니다.

  아래에서 사용된 예제는, `이메일, 전화번호, 나이`를 입력받고, 검증로직을 세우는 과정입니다.

### 클라이언트(JS)에서 정규식 검증

  자바스크립트에서 함수를 만들어서 사용하기 위해서는, JSP에서 input이나 form태그(자바스크립트에서 사용될)에 `id 속성`을 붙여줘야합니다.
  자바스크립트에서는 `#`을 이용해서 접근하기 때문입니다. (EX)  #email, #phone)

  - JSP

```html
  <body>
    <form action="${pageContext.request.contextPath}/survey/usrins" id="surveyVo" name="surveyVo" method="post">
      <a href="#" onclick="survey(); return false;" class="btn ty_2">등록</a>

      <input type="text" name="email" id="email" maxlength="100" />
      <input type="text" name="phone" id="phone" maxlength="20" />
    </form>
  </body>
```

  이메일과 전화번호를 입력받고 등록 버튼을 누르면 `survey()`라는 자바스크립트 함수를 찾아갑니다.

  - Javascript

```javascript
  // 상수 선언
  const SURVEY_BASE_URL = `${CONTEXT_PATH}/survey`;

  function survey() {
    if ($("input[name='name']").val() == '') {
      alert('이름을 입력해주세요.');
      $("input[name='name']").focus();
      return false;
  }

  // 이메일 정규식
  var emailVal = $("#email").val();
  if (!verifyEmail(emailVal)) {
      alert('올바른 이메일 형식이 아닙니다.');
      $("#email").focus();
      return false;
  }

  // 핸드폰 정규식
  var phoneVal = $("#phone").val();
  if(!verifyPhone(phoneVal)) {
      alert('올바른 핸드폰 방식이 아닙니다.');
      $("#phone").focus();
      return false;
      }
  }

  // ${SURVEY_BASE_URL}/usrins : Controller의 ReqeustMapping
  $("#surveyVo").attr("action",`${SURVEY_BASE_URL}/usrins`);
  // POST 방식을 사용
  $("#surveyVo").method = METHOD.POST;
  // surveryVo라는 id값을 가진 Form을 제출
  $("#surveyVo").submit();
```

### 서버(Java)에서 Validator를 상속 받아서 검증

  JAVA에서 검증을 하기 위해서 `Hibernate-Validation`을 사용하는데, 이때 같이 사용하는것이 `BindingResult` 객체입니다.

  BindingResult 객체는 `도메인 클래스(VO, DTO)`의 속성(필드)에 적용된 `@Pattern, @Email`과 같은 어노테이션이 적용되어있을 경우 아래와 같은 방식으로, `어노테이션에 의한 검증` 후, 에러가 담기면, `hasErrors()` 메서드로 에러를 잡아줍니다.

  - Domain Class(VO)

```java
  public class Member {
    @Email
    private String email;

    .....
  }
```

  - Controller

```java
  @PostMapping("/userInsert")
  public String usrcreate(@Valid @ModelAttribute("memberVo") SurveyMemberVo memberVo, BindingResult result) {
    if (result.hasErrors()) {
      return "survey";
    }
  }
```

  어노테이션에 의한 검증 방식은 위에서 이미 배웠습니다. 이번에는 `Validator` 클래스를 상속받는 클래스를 만들어서 `supports()` 메서드와 `validate()` 메서드를 오버라이딩 하여, 검증을 할 수도 있습니다.

  Validator Interface는 아래와 같이 구성되어있습니다.

```java
  package org.springframework.validation;

  public interface Validator {
    // 주어진 객체(clazz)에 대해 Validator가 지원 가능한가?
    boolean supports(Class<?> var1);

    // 주어진 객체(target)에 대해서 유효성 체크를 수행하고,
    // 유효성 에러 발생시 주어진 Errors객체에 관련 정보가 저장된다.
    void validate(Object target, Errors errors);
  }
```

  위 Vaildator 인터페이스를 상속받아서 `이메일, 전화번호, 나이`에 대한 정규식을 상수로 만든 다음, 메서드를 오버라이딩 하여 `검증 로직(Validation Logic)`을 구현 해보겠습니다.

```java
public class SurveyMemberValidator implements Validator {

  private final SurveyPrivacyService surveyPrivacyService;

  /**
   * 생성자가 오직한개이므로 빈 주입 가능
   * @param surveyPrivacyService
   */
  public SurveyMemberValidator(SurveyPrivacyService surveyPrivacyService) {
      this.surveyPrivacyService = surveyPrivacyService;
  }

  private final String EMAIL_REG_EX = "/^[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]{2,3}$/i";
  private final String PHONE_REG_EX = "/^\\d{3}-\\d{3,4}-\\d{4}$/";
  private final String AGE_REG_EX = "/^[0-9]*$/";

  /**
    * 주어진 객체(clazz)에 대해 Validator가 지원 가능한가?
    * @param aClass
    * @return
    */
  @Override
  public boolean supports(Class<?> aClass) {
      return SurveyMemberVo.class.isAssignableFrom(aClass);
  }

  /**
    * 주어진 객체(target)에 대해서 유효성 체크를 수행하고,
    * 유효성 에러 발생시 주어진 Errors 객체에 관련 정보가 저장된다.
    * @param o
    * @param errors
    */
  @Override
  public void validate(Object o, Errors errors) {
      SurveyMemberVo vo = (SurveyMemberVo) o;
      SurveyPrivacyVo privacyVo = surveyPrivacyService.findSurveyPrivacyBySurSeq(vo.getSurSeq());

      if((!(Pattern.matches(EMAIL_REG_EX, vo.getEmail()))) && privacyVo.getEmail().equals("1")) {
          errors.rejectValue("email", "notValid",  Message.DATA_ACCESS_ERROR.getMsg());
      }
      if((!(Pattern.matches(PHONE_REG_EX, vo.getPhone()))) && privacyVo.getPhone().equals("1")) {
          errors.rejectValue("phone", "notValid",  Message.DATA_ACCESS_ERROR.getMsg());
      }
      if((!(Pattern.matches(AGE_REG_EX, vo.getAge()))) && privacyVo.getAge().equals("1")) {
          errors.rejectValue("age", "notValid",  Message.DATA_ACCESS_ERROR.getMsg());
      }

    }

}
```

  그리고 이 Validator를 상속받은 클래스(SurveyMemberValidator)를 사용하기 위해서는 [WebDataBinder](https://baekjungho.github.io/spring-propertyEditor/)를 사용하여 `커스텀 프로퍼티 에디터(Custom Property Editor)`를 등록하는거와 비슷하게 addValidators를 사용하여 `Custom Validator Object`를 넣어주면 됩니다.

  > Custom Validator Object : Validator Interface를 상속받아서 구현한 클래스

  `@InitBinder`의 특성상 @ModelAttribute 메서드보다 먼저 호출되기 때문에 @InitBinder 메서드가 먼저 호출되어 커스텀 Validator를 실행하게 됩니다.

  하지만, `@Valid`와 `@ModelAttribute("키값")`이 파라미터에 적용된 메서드가 있으면, 그 메서드를 타고 키값에 담긴 객체를 받아와서 `@InitBinder`를 실행하게 됩니다.

	@InitBinder("memberVo")의 괄호안의 문자열은, @Valid 어노테이션을 지정한 메서드의 @ModelAttribute의 키값으로 지정하면 됩니다.

  - @Valid 어노테이션을 지정한 Controller Handler Method

```java
  @PostMapping("/userInsert")
  public String usrcreate(@Valid @ModelAttribute("memberVo")SurveyMemberVo memberVo, BindingResult result) {
    if (result.hasErrors()) {
      return "survey";
    }
  }
```

  - @InitBinder

```java
  @InitBinder("memberVo")
  public void SurvecyMemberValidator (WebDataBinder webDataBinder) {
    webDataBinder.addValidators(new MurveyMemberValidator(privacyService));
  }
```

  `new MurveyMemberValidator(privacyService)`에 privacyService 참조변수를 넣은 이유는 위 Custom Validator Object(SurveyMemberValidator)에서 생성자를 통한 빈 주입을 위해,
  생성자를 생성했기 때문입니다.

  WebDataBinder에 커스텀 Validator를 지정하기위해 `addValidators()`메서드를 사용했으며, 메서드의 파라미터로 `Custom Validator Object`를 넣었습니다.

  따라서, 클라이언트와, 서버 검증로직이 수행되는 과정은 다음과 같습니다.

  1. 웹 홈페이지에서 사용자가 `이메일, 전화번호, 나이`를 입력 후 등록 클릭
  2. 등록버튼 클릭시, onclick 이벤트가 실행되면서, id속성에 지정한 이름의 `Javascript 함수`를 찾아감
  3. 자바스크립트에서 검증로직을 수행한 후에, 지정한 매핑 URL로 `컨트롤러`를 찾아가서 실행
  4. 컨트롤러에서 `@Valid와 @ModelAttribute("키값")`을 찾고, @ModelAttribute에 담긴 키값이름으로 `@InitBinder`가 지정된 메서드를 찾아가서 객체를 넘김
  5. @InitBinder 메서드를 통해 `Custom Validator Object`를 실행
  6. supports 메서드를 통해서 Validator가 지원 가능한지 체크
  7. validate 메서드를 통해서 주어진 객체(target)에 대해서 유효성 체크를 수행하고, 유효성 에러 발생시 주어진 Errors 객체에 관련 정보가 저장됨
  8. Erros 객체에 저장된 정보를 컨트롤러에서 `BindingResult의 hasErrors()`메서드를 통해 에러 체크

## 참조

  > Validation It is common to validate a model after binding user input to it. Spring 3 provides support for declarative validation with JSR-303. This support is enabled automatically if a JSR-303 provider, such as Hibernate Validator, is present on your classpath. When enabled, you can trigger validation simply by annotating a Controller method parameter with the @Valid annotation
  >
  > [https://offbyone.tistory.com/281](https://offbyone.tistory.com/281)
  >
  > [https://medium.com/@SlackBeck/javabean-validation%EA%B3%BC-hibernate-validator-%EA%B7%B8%EB%A6%AC%EA%B3%A0-spring-boot-3f31aee610f5](https://medium.com/@SlackBeck/javabean-validation%EA%B3%BC-hibernate-validator-%EA%B7%B8%EB%A6%AC%EA%B3%A0-spring-boot-3f31aee610f5)
  >
  > [https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte:ptl:validation](https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte:ptl:validation)
