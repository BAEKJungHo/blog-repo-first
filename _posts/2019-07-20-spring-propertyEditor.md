---
title: "PropertyEditor, DataBinder Mechanism"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링에서 제공하는 TypeConverter와 DataBinding에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링 데이터 바인딩 추상화 : PropertyEditor와 스프링에서 제공하는 TypeConverter와 DataBinding에 대해서 배워 봅시다.

## Binding ?

  > 바인딩 (Binding)
  >
  > [일반] 이름을 어떤 속성과 연결짓는 과정을 말함
  >
  > [전산] 변수, 프로시저, 상수 등의 이름(식별자)을 속성(값)과 연관(association)짓는 것

## TypeConverter

  스프링에서는 두 가지 `TypeConverter`가 존재합니다.

  하나는 SpringMVC가 HttpRequest를 커맨드(모델)로 전환할 때 사용하는 `(Web)DataBinder`이고, 다른 하나는 스프링의 빈의 프로퍼티 설정을 위해서 사용하는 `BeanWrapper`입니다. 두가지 모두 TypeConverter 인터페이스를 구현하고 있다.

  두 경우 모두 타입전환이 필요로 하는 이유는 텍스트 포맷으로 존재하는 XML설정이나 HTTP 파라미터를 임의의 오브젝트로 전환해야 하기 때문입니다.

  __스프링이 이 타입변환을 위해서 사용하는 방법은 JavaBeans의 PropertyEditor이다.__ 각각 PropertyEditorRegistry가 되어서 기본타입을 포함해서 Class, Resource 나 콜렉션 같은 복잡한 타입도 간단하게 변환이 가능하도록 만들고 있다. 또한 새로운 Editor를 추가하는 것도 어려운 일이 아닙니다. SpringMVC를 마스터하기 위해서(@MVC도 마찬가지이다) 중요한 것이 이 PropertyEditor와 DataBinder 메카니즘입니다.

## DataBinding

  스프링에서는 사용자가 입력한 값을 `타겟 객체에 설정`하는 데이터 바인딩 기능을 지원한다. `org.springframework.validation.DataBinder`인터페이스를 통해서 데이터 바인딩 기능을 지원하며 __사용자가 입력한 값을 도메인 모델에 동적으로 변환해서 넣어줍니다.__

  이 기능이 필요한 이유는 다음과 같은 이유에서다. 사용자의 입력값은 문자열이지만 자바의 객체가 가지고 있는 데이터 타입은 int, long, Data, Double 혹은 사용자가 작성한 클래스로 만들어진 객체 ENUM, Event, Book 같은 것입니다. 따라서 사용자의 이런 문자열 입력값을 자바의 데이터 타입으로 변환해서 넣어줘야하는 데, 이때 적절하게 데이터를 넣어주는 데이터 바인딩이 필요한 것이다.

  > 즉, 문자열 입력값을 Primitive Type혹은 Object로 변환하기 위해서, 데이터 바인딩이 필요 !!

## PropertyEditor(스프링 3.0 이전)

  타입변환과 데이터 바인딩을 위해서 스프링 3.0 이전까지 DataBinder가 변환 작업을 사용하던 인터페이스는 `PropertyEditor`였습니다.

  데이터 바인딩용 클래스를 만들기 위해서는 PropertyEditor를 구현한 `PropertyEditorSupport` 클래스를 상속받아야 합니다.

  PropertyEditor는 한가지 큰 약점이 있습니다. 그것은 Thread-safe하지 않다는 것입니다. 아주 잠깐이지만 __인스턴스 변수 안에 value를 저장해 두고 그것을 다시 읽어오도록 되어있습니다.__ 원래 GUI툴에서 프로퍼티 뷰와 실제 오브젝트의 프로퍼티의 값을 전환하기 위해서 설계한 것이기 때문에 구지 쓰레드쎄이프하게 만들어지지 않은 것입니다.

  따라서 모든 PropertyEditor는 순차적으로 사용되는 것이 보장되지 않는다면 반드시 쓰레드마다 새 인스턴스를 만들어서 써야 한다. __initBinder 등에서 에디터를 등록할 때 new로 새로운 인스턴스를 만드는 것이 필수입니다.__

  - EventVO

  ```java
public class Event {

  Integer id;
  String title;

  public Event(Integer id) {
    this.id = id;
  }

  public Event(Integer id, String title) {
    this.id = id;
    this.title = title;
  }

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }
}
  ```

  - 인스턴스 변수 안에 value를 저장해 두고 그것을 다시 읽어오는 방식

  ```java
public class EventEditor extends PropertyEditorSupport {

  @Override
  public String getAsText() {
    Event event = (Event)getValue();
    return event.id.toString();
  }

  @Override
  public void setAsText(String text) throws IllegalArgumentException {
    setValue(new Event(Integer.parseInt(text)));
  }
}
  ```

  setAsText는 사용자가 입력한 데이터를 Event 객체로 변환해 주는 역할을 합니다. 만일 /event/1 이라는 요청이 올 시 1이라는 데이터를 Event로 변환할 때 위 setAsText의 메서드가 호출되어 사용자 데이터를 변환합니다.

  getAsText는 프로퍼티가 받은 객체를 getValue 메서드를 통해 가져오고, 값은 객체에 잠시 저장해서, 이 Event 객체를 문자열 정보로 변환해서 반환하는 역할을 합니다.

  - InitBinder에서 에디터 등록 시 new를 사용하여 새로운 객체를 만듬

  ```java
  @RestController
  public class EventController {

      @InitBinder
      public void init(WebDataBinder webDataBinder){
          webDataBinder.registerCustomEditor(Event.class, new EventEditor());
      }

      @GetMapping("/event/{event}")
      public String getEvent(@PathVariable Event event){
          System.out.println(event);
          return event.getId().toString();
      }
  }
  ```

  조심해야할 것은 EventEditor의 getValue와 setValue 메서드는 thread-safe하지 않습니다. 따라서 __절대 스프링 빈으로 등록해서 쓰면 안됩니다.__

  위의 EventEditor는 빈으로 사용하지 않고 @InitBinder 어노테이션을 통해 컨트롤러에 데이터 바인더로서 등록할 수 있습니다.

