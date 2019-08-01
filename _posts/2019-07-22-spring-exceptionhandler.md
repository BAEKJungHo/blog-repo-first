---
title: "Exception Handling in Spring MVC"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링 MVC에서 예외를 처리하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 MVC에서 예외를 처리하는 방법에 대해서 배워 봅시다.

## HTTP 상태코드 사용하기 Using HTTP Status Codes

  몇 가지 스프링의 예외들은 자동으로 명시된 HTTP 상태 코드로 매핑됩니다. `HTTP 상태 코드`로 매핑하기 위해 예외에는 `@ReponseStatus` 어노테이션을 붙여 줄 수 있습니다. 예외 처리를 위한 메소드에는 @ExceptionHandler 어노테이션을 붙여 줄 수 있습니다.

  예외 처리를 위한 가장 간단한 방법은 예외를 HTTP 상태 코드에 매핑시켜 응답에 포함시켜 주는 것입니다.

### HTTP 상태코드로 매핑되는 예외

  - BindException : 400 - Bad Request

  - ConversionNotSupportedException : 500 - Internal Server Error

  - HttpMediaTypeNotAcceptableException : 406 - Not Acceptable

  - HttpMediaTypeNotSupportedException : 415 - Unsupported Media Type

  - HttpMessageNotReadableException : 400 - Bad Request

  - HttpMessageNotWritableException : 500 - Internal Server Error

  - HttpRequestMethodNotSupportedException : 405 - Method Not Allowed

  - MethodArgumentNotValidException : 400 - Bad Request

  - MissingServletRequestParameterException : 400 - Bad Request

  - MissingSerlvetRequestPartException : 400 - Bad Request

  - NoSuchRequestHandlingMethodException : 404 - Not Found

  - TypeMismatchException : 400 - Bad Request

  표의 예외들은 일반적으로 `DispatcherServlet` 동작이나 검증 수행 과정에서 오류에 대한 결과로서 발생합니다.

  예를 들어, DispatcherServlet이 요청을 처리하는 적절한 컨트롤러 메소드를 찾아내지 못한다면 `NoSuchRequestHandlingMethodException`이 발생하여 `404(Not Found)`에 해당하는 상태 코드를 포함한 응답이 결과로 나온다.

### @ReponseStatus EXAMPLE

  보통 웹요청 처리시 발생하는 미처리 예외(unhandled exception)는 서버가 `HTTP 500` 응답을 리턴합니다. 그러나 당신이 작성한 커스텀 예외를(HTTP명세서에 의해 정의된 모든 HTTP 상태코드를 지원하는) @ResponseStatus 어노테이션과 함께 사용할 수 있습니다. 어노테이션된 예외(annotated exception)가 컨트롤러 메소드에서 발생할 때, 그리고 다른 곳에서 처리되지 않을때, 이는 자동적으로 특정한 상태코드를 가지고 리턴되는 적절한 HTTP응답을 발생할 것입니다.

  - Order가 빠진 경우의 예외(Exception) : OrderNotFoundException Class

  ```java
  @ResponseStatus(value=HttpStatus.NOT_FOUND, reason="No such Order") // 404
  public class OrderNotFoundException extends RuntimeException {
    // ...
  }
  ```

  - Order를 사용하는 컨트롤러

  ```java
  @RequestMapping(value="/orders/{id}", method=GET)
  public String showOrder(@PathVariable("id") long id, Model model) {
    Order order = orderRepository.findOrderById(id);
    if (order == null) throw new OrderNotFoundException(id);
    model.addAttribute(order);
    return "orderDetail";
   }
   ```

## 컨트롤러 기반 예외처리(Controller Based Exception Handling)

