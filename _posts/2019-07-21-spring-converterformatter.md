---
title: "Converter, Formatter"
layout: post
category: Spring
tags: [Spring]
excerpt: "Converter, Formatter에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Converter, Formatter에 대해서 배워 봅시다.

## 배경

  자바빈 기술이 기원인 `PropertyEditor` 는 스프링 @MVC 의 바인딩 기능에 효과적으로 활용되고 있습니다. 이를 확장해서 다양한 HTTP 요청정보를 원하는 타입의 오브젝트로 변환해서 컨트롤러로 가져오는 것도 어렵지 않습니다. 여러 가지 장점에도 불구하고 `PropertyEditor는 매번 바인딩을 할 때마다 새로운 오브젝트를 만들어야 한다`는 약점이 있습니다. 물론 오브젝트에 저장되는 정보는 value 오브젝트 하나뿐인 매우 가벼운 오브젝트이므로 자주 만들어 쓴다고 해서 크게 문제 될 것은 없지만, 아무래도 싱글톤 서비스 오브젝트 중심의 스프링과는 어울리지 않는 느낌을 준다. 특히 빈으로 등록해서 사용할 때는 반드시 프로토타입 스코프를 사용해야 하기 때문에  불편하기도 하다. 제법 스프링을 잘 안다는 개발자들이 프로퍼티 에디터를 무책임하게 싱글톤 빈으로 사용하는 모습도 종종 발견된다. 그만큼 프로퍼티 에디터는 근본적인 위험성을 지니고 있고 불편하다.

  그래서 스프링 3.0에는 PropertyEditor를 대신할 수 있는 새로운 타입 변환 API가 도입됐다. 바로 `Converter 인터페이스` 입니다. PropertyEditor 와 다르게 Converter는 변환과정에서 메소드가 한번만 호출된다. 즉 변환 작업중에 상태를 인스턴스 변수로 저장하지 않는다는 뜻이다. 그래서 멀티쓰레드 환경에서 안전하게 공유해서 쓸 수 있다. 당연히 스프링의 싱글톤 빈으로 등록해 두고 모든 변환 작업이 필요한 오브젝트에서 DI 받아 사용할 수 있습니다.

## Converter

  문자열과 오브젝트 사이의 양방향 변환 기능을 제공하는 PropertyEditor와 다르게 Converter 인터페이스 메소드는 소스 타입에서 타깃 타입으로의 단방향 변환만을 지원합니다. 물론 소스와 타깃을 바꿔서 컨버터를 하나 더 만들면 양방향 변환이 가능해 집니다. Converter는 소스와 타깃의 타입을 임의로 지정할 수 있습니다. PropertyEditor 처럼 한쪽이 스트링 타입의 문자열로 고정되어 있지 않다. 따라서 범용적으로 사용할 수 있는 컨버터를 정의할 수 있습니다.

  Converter 인터페이스는 다음과 같이 정의되어 있습니다. 제네릭을 이용해 소스 타입과 타깃 타입을 미리 지정해둘 수 있습니다. 따라서 컨버터 API 를 사용할 때 지저분한 타입 캐스팅 코드를 사용하지 않아도 됩니다.

  ```java
public interface Converter<S, T> {

  T convert(S source);

}
  ```

  convert() 메소드는 매우 단순합니다. 소스 타입의 오브젝트를 받아서 타깃 타입으로 변환해주면 됩니다. 아래 코드는 LevelPropertyEditor처럼 Level 이늄 오브젝트를 스트링 타입으로 변환하는 컨버터 입니다.

  ```java
  public class LevelToStringConverter implements Converter<Level, String> {

    @Override
    public String convert(Level level) {
      return String.valueOf(level.intValue());
    }

  }
  ```

  반대로 스트링 타입 오브젝트를 Level 오브젝트로 변환하는 컨버터도 다음과 같이 만들 수 있습니다.

  ```java
  public class StringToLevelConverter implements Converter<String, Level> {

    @Override
    public Level convert(String text) {
      return Level.valueOf(Integer.parseInt(text));
    }
  }
  ```

  이 두개의 컨버터를 함께 사용하면 LevelPropertyEditor와 동일한 기능을 하며, 멀티스레드 환경에서도 안전하게 사용할 수 있는 변환 기능을 제공할 수 있습니다.

  > 즉, Converter를 사용하여 양방향 타입변환이 되게 하려면 소스와 타깃을 서로 바꾸어서 클래스를 2개로 만들어 사용해야합니다. 클래스는 PropertyEditor에 비해서 1개 늘어나지만, Thread-Safe합니다.