### 구현체

  > org.springframework.validation.DataBinder
  >
  > java.beans.PropertyEditor

### @InitBinder와 WebDataBinder

  @InitBinder가 하는 역할은 크게 2가지가 있습니다.

  기본적으로 @InitBinder가 등록된 메서드는 컨트롤러 내에서 다음과 같은 순서로 동작합니다.

  __@InitBinder가 등록된 메서드는 해당 컨트롤러 내에서만 작동합니다. 또한 @InitBinder는 메서드 위에 @ModelAttribute가 등록된 메서드보다 먼저 동작합니다. 즉, 컨트롤러내에서 @InitBinder 메서드, @ModelAttribute 메서드, HandlerMethod가 있을 경우 @InitBinder - @ModelAttribute - HandlerMethod 순서로 동작합니다.__

#### 역할1. Validation과 같이 사용되는 경우

  @InitBinder란 Spring Validator를 사용 시 @Valid Annotation으로 __검증이 필요한 객체를 가져오기 전에 수행할 method를 지정__ 해주는 Annotation 역할을 합니다.

  예를들어 고객정보를 입력하는 form이 있고, 고객 정보 입력시 앞, 뒤에 white space("")가 있는경우, 이를 제거 하고 Validation을 하기 위해서는
  검증을 거치기 전에 먼저 수행되어야할 작업(white space 제거)을 메서드로 만들어야 합니다.

  ```java
@Controller
@RequestMapping("/customer")
public class CustomerController {

	// input 스트링으로 들어오는 String 데이터들의 white space를 trim해주는 역할을 한다.
	// 모든 요청이 들어올때마다 해당 method를 거침 (node의 middleware 같은 것 )
	@InitBinder
	public void InitBinder(WebDataBinder dataBinder) {
		StringTrimmerEditor stringTrimmerEditor = new StringTrimmerEditor(true);
		dataBinder.registerCustomEditor(String.class, stringTrimmerEditor);
	}


	@RequestMapping("/showForm")
	public String showForm(Model model) {

		model.addAttribute("customer", new Customer());

		return "customer-form";
	}

	@RequestMapping("/processForm")
	public String processForm(@Valid @ModelAttribute("customer") Customer customer, BindingResult theBindingReuslt) {
		System.out.println("customer lastname | " + customer.getLastName() + " |");
		System.out.println("BindingResult | " + theBindingReuslt + " | ");
		System.out.println("\n\n");
		if( theBindingReuslt.hasErrors()) {
			return "customer-form";
		}
		return "customer-confirmation";
	}
}
  ```

  폼에서 white space (" ")만 입력하고 submit을 눌러보면, 원하는 대로 validation 체크가 잘 된 것을 확인할 수 있습니다.

#### 역할2. WebDataBinder에 PropertyEditor를 등록하기 위한 초기화 메서드 지정

  @InitBinder가 붙은 메서드는 메서드 파라미터를 바인딩하기 전에 자동으로 호출됩니다.

  > 컨트롤러 메서드에서 바인딩이 일어나는 경우 !!
  >
  > @Controller 메소드를 호출해 줄 책임이 있는 AnnotationMethodHandlerAdapter는 @RequestParam 이나 @ModelAttribute, @PathVariable 등 처럼 HTTP 요청을 파라미터 변수에 바인딩해주는 작업이 필요한 애노테이션을 만나면 먼저 WebDataBinder를 만든다.

  WebDataBinder는 여러 가지 기능을 제공하는데, 그 중에 __HTTP 요청으로부터 가져온 문자열을 파라미터 타입의 오브젝트로 변환하는 기능도 포함되어 있다.__ 물론 이 변환 작업은 PropertyEditor를 이용합니다. 따라서 개발자가 직접 만든 커스텀 프러퍼티 에디터를 @RequestParam과 같은 메소드 파라미터 바인딩에 적용하려면 바로 이 WebDataBinder에 프로퍼티 에디터를 직접 등록해줘야 합니다.

  문제는 WebDataBinder는 AnnotationMethodHandlerAdapte 가 복잡한 과정을 통해 메소드 파라미터와 애노테이션 등을 분석하고 바인딩을 진행하는 과정 내부에서 만들어지기 때문에 외부로 직접 노출되지 않는다는 점이다. 그렇다면 어떻게 이 WebDataBinder에 프로퍼티 에디터를 등록해줄 수 있을까? 이때는 스프링이 특별히 제공하는 `WebDataBinder 초기화 메서드`를 이용해야 합니다.

  WebDataBinder 초기화 메서드에는 @InitBinder를 지정해야하며, __@InitBinder는 스프링의 Default PropertyEditor만 갖고 있는 WebDataBinder에 커스텀 프로퍼티 에디터(Custom PropertyEditor)를 추가할 수 있는 기회를 제공해 줍니다.__ `registerCustomerEditor()` 는 프로퍼티 에디터를 적용할 타입과 프로퍼티 에디터 오브젝트를 넣어서 WebDataBinder에 새로운 프로퍼티 에디터를 추가해 주는 메소드다.

  > Summary !!
  >
  > WebDataBinder는 HTTP 요청 (ex) @RequesMapping("{type}"))으로부터 가져온 문자열을 파라미터 타입의 오브젝트로 변환하는 기능을 담당하는데(String to Object), 변환 하기 위해서 PropertyEditor를 사용합니다. WebDataBinder에 PropertyEditor를 등록하는 방법은 @InitBinder를 적용한 메서드(즉, WebDataBinder 초기화 메서드)를 만들어서 사용해야 합니다.

  ```java
@InitBinder
public void initBinder(WebDataBinder dataBinder) {
    dataBinder.registerCustomEditor(Level.class, new LevelPropertyEditor());
}
  ```

  @InitBinder 가 붙은 initBinder() 메소드는 메소드 파라미터를 바인딩하기 전에 자동으로 호출됩니다. 그래서 스프링의 디폴트 프로퍼티 에디터만 갖고 있는 WebDataBinder 에 커스텀 프로퍼티 에디터를 추가할 수 있는 기회를 제공해 줍니다. `registerCustomerEditor()` 는 프로퍼티 에디터를 적용할 타입과 프로퍼티 에디터 오브젝트를 넣어서 WebDataBinder 에 새로운 프로퍼티 에디터를 추가해 주는 메소드입니다. 즉, new LevelPropertyEditor()는 커스텀 프로퍼티 에디터 입니다.

  > @InitBinder에 의해 등록된 커스텀 에디터는 같은 컨트롤러의 메소드에서 HTTP 요청을 바인딩하는 모든 작업에 적용됩니다.

  __WebDataBinder의 바인딩 적용 대상은, @RequestParam 파라미터, @CookieValue 파라미터, @RequestHeader 파라미터, @PathVariable 파라미터, @ModelAttribute 파라미터의 프로퍼티입니다.__

  따라서 이런 요청 정보는 프로퍼티 에디터를 통해 적절한 타입 변환을 기대할 수 있다면 String 대신 다른 타입으로 메소드 파라미터를 선언해 두는 게 좋습니다. 컨트롤러 메소드 내에서 지저분한 변환 과정을 다시 거칠 필요가 없기 때문입니다.

