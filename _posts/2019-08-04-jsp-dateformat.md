---
title: "날짜를 원하는 형식으로 출력"
layout: post
category: JSP
tags: [JSP]
excerpt: "날짜를 원하는 형식으로 출력하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 날짜를 원하는 형식으로 출력

## 날짜를 원하는 형식으로 출력

```html
<c:out value="${util:formattedDateTimeCustom(result.startDate,'yyyy-MM-dd HH:mm')}"/>
~
<c:out value="${util:formattedDateTimeCustom(result.endDate,'yyyy-MM-dd HH:mm')}"/>
```

  위와 같은 방식으로 원하는 날짜 형식으로 출력하고 싶은 경우에는, 커스텀 태그 라이브러리를 만들어서 사용하면 됩니다.