## ConversionService

  그렇다면 컨트롤러 바인딩 작업에도 이렇게 만든 컨버터를 적용할 수 있을까? 물론 가능하다. 하지만 PropertyEditor처럼 Converter 타입의 컨버터를 개별적으로 추가하는 대신 ConversionService 타입의 오브젝트를 통해 WebDataBinder에 설정해줘야 합니다. ConversionService 는 여러 종류의 컨버터를 이용해서 하나 이상의 타입 변환 서비스를 제공해주는 오브젝트를 만들 때 사용하는 인터페이스입니다. 보통 ConversionService를 구현한 `GenericConversionService` 클래스를 빈으로 등록해서 사용하면 됩니다. GenericConversionService는 스프링의 다양한 타입 변환 기능을 가진 오브젝트를 등록할 수 있도록 `ConverterRegistry` 인터페이스도 구현하고 있습니다.

  스프링 3.0에 추가된 새로운 타입 변환 오브젝트는 이미 살펴본 Converter 외에도 GenericConverter 와 ConverterFactory 를 이용해서도 만들 수 있습니다. GenericConveter 를 이용하면 하나 이상의 소스-타깃 타입 변환을 한 번에 처리할 수 있는 컨버터를 만들 수 있습니다. 또 필드 컨텍스트를 제공받을 수 있습니다. 필드 컨텍스트는 모델의 프로퍼티에 대한 바인딩 작업을 할 때 제공받을 수 있는 메타정보를 말합니다. 따라서 GenericConverter 를 이용하면 단순히 타입의 종류뿐 아니라 이런 부가적인 정보를 활용한 변환 로직을 작성할 수 있습니다. ConverterFactory,는 제너릭스를 활용해서 특정 타입에 대한 컨버터 오브젝트를 만들어주는 팩토리를 구현할 때 사용합니다.

  GenericConversionService 는 일반적으로 빈으로 등록하고 필요한 컨트롤러에서 DI 받아서 @InitBinder 메소드를 통해 WebDataBinder 에 설정하는 방식으로 사용합니다. Converter 와 같은 새로운 타입 변환 오브젝트는 모두 멀티스레드에서 동시 접근이 허용되는 안정성이 보장되므로 프로퍼티 에디터처럼 매번 오브젝트를 만들 필요가 없습니다. 따라서 다양한 타입 변환 오브젝트를 갖고 있는 ConversionService 오브젝트를 하나 정해두고 이를 모든 바인더에 적용한다고 해도 성능 면에서 부담이 없습니다.

  @MVC 컨트롤러의 메소드 파라미터를 위해 사용하는 WebDataBinder 에 GenericConversionService 를 설정하는 방법은 두 가지가 있습니다.