### WebDataBinder에 커스텀 프로퍼티 에디터를 등록하는 방법

  WebDataBinder에 커스텀 프로퍼티 에디터를 등록하는 방법은 2가지가 있습니다.

#### 특정 타입에 무조건 적용

  위의 예처럼 적용 타입과 프로퍼티 에디터 두 개를 파라미터로 받는 registerCustomerEditor()를 사용해 프로퍼티 에디터를 등록했다면, 해당 타입을 가진 바인딩 대상이 나오면 항상 프로퍼티 에디터가 적용됩니다. 디폴트 프로퍼티 에디터에서는 지원하지 않는 타입이라면 기본적으로 이 방식을 사용하는 것이 적절합니다.

#### 특정 이름의 프로퍼티에만 적용

  두 번째 등록 방법은 타입과 프로퍼티 에디터 오브젝트 외에, 추가로 적용할 프로퍼티 이름을 지정하는 것입니다. 따라서 같은 타입이지만 프로퍼티 이름이 일치하지 않는 경우에는 등록한 커스텀 프로퍼티 에디터가 적용되지 않습니다.

  프로퍼티 이름이 필요하므로 @RequestParam 과 같은 단일 파라미터 바인딩에는 적용되지 않는다. @ModelAttribute 로 지정된 모델 오브젝트의 프로퍼티 바인딩에 사용할 수 있습니다. 모델 오브젝트 안에 같은 타입의 프로퍼티가 여러 개 있다고 해도, 그중에서 프로퍼티 에디터를 등록할 때 지정한 이름과 일치하는 프로퍼티에만 프로퍼티 에디터가 적용됩니다.

  그렇다면 이렇게 프로퍼티 이름까지 직접 지정해서 제한적으로 커스텀 프로퍼티 에디터를 적용해야 할 경우는 언제일까? 또, 프로퍼티 이름이 일치하지 않아서 커스텀 프로퍼티 에디터가 적용되지 않는 파라미터가 있다면 그 파라미터 바인딩 작업에서는 예외가 발생하지 않을까?

  이름이 포함된 프로퍼티 에디터의 등록은 이미 프로퍼티 에디터가 존재하는 경우에 사용하기 적합합니다. __WebDataBinder 는 바인딩 작업 시 커스텀 프로퍼티 에디터를 먼저 적용해보고 적절한 프로퍼티 에디터가 없으면 그 때 디폴트 프로퍼티 에디터를 사용합니다. 즉, 커스텀 프로퍼티 에디터가 우선순위를 갖습니다.__

  이미 스프링의 디폴트 프로퍼티 에디터에서 지원하는 int 타입에 대해 커스텀 프로퍼티 에디터를 만든다고 생각해 봅시다. 단순히 문자열을 int 타입으로 변환하는 것이 전부가 아니라 부가적인 조건을 부여해줄 필요가 있기 때문입니다. 아래 코드는 int 타입을 지원하는 커스텀 프로퍼티 에디터의 예입니다. 이름 그대로 최소값과 최대값을 지정할 수 있고, 이 범위에 포함되지 않는 값이 있으면 강제로 최소값이나 최대값으로 변경합니다. 다음은 이렇게 만들어진 int 타입에 대한 커스텀 프로퍼티 에디터입니다.

  ```java
import java.beans.PropertyEditorSupport;
public class MinMaxPropertyEditor extends PropertyEditorSupport {
  private int min;
  private int max;

  public MinMaxPropertyEditor(int min, int max) {
    this.min = min;
    this.max = max;
  }

  public String getAsText() {
    return String.valueOf((Integer)getValue());
  }

  public void setAsText(String text) throws IllegalArgumentException {
    Integer value = Integer.parseInt(text);
    if(value < min)  value = min;
    else if (value > max) value = max;
    setValue(value);
  }
}
  ```

  다음과 같이 두 개의 int 타입을 갖는 오브젝트가 있습니다.

  ```java
public class Member {
    int id;
    int age;
    ....
}
  ```

  컨트롤러 메소드는 다음과 같이 Member 타입의 파라미터에 HTTP 요청 파라미터를 바인딩해주도록 @ModelAttribute로 선언합니다.

  ```java
  @RequestMapping("/add")
  public void add(@ModelAttribute Member member) { ... }
  ```

  별도의 프로퍼티 에디터를 등록하지 않았다고 하면, URL 이 /add?id=1000&age=1000 이라고 주어졌을 때 디폴트로 등록된 int 타입의 프로퍼티 에디터가 동작해서 Member 오브젝트의 id, age 에 각각 1000 이라는 값을 넣어줄 것입니다. 그런데 age 는 단순 일련번호인 id 와 달리 사람의 나이를 나타내므로 int 타입이긴 하지만 적절한 범위 안의 값만 사용해야 합니다. 그래서 앞에서 만든 MinMaxPropertyEditor 를 등록해버리면 id 에도 적용되는 문제가 발생합니다. 커스텀 프로퍼티 에디터는 디폴트 프로퍼티 에디터보다 우선하기 때문에 모든 int 타입의 파라미터에 다 MinMaxPropertyEditor 가 적용됩니다.

  그래서 MinMaxPropertyEditor 는 int 타입의 프로퍼티 중에서도 이름이 age 인 경우에만 적용하도록 만들어줘야 합니다. 이를 위해 다음과 같이 프로퍼티 이름을 함께 지정하는 커스텀 프로퍼티 에디터 등록방식을 사용해야 합니다.

  ```java
@InitBinder
public void initBinder(WebDataBinder dataBinder) {
    dataBinder.registerCustomEditor(int.class, "age", new MinMaxPropertyEditor(0, 200));

}
  ```

  이렇게 해주면 MinMaxPropertyEditor 는 Member 오브젝트의 int 타입 프로퍼티 중에서 age 라는 이름의 프로퍼티에만 적용된다. id 프로퍼티는 MinMaxPropertyEditor의 적용 대상이 되지 않으므로 디폴트로 등록된 int 타입의 프로퍼티 에디터에 의해 정수 값으로 변환해줄 것입니다.

  따라서 /add?id=1000&age=1000 URL 이 주어졌을 때 바인딩을 거쳐서 컨트롤러 메소드가 전달받는 member 오브젝트에는 id 프로퍼티에 1000, age 프로퍼티에는 최대값으로 설정한 200이 들어갑니다.

  @InitBinder 메소드의 파라미터로 WebDataBinder 외에 WebRequest 도 사용할 수 있다. WebRequest는 HttpServletRequest에 들어 있는 거의 모든 HTTP 요청 정보를 담고 있습니다. 따라서 GET 또는 POST 와 같은 요청 메소드에 따라 다른 방식으로 WebDataBinder 에 프로퍼티 에디터등을 설정하는데 활용할 수 있습니다.

