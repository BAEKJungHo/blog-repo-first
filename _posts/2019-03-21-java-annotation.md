---
title: "Annotation"
layout: post
category: Java
tags: [Java]
excerpt: "메타데이터(metadata) 역할을 하는 Annotation에 대해서 알아 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Annotation

  어노테이션은 자바소스코드에 추가하여 사용하는 메타데이터(metadata)라고 볼 수 있습니다.

  __즉, 소스코드 위에 작성하여 추가정보를 알려주는 역할입니다.__

  Java 5에서부터 추가되었으며, JDK 1.5 버전부터 사용이 가능합니다.

  자바에서 기본적으로 제공하는 어노테이션이 있으며 사용자가 직접 작성하여 사용할 수도 있는데
  이런 어노테이션을 `Custom Annotation(커스텀 어노테이션)`이라고 합니다.

  > Annotation은 클래스나 메소드, 필드위에 @override와 같이 표시하여 사용합니다.
  >
  > Annotation은 클래스를 만드는것처럼 이클립스 패키지 오른쪽 마우스 클릭 후
  >
  > New - Annotation을 클릭하면 됩니다.

  - 용도

  > 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공
  >
  > 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동 생성할 수 있도록 정보를 제공
  >
  > 실행(런타임)시 특정 기능을 실행하도록 정보를 제공

## Custom Annotation

  커스텀 어노테이션은 다음과 같은 방식으로 진행 됩니다.

  > 정의 - 사용 - 실행

  - 커스텀 어노테이션 Frame 만들기

  > public @interface CustomAnnotationName { ..... }

  위와같이 접근제한자를 `public`으로 두고 `@interface 키워드`를 사용하여 만듭니다.

  위에서 생성한 어노테이션을 __JVM 실행 시 감지할 수 있게 하기 위해서__ 는 커스텀 어노테이션 위에
  리플렉션(Reflection)을 사용해야하는데 키워드는 `@Retention` 어노테이션 입니다.

  리플렉션을 이용할때 어노테이션 유지정책을 사용해야하는데 대부분 RUNTIME을 사용합니다.

  ```java
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;

  @Retention(RetentionPolicy.RUNTIME)
  public @interface Anno {

  }
  ```

##  Bulit-in Annotation

  자바에서 기본으로 제공하는 어노테이션을 알아 보겠습니다.

  **Bulit-in Annotation**  

  |종류    |특징|
  |--------|----|
  |@override|  메소드 선언시 사용, 메소드 오버라이드가 제대로 되었는지 검사하는 역할|
  |@Deprecated|  이 메소드는 쓸모 없으니 사용하지 말라라는 의미, 사용시 컴파일 경고발생|
  |@SuppressWarnings| 컴파일 경고를 무시하는 역할|
  |@FunctionalInterface|  람다 함수등을 위한 인터페이스를 지정, 메소드가 없거나 두개 이상 되면 컴파일 오류가 납니다.|

  > @FunctionalInterface는 자바8부터 지원합니다.
  >
  > @SuppressWarnings는 문제가 없는데 컴파일러가 경고를 일으킬때, 경고를 보지 않기 위해 사용합니다.

## Meta Annotations

  메타 어노테이션은 어노테이션을 선언할때 사용합니다.

  **Meta Annotations**  

  |종류    |특징|
  |--------|----|
  |@Retention|  어떤 시점까지 어노테이션이 영향을 미치는지 결정합니다.|
  |@Documented|  문서에도 어노테이션의 정보가 표현됩니다.|
  |@Target| 어노테이션이 적용할 위치를 결정합니다.|
  |@Inherited|  이 어노테이션을 선언하면 자식클래스가 어노테이션을 상속 받을 수 있습니다.|
  |@Repeatable| 반복적으로 어노테이션을 선언 할 수있게 만듭니다.|

  **RetentionPolicy**

  |종류    |특징|
  |--------|----|
  |SOURCE|  소스상에서만 어노테이션 정보 유지|
  |CLASS|  컴파일된 바이트 코드 파일까지 어노테이션 정보 유지|
  |RUNTIME| 바이트 코드 파일까지 어노테이션 정보유지 및 리플렉션을 이용해 런타임시에 어노테이션 정보를 얻을 수 있습니다.|

  -----------------------------------------------------------------------------

  > 참고자료
  >
  > [https://jdm.kr/blog/216](https://jdm.kr/blog/216)
