---
title: "@ModelAttribute"
layout: post
category: Spring
tags: [Spring]
excerpt: "@ModelAttribute 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @ModelAttribute 어노테이션에 대해서 배워 봅시다.
  >
  > [커맨드 객체(Command Object)](https://baekjungho.github.io/spring-kcommandObject/)의 연장선이므로, 커맨드객체를 모르시면 배우고 오시는게 좋습니다.

## @ModelAttribute

  @ModelAttribute를 배우기전에 커맨드 객체의 특징중 하나를 말하면, 커맨드 객체를 사용하면 model.addAttribute를 사용하지 않아도 됩니다.

  이게 무슨말인가 하면, View에서 GET이든 POST든 넘어온값들이 커맨드 객체에 담기고, 담긴 속성들을 사용하기 위해서
  우리는 model.addAttribute("boardDTO" boarDTO)와 같은 방식으로 넘겨서 View단에서 EL로 커맨드 객체의 속성들을 찍어냈습니다.

  하지만 사실상 `Debugger`를 돌려보면 커맨드 객체가 model에 담긴것을 볼 수 있습니다.

  그 이유는 커맨드 객체 앞에는 `@ModelAttribute` 어노테이션이 생략되어 있기 때문입니다.

  > 폼으로 input text나 hidden을 사용하여 POST방식으로 데이터를 컨트롤러로 보내던, GET방식으로 컨트롤러로 보내던 커맨드 객체를 사용하면 커맨드 객체의 속성(필드)에 바인딩이 되며, 자동으로 Model에 담긴다.

  ```java
  @RequestMapping(value="/boardWrite", method=RequestMethod.POST)
  public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
    if(bindingResult.hasErrors()) {
      return "boardWrite"; // ViewResolver로 보냄
    } else {
      return "redirect:/boardSearchList";
    }
  }
  ```

  위 코드에서 @Valid BoardDTO boardDTO 이 부분에 `@ModelAttribute` 어노테이션이 생략 되어 있습니다. 즉, __커맨드 객체와 @ModelAttribute는 항상 붙어 다닌다고 생각하시면 됩니다.__

  > @ModelAttribute !! 바인딩과 함께 자동으로 모델에 데이터를 담아줍니다.

  @ModelAttribute 어노테이션의 기능 덕분에 가능한 일입니다.

## Alias 지정

  @ModelAttribute는 Alias를 지정하여 사용할 수 있습니다. 커맨드 객체의 파라미터명이 너무 길거나 줄여쓰고 싶은 경우에 사용할 수 있습니다.

  ```java
  @ModelAttribute("seacrh") SearchCriteria searchCriteria
  ```

  위 처럼 search로 지정한 경우에 View단에서 EL로 찍을때 `search.필드명`으로 찍을 수 있습니다.

  @ModelAttribute에 alias를 지정하면 이 이름으로 모델의 키값을 설정합니다.

  만약 지정하지 않으면, 기본 키값은 SearchCriteria객체의 이름을 첫글자는 소문자로하고 나머지는 [카멜 케이스(Camel Case)](https://baekjungho.github.io/spring-camelcase/)로 변환하여 키값을 설정합니다.

  즉, 위에서 커맨드 객체는 @ModelAttribute가 생략되어있기 때문에 model.addAttribute를 사용하여 담지 않아도 된다고 했는데,
  그럼 위 코드에서 파라미터명이 BoardDTO board라고 되어있는경우 View단에서 찍어낼때 board.필드명으로 하면안되고, 카멜케이스 규칙에 따라
  boardDTO.필드명으로 찍어야 합니다.

  > @ModelAttribute("search") SearchCriteria search  / 키값 : search
  >
  > @ModelAttribute SearchCriteria search / 키값 : searchCriteria
  >
  > SearchCriteria search / 키값 : searchCriteria

  그러면 굳이 @ModelAttribute를 써야 하나라고 생각하실 수도 있는데, 실무에서는 검색 조건을 유지하는 곳에는 @ModelAttribute를 명시해주는게 좋고, 수정, 삭제, 등록의 경우에는 생략해도 됩니다.

## 일반 메소드 위에 @ModelAttribute를 사용한 경우

  우리가 알고 있는 일반적인 스프링 MVC의 작동구조는 다음과 같습니다.

  Filter - Intercetpor - Controller - Intercetpor - Filter

  즉, Filter와 Intercetpor는 전처리 후처리가 가능합니다. 메소드 위에 @ModelAttribute를 지정한 경우 스프링 MVC 작동구조는 다음과 같이 됩니다.

  Filter - Intercetpor - @ModelAttribute - Controller - Intercetpor - Filter

  즉, @Modelattribute는 전처리만 가능합니다.

  ```java
  // form:form 태그를 사용하기 위해
  // @Modelattribute를 일반 메서드 위에 적용해서 Model에 담아 바인딩
  @ModelAttribute("xxxPolicyVo")
  public XXXPolicyVo xxxPolicyVo() {
      return new XXXPolicyVo();
  }
  ```

  위와 같이 일반 메서드 위에 @Modelattribute가 설정되어있는 경우, 이 메서드의 역할은, 굳이 Intercetpor까지 쓸 필요가 없고, 이 메서드가 포함된 컨트롤러 한정에서 사용하는 `공통 로직` 의 경우에 따로 빼서 사용할 수 있습니다.

  즉, `Handler Method(@RequestMapping 어노테이션이 적용된 메소드)`가 실행되기 전에 @Modelattribute를 적용한 메서드가 먼저 실행됩니다.

  __위 코드는 form:form을 사용할 때에 공통 로직을 뽑아 메서드로 만든 코드이며 Handler Method를 타기전에 먼저 실행되는 메서드 입니다.__ form:form을 생성하기 위해서 커맨드 객체를 모델로 담아서 바인딩 해줘야하는데, 모델에 @ModelAttribute("xxxPolicyVo") 키값으로된 객체가 없을경우 @ModelAttribute에 의해서 Model에 담아주며 바인딩 까지 해줍니다.

  form:form태그가 나오기 위해 바인딩을 해주려면 Handler Method에 아래와 같은 코드를 써야 했습니다. 아래 코드는 공통 로직을 뽑지 않았을 때에 각 Handler 메서드에 적용해야 할 코드입니다.

  ```java
  // 이 코드는 위 @Modelattribute를 적용한 메서드 코드와 같은 역할을 한다.
  model.addAttribute("xxxPolicyVo", new XXXPolicyVo());
  ```

  즉, 커맨드 객체를 모델로 담아 JSP로 보내는 것입니다. 하지만 이런 코드를 Handler Method마다 써준다면, 비효율적이므로 공통 로직을 뽑아내어(마치 Intercetpor와 AOP와 비슷한 역할) 일반 메서드를 컨트롤러 내에 작성하고, 메서드 위에 @Modelattribute를 지정하는 것입니다. `CamelCase` 규칙을 적용하여 커맨드 객체의 Key값을 생성하고 싶은 경우 @Modelattribute에 Alias를 지정하지 않아도 되는데, 그것보다는 지정하는 편이 가독성이 좋습니다.

  @ModelAttribute는 검증 로직에 많이 사용되는데, 정상적인 방법으로 들어오지 않고, URL로 타고오는 보안 공격을 하는 사용자가 있기 때문에, 이런 부분을 차단하기 위해서 검증 로직을 사용합니다. 아래는 검증 로직 코드의 한 부분입니다.

  ```java
  @ModelAttribute("xxxTestVo")
  public XXXTestVo xxxTestVo(@PathVariable Integer seq) {
      XXXTestVo xxxTestVo = new XXXTestVo();
      xxxTestVo.setSeq(seq);
      return xxxService.findXXX(xxxTestVo);
  }
  ```

  특히 게시판 - 댓글의 경우, 댓글 작성중 게시글이 날라갈 경우 등의 검증을 거쳐야 하므로 컨트롤러의 Handler Method를 타기 전에 위 검증 로직을 먼저 실행합니다.
