---
title: "JS로 form으로 submit 하는 방법"
layout: post
category: Javascript
tags: [Javascript]
excerpt: "JS로 form으로 submit 하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > JS로 form으로 submit 하는 방법에 대해서 배워 봅시다.

## JS로 form으로 submit 하는 방법

```javascript
$("#surveyVo").attr("action",`${SURVEY_BASE_URL}/edit`);
$("#surveyVo").method = METHOD.POST;
$("#surveyVo").submit();
```

위와 같은 방식으로 되어 있을때, 해당 url을 가진 post방식의 `surveyVo`라는 name값의 form을 찾아서 Submit 해줍니다.

따라서 해당 폼 안에 hidden 속성으로 된 값들도 넘길 수 있습니다.

> hidden 속성을 사용하면, 페이지 이동(화면 전환)이 있어도 유지 되어야 할 값들을, 유지 시킬 수 있다.

- hidden 속성 값 EXAMPLE
    - pageIndex
    - sitecode
    - searchKeyword
    - searchCondition
    - .....
