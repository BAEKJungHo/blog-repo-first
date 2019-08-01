---
title: "Lombok(롬복)"
layout: post
category: Spring
tags: [Spring]
excerpt: "Lombok(롬복) 설치방법및 각종 어노테이션 사용법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Lombok(롬복) 설치방법및 각종 어노테이션 사용법에 대해서 배워 봅시다.

## 설치방법

### Dependency 설정

  - 프로젝트의 pom.xml에 lombok 의존성 추가 (필요한 버전이 있다면 버전을 표시)

  ```xml
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
  </dependency>
  ```

### 설치

  lombok.jar가 다운로드된 경로로 가서 __shift+우클릭 -> 여기서 명령 창 열기(W)__ 클릭 하면 cmd창이 뜹니다.
  lombok.jar를 실행하기 위하여 더블클릭 또는 __java -jar lombok-1.16.10.jar__ 입력

  - Installer 화면에서 Specify location...을 클릭

  ![l1](/images/posts/201907/l1.jpg)

  그리고 eclipse 폴더에서 실행파일을 선택한 후에 Install / Update 클릭

  ![l2](/images/posts/201907/l2.jpg)

  프로젝트에 lombok.jar를 추가하는 것을 잊지말고, eclipse 재시작을 해야하고, -vm parameter로 -vmargs -javaagent: lombok.jar가 추가 되었다는 메시지가 나옵니다. eclipse를 재시작 하고나서, `@Data` 어노테이션이 적용되는지 확인합니다.

## @Getter @Setter

  가장 많이 사용되는 어노테이션 입니다. 필드에 설정할 경우 해당 필드의 Getter와 Setter를 생성하며,
  클래스 위에 선언할 경우 클래스에 있는 모든 필드에 대한 Getter와 Setter를 생성합니다.

  > 주의할 점 !!
  >
  > Setter는 그 의도가 분명하지 않고 객체를 언제든지 변경할 수 있는 상태가 되어서 객체의 안전성이 보장받기 힘듭니다. 만약 email의 변경 기능이 제공 되지 않는다고 가정한다면 email 관련된 setter도 제공되지 않아야 안전합니다. 단순 안전함을 넘어서 해당 객체가 자기 자신을 가장 잘 표현하는 구조 즉 email의 변경 포인트를 제공하지 않음으로써 email 변경 기능이 없다는 것을 표현한다고 생각합니다.

## @Alias

  ```java
@Alias("popup")
public class XXXPopupVo { }
  ```

  위 처럼 alias를 지정할 경우 `<select id="countPopup" parameterType="popup" resultType="int">`와 같이 Mapper.xm에서 파라미터 타입의 Key값을 줄여 쓸 수 있습니다.

## @NonNull

  @NonNull 어노테이션을 붙이면 자동으로 null을 체크합니다.

  즉, 해당 변수가 null로 넘어온 경우, __NullPointerException__ 예외를 일으켜 줍니다.

## @Cleanup

  IO 처리나 JDBC 코딩을 할 때, try-catch-finally 문의 finally 절을 통해서 close() 메소드를 호출하는 게 여간 번거로운 일이 아니었는데, @Cleanup 어노테션을 사용하면 해당 자원이 자동으로 닫히는 것이 보장됩니다.

  ```java
  @Cleanup Connection con = DriverManager.getConnection(url, user, password);
  ```

## @SneakyThrows

  __Checked Exception__ 때문에 반드시 throws나 try-catch 구문을 통해서 번거롭게 명시적으로 예외 처리를 해줘야할 때 가 있습니다. 이럴 때, @SneakyThrows 어노테이션을 사용하면 명시적인 예외 처리를 생략할 수 있습니다.

## 생성자 자동생성 어노테이션

### @NoArgsConstructor

  @NoArgsConstructor 어노테이션은 파라미터가 없는 기본 생성자를 생성해줍니다.

### @AllArgsConstructor

  @AllArgsConstructor 어노테이션은 모든 필드 값을 파라미터로 받는 생성자를 만들어줍니다.

### @RequiredArgsConstructor

  @RequiredArgsConstructor 어노테이션은 `final`이나 `@NonNull`인 필드 값만 파라미터로 받는 생성자를 만들어줍니다.

## ToString 메서드 자동생성

  @ToString 어노테이션만 클래스에 붙여주면 자동으로 생성해줍니다. @ToString 어노테이션에
  `exclude` 속성을 사용하면, 특정 필드를 toString() 결과에서 제외시킬 수도 있습니다.

  ```java
@ToString(exclude = "password")
public class User {
  private String id;
  private String username;
  private String password;
}
  ```

  위 처럼 @ToString 어노테이션을 사용하고 아래처럼 출력만 하면, toString() 메서드처럼 출력됩니다.

  ```java
  User user = new User();
  user.setId("BAEKJH_GZ");
  user.setUsername("BAEKJH");
  System.out.println(user);
  ```

## @EqualsAndHashCode

  자바 빈을 만들 때 `equals`와 `hashCode` 메소드를 자주 오버라이딩 하는데 @EqualsAndHashCode 어노테이션을 사용하면 자동으로 이 메소드를 생성할 수 있습니다.

  `callSuper` 속성을 통해 equals와 hashCode 메소드 자동 생성 시 __부모 클래스의 필드까지 감안할지 안 할지에 대해서 설정__ 할 수 있습니다.

  즉, callSuper = true로 설정하면 부모 클래스 필드 값들도 동일한지 체크하며, callSuper = false로 설정(기본값)하면 자신 클래스의 필드 값들만 고려합니다.

  ```java
@EqualsAndHashCode(callSuper = true)
public class Man extends People {
  private String ssn;
  private String name;
  private String age;
}
  ```

## @Data

  @Data 어노테이션은 @Getter, @Setter, @RequiredArgsConstructor, @ToString, @EqualsAndHashCode을 한꺼번에 설정해주는 매우 유용한 어노테이션입니다.

  클래스 레벨에서 @Data 어노테이션을 붙여주면, 모든 필드를 대상으로 접근자와 설정자가 자동으로 생성되고, final 또는 @NonNull 필드 값을 파라미터로 받는 생성자가 만들어지며, toStirng, equals, hashCode 메소드가 자동으로 만들어집니다.

## 참조

  > [https://countryxide.tistory.com/16](https://countryxide.tistory.com/16 )
  >
  > [https://www.daleseo.com/lombok-popular-annotations/](https://www.daleseo.com/lombok-popular-annotations/)