### WebBindingInitializer

  @InitBinder 메서드에서 WebDataBinder에 추가한 커스텀 프로퍼티 에디터는 메서드가 있는 컨트롤러 클래스 안에서만 동작합니다.

  Level 타입의 바인딩이 애플리케이션 전반에 걸쳐 폭넗게 필요한 경우라면,  @InitBinder 메소드를 컨트롤러마다 추가하고 LevelPropertyEditor 를 등록하는 코드를 반복적으로 추가해 줘야 합니다. 같은 컨트롤러내에서는 모든 컨트롤러 메소드에 한 번에 적용되긴 하지만, 컨트롤러 클래스가 다를 경우 다시 등록해주는 코드를 넣어야 한다면 매우 번거로운 일입니다.

  모든 컨트롤러에 적용해도 될 만큼 많은 곳에서 필요한 프로퍼티 에디터라면 등록하는 방법을 달리해서 한 번에 모든 컨트롤러에 적용하는 편이 좋습니다. 이런 용도로 만들어진 `WebBindingInitializer`를 이용하면 됩니다.

  먼저 WebBindingInitializer 인터페이스를 구현해서 다음과 같은 클래스를 작성한다. 구현할 메소드는 @InitBinder를 적용했던 메소드와 비슷합니다.

  ```java
public class MyWebBindingInitializer implements WebBindingInitializer {
    @Override
    public void initBinder(WebDataBinder binder, WebRequest request) {
        binder.registerCustomEditor(Level.class, new LevelPropertyEditor());
    }
}
  ```

  WebBindingInitializer 를 구현해서 만든 클래스를 빈으로 등록하고 @Controller 를 담당하는 어댑터 핸들러인 AnnotationMethodHandlerAdapter의 webBindingInitializer 프로퍼티에 DI 해줍니다. 이 프로퍼티 설정을 위해서는 AnnotationMethodHandlerAdapter도 빈으로 직접 등록해야 합니다. WebBindingInitializer 는 다른 곳에서 참조할 일은 없으므로 다음과 같이 내부 빈 설정방식을 이용해 등록해도 좋습니다

  ```xml
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
    <property name="webBindingInitializer">
        <bean class="com.happyhouse.rednics.tutorial.....MyWebBindingInitializer"/>
    </property>
</bean>
  ```

  이렇게 AnnotationMethodHandlerAdapter 레벨에 바인딩을 초기화하는 빈을 등록해 주면 이 빈의 initBinder() 메서드는 모든 컨트롤러의 모든 바인딩 작업에 일괄 적용된다. 따라서 LevelPropertyEditor 를 매번 @InitBinder 메서드를 통해 컨트롤러마다 등록해줄 필요가 없게 됩니다.

  물론 Level 타입의 바인딩이 필요 없는 컨트롤러에도 매번 적용되는 낭비가 있기는 합니다. 하지만 프로퍼티 에디터는 워낙 간단한 클래스이므로 자주 생성돼도 별 문제가 되지는 않습니다. 적절한 기준을 정해두고 일정 비율 이상의 컨트롤러에서 필요로 하는 커스텀 프로퍼티 에디터는 WebBindingInitializer 를 이용해 등록하는 방법을 검토해 보자.

