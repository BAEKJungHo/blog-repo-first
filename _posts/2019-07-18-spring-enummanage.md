---
title: "스프링에서 제공하는 ENUM 관리"
layout: post
category: Spring
tags: [Spring]
excerpt: "스프링에서 제공하는 ENUM 관리 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링에서 제공하는 ENUM 관리 방법에 대해서 배워 봅시다.

## 스프링에서 제공하는 ENUM 관리

  관리자 파일 관리 시스템을 개발하다가 파일명 JS, CSS, JSP, IMAGE에 따라 경로를 다르게 지정해줘야 하는데
  이부분에 대해서 고민하다가, 선임분께 배워서 터득했습니다.

  ```java
@Getter
public enum FileTypes {
  JS, CSS, JSP, IMAGE;
}
  ```

  Lombok을 사용하여 @Getter 어노테이션을 사용했습니다. 위 처럼 FileTypes라는 ENUM 클래스가 있고, 파일명에 따른 ENUM 상수를 선언 해줬습니다.

  스프링을 깊게 공부하지 않은 상태이면 아마도 아래와 비슷하게 코딩을 했을 거라 생각합니다.

  ```java
  // set type
  String type = FilenameUtils.getExtension(XXXVo.getPath()).toLowerCase();
  FileTypes fileTypes = FileTypes.valueOf(type);
  switch(fileTypes) {
    case CSS :
      historyVo.setType(FileTypes.CSS);
      break;
    case JS :
      historyVo.setType(FileTypes.JS);
      break;
    case JSP :
      historyVo.setType(FileTypes.JSP);
      break;
    case IMAGE :
      historyVo.setType(FileTypes.IMAGE);
      break;
      default :
        break;
    }
  ```

  하지만 스프링에서 제공하는 기능중 하나인 __@RequestMapping 어노테이션에 템플릿변수를 사용하면 ENUM 클래스를 효과적으로 관리할 수 있습니다.__

  위 처럼 String을 ENUM으로 바꿔서 가공할 필요 없이 컨트롤러 클래스 위에 `@RequestMapping(/XXX/fileManage/{fileType})` 이런식으로 템플릿 변수를 받아서 사용하면, 스프링에서 알아서 ENUM 상수를 찾아서 비교해줍니다. 즉, JS가 들어오면 /XXX/fileManage/JS를 타고 HandlerMethod를 실행하게 됩니다.

  여기서 문제가있는데, ENUM 상수 타입이 아닌 다른것들 올 경우, 오류가 발생할 수 있습니다.

  HandlerMethod에서 해당 ENUM 상수 타입들이 아닌경우 실행이 안되게 하고싶으면, 2가지 방법을 사용할 수 있습니다. HandlerMethod를 타기전 `@ModelAttribute를 메서드 위에 선언`해서 처리하는 방식과 스프링에서만 지원하는 `Formatter 인터페이스`를 구현하여 사용하는 방법이 있습니다.

### @ModelAttribute로 HandlerMethod를 타기전에 ENUM 상수 타입이 아닌것을 막기

  ```java
  @RequestMapping("/fileManage/{fileType}")
  public class FileManageController {
    @ModelAttribute
    public void fileType (@PathVariable FileTypes fileType) {
      if (type == null) {
        throw new RuntimeException("해당 하는 파일타입이 존재하지않습니다.");
      }
    }
  }
  ```

  ```java
  if (type == null) {
    throw new RuntimeException("해당 하는 파일타입이 존재하지않습니다.");
  }
  ```

  위 코드는 아래와 같은 역할을 합니다.

  ```java
  Assert.notNull(fileType, "해당 하는 파일타입이 존재하지않습니다.");
  ```

  @ModelAttribute를 사용할 경우 다른 HandlerMethod를 타기전 얘를 먼저 실행하므로, type이 다를경우 `RuntimeException`을 발생시키고 종료합니다.
  Interceptor를 사용해도 되지만, 해당 클래스 내에서만 사용할 경우는 @ModelAttribute를 사용하는 것이 낫습니다.

### Formatter 인터페이스를 상속받아서 ENUM 상수 타입이 아닌것을 막기

  ```java
public class FileTypeFormatter implements Formatter<FileTypes> {
  @Override
  public FileTypes parse(String s, Locale locale) throws ParseException {
      return FileTypes.findByTypeName(s);
  }

  @Override
  public String print(FileTypes fileTypes, Locale locale) {
      return fileTypes.name();
  }
}
  ```

  Formatter인터페이스를 상속받으면 필수로 구현해줘야 하는 메서드가 parse랑 print가 있습니다.

  parse메서드는 문자열을 제네릭 `< >`안에 선언한 타입으로 변환해줍니다. print메서드는 제네릭에 선언한 타입을 어떻게 문자열로 출력할껀지를 정해줍니다.

  parse메서드에서 문자열을 입력받아서 ENUM 상수에 있는 값들과 일치하는게 있는지 찾아야 하므로, ENUM 클래스에서 문자열과 일치하는 ENUM 상수를 찾는 메서드를 생성해야 합니다.

  ```java
@Getter
public enum FileTypes {
  JS, CSS, JSP, IMAGE;

  public static FileTypes findByTypeName(String typeName) {
    FileTypes[] fileTypes = FileTypes.values();
    for (FileTypes fileType : fileTypes) {
      if (fileType.name().equalsIgnoreCase(typeName)) {
        return fileType;
      }
    }
    throw new RuntimeException("해당하는 파일타입은 존재하지 않습니다.");
  }
}
  ```

  equalsIgnoreCase()는 대소문자를 구분하지 않습니다.

  컨트롤러에서는 아래와 같이 사용하면 됩니다.

  ```java
  @GetMapping
  public String list(@PathVariable FileTypes type)
  ```

	@PathVariable FileTypes type 파라미터를 선언해주면 `@RequestMapping(/XXX/fileManage/{fileType})`를 통해 나온 결과를 얻을 수 있습니다.

  즉, @RequestMapping의 fileType 템플릿 변수에 ENUM 상수 JS가 들어오면, FileTypeFormatter를 타고 parse 메서드를 실행시켜서, JS에 대한 ENUM 상수와 일치하는게 있는지 찾고, 있으면 정상적으로 반환을 하고, 없으면 RuntimeException을 발생시켜 종료합니다.

## 정리

  따라서 스프링에서 ENUM 상수를 사용해야하며, 해당 값에 따라 매핑을 다르게 가져가야 할 경우, 위에서 배운 스프링 특징을 살려서 개발을 하면
  더 깔끔하고 쉽게 코딩하실 수 있습니다.