### @ExceptionHandler

  컨트롤러에서 예외처리를 하기 위해서는 HandlerMethod 위에 `@ExceptionHandler`를 사용하면 됩니다.
  스프링은 컨트롤러 안에 @ExceptionHandler를 사용한 메서드가 존재하면 그 메서드가 컨트롤러 내부의 예외 처리를 담당하게 합니다.

  즉, @ExceptionHandler가 있는 컨트롤러 내에서만 예외처리가 가능, @ExceptionHandler의 파라미터로 들어오는 예외가 발생하는 경우에 해당 HandlerMethod가 실행 됩니다. 주로 `필수값`에 대해서 검증을하고 예외처리를 해주어야 합니다.

  - @ExceptionHandler가 적용된 HandlerMethod가 하는 일
    - @ResponseStatus 어노테이션 없이 예외를 처리
    - 사용자를 특정한 에러 페이지로 리다이렉트
    - 완전히 컨스텀 에러 응답을 만듬

### 예외 처리 단계   

  - 컨트롤러의 HandlerMethod에서 __@Valid와 BindingResult__ 를 사용하여 검증

  - @Valid를 사용했으므로, VO의 해당 필수값(필드)에 `@NotEmpty`와 같은 어노테이션 적용

  - 로직(메서드)에서 __Assert.notNull__ 을 사용하여 한번 더 검증
    - EX) Assert.notNull(updateRequest.getFilePath(), "filePath 는 null이 올 수없습니다.");

  - 컨트롤러에서 __@ExceptionHandler(FileNotFoundException.class)__ 와 같은 방식으로 예외처리
    - 사용자에게 오류에 대한 화면 처리를 위함.

  - Ajax로 fail(err => alert(err))과 같은 방식으로 처리
    - err은 에러에 대한 정보를 가지고 있습니다.

  즉, 필수값에 대해서 예외 처리를 하기 위해서는 `총 5단계`를 거칩니다. 생략되는 단계도 있습니다.

  - FileContentsNotFoundException 클래스 생성(RuntimeException 상속)

  ```java
public class FileContentsNotFoundException extends RuntimeException {
  public FileContentsNotFoundException(String message) {
    super(message);
  }

  public FileContentsNotFoundException(String message, Throwable cause) {
    super(message, cause);
  }

  public FileContentsNotFoundException(Throwable cause) {
    super(cause);
  }
}
  ```

  - @Valid와 BindingResult

  ```java
  @PostMapping("edit")
  public ResponseEntity update(@PathVariable FileTypes type, @Valid @RequestBody FileManageDto.UpdateRequest updateRequest, BindingResult result) {
    if (result.hasErrors()) {
      ErrorResponse errorResponse = new ErrorResponse(ErrorCode.BAD_REQUEST, result.getFieldErrors());
      return ResponseEntity.badRequest().body(errorResponse);
    }
    return ResponseEntity.ok(updateRequest);
  }
  ```

  - @ExceptionHandler

  ```java
  @ExceptionHandler(FileContentsNotFoundException.class)
  public ResponseEntity fileContentsNotFoundExceptionHandler (FileContentsNotFoundException e) {
    return ResponseEntity.badRequest().body(e.getMessage());
  }
  ```

### @ExceptionHandler EXAMPLE

  ```java
  @Controller public class ExceptionHandlingController {
  // @RequestHandler methods
  ...
  // Exception handling methods

  // Convert a predefined exception to an HTTP Status code
  @ResponseStatus(value=HttpStatus.CONFLICT, reason="Data integrity violation") // 409
  @ExceptionHandler(DataIntegrityViolationException.class) public void conflict() {
    // Nothing to do
  }

  // Specify the name of a specific view that will be used to display the error:
  @ExceptionHandler({SQLException.class,DataAccessException.class})
  public String databaseError() {
    // Nothing to do. Returns the logical view name of an error page, passed to
    // the view-resolver(s) in usual way.
    // Note that the exception is _not_ available to this view (it is not added to
    // the model) but see "Extending ExceptionHandlerExceptionResolver" below. return "databaseError";
  }

  // Total control - setup a model and return the view name yourself. Or consider
  // subclassing ExceptionHandlerExceptionResolver (see below).
  @ExceptionHandler(Exception.class)
  public ModelAndView handleError(HttpServletRequest req, Exception exception) {
    logger.error("Request: " + req.getRequestURL() + " raised " + exception);
    ModelAndView mav = new ModelAndView(); mav.addObject("exception", exception);
    mav.addObject("url", req.getRequestURL());
    mav.setViewName("error");
    return mav;
   }
  }
  ```

  이 메소드들중 아무거나, 추가적인 처리를 위해 고를 수 있습니다. 가장 일반적인 예제는 예외를 로그하는 것입니다.

  처리 Handler 메서드는 유연한 특징을 가지고 있어 당신이 `HttpServletRequest, HttpServletResponse, HttpSession` 그리고/또는 Principl. 와 같은 명확히 서블릿 관련 객체들을 패스할 수 있습니다. 중요한 알림:  Model 은 @ExceptionHandler 메소드의 파라미터가 될 수 없습니다. 대신, 위의 handleError()에 의해 보여진. ModelAndView를 사용하여 메소드안에 하나의 모델을 설정할 수 있습니다.

