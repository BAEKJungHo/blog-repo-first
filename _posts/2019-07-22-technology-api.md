---
title: "API(Application Programming Interface)"
layout: post
category: Technology
tags: [Technology]
excerpt: "API(Application Programming Interface)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > API(Application Programming Interface)에 대해서 배워 봅시다.

## API

  > Wikipedia
  >
  > API는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.

  API는 Application Programing Interface라는 용어로써, 어떠한 응용프로그램에서 데이터를 주고 받기 위한 방법을 의미합니다. 어떤 특정 사이트에서 특정 데이터를 공유할 경우 어떠한 방식으로 정보를 요청해야 하는지, 그리고 어떠한 데이터를 제공 받을 수 있을지에 대한 규격들을 API라고 하는것입니다! API라고 하는것이죠!

## 스프링에서 API 가공처리

  스프링에서 API는 HTML 페이지가 아닌, DATA를 받아오는 경우를 의미하기도 합니다.

  - 게시판 API
    - 목록 조회
		  - SEQ, 제목, 작성자, 등록일(그 외의 정보는 노출하면 안됨)
    - 상세 조회
		  - SEQ, 제목, 내용, 작성자, 등록일

  ```java
  // seq와 modDate 속성만 추출하고 싶은 경우
  // fileManageHistoryVoList를 seq속성과 modDate속성만을 가진 VO에 넣어서 가공해야함
  "fileManageHistoryVoList":[{"seq":null,"modDate":"2019-07-22 10:09:43.0", "writer":"JACK", "title":"tree"}]}
  ```

  JSON형식에서 seq와 modDate 속성만 추출하고 싶은 경우, (위 경우는 API 방식으로, 데이터를 그대로 내보냅니다. 따라서 불필요한 정보까지 노출되어, 외부 사용자에게 어떤 속성이 있는지 유추할 수 있습니다. 보안성에 취약하게 됩니다.) `가공`을 해야 합니다.

  즉, 필요한 속성 seq와 modDate만을 가지는 VO 클래스를 만들어서, 해당 VO객체로 반환하여 사용해야합니다.

  > API => 데이터를 그대로 내보냄 (가공 필요 O)
  >
  > JSP => 데이터를 가지고 HTML랜더링해서 내보냄 (가공 필요 X)