### @InitBinder를 통한 수동 등록

  일부 컨트롤러에만 직접 구성한 ConversionService 를 적용한다거나, 하나 이상의 ConversionService를 만들어 두고 컨트롤러에 따라 다른 변환 서비스를 선택하고 싶다면 컨트롤러의 @InitBinder 메소드를 통해 직접 원하는 ConversionService를 설정해 줄 수 있습니다. ConversionService와 그 안의 모든 타입 변환 오브젝트는 싱글톤으로 등록해서 사용해도 무방하므로 매번 new로 오브젝트를 만들어서 등록할 필요는 없습니다. 대신 ConversionService 를 빈으로 등록해두고 이를 컨터롤러가 DI 받아서 사용하도록 만들면 됩니다.

  GenericConversionService 에 직접 만든 컨버터 등의 변환 오브젝트를 추가하는 방법은 두 가지가 있습니다. 첫째는 GenericConversionService를 상속해서 새로운 클래스를 만들고, 생성자에서 addConverter() 메소드를 이용해 추가할 컨버터 오브젝트를 등록하는 방법입니다. 이렇게 확장한 클래스를 빈으로 등록해서 사용합니다. 두 번째 방법은 추가할 컨버터 클래스를 빈으로 등록해 두고 ConversionServiceFactoryBean 을 이용해서 프로퍼티로 DI 받은 컨버터들로 초기화된 GenericConversionService 를 가져오는 방법입니다. 클래스를 새로 만드는 대신 설정만으로 사용할 컨버터를 만들 수 있기 때문에 편리합니다. 여기서는 두번째 방법을 사용해 보자. 다음과 같이 내부 빈으로 등록된 두 개의 컨버터를 converters 프로퍼티로 갖고 있는 ConversionServiceFactoryBean 을 등록합니다.

  ```xml
<bean class="org.springframework.context.support.ConversionServiceFactoryBean">
  <property name="converters">
    <set>
      <bean class="com.happyhouse.rednics.....LevelToStringConverter" />
      <bean class="com.happyhouse.rednics.....StringToLevelConverter" />
    </set>
  </property>
</bean>
  ```

  컨트롤러에서는 다음과 같이 ConversionService 타입의 빈을 DI 받아 두고 @InitBinder 메소드에서 WebDataBinder에 넣어주면 됩니다.

  ```java
@Controller
public class SearchController {

  @Autowired
  private ConversionService conversionService;

  @InitBinder
  public void initBinder(WebDataBinder dataBinder) {
    dataBinder.setConversionService(conversionService);
  }

}
  ```

  이렇게 해주면 ConversionServiceFactoryBean 빈을 통해 등록된 컨버터들이 모든 컨트롤러의 메소드 파라미터 바인딩에 사용될 것이다.

  매번 개별적으로 오브젝트를 만들어줘야 하는 프로퍼티 에디터와 달리, 컨버터를 비롯한 새로운 타입변환 오브젝트는 싱글톤으로 사용 가능하기 때문에 이렇게 하나의 컨버전 서비스에 모두 등록해두고 한번에 지정할 수 있습니다. 아무리 많은 컨트롤러에서 사용한다고 하더라도 각 컨버터는 한 번만 만들어 집니다.

  물론 여러 개의 컨버전 서비스를 만들어두고 컨트롤러마다 다르게 지정해도 상관없습니다. __WebDataBinder는 하나의 ConversionService 타입 오브젝트만 허용한다는 사실을 기억하고 있으면 됩니다.__

### ConfigurableWebBindingInitializer를 이용한 일괄등록

  어차피 컨버터는 싱글톤이라서 모든 컨트롤러의 WebDataBinder 에 적용해도 별 문제가 되지 않습니다. 따라서 귀찮게 DI 와 @InitBinder 메서드를 사용해서 일일이 컨버전 서비스를 지정해주는 대신 모든 컨트롤러에 한 번에 적용하는 방법도 있습니다. 모든 컨트롤러에 적용하는 프로퍼티 에디터를 정의할 때 사용했던 `WebBindingInitializer`를 이용하면 된다. __WebBindingInitializer는 모든 컨트롤러에 일괄 적용되는 @InitBinder 메서드를 정의한 것이라고 볼 수 있습니다.__

  프로퍼티 에디터의 경우처럼 WebBindingInitializer 구현한 클래스를 만들고 이를 빈으로 등록해도 되지만, ConversionService를 적용할 때는 ConfigurableWebBindingInitializer를 사용하면 편리하다. 코드를 따로 작성하지 않고 빈 설정만으로도 WebBindingInitializer 빈을 등록할 수 있기 때문입니다.

  다음 설정은 모든 컨트롤러에 적용할 변환 서비스를 등록해주는 XML 설정의 예입니다. 컨버전 서비스는 프로퍼티 에디터처럼 코드에서 new 키워드로 생성해줄 필요 없이 독립된 빈으로 등록할 수 있기 때문에 DI 로 가져온 컨버전 서비스 빈을 WebDataBinder 에 추가해 주는 ConfigurableWebBindingInitializer를 만들 수 있는 것입니다. 사실 프로퍼티 에디터도 싱글톤 빈의 형태로 등록하고 DI 받아서 사용하는 방법이 있습니다. 이때는 PropertyEditorRegister를 구현해서 프로퍼티 에디터를 등록해 주는 기능을 가진 빈을 만들어야 합니다. PropertyEditorRegister 는 ConfigurableWebBindingInitializer를 이용해 일괄 적용이 가능합니다.

  ```xml
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
    <property name="webBindingInitializer" ref="webBindingInitializer" />
</bean>

<bean class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer">
    <property name="conversionService" ref="conversionService">
</bean>

<bean class="org.springframework.context.support.ConversionServiceFactoryBean">
  <property name="converters">
    <set>
      <bean class="com.happyhouse.rednics.....LevelToStringConverter" />
      <bean class="com.happyhouse.rednics.....StringToLevelConverter" />
    </set>
  </property>
</bean>
  ```

  위 설정은 좀 많아져서 복잡해 보이지만, 이렇게만 해두면 컨트롤러에서 직접 ConversionService 타입 빈을 DI 받고 @InitBinder 메소드를 이용해 바인더를 초기화하는 코드를 모두 제거할 수 있기 때문에 전체적으로 컨트롤러 코드를 단순하게 만들 수 있다는 장점이 있습니다. 모든 컨트롤러에 일괄 적용하는 타입 변환기능은 프로퍼티 에디터보다는 이렇게 컨버터와 같은 최신 타입 변환기술을 사용하는 것이 좋습니다. 컨버터 등록 때문에 복잡해진 XML 설정은 mvc 스카마의 캐그를 이용하여 간단하게 줄일 수도 있습니다. ConversionServiceFactoryBean 에서 자동으로 만들어주는 GenericConversionService에는 추가로 등록한 컨버터 말고도 기본으로 등록된 여러 가지 디폴트 컨버터가 있습니다. 이 디폴트 컨버터는 @MVC 파라미터 바인딩보다는 주로 애플리케이션 내에서의 일반적인 타입 변환 작업을 진행할 때 사용할 용도인 것들이 대부분이니 크게 신경쓰지 않아도 좋습니다. 일반적인 타입 변환 서비스를 활용하고 싶다면 ConversionServiceFactory 클래스를 참고해서 디폴트로 등록되는 컨버터들이 어떤 것인지 살펴보기 바랍니다.

