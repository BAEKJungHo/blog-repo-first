---
title: "c:set과 c:forEach를 같이 사용"
layout: post
category: JSP
tags: [JSP]
excerpt: "c:set과 c:forEach를 같이 사용하여 사용자가 선택한 답변을 뿌리는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 예를들어 설문 조사가 있고, 설문조사에는 문제 문항이 있다고 가정하겠습니다.
  >
  > EX) 우리가 학교에서 시험볼때 1번문제에 5번까지 선택지가 있는거와 똑같음.

  하지만 복수 선택이 가능한 문항도 있습니다. 이런 문항은 JSP에서 checkbox로 나타내야 하는데 문제와 문항을 JSP에서 나타내기 위해서는
  forEach문을 바깥쪽(문제)한번이랑 안쪽 문항 forEach 총 2번을 돌려야 합니다. 만약 여기서 사용자가 선택한 답변을 `checked` 옵션을 줘서
  View로 뿌리기 위해서는 forEach가 한 번더 필요할 것입니다. 하지만 바깥쪽이나 안쪽에 `위에 있는 forEach문이랑 감싸게 되는 경우` 폼이 중복되서 나오게 됩니다.

  따라서 `c:set`의 특성을 이용해서 위 문제를 해결할 수 있습니다.

## c:set과 c:forEach를 같이 사용

```html
<!-- 바깥쪽 forEach문 생략 -->
<c:set var="checkanswer" value="" />
<c:forEach items="${list}" var="list" varStatus="varStatus">
	<c:if test="${list.answerNo eq quest.questNo and list.questionNo eq question.questionNo}">
		<c:set var="checkanswer" value="checked" />
	</c:if>
</c:forEach>

  <!-- 안쪽 forEach문 생략 -->

<!-- 바깥쪽 forEach문 닫기 -->
```

  위 처럼 구성할 경우 문제 번호와, 사용자가 선택한 답변의 번호와, 문항 번호가 일치 하는 경우 `checked` String Value값을
  c:set var로 선언한 checkanswer에 저장하는 것입니다.

  그리고 안쪽 forEach문에 선언된 input 태그에 다음과 같이 해주면 됩니다.

  ```html
  <input type="checkbox" value="${XXX}" ${checkanswer} />
  ```

  - 위 반복문이 도는 과정
    - 1번 문제 루핑
      - 1번 문항 루핑, 1번 문항이 사용자가 선택한 답변의 문항 번호와 같은지 루핑
      - 같을 경우 checkanswer에 checked 저장
      - 2번문항 루핑 ..
      - .....
    - 2번 문제 루핑
      - 1번 문항 루핑
      - checkanswer 빈 값으로 초기화
      - 1번 문항이 사용자가 선택한 답변의 문항 번호와 같은지 루핑
      - 같을 경우 checkanswer에 checked 저장

  > 즉, forEach가 꼭 뿌려줘야 한다는 고정관념에 벗어나면 됩니다.
