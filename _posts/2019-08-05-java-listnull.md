---
title: "List에서 null값 체크하는 로직 없애는 방법"
layout: post
category: Java
tags: [Java]
excerpt: "List에서 null값 체크하는 로직 없애는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}


## List에서 null값 체크하는 로직 없애는 방법

  코딩을 하다보면, XXXVO에 list 속성이 선언되어 있고, 서비스 로직에서 이 속성 list가 null값인지 체크를 해줘야 하는 경우가 있습니다.
  그럴때 XXXVO 클래스에 아래와 같이 선언해주면, list가 null값인지 체크하는 `if 조건문`을 없앨 수 있습니다.

  - 방법 1. Getter에 List 참조변수가 null일경우 ArrayList 생성

  ```java
public class XXXVO {
	private List<SurveyQuestionVo> questionList;

	public List<SurveyQuestionVo> getQuestionList() {
		if (questionList == null) {
			questionList = new ArrayList<>();
		}
		return questionList;
	}
}
	```

  - 방법 2. ArrayList를 생성

  ```java
public class XXXVO {
  .....
  private List<SurveyQuestionVo> questionList = new ArrayList<>();
  .....
}
```

  두 가지 방식 중, 자신의 스타일에 맞게 골라 사용하면 됩니다.