### ProtoType Bean PropertyEditor

  지금까지 등장한 모든 커스트 프로퍼티 에디터 등록 코드를 잘 살펴보면, 모든 메소드에서 매번 new 키워드를 사용해 프로퍼티 에디터 오브젝트를 새로 만들고 있음을 알 수 있다. @InitBinder 메소드나 WebBindingInitializer 메소드에서도 마찬가지다. 프로퍼티 바인딩을 할 때마다 매번 새로운 프로퍼티 에디터 오브젝트를 만들어 사용한다는 것입니다.

  오브젝트를 매번 새로 만드는 대신 프로퍼티 에디터를 싱글톤 빈으로 등록해두고 이를 공유해서 쓸 수는 없을까? 안타깝지만 프로퍼티 에디터는 싱글톤 빈으로 등록될 수 없습니다. 프로퍼티 에디터를 테스트하기 위해 작성했던 코드를 다시 잘 살펴보면 모든 변환 과정은 하나의 메소드가 아니라 두 개의 메소드가 사용됨을 알 수 있습니다. 프로퍼티 에디터에 setValue() 를 이용해 오브젝트를 일단 넣은 후에 getAsText() 로 변환된 문자열을 가져오는 식입니다.

  프로퍼티 에디터에 의해 타입이 변경되는 오브젝트는 __한 번은 프로퍼티 오브젝트 내부에 저장된다는 사실을 알 수 있다.__ 따라서 프로퍼티 에디터는 짤은 시간이자만 `상태`를 갖고 있게 됩니다. __이 때문에 프로퍼티 에디터는 멀티스레드 환경에서 싱글톤으로 만들어 여러 오브젝트가 공유해서 사용하면 안된다.__ 자칫하면 다른 스레드에서 변환하는 값으로 덮어쓰일 수 있습니다. 그래서 바인딩을 할 때마다 매번 `initBinder()` 같은 초기화 메소드를 호출해서 새로운 프로퍼티 에디터 오브젝트를 만들어 사용하는 것입니다.

  프로퍼티 에디터는 싱글톤 빈으로 등록해서 공유해서는 안된다는 사실을 꼭 기억하기 바랍니다. 따라서 프로퍼티 에디터는 지금까지 등록했던 코드에서처럼 매번 new 키워드로 새로 오브젝트를 만들어 사용해야 합니다.

  그런데 프로퍼티 에디터가 다른 스프링 빈을 참조해야 한다면 어떨까? 다른 빈을 참조해서 DI 받으려면 자신도 스프링 빈으로 등록돼야 하는 것이 원칙입니다. 그렇다면 프로퍼티 에디터를 빈으로 등록해야 하는데, 설명한 것처럼 프로퍼티 에디터는 싱글톤빈으로 등록해서 공유할 수 없습니다. 그래서 이렇게 프로퍼티 에디터가 다른 빈을 DI 받을 수 있도록 자신도 빈으로서 등록되면서 동시에 매번 새로운 오브젝트를 만들어서 사용할 수 있으려면 프로토타입 빈으로 만들어져야 합니다. 프로토타입 빈은 매번 빈 오브젝트를 요청해서 새로운 오브젝트를 가져올 수 있으면서 DI 도 가능하니 이런 경우에 적격입니다.

  어떤 경우에 프로퍼티 에디터가 다른 빈을 참조해야 할까? 여러 가지 시나리오를 생각해 볼 수 있습니다. 그중에서 HTTP 요청 파라미터로 도메인 오브젝트의 ID 를 제공받으면 이를 바인딩하는 과정에서 ID에 해당되는 실제 도메인 오브젝트로 만들어 줄 필요가 있는 경우를 생각해 볼 수 있습니다. 즉, ID를 가지고 DB에 도메인 오브젝트의 전체를 가져오는 과정이 필요하다는 뜻입니다.

  물론 ID를 int 타입등으로 그냥 받아서 서비스 계층으로 전달한 뒤에 거기서 DAO를 통해 해당 ID에 대한 도메인 오브젝트를 가져오게 할 수도 있습니다. 하지만 좀 더 객체 지향적인 서비스 계층을 설계한다면 도메인 오브젝트의 ID 값보다는 도메인 오브젝트 자체를 전달하는 비즈니스 로직 메소드를 작성하는 편이 낫다.

  또한 폼에서 사용되는 모델 오브젝트의 특정 프로퍼티가 다른 도메인 오브젝트 타입인 경우가 많다. 이런 경우의 폼 데이터 바인딩 방법을 생각해보자.

  다음과 같은 프로퍼티를 갖는 User 클래스가 있다고 해보겠습니다.

  ```java
  public class User {

    private int id;
    private String name;
    private Code userType;

}
  ```

  세 번째 프로퍼티인 userType은 그 타입이 Code입니다. 이 Code가 이늄처럼 프로퍼티 에디터를 만들어서 처리할 수 있는 것이라면 문제가 되지 않을 것입니다. 하지만 Code 도 DB에 저장되는 독립적인 도메인 오브젝트라면 어떨까? 애플리케이션에서 다이나믹하게 사용자 타입을 추가하거나 삭제할 수 있도록, 이렇게 DB의 코드 테이블을 활용하는 건 매우 흔한일입니다. 일단 Code 는 id와 name이라는 두 가지 프로퍼티만 가진 간단한 도메인 오브젝트로 가정하자.

  보통 사용자 정보를 수정하는 폼에는 userType 에 해당되는 코드의 내용이 출력된 <select> 박스가 표시되고 그 중에 하나를 선택하게 됩니다. 폼을 출력하는 컨트롤러에는 다음과 같이 @ModelAttribute 메소드를 이용해서 userType 에 적용될 수 있는 코드 목록을 참조정보 모델로 제공하는 메소드가 만들어질 것입니다.

  ```java
@ModelAttribute("userTypes")
public List<Code> userTypes() {
  return codeService.findCodesByCodeCategory(CodeCategory.USERTYPE);
}
  ```

  화면에 출력된 선택박스에는 다음과 같이 Code의 name 과 값에 의미 있는 내용이 들어 있겠지만 폼을 전송할 때 사용되는 value 에는 Code의 id값만이 들어 있을 것입니다.

  ```html
<select name="userType">
  <option value="1">관리자</option>
  <option value="2">회원</option>
  <option value="3">손님</option>
</select>
  ```

  이 중에 "관리자"를 선택하고 폼을 서브밋하면 서버에 전달되는 HTTP 요청에는 userType 이라는 이름의 파라미터에 1이라는 값이 들어 있을 것입니다. 이 폼의 모델은 User일 것이고, 따라서 폼의 수정을 처리하는 컨트롤러 메소드는 다음과 같이 정의되어 있을 것입니다.

  ```java
  @RequestMapping("/add")
  public void add(@ModelAttribute User user) { ... }
  ```

  User 의 id, name 의 프로퍼티에 해당하는 HTTP 요청 파라미터는 별 문제 없이 간단히 바인딩됩니다. 하지만 userType 프로퍼티에 전달되는 1이라는 값은 간단히 Code 타입으로 전환될 수 없습니다. 프로퍼티 이름을 따라서 값을 넣으려면 setUserType(Code userType)이라는 수정자 메소드를 만날텐데, 스프링은 1이라는 값을 Code 오브젝트로 만들 방법을 알 수가 없습니다. 따라서 어떻게든 프로퍼티 에디터를 사용하는 방식이 필요할 것 같다.

  __이렇게 폼의 파라미터가 모델의 프로퍼티에 바인딩될 때 단순 타입이 아닌 경우 어떻게 이를 바인딩할 수 있는지, 몇 가지 방법을 살펴봅시다.__

