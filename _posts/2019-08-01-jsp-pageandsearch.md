---
title: "N-Depth 구조에서 페이지와 검색조건 유지하는 방법"
layout: post
category: JSP
tags: [JSP]
excerpt: "N-Depth 구조에서 페이지와 검색조건 유지하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 페이지와 검색조건 유지하기

  > N-Depth : 깊이가 N인, 계층형 구조

  설문조사에 대한 게시판을 예를 들어 설명하겠습니다.

  - `페이지 구성(3Depth)`
    - 메인 페이지(페이지 + 검색)
      - 설문 참여 인원 확인 페이지(페이지 + 검색)
        - 참여한 사람의 답변이 들어있는 페이지(페이지, 검색 X)

  페이지 구성이 3뎁스로 되어있고, 마지막 페이지에는 페이지네이션과 검색 조건이 없다고 가정하겠습니다.
  구조가 위와 같을 경우 우리가 생각해야하는 부분은 다음과 같습니다.

  - 메인 페이지에서 검색(ex 제목으로 검색)후 나온 페이지에서, 설문 참여 인원 확인 페이지로 이동
    - 설문 참여 인원 확인 페이지에서는 메인 페이지의 페이지 정보와 검색 조건을 가지고 있어야함
      - 설문 참여 인원 확인 페이지에서 2페이지 클릭 후, 참여한 사람의 이름을 클릭해서 답변 페이지로 이동
        - 답변 페이지에서는 메인 페이지의 `페이지 정보1, 검색조건1` + 설문 참여 인원 확인 페이지의 `페이지 정보2, 검색조건2`를 가지고 있어야함
      - 설문 참여 인원 확인 페이지에서 검색 후, 답변 페이지로 이동
        - 답변 페이지에서는 메인 페이지의 `페이지 정보1, 검색조건1` + 설문 참여 인원 확인 페이지의 `페이지 정보2, 검색조건2`를 가지고 있어야함

  위와 같이 구성이 되야, `이전 혹은 목록` 버튼을 클릭 했을 때, 전 페이지의 검색조건과, 페이지 조건이 유지되어서, 원래 있던 페이지로 돌아 갈 수 있습니다.

  - JSP(MainPage)

  ```html
    <a href="/survey/statistic?surSeq=${result.seq }&mno=${param.mno}&mainPageIndex=${searchVo.pageIndex}&searchKeyword=${searchVo.searchKeyword}" class="btn small ty_2">참여인원관리</a>
  ```

  위와 같은 방식으로 참여인원관리 버튼클릭 시 GET 방식으로 `쿼리 파라미터(Query Parameter)` 형식으로 데이터를 보내야 합니다.

  - JSP(참여인원관리페이지)

  ```html
  <!-- 목록 버튼 클릭 시 메인 페이지로 이동 -->
  <a href="#" class="btn ty_2" onclick="survey.list('<c:out value="${searchVo.mainPageIndex}" />');return false;">목록</a>
  <a href="<c:url value="${pageContext.request.contextPath}/survey/statistic/answer?seq=${result.surSeq}&surSeq=${result.surSeq}&privacySeq=${result.privacySeq}&partPageIndex=${searchVo.pageIndex}&mainPageIndex=${searchVo.mainPageIndex}&mno=${param.mno}&searchKeyword2=${searchVo.searchKeyword2}&searchKeyword=${searchVo.searchKeyword}"/>" >  <c:out value="${result.userName}"/></a>

  <!-- 해당 페이지에서 검색 후, 메인 페이지에서 받은 쿼리 파라미터 데이터 값이 날라가지 않게, hidden으로 가지고 있어야 합니다. -->
  <form action="<c:url value="/survey/statistic" />" name="searchForm" id="searchForm">
    <input type="hidden" name="mainPageIndex" id="mainPageIndex" value="<c:out value='${searchVo.mainPageIndex}'/>" />
    <input type="hidden" name="surSeq" value="${searchVo.surSeq}"/>
    <input type="hidden" name="searchKeyword" id="searchKeyword" value="<c:out value="${searchVo.searchKeyword}" />" />
    .....
  </form>
  ```

  위와 같은 방식으로 목록 페이지 버튼 클릭 시, 메인 페이지로 이동하기 위해서 쿼리 파라미터로 받은 mainPageIndex값을 넣어주고, 사람 이름 클릭 시, 해당 사람의 답변 페이지로 가기위해서
  searchKeyword, searchKeyword2, mainPageIndex, partPageIndex(참여인원관리페이지)를 쿼리파라미터로 넘겨야 합니다.

  이렇게 Depth가 깊어질 수 록, 쿼리 파라미터로 넘겨야 하는 조건들이 많이 생깁니다.

  즉, 위 프로그램의 구조는 다음과 같습니다.

  - `검색과 페이지조건이 있는 첫 번째 페이지`
    - searchKeyword와 pageIndex를 다음 페이지로 넘겨야함

    - `검색과 페이지 조건이 있는 두 번째 페이지`
      - 검색과 페이지 조건이 있기 때문에 쿼리 파라미터로 받은 searchKeyword와 pageIndex값을 form안에 hidden으로 들고 있어야, 전에 있던 값이 유지됨
      - searchKeyword2와 pageIndex2를 다음 페이지로 넘겨야함

      - `검색과 페이지 조건이 있는 세 번째 페이지`
        - 검색과 페이지 조건이 있기 때문에 첫 번째 페이지와 두 번째 페이지의 쿼리 파라미터 값인 searchKeyword, searchKeyword2, pageIndex, pageIndex2를 form에 hidden으로 들고있어야함
        - searchKeyword3와 pageIndex3를 다음 페이지로 넘겨야함

        - `반복`
          - .....
          - `검색과 페이지 조건이 없는 마지막 페이지`
            - hidden으로 쿼리 파라미터 데이터를 가지고 있을 필요가 없음


  > 위 과정을 보면 알 수 있듯이, 검색 조건이 있는 폼에서는 폼 안에 `hidden 속성`으로 `자신의 pageIndex와, 혹은 전 페이지의 pageIndex 등 유지시켜야할 값(데이터) 등`을 필수적으로 가지고 있어야, 검색 후에도 조건이 유지됩니다.