## 예외와 뷰(Exceptions and Views)

  예외를 모델에 추가할 때 조심해야할 점은, 당신의 사용자들은 구체적인 자바 예외나 stack-trace가 포함된 웹페이지를 보고 싶지않아 한다는 것입니다. 하지만 페이지 소스안에 코멘트로서 구체적인 예외 상태를 넣어 당신을 서포트하는 사람들을 도와주려는 것은 유용할 수 있습니다. 만일 JSP를 사용한다면 당신은 아래와 같이 예외 메세지나 (숨겨진 `<div>` 를 사용하여) stack-trace를 출력하는 등을 할 수 있을 것입니다.

  ```html
  <h1>Error Page</h1>
  <p>Application has encountered an error. Please contact support on ...</p>
  <!-- Failed URL: ${url}
  Exception: ${exception.message}
  <c:forEach items="${exception.stackTrace}" var="ste">
  ${ste}
  </c:forEach> -->
  ```

  `타임리프(Thymeleaf)`에서 이와 같은 일을 하려면 support.html를 보자, 예제 어플리케이션에선 다음과 같은 결과를 볼 수 있습니다.

  ![ex1](/images/posts/201907/ex1.jpg)

## 전역 예외 처리(Global Exception Handling)

### @ControllerAdvice

  컨트롤러 어드바이스(@ControllerAdvice)는 당신에게 똑같은 예외처리 기술을 사용하지만, 개별 컨트롤러가 아니라 전체 어플리케이션에 적용할 수 있게 만들어 줍니다. 즉, 전역 범위로 예외 처리를 할 수 있습니다. 이들을 `어노테이션 기반 인터셉터(annotation driven interceptor)`로 이해하면 될 것입니다.

  `@ControllerAdvice`는 스프링 3.2 이상에서 사용 가능하며, `@Controller`나 스프링 4.0 이상에서 지원하는 `@RestController`에서 발생하는 예외
  등을 catch하는 기능을 가지고 있습니다. 클래스 위에 `@ControllerAdvice`를 붙이고 어떤 예외를 잡아낼 것인지 메서드 상단에 `@ExceptionHandler(예외 클래스명.class)`를 기술합니다. 스프링 4.0 이상에서는 특정한 컨트롤러만 지정하여 캐치할 수 있습니다.

  @ControllerAdvice 어노테이션을 가지는 클래스는 `컨트롤러 어드바이스(controller-advice)`가 되며 3가지 타입으로 메소드를 지원할 수 있습니다.

  - @ExceptionHandler으로 어노테이션된 예외처리 메소드
  - @ModelAttribute으로 어노테이션된(추가적인 데이터를 모델에 추가하기 위한) 모델 향상(Model enhancement) 메서드
    - [Note] 이들 속성들은  예외처리 뷰에서 사용할 수 없다.
  - @InitBinder로 어노테이션된 (폼 처리를 설정하는데 사용되는) 바인더 초기화(Binder initialization) 메서드

