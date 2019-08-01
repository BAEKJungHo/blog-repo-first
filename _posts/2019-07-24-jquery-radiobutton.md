---
title: "라디오 버튼이 선택된 INDEX값 가져오기"
layout: post
category: JQurey
tags: [JQuery]
excerpt: "라디오 버튼이 여러개 존재할 경우, 선택된 버튼의 Index값을 가져오는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 라디오 버튼이 여러개 존재할 경우, 선택된 버튼의 Index값을 가져오는 방법에 대해서 배워 봅시다.

## 라디오 버튼이 선택된 INDEX값 가져오기

  예를들어, 설문조사에 대한 개발을 한다고 가정하겠습니다.

  ![ra1](/images/posts/201907/ra1.jpg)

  다음과 같이 질문이 있고, 질문에 대한 A, B, C 문항이 있다고 하겠습니다.

  라디오 버튼의 특징은 체크박스와 달리, 항목을 오직 한 개만 선택할 수 있습니다.

  사용자가 해당 질문에 대해서 답변을 하게되면(즉, 문항 A, B, C중 한개를 선택) ANSWER 테이블에 들어가게 될 것입니다.
  이때 ANSWER 테이블은 질문에 대한 번호 SEQ와, 문항에 대한 번호 SEQ를 가지고 있어야 합니다. 이때 문항에 대한 SEQ를
  input hidden으로 넘기고 싶어도, 라디오 버튼은 name이 같아야 하므로, A 라디오 버튼의 value값으로 B, C가 덮어 씌워집니다.

  즉, A를 선택하든, B를 선택하든, C를 선택하든 DB 테이블에는 1이라는 값으로 들어가게 됩니다.

  A를 선택하면 1, B는 2, C를 선택하면 3으로 DB 테이블에 세팅하기 위해서는, 자바스크립트 또는 제이쿼리로 제어를 해야 합니다.

  - 기존 JSP 코드

  ```html
  <input type="radio" name="answerList[${index }].questionSeq" id="question_${questIndex.index }_${questionIndex.index }" value="${question.seq }" >
  <input type="hidden" name="answerList[${index }].answerNo" value="${quest.questNo }" />
  <input type="hidden" name="answerList[${index }].questionNo" id="questionNum" value="${question.questionNo}" />
  ```

  기존 JSP 코드와 같은 방식으로 하면 hidden의 값들은 1, 2, 3 모두가 넘어가게됩니다. 따라서 선택한 항목에 대해서만 값을 넘기기 위해서는
  아래와 같은 로직을 거쳐야 합니다.

  - answerList[0].questionSeq 클릭시 다음 요소들도 같이 선택되게 해야 합니다. (answerList[0].answerNo, answerList[0].questionNo)
  - 해결하기 위해서는 hidden타입을 radio로 바꾸고, style을 display:none으로 바꿉니다.
  - 그리고 자바스크립트 또는 제이쿼리를 사용하여 첫번째 라디오버튼이 클릭 될 경우, 옆쪽의 라디오버튼 2개도 같이 클릭되게 하는 것입니다.

  ![ra2](/images/posts/201907/ra2.jpg)

  즉, A 문항을 선택 했을 때, 옆에 있는 라디오 버튼 1개는 answerNo의 값을 가지고 있으며, 맨 끝의 라디오 버튼은 questionNo의 값을 가지고 있습니다.
  따라서 A를 선택하면 옆에 있는 라디오 버튼 2개가 같이 선택되야 합니다. 또한 옆에 있는 라디오 버튼은 `style="diplay:none"`으로 바꿔서 안보이게 해야 합니다.

  - Javascript

  ```javascript
  $(() => {
  const firstRadioButtons = $('input[name$=questionSeq]'); // 라디오 버튼 name

  firstRadioButtons.on('change', function () {
    const $this = $(this); // 라디오 버튼을 선택 했을 경우
    const checked = $this.prop('checked'); // true, false
    const answerNo = $this.next(); // 선택한 라디오 버튼의 옆 버튼
    const questionNo = answerNo.next(); // answerNo 라디오 버튼의 옆 버튼

    answerNo.prop('checked', checked); // checked가 true이면 checked 상태로 변경
    questionNo.prop('checked', checked); // checked가 true이면 checked 상태로 변경
    });
  });
  ```

  - 바뀐 JSP 코드

  ```html
  <input type="radio" name="answerList[${index }].questionSeq" id="question_${questIndex.index }_${questionIndex.index }" value="${question.seq }" >
  <input type="radio" name="answerList[${index }].answerNo" value="${quest.questNo }" style="display: none;"/>
  <input type="radio" name="answerList[${index }].questionNo" id="questionNum" value="${question.questionNo}" style="display: none;"/>
  ```
