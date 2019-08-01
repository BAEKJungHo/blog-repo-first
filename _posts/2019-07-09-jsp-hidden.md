---
title: "input태그 hidden 속성(검색 조건 유지)"
layout: post
category: JSP
tags: [JSP]
excerpt: "input태그 hidden 속성의 사용 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > input태그 hidden 속성의 사용 방법에 대해서 배워 봅시다.

## input type hidden

  사용자가 입력하거나 선택하는 정보는 아니지만 __form태그에서 같이 전송해줘야 하는 정보를 담기 위해서 히든 필드(Hidden Field)를 사용__ 합니다.

  예를 들어서 회원가입시 사용자의 아이피를 받는 경우 히든필드에 넣어서 폼 전송시 함께 전송합니다.

  ```html
  <input type="hidden" name="UserIP" value="<?echo $REMOTE_ADDR?>">
  ```

  value 속성에 들어있는 "<?echo $REMOTE_ADDR?>"라는 값은 PHP 코드의 일종입니다. 사용자의 아이피를 인식하는 코드입니다.
  히든필드는 화면에 출력되는 부분이 아니기 때문에 폼의 외형을 제작할때는 아무런 영향을 미치지 않습니다. 그러나 웹 프로그램을 할때는 아주 빈번하게 사용되는 중요한 폼 필드 중 하나입니다.

  또한, 게시판에서 첨부파일 기능을 구현할 때에 board 테이블은 file 테이블의 기본키인 file_key를 가지고 있습니다.
  따라서 View 파일에서 input태그의 hidden 속성을 이용하여 기본키를 보이지 않게 하여 컨트롤러로 보낼 수 있습니다.

## hidden을 사용하여 POST 방식으로 컨트롤러에게 전달

  input hidden으로 post로 데이터를 넘기는 방법은 다음과 같습니다.

  ```html
  <input type="hidden" name="num" value="${boardDTO.num}" />
  ```

  input hidden 태그를 form에 감싸서 보내면
 requestmethod=post를 적용한 컨트롤러에서도 `템플릿 변수와 @PathVariable`을 사용하여, input hidden의 `name` 이름으로 사용할 수 있습니다.

  ```java
  @RequestMapping(value="/boardEdit/{num}", method=RequestMethod.POST)
 	public String boardEdit(@Valid BoardDTO boardDTO, @PathVariable int num, BindingResult bindingResult) { }
  ```

## 게시판 검색 조건 URI유지를 위해 hidden 및 커맨드 객체 사용

  게시판에서 검색을 하고, 검색조건을 URI를 통해 가져가기 위해서는 View에서 hidden을 사용하는것이 좋습니다.

  예를들어 boardSearchList.jsp에서 검색 -> boardSearchList 컨트롤러로 이동 -> boardSearchList.jsp 검색 결과 출력 -> 제목 클릭 시 boardRead 컨트롤러로 이동 -> boardRead.jsp 이동 -> 해당 게시글 보고, 목록 클릭시 검색 조건을 유지하여 원래 있던 페이지로 와야함

  따라서 위 기능을 구현하기 위해서는 검색조건을 URI로 달고가야 합니다.

  제목 클릭시 boardRead.jsp로 이동할 때에 전 페이지에 있던 URI에 적힌 Key와 Value를 boardRead.jsp에서 input hidden으로 사용하면 됩니다.

  - boardRead Controller

  ```java
@RequestMapping(value="/boardRead/{num}")
public String boardRead(@ModelAttribute SearchCriteria cri, Model model, @PathVariable int num) {
  .....
  return "boardRead";
}
  ```

  제목을 클릭시 검색 조건이 URI에 있으므로 해당 `KEY와 Value`가 컨트롤러의 커맨드 객체로 바인딩이 됩니다.
  그리고 컨트롤러를 통해 boardRead.jsp로 갈때에 input hidden속성을 통해 EL로 커맨드 객체 속성에 저장된 값을 찍습니다.

  ```html
  <form id="frm" action="<c:url value="/boardSearchList" />" >
    <input type="hidden" name="searchType" value="${searchCriteria.searchType }"/>
    <input type="hidden" name="keyword" value="${searchCriteria.keyword }"/>
    <input type="hidden" name="page" value="${searchCriteria.page }"/>
    <input type="hidden" name="perPageNum" value="${searchCriteria.perPageNum }"/>
    <button>목록</button>
  </form>
  ```

  그리고 boardRead에서 다시 목록 버튼을 클릭하게 되면 boardSearchList의 SearchCriteria 커맨드 객체를 통해
  검색 조건이 바인딩되고, 원래 있던 페이지로 오게 되는 것입니다.