### @ControllerAdvice EXAMPLE

  - EXAMPLE 1.

  ```java
  @ControllerAdvice // 모든 컨트롤러에 대응
  public class ExceptionControllerAdvice {

    @ExceptionHandler(Exception.class) // 모든 예외를 받음
    public ModelAndView exception(Exception e) {
      ModelAndView mav = new ModelAndView("exception");
      mav.addObject("name", e.getClass().getSimpleName());
      mav.addObject("message", e.getMessage());
      return mav;
    }
  }
  ```

  - EXAMPLE 2.

  ```java
  @ControllerAdvice class GlobalControllerExceptionHandler {
    @ResponseStatus(HttpStatus.CONFLICT) // 409
    @ExceptionHandler(DataIntegrityViolationException.class)
    public void handleConflict() { // Nothing to do
      .....
    }
  }
  ```

  어떠한 예외에 처리되는 기본 처리자가 필요하면, 다음과 같이 약간만 손보면 된다. 어노테이션된 예제는 프레임워크에 의해 처리된다는 것을 명심하합시다.

  ```java
  @ControllerAdvice class GlobalDefaultExceptionHandler {
    public static final String DEFAULT_ERROR_VIEW = "error";
    @ExceptionHandler(value = Exception.class)
    public ModelAndView defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {
      // If the exception is annotated with @ResponseStatus rethrow it and let
      // the framework handle it - like the OrderNotFoundException example
      // at the start of this post.
      // AnnotationUtils is a Spring Framework utility class.
      if (AnnotationUtils.findAnnotation(e.getClass(), ResponseStatus.class) != null) throw e;
      // Otherwise setup and send the user to a default error-view.
      ModelAndView mav = new ModelAndView();
      mav.addObject("exception", e);
      mav.addObject("url", req.getRequestURL());
      mav.setViewName(DEFAULT_ERROR_VIEW); return mav;
     }
   }
   ```

## HandlerExceptionResolver

  `HandlerExceptionResolver`를 구현한 DispatcherServlet의 application/context에서 선언된 모든 스프링빈은 MVC 시스템에서 올라오는 모든 예외를 처리하고 인터셉트하는데 사용되어지며 컨트롤러에 의해 처리되지않습니다. 인터페이스는 아래와 같습니다:

  ```java
  public interface HandlerExceptionResolver {
    ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex);
  }
  ```

  Handler는 예외가 발생한 컨트롤러를 참조합니다. (@Controller 인스턴스들은 스프링 MVC가 지원하는 핸들러의 하나의 타입일 뿐이라는 것을 기억하자. 예를 들면, `HttpInvokerExporter`와 `WebFlow Executor` 또한 핸들러의 타입들입니다.)

  이 뒷단에서 MVC는 세가지 resolver를 기본으로 생성합니다.

  - `ExceptionHandlerExceptionResolver`는 핸들러(컨트롤러)와 컨트롤러-어드바이스들상의 적절한 @ExceptionHandler 메서드를 위한 `uncaught exception`에 맞닿아 매치됩니다.

  - matches uncaught exceptions against for suitable @ExceptionHandler methods on both the handler (controller) and on any controller-advices.

  - `ResponseStatusExceptionResolver`는 @ResponseStatus에 의해 어노테이션된 `uncaught exception`들을 찾습니다.

  - `DefaultHandlerExceptionResolver`는 표준 스프링 예외를 변환하고 그들을 HTTP상태코드로 변환합니다.

  이들은 서로 순서에 따라 연쇄작동하고 처리합니다. (내부적으로 스프링은 이를 담당하는 빈들 생성하는데 - `HandlerExceptionResolverComposite`가 이를 담당합니다.)

  `resolveException`의 메서드 시그니처는 Model을 포함하지않는다는 것을 상기합시다. 아래에 이유가 나와있습니다.

  필요시 자신의 커스텀 예외처리 시스템을 설정하기 위해, 커스텀 `HandlerExceptionResolver`를 구현할 수 있습니다. Handlers는 보통 스프링의 `Ordered`인터페이스를 구현하여 당신이 실행하는 핸들러의 순서를 정의 할 수 있습니다.

