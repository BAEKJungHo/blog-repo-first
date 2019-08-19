---
title: "Interceptor"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링의 Interceptor에 대해서 자세히 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링의 Interceptor에 대해서 자세히 배워 봅시다.

## Interceptor

  인터셉터의 정식 명칭은 `핸들러 인터셉터(Handler Interceptor)`입니다. 클라이언트의 요청이 컨트롤러에 가기 전에 가로채고, 응답이 클라이언트에게 가기전에 가로챕니다. 즉, 인터셉터는 DispatcherServlet이 컨트롤러를 요청하기 전, 후에 요청과 응답을 가로채서 가공할 수 있도록 해줍니다.

  스프링에서는 인터셉터를 지원하기 위해서 `HandlerInterceptor` 인터페이스와 `HandlerInterceptorAdapter` 추상 클래스를 지원합니다. 따라서 인터셉터를 구현하기 위해서는 implements로 HandlerInterceptor 인터페이스를 구현하거나 HandlerInterceptorAdapter 추상 클래스를 상속 받아야 합니다.

  HandlerInterceptorAdapter는 위의 인터페이스를 사용하기 쉽게 구현해 놓은 추상클래스를 뜻합니다.

  > 참고 : [스프링의 RETURN TYPE에 대해서 공부하기](https://baekjungho.github.io/spring-modelandviewreturn/)

  - `HandlerInterceptorAdapter Method`

  ```java
/** preHandle()
* Controller로 요청이 들어가기 전에 수행
* handler : preHandle() 메서드를 수행하고 수행될 컨트롤러 메서드에 대한 정보를 담고 있다.
* preHandle() 의 결과가 return ture일 경우 : preHandle()에서 저장한 List에 저장된 참조 변수들을, HandlerMethod에서 받아서 쓸 수 있다.
* preHandle() 의 결과가 return false일 경우 : HandlerMethod를 타지 않는다.
* 핸들러 메서드에서 preHandle()에서 사용한 정보들을 꺼내 쓰는 방법
* 1. request.setAttribute("allowUrls", allowUrls);
* 2. 세션
* 3. context 만들기
*/
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

/** postHandle()
* 컨트롤러의 메서드의 처리가 끝나 return 되고 화면을 띄워주는 처리가 되기 직전에 이 메서드가 수행
* ModelAndView 객체에 컨트롤러에서 전달해 온 Model 객체가 전달됨으로 컨트롤러에서 작업 후
* postHandle() 에서 작업할 것이 있다면 ModelAndView를 이용하면 된다.
* EX) int pageIndex = (Integer) modelAndView.getModel().get("pageIndex");
*/
postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)

/** afterCompletion()
* 컨트롤러가 수행되고 화면처리까지 끝난 뒤 호출된다.
* 즉, View return 후에 수행되는 메서드
*/
afterCompletion()
  ```

### preHandle()에서 HttpServletRequest 객체 사용하기

  preHandle() 메서드에서 HttpServletRequest 객체를 이용하면, 키값으로 JSP에서 사용할 수 있습니다.

  ```java
  request.setAttribute("CMS_MENU", menuList);
  ```

  HttpServletRequest 객체를 이용해서 setAttribute로 키값과 Value값을 담으면,

  JSP에서 키값으로 꺼내 쓸 수 있습니다. 만약 CMS_MENU라는 키값안에 List를 속성으로 가지고 있는경우(EX) 2뎁스 이상), JSP에서 forEach를 두 번 써서 꺼내 쓸 수 있습니다.

  ```html
  <c:forEach items="${CMS_MENU}" var="cmsMenu">
      <li><a href="#"><c:out value="${cmsMenu.menuName}"/></a>
          <ul>
              <c:forEach items="${cmsMenu.children}" var="children">
                  <li><a href="<c:url value="${children.url}?mno=${children.id}"/>" class="code_${children.id}"><c:out value="${children.menuName}"/></a></li>
              </c:forEach>
          </ul>
      </li>
  </c:forEach>
  ```

### 인터셉터를 사용하기 위한 설정(WebConfig.java)

  ```java
  @Configuration
  @ComponentScan(basePackages = "com.tech")
  @ServletComponentScan(basePackages = "com.tech.blog")
  public class WebConfig extends WebMvcConfigurerAdapter {

      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
          registry.addViewController("/error/400").setViewName("/errors/400");
          registry.addViewController("/error/403").setViewName("/errors/403");
          registry.addViewController("/error/exception").setViewName("/errors/exception");
          registry.addViewController("/tech/login").setViewName("login.pop");
      }

      @Override
      public void addInterceptors(InterceptorRegistry registry) {
          registry.addInterceptor(new TechMnoInterceptor())
                      .addPathPatterns("/mec/**")
                      .excludePathPatterns("/mec/**/api/**");
          registry.addInterceptor(techMenuRenderInterceptor())
                      .addPathPatterns("/tech/**")
                      .excludePathPatterns("/tech/**/api/**", "/tech/login", "/tech/logout", "/tech/pwd");
      }

      @Bean
      public HandlerInterceptor techMenuRenderInterceptor() {
          return new TechMenuRenderInterceptor();
      }
  }
  ```

  인터셉터를 사용하기 위해서는 `WebMvcConfigurerAdapter` 클래스를 상속 받아서 `addInterceptors` 메서드를 오버라이딩 하여 사용해야 합니다.

### WebMvcConfigurerAdapter

스프링 MVC부터 제공, 자바 8부터 사용가능 합니다.

WebMvcConfigurer빈 클래스를 사용하여 서브 클래스가 관심있는 메소드 만 재정의 할 수 있도록 구현 합니다.

WebMvcConfigurer 빈 클래스를 구현하여 서브 클래스가 관심 있는 메서드만 재정의 합니다.

> [WebMvcConfigurerAdapter Documents](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurerAdapter.html)

```java
import java.util.List;
import org.springframework.format.FormatterRegistry;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.validation.MessageCodesResolver;
import org.springframework.validation.Validator;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.HandlerMethodReturnValueHandler;
import org.springframework.web.servlet.HandlerExceptionResolver;

public abstract class WebMvcConfigurerAdapter implements WebMvcConfigurer {

  public WebMvcConfigurerAdapter() {
  }

  public void configurePathMatch(PathMatchConfigurer configurer) {
  }

  public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
  }

  public void configureAsyncSupport(AsyncSupportConfigurer configurer) {
  }

  public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
  }

  public void addFormatters(FormatterRegistry registry) {
  }

  public void addInterceptors(InterceptorRegistry registry) {
  }

  public void addResourceHandlers(ResourceHandlerRegistry registry) {
  }

  public void addCorsMappings(CorsRegistry registry) {
  }

  public void addViewControllers(ViewControllerRegistry registry) {
  }

  public void configureViewResolvers(ViewResolverRegistry registry) {
  }

  public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
  }

  public void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> returnValueHandlers) {
  }

  public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
  }

  public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
  }

  public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers) {
  }

  public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers) {
  }

  public Validator getValidator() {
    return null;
  }

  public MessageCodesResolver getMessageCodesResolver() {
    return null;
  }
}
```

## 인터셉터를 사용한 예제

  > [QueryString을 param 키워드를 이용해서 꺼내기, 인터셉터를 이용해서 모든 클래스 및 브라우저 URI에 공통으로 들어가야할 속성을 Interceptor로 선언하기](https://baekjungho.github.io/spring-querystring/)

## 참고

  > [인터셉터와 필터의 차이 공부하기](https://baekjungho.github.io/spring-interceptorfilter/)
