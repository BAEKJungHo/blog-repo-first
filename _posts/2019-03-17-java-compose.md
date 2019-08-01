---
title: "Compose"
layout: post
category: Java
tags: [Java]
excerpt: "Java에서 Compose(합성)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바 공부를 하면서 참고한 서적입니다.

_명품자바에센셜(생능출판사, 황기태), 이것이 자바다(한빛미디어, 신용권), 디자인패턴과 리팩토링_

## Compose

  [상속(Inheritance)](https://baekjungho.github.io/java-inheritance/)은 `IS-A`관계 즉, 패밀리 개념이 성립될때 사용하는 것이라고 배웠습니다. 반면에 __합성(Compose)__ 은 `Has-A`관계, 상속의 의미가 통하지 않지만 기존에 있는 클래스들의
  속성과 메소드들을 `재사용`하기 위해 사용되며, 다른 객체들에게 자신의 할 일을 `위임(delegation)`하여 실행하게 하는 것입니다.

  즉, IS-A관계가 아닌데 재사용해야할 클래스가 있다면 `합성`을 사용합니다.

  또한 IS-A관계가 아닌 시간에 따라 변할 수 있는 `역할`을 표현할때 좋습니다.

  합성은 `객체를 속성`으로 사용하는것을 뜻합니다.

## EXAMPLE

  예를들어 문제와 문항에 대한 클래스를 구현한다고 가정하겠습니다.

  - 문제 1. OOP의 특성을 고르시오
    - 문항 1. 캡슐화
    - 문항 2. 상속
    - 문항 3. 다형성
    - 문항 4. 추상화

  이런 식으로 되어있을때 문제 클래스를 Quest라하고 문항 클래스를 Question이라 하겠습니다.
  문제 클래스 안에는 여러개의 문항이 있으므로 문항에 대한 리스트를 가지고 있어야 합니다.
  또한 문항에 대한 정보를 가지고 있어야 합니다. 이때 Quest 클래스를 상속 받는것이 마땅치 않은 경우 `합성(Compose)`를 사용하여 구현할 수 있습니다.

  - 문항 Class(QuestionVo)

  ```java
  @Getter @Setter
  @RequiredArgsConstructor
  class QuestionVo {
    private int questionSeq;
    private String content;
    .....
  }

  - 문제 Class(QuestVo)

  ```java
  @Getter @Setter
  @RequiredArgsConstructor
  class QuestVo {
    private int questSeq;
    private String content;
    private List<QuestionVo> questionList;
    private QuestionVo questionVo;
    .....
  }
  ```

  이런 식으로 Quest 클래스 내에 Question 객체를 속성으로 가지고있으면, 해당 속성을 이용할 수 있습니다.