#### 별도의 단순타입 필드로 바인딩하는 방법

  첫 번째 방법은 가장 단순하다. 대신 서비스 계층이나 컨트롤러에서 추가적으로 해줘야 할 작업이 있습니. 이 방법은 Code userType 프로퍼티로 직접 바인딩하는 대신 참조 ID 값을 저장할 수 있는 별도의 임시 프로퍼티를 만들고 이 프로퍼티로 값을 받는 것입니다.

  비즈니스 로직이나 DAO 에서는 직접 사용되지 않지만 웹 파라미터 바인딩을 위해 다음과 같이 userTypeId 라는 프로퍼티를 User 클래스에 추가합니다.

  ```java
public class User {

  private int userTypeId;
  private Code userType;

}
  ```

  그리고 select 이름을 userTypeId로 바꿉니다.

  ```html
<select name="userTypeId">
  <option value="1">관리자</option>
  <option value="2">회원</option>
  <option value="3">손님</option>
</select>
  ```

  이렇게 바꿔주면 폼의 선택 박스를 통해 선택한 사용자 타입의 값은 userTypeId 라는 이름의 요청 파라미터로 전달되어 User 오브젝트의 userTypeId 프로퍼티에 들어갑니다. 물론 바인딩이 끝나도 Code 타입인 userType 프로퍼니는 비어 있거나 수정하기 전 상태의 값을 갖고 있을 것입니다.

  이렇게 전달받은 userTypeId 프로퍼티를 이용해 컨트롤러나 서비스 계층에서 적절한 Code 타입의 오브젝트로 변경해서 userType 프로퍼티를 설정해주면 된다. 컨트롤러라면 다음과 같이 작성할 수 있습니다.

  ```java
@RequestMapping("/add")
public void add(@ModelAttribute User user) {
    user.setUserType(codeService.getCode(user.getUserTypeId()));
}
  ```

  또는 이런 컨트롤러의 바인딩 결과를 알고 있는, 강한 결합도를 가진 서비스 계층 메소드에서 같은 작업을 먼저 해줄 수도 있다. 어떻게든 userTypeId로 받은 Code의 id에 해당하는 값을 이용해 DB에서 Code 오브젝트를 가져와서 userType 프로퍼티에 넣어줘야 합니다.

  이 방식을 적용하면 폼의 결과를 바인딩받은 User 는 그 자체로 완벽하게 프로퍼티 정보를 담고 있는 오브젝트가 되고, 서비스 계층의 비즈니스 로직이나 DAO 에서 DB에 저장하기 위해 Code 테이블의 외래키를 가져올 때도 유용하게 쓸 수 있습니다.

  가장 간단한 방법이지만 그만큼 단점이 있습니다. 매번 Code 정보를 DB에서 가져와야 하는 부담이 있습니다. 물런 JPA나 하이버네이트를 사용한다면 Code 같은 참조 위주의 도메인 오브젝트는 메모리 캐시에 저장해 두고 이를 활용하도록 설정할 수 있기 때문에 성능 자체에는 별 부담이 되지 않을 수 있긴 합니다. 설령 DB를 한 번도 조회하지 않더라도 프로퍼티의 값이 모두 잘 들어가 있는 User 오브젝트를 사용할 수 있으므로 서비스 계층의 코드를 작성하기가 매우 편해집니다. 사실 ID 와 같은 기본키로 코드를 조회하는 정도는 DB 최적화만으로도 성능에 별 지장을 주지 않도록 만들 수 있습니다.

  이 방식의 가장 큰 단점은 매번 컨트롤러나 서비스 계층 코드에서 위와 같이 id에 해당하는 임시 프로퍼티 값을 이용해서 도메인 오브젝트 타입의 프로퍼티를 설정해주는 작업을 해야 한다는 것입니다. 이런 식의 오브젝트 타입 프로퍼티 개수가 많아지면 코드가 매우 지저분해지기 쉽습니다. 실수로 빼먹을 위험도 있습니다.

  또 한가지 문제는 User 오브젝트에 굳이 필요하지 않은 userTypeId 와 같은 임시 저장용 프로퍼티가 추가돼야 한다는 점입니다. 도메인 오브젝트의 프로퍼티 구성이 깔끔하지 않은 건 별로 좋아 보이지 않습니다. 이 문제를 피하려면 차라리 @RequestParam int userTypeId 라고 파라미터를 별도로 추가해서 ID 값을 받는 편이 User 라는 중요한 도메인 오브젝트를 깔끔하게 유지할 수 있으므로 더 낫다고 볼 수 있습니다. 대신 비슷한 종류의 프로퍼티가 많아진다면 컨트롤러 메소드 선언이 길어질 위험이 있긴 합니다.