## Formatter

  Converter, GenericConverter, ConverterFactory 라는 세 가지의 새로운 타입 변환 API는 모두 범용적인 타입 변환을 목적으로 설계됬습니다. 따라서 프로퍼티 에디터처럼 문자열과 오브젝트 사이의 상호 변환이라는 특화된 변환작업보다 유연합니다. 반대로 특화되어 있지 못해서 불편한 점도 있습니다.

  그래서 스프링은 `Formatter` 타입의 추가 변환 API를 제공하고 있습니다. 이 포맷터는 스트링 타입의 폼 필드 정보와 컨트롤러 메소드의 파라미터 사이에 양방향으로 적용할 수 있도록 두 개의 변환 메소드를 가지고 있습니다.

  Formatter는 Converter와 같이 스프링이 기본적으로 지원하는 범용적인 타입 변환 API가 아니다. 따라서 Formatter를 구현해서 만든 타입 변환 오브젝트를 GenericConversionService등에 직접 등록할 수는 없습니다. 대신 Formatter 구현 오브젝트를 GenericConverter 타입으로 포장해서 등록해주는 기능을 가진 `FormattingConversionService`를 통해서만 적용될 수 있습니다.

  Formatter를 사용하기 위해서 인터페이스를 구현한 XXXFormatter 클래스를, 스프링부트에서 사용하기 위해서는 빈으로 등록해야 하는데 `@Component (@Repository 등)`을 사용해서 등록하는 방식은, 스프링 부트에서만 가능합니다.

  스프링에서 @Component를 사용하여 구현체를 빈으로 등록해주려해도, 적용이 안되서, 수동으로 빈으로 등록해야한다. 하지만 스프링 부트는 자동으로 빈으로 등록해줍니다.

  스프링과 스프링 부트 둘다 formatter를 빈으로 적용시키기 위해서 자바설정파일과 xml로 설정하는 방식이 있습니다.

  ```java
  // 프로젝트 전역에 영향을 미치는 설정 방식
  @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new FileTypeFormatter());
    }
  ```

  registry객체는 각종 빈으로 등록하려는 객체를 관리해줍니다.

  ```java
  // 해당 컨트롤러 내에서만 영향을 미치는 설정 방식
  @InitBinder
    public void fileTypesFormatter (WebDataBinder webDataBinder) {
        webDataBinder.addCustomFormatter(new FileTypeFormatter());
    }
  ```

  __Formatter 인터페이스는 오브젝트를 문자열로 변환해주는 print()메소드와 문자열을 오브젝트로 변환해주는 parse()메소드, 두 개로 구성되어 있다.__

  지금까지는 주로 문자열로 들어오는 HTTP 요청 파라미터를 모델의 프로퍼티 타입으로 변환해주는 기능을 살펴봤는데, 이와 반대로 모델 프로퍼티를 문자열로 변환해주는 기능도 자주 사용됩니다. 뷰에서 모델 오브젝트의 내용을 HTML로 만들어 줄 때 바로 `오브젝트-문자열` 타입 변환기능이 사용된다. 그런데 `다국어 서비스`를 위해 지역화 메시지를 지원하는 웹 애플리케이션이라면, 같은 모델 프로퍼티 값이라도 지역설정에 따라 다른 내용으로 바꿔주는 기능이 있다면 좋을 것이다. 문제는 PropertyEditor나 단순 Converter에는 현재 지역정보가 어떤 것으로 설정되어 있는지 간단히 알 수 있는 방법이 없다는 점이다. 반면에 Formatter의 변환 메소드에는 오브젝트나 문자열뿐이나라 `Locale 타입의 현재 지역정보`도 함께 제공된다.

  Formatter 인터페이스가 가진 두 개의 메소드는 다음과 같다. 두 가지 모두 Locale 정보가 함께 제공되므로 이를 문자열 변환이나 오브젝트 변환에 참고할 수 있다.

  ```java
  String print(T object, Locale locale); // OBJECT TO STRING
  T parse(String text, Locale locale) throws ParseException; // STRING TO OBJECT
  ```

  Formatter는 이렇게 컨트롤러 내의 바인딩과 타입 변환에 사용되도록 특화된 인터페이스입니다.

  모델 필드의 어노테이션 정보를 활용해서 Formatter를 생성해 주는 AnnotationFormatterFactory 팩토리를 사용해서 Formatter를 생성할 수도 있습니다. 이를 활용하면 타입 변환시 필드의 어노테이션 정보를 활용할 수 있습니다.

  직접 만든 포멧터를 적용하려면 FormattingConversionService를 초기화해주는 기능을 가진 FormattingConversionServiceFactoryBean을 상속해서 Formatter의 초기화 기능을 담당하는 installFormatters() 메소드를 오버라이드하는 방법을 사용해야 합니다.

  지역정보가 타입 변환에 꼭 필요한게 아니라면, 굳이 Formatter를 사용하지 말고 간단히 Conveter 를 구현해서 사용하는 편이 나을 것이다. 본격적으로 Formatter 를 적용하려면 지역정보뿐 아니라 필드의 애노테이션까지 참조할 수 있는 AnnotationFormatterFactory 를 사용해야 한다. 대신 구현 방법이 간단하지 않으니 제법 많은 시간을 들여서 개발 방법을 연구해야 할 것이다. 스프링이 이미 제공하고 있는 AnnotationFormatterFactory 타입의 클래스 코드를 참고해 보면 도움이 된다.

  당장에는 Formatter를 직접 구현해서 사용하기보다는 FormatterConversionServiceFactoryBean을 통해 기본적으로 등록되는 몇 가지 유용한 애노테이션 기반 포멧터를 활용하는 방법만 익혀도 충분히 그 유용함을 느낄 수 있을 것입니다.

  FormattingConversionServiceFactoryBean 을 사용했을 때 자동으로 등록되는 포맷터에는 다음 두 가지가 있다. 이 두가지 모두 애노테이션을 이용해 포맷정보를 제공해 줄 수 있도록 만들어졌다. 기존의 프로퍼티 에디터나 컨버터가 항상 동일한 방식으로 변환을 했다면, 애노테이션을 활용하는 포맷터는 애노테이션으로 지정한 메타 정보를 활용해서 세밀한 변환조건을 부여해줄 수 있다.

  예를들어 @RequestMapping("{fileType}") HTTP 요청으로 fileType인 문자열을 받아서 ENUM클래스로 변환하기 위해서는 Formatter의 parse() 메서드를 사용하여 String to Enum으로 변환시키고, @InitBinder 메서드를 통해서 바인딩 시켜줍니다. 그리고 HandlerMethod에서는 @PathVariable을 사용하여 fileType 파라미터를 사용할 수 있습니다.



## 참조

  > [https://springsource.tistory.com/16?category=465780](https://springsource.tistory.com/16?category=465780)