## 언제 무엇을 써야하나? What to Use When?

  보통 스프링은 당신에게 선택할 수 있게 제공하는 것을 선호합니다. 그래서 당신이 해야하는게 뭘까? 여기 몇가지 중요한 룰이 있습니다. 하지만 XML설정이나 어노테이션을 선호한다면 그 역시 상관없습니다.

  - 당신이 작성한 예외들에 `@ResponseStatus`를 추가하는 것을 고려.

  - `@ControllerAdvice` 클래스에 `@ExceptionHandler` 메서드를 구현하거나 `SimpleMappingExceptionResolver`의 인스턴스를 사용하는 모든 종류의 예외들에 대해 아마 당신의 어플리케이션 설정에 이미 SimpleMappingExceptionResolver를 이미 사용하고 있다면, 여기에 새로운 예외클래스를 추가하는게 @ControllerAdvice를 구현하는 것보다 더 쉬울 것입니다.

  - Warning: 같은 어플리케이션에 이들 옵션을 너무 많이 혼용하여 사용하지 않아야합니다. 같은 예외 하나 이상의 방식으로 처리 되어질 수 있고, 이경우 원치않는 행동을 얻을 수 있습니다. 컨트롤러에서 @ExceptionHandler 메서드는 항상 @ControllerAdvice 인스턴스의 메소드들 전에 선택되어집니다. 무슨 컨트롤러 어드바이스가 먼저 처리되는지 정의하지 않습니다.

## 스프링 부트와 에러처리(Spring Boot and Error Handling)

  `스프링 부트`는 스프링 프로젝트를 최소한의 설정으로 돌릴 수 있게 만들어줍니다. 스프링부트는 클래스 패스의 키 클래스과 패키지들을 찾아 자동으로 민감한 기본값들을 생성합니다. 예를 들면 당신이 서블릿 환경을 사용중이라면 스프링 MVC를 가장 일반적으로 많이 사용하는 `뷰-리졸버view-resolvers, 핸들러 매핑handler mappings` 등을 설정해줍니다. 만일 JSP나 타임리프 파일이 있으면 그에 맞는 해당 뷰 기술을 자동으로 설정합니다.

  스프링 MVC는 기본적으로 제공하는 에러페이지가 없습니다. 기본 에러페이지를 설정하는 가장 흔한 방법은 언제나 `SimpleMappingExceptionResolver` 를 가지는 것입니다. (스프링 버전1 이후로)그러나 스프링 부트는 또한 에러 처리 `fallback error-handling` 페이지를 제공하고 있습니다.

  시작시 스프링 부트는 `/error`를 위한 매핑을 찾습니다. 명명법에 의해 /error 로 끝나는 URL은 같은 이름의 논리적 뷰와 매핑됩니다: error. 데모 어플리케이션에서, 이 뷰는 타임리프 템플릿의 error.html로 매핑됩니다. (만일 JSP를 사용하고 있다면 InternalResourceViewResolver가 설정되면서 error.jsp로 매핑될 것입니다.)

  /error 와 매핑되는 뷰가 없다면, 스프링 부트는 `Whitelabel Error Page` 라 불리는 그 자신의 에러페이지를 정의합니다. (__HTTP상태 정보와 uncaught exception로 부터의 메세지와 같은 어떠한 에러 디테일 가지는 최소한의 페이지__). 만일 error.html 템플릿을 이를테면, error2.html로 이름을 바꾸고 재시작하면 이것이 사용되어지는 것을 확인할 수 있습니다.

  `defaultErrorView()`로 불리는 @Bean 메서드를 자바 설정으로 정의함으로서, 당신은 자신만의 에러 View인스턴스를 리턴할 수 있습니다. (더 자세한 정보는 스프링 부트의 `ErrorMvcAutoConfiguration` 클래스를 보자)

  기본 에러 뷰를 설정하기 위해 이미 `SimpleMappingExceptionResolver`를 사용중이라면? 간단히 `defaultErrorView`에 스프링 부트에서 사용하는 같은 뷰: error를 정의해주면 됩니다. 또는 `application.properties` 파일에 error.whitelabel.enabled를 false로 설정하여 스프링 부트의 기본 에러페이지를 disabled하면 됩니다. 이경우 당신의 컨테이너의 기본 에러페이지가 사용될 것입니다.






## 참조

  > [https://springboot.tistory.com/25](https://springboot.tistory.com/25)
  >
  > [https://m.blog.naver.com/PostView.nhn?blogId=kimnx9006&logNo=220625947148&proxyReferer=https%3A%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=kimnx9006&logNo=220625947148&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
