---
title: "View에서 EL을 사용하여 함수 호출"
layout: post
category: JSP
tags: [JSP]
excerpt: "View에서 EL을 사용하여 함수 호출하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > View에서 EL을 사용하여 함수 호출하는 방법에 대해서 배워 봅시다.

## View에서 EL을 사용하여 함수 호출

  ```java
  public class PageMaker {

	private Criteria cri;
	private int totalCount;
	private int startPage;
	private int endPage;
	private boolean prev;
	private boolean next;
	private int displayPageNum = 5;

	/**
	 *  검색 조건과 검색 키워드 처리
	 *  URI 자동 생성 메서드
	 *  페이지와 페이지번호, 검색 타입과 키워드 조건이 URI에 붙어서 간다.
	 */
	public String makeSearch(int page) {
		UriComponents uriComponents = UriComponentsBuilder.newInstance()
			.queryParam("page", page)
			.queryParam("pagePageNum", cri.getPerPageNum())
			.queryParam("searchType", ((SearchCriteria)cri).getSearchType())
			.queryParam("keyword", encoding(((SearchCriteria)cri).getKeyword()))
			.build();

		return uriComponents.toUriString();
	}

  .....
}
  ```


  ```html
<a href='<c:url value="/boardSearchList?page=${pageMaker.makeSearch(pageMaker.startPage-1)}"/>'>&laquo;</a>
  ```

  위 처럼 사용할 경우 pageMaker객체를 불러 함수를 호출 할 수 있습니다.