#### 모조 오브젝트 프로퍼티 에디터

  두 번째 방법은 모조 프로퍼티 에디터를 만들어 사용합니다. userType 이라는 이름으로 전달되는 1,2,3 과 같은 id 값을 Code 오브젝트로 변환해주는 프로퍼티 에디터를 만드는 것입니다. 단, 이 프로퍼티 에디터가 변환해주는 Code 오브젝트는 아직 id 값만 가진 불완전한 오브젝트입니다. 이렇게 id 만 가진 오브젝트를 `모조 오브젝트(fake object)` 라고 하고, 이런 오브젝트를 만드는 프로퍼티 에디터를 모조 프로퍼티 에디터라고 부릅니다.

  ```java
public class FakeCodePropertyEditor extends PropertyEditorSupport {
  @Override
  public void setAsText(String text) throws IllegalArgumentException {
    Code code = new Code();
    code.setId(Integer.parseInt(text));
    setValue(code);
  }

  @Override
  public String getAsText() {
    return String.valueOf(((Code)getValue()).getId());
  }
}
  ```

  이제 FakeCodePropertyEditor 를 다음과 같이 Code 타입에 대한 커스텀 프로퍼티 에디터로 등록해 주기만 하면 됩니다.

  ```java
@InitBinder
public void initBinder(WebDataBinder dataBinder) {
    dataBinder.registerCustomEditor(Code.class, new FakeCodePropertyEditor());
}
  ```

  폼의 select의 id는 Code 타입 프로퍼티 이름인 userType으로 다시 변경해 둡니다.

  폼을 서브밋해보면 add() 메소드의 파라미터로 전달되는 User 오브젝트에는 id, name 뿐아니라 Code 타입의 userType 오브젝트도 들어 있음을 확인할 수 있습니다.

  그런데 문제는 이 userType 프로퍼티에 들어간 Code 오브젝트는 사실 id 를 제외한 나머지가 다 null 인 비정상적인 오브젝트라는 점입니다. 이런 Code 오브젝트를 userType 프로퍼티에 담아서 User 오브젝트를 서비스 계층으로 보낸다면 문제가 되지 않을까? 예를 들어 비즈니스 로직을 처리하는 중에 현재 userType 에 대한 코드 이름을 꺼내려고 한다든지, 수정된 user 를 업데이트하면서 userType 에 해당하는 Code 오브젝트도 같이 업데이트하면 예상치 못한 문제가 발생할 것입니다.

  그래서 모조 프로퍼티 에디터를 사용하는 건 조금 위험한 발상입니다. 그럼에도 이 방식은 잘 활용하면 꽤나 유용합니다. User 타입 정보를 업데이트 하는 상황을 생각해 보자. User 클래스에는 사용자 타입 정보가 Code 타입의 오브젝트로 담겨 있다. 객체 지향적인 도메인 모델 방식을 사용했기 때문입니다. 하지만 DB의 User 테이블에 저장할 때는 userType 프로퍼티의 Code 오브젝트를 통째로 저장할 필요가 없습니다. 단지 Code 테이블의 레코드 하나를 참조하면 되므로, User 테이블에는 usertype_code_id 와 같은 컬럼에 Code 테이블의 키 값이 외래키로 들어갈 뿐입니다. Code 테이블의 기본 키 값에 해당하는 usertype_code_id 컬럼의 값만 바꿔주면 Code 를 통째로 바꾸는 것과 같은 효과가 나는 것입니다.

  폼에서 수정한 사용자 정보를 User 오브젝트에 바인딩해서 DAO 에서 업데이트할 때는 이렇게 Code 와 같이 참조하는 오브젝트의 id 값만 사용하는 경우가 대부분입니다. 사용자 정보를 업데이트하면서 Code 테이블도 같이 업데이트하는 경우는 거의 없습니다.

  엔티티 오브젝트를 직접 DB 에 반영해주는 하이버네이트나 JPA 같은 ORM 에서도 마찬가지다. User 엔티티를 업데이트할 때 userType 처럼 Code 타입의 다른 엔티티가 연결되어 있으면 역시 Code의 id 값만 가져와서 DB에 저장할 때 활용합니다. 항상 User 와 userType 프로퍼티로 연결된 Code 오브젝트를 동시에 업데이트하도록 특별한 설정을 해놓지 않았다면 Code 에서 참고하는 것은 id 값뿐입니다. ORM 도 결국 DB의 User 테이블에 정보를 저장하기 때문에 외래키 저장 방식은 다르지 않습니다.

  그래서 이런 식으로 폼에서 수정하거나 등록한 사용자 정보를 단순히 저장하는 것이 목적이라면 모조 오브젝트 방식이 유용합니다. 번거롭게 DB를 읽어서 userType 오브젝트를 채워주는 코드를 넣을 필요도 없고, 도메인 오브젝트에 임시로 참조 키값을 저장할 프로퍼티를 추가하지 않아도 됩니다.

  단, 서비스 계층에서 userType 에 연결되어 있는 Code 오브젝트를 다른 용도로 활용하거나, 강제로 업데이트를 하지 않아야 한다는 제한이 있습니다. 결국 서비스 계층의 일부 메소드는 자신에게 전달되는 User 오브젝트의 userType 프로퍼티는 모조 오브젝트라는 사실을 인식하고 있어야 한다는 부담이 있습니다. 혹시 실수로 누군가 사용자 정보 업데이트를 담당하는 서비스 계층 메소드에서 userType 에 연결된 Code 오브젝트의 name 프로퍼티를 가져다 사용하려고 하면 위험합니다.

  그래서 실수로 모조 오브젝트를 사용하는 일을 미연에 방지하고 싶다면, 다음과 같이 Code 를 확장한 FakeCode 클래스를 만들어 적용하는 방법이 있다.

  ```java
public class FakeCode extends Code {
  public String getName() {
    throw new UnsupportedOperationException();
  }

  public void setName(String name) {
    throw new UnsupportedOperationException();
  }
}
  ```

  FakeCode 클래스는 모조 오브젝트를 사용할 때 필요한 id 값을 가져오는 메소드외의 모든 메소드를 오버라이드 해서 UnsupportedOperationException 예외를 던지도록 만든 것입니다. 그리고 프로퍼티 에디터에서 Code 대신 FakeCode 오브젝트를 돌려주도록 만듭니다.

  ```java
public class FakeCodePropertyEditor extends PropertyEditorSupport {
  @Override
  public void setAsText(String text) throws IllegalArgumentException {
    Code code = new FakeCode();
    code.setId(Integer.parseInt(text));
    setValue(code);
  }

  @Override
  public String getAsText() {
    return String.valueOf(((Code)getValue()).getId());
  }
}
  ```

  이렇게 만들어 두면 개발자가 깜빡하고 서비스 계층에서 userType 프로퍼티로부터 Code 오브젝트를 가져와 값이 들어 있지 않은 name과 같은 프로퍼티를 사용하려고 하는 순간 바로 지원되지 않는 기능이라는 UnsupportedOperationException 예외가 발생합니다. 적어도 실수로 모조 오브젝트를 가져다 참조하거나 업데이트하는 실수는 사전에 방지할 수 있게 해줍니다.

  모조 오브젝트는 도메인 오브젝트 중심의 아키텍처를 선호한다면 매우 매력적인 접근방법이다. 프로퍼티 에디터도 간단히 만들 수 있습니다. 도메인 오브젝트에 불필요한 userTypeId 같은 임시 저장용 프로퍼티를 추가할 필요도 없습니다. 컨트롤러나 서비스 계층에 Code 오브젝트를 일일이 불러오는 코드를 넣을 필요도 없습니다. 시나리오에 따라 도메인 오브젝트를 등록, 수정할 때 선택 박스를 통해 선택하는 레퍼런스 성격의 오브젝트에는 유용하게 쓸 수 있습니다.

