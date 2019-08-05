---
title: "웹 요청 리스트 처리하기"
layout: post
category: JSP
tags: [JSP]
excerpt: "웹 요청 리스트 처리하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 웹 요청 리스트 처리하기

```html
<input type="hidden" name="questList[${questIndex}].questNo" id="questNo" value="<c:out value='${quest.questNo }'/>" />
<input type="hidden" name="questList[${questIndex}].content" id="content" value="<c:out value='${quest.content }'/>" />
<input type="hidden" name="questList[${questIndex}].questionNo" id="questionNo" value="<c:out value='${quest.questionNo }'/>" />
```

  원래 List의 특징은 다음과 같습니다. 예를들어 0부터 5까지의 인덱스가 있을 때, 인덱스 3을 삭제하게 되면, 3번째 인덱스가 사라지고 4번째, 5번째 인덱스가 앞으로 땡겨져서 1, 2, 3, 4 인덱스를 유지하게 됩니다.

- list.add()로 리스트를 생성한 경우
  - 0 1 2 3 4 5
    - 인덱스 3 삭제 후, 리스트 구성
      - 0 1 2 3 4

  반면, 위 코드와 같이 웹 요청으로 List에 index번호를 강제로 지정하는 경우, 1, 2, 3, 4, 5 인덱스 중에서 3인덱스를 삭제하게 되면, 3번째 인덱스 자리는 null로 채워지게 됩니다. 즉, 1, 2, null, 4, 5와 같이 구성됩니다.

- 웹 요청에 의해, 리스트 인덱스를 강제로 지정한 경우
  - 0 1 2 3 4 5
    - 인덱스 3 삭제 후, 리스트 구성
      - 0 1 2 null 4 5

  따라서, 해당 리스트 값을 삭제하고, 남은 리스트를 DB에 넣기 위해서는 서비스쪽에서 필수값에 대해서 null 체크를 해줘야 합니다.

  즉, 위 코드를 기준으로 questNo, content, questionNo 값에 대해 null이 아닐 경우에 대해서만 DB에 삽입 되게 해야 합니다.

  seq의 성질을 가진 questNo나, questionNo는 DB에 설정된 타입이 Integer일 경우 null 값으로 체크할 수 있지만, int 타입일 경우, 기본이 0이기 때문에 굳이 체크하지 않아도 됩니다.

  따라서 위 코드에서 검증해야할 필 수 값은 content 하나 입니다.