#### 프로토타입 도메인 오브젝트 프로퍼티 에디터

  세 번째 방식도 두 번째와 마찬가지로 Code 타입의 프로퍼티 에디터를 적용합니다. 따라서 select의 아이디는 userType 이라고 지정해도 되고, 임시 id 저장용 userTypeId 등은 만들지 않아도 됩니다. 프로퍼티 에디터를 만들어서 select로부터 전달된느 ID 값을 프로퍼티 에디터를 통해 Code 오브젝트로 만들어 준다는 점에선 모조 오브젝트 프로퍼티 에디터 방식과 동일합니다. 하지만 도메인 오브젝트 프로퍼티 에디터에서는 id만 들어 있는 모조 오브젝트가 아닌 DB에서 읽어온 완전한 Code 오브젝트로 변환해준다는 점이 다릅니다. 이렇게 바인딩한 Code 타입 오브젝트는 모조 오브젝트처럼 다른 계층에서 사용할 때 제한을 받지 않습니다.

  이를 위해서 프로퍼티 에디터가 DB로부터 Code 오브젝트를 가져올 수 있어야 합니다. 프로퍼티 에디터가 CodeService나 CodeDao 같은 빈을 DI 받아서 사용해야 한다는 뜻입니다. 그러기 위해서는 프로퍼티 에디터 자체가 빈으로 등록되어야 합니다. 다른 빈을 DI 받는 오브젝트는 자신도 빈이어야 하기 때문입니다. 따라서 new CodePropertyEditor()와 같은 식으로 직접 오브젝트를 만들어서는 안됩니다. DI 받을 수 있도록 프로퍼티 에디터를 빈으로 등록해줘야 합니다. 단, 프로퍼티 에디터는 싱글톤 빈으로 만들어 공유하면 안되기 때문에 프로토타입 빈으로 만들어 매번 새로운 빈 오브젝트를 가져와 사용해야 합니다.

  프로토 타입 빈을 등록하고 이를 컨트롤러 같은 싱글톤 빈에서 사용하는 방법으로 몇 가지가 있지만 여기서는 그 중 `Provider`를  이용하는 방식을 이용해 봅시다.

  먼저 다음과 같이 CodePropertyEditor를 작성합니다. 빈으로 등록될 것이므로 @Autowired 등을 이용해 다른 빈을 참조할 수 있습니다. FakeCodePropertyEditor와 다른 점은 id를 받아서 Code 로 변환할 때 CodeService 를 이용해 Code 오브젝트를 가져온다는 것입니다.

  ```java
@Component
@Scope("prototype")
public class CodePropertyEditor extends PropertyEditorSupport {
    @Autowired
    private CodeService codeService;

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        setValue(codeService.getCode(Integer.parseInt(text)));
    }

    @Override
    public String getAsText() {
        return String.valueOf(((Code)getValue()).getId());
    }
}
  ```

  이제 CodePropertyEditor를 프로토타입 빈으로 등록하고 다음과 같이 UserController에서 사용할 수 있도록 javax.injext.Provider 를 이용해 선언해줍니다. 그리고 @InitBinder 메서드에서 매번 새로운 CodePropertyEditor 빈 오브젝트를 가져와서 등록해 주면 됩니다.

  ```java
public class UserController {
  @Inject
  private Provider<CodePropertyEditor> codeEditorProvider;

  @InitBinder
  public void initBinder(WebDataBinder dataBinder) {
    dataBinder.registerCustomEditor(Code.class, codeEditorProvider.get());
  }
}
  ```

  본격적으로 도메인 오브젝트 중심의 아키텍처를 사용하고 스프링의 이런 기능을 최대한 활용하려면 아무래도 도메인 오브젝트 단위로 퍼시스턴스 기능을 제공하는 JPA/하이버네이트 같은 ORM 이 SQL 을 직접 사용하는 JDBC/iBatis 보다 개발하기 싶고 성능을 최적화하기도 편리합니다.

### Custom Validator Object

  > [https://baekjungho.github.io/spring-vhibernate/](https://baekjungho.github.io/spring-vhibernate/)
  
## 참조

  > [http://toby.epril.com/?p=888](http://toby.epril.com/?p=888)
  >
  > [https://hsoh1990.github.io/2019/01/25/spring-core-binding/](https://hsoh1990.github.io/2019/01/25/spring-core-binding/)
  >
  > [https://engkimbs.tistory.com/733](https://engkimbs.tistory.com/733)
  >
  > [https://springsource.tistory.com/15](https://springsource.tistory.com/15)
