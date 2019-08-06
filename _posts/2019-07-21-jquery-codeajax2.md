---
title: "[AJAX] ckeditor 비동기 처리 방식"
layout: post
category: JQurey
tags: [JQuery, Ajax]
excerpt: "ckeditor를 사용한 비동기 처리 방식에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > ckeditor를 사용한 비동기 처리 방식에 대해서 배워 봅시다.

## ckeditor를 사용한 비동기 처리 방식

  - ckeditor를 라디오버튼(출력, 미출력) 클릭 시 비동기로 처리하여 ckeditor를 나타내는 방법

  - ckeditor를 사용하기 위해서 script 추가

  ```javascript
  <script src="<c:url value="/js/ckeditor/ckeditor.js"/>"></script>
  ```

  - ckeditor를 textarea에 클래스 명으로 추가

	```html
	<textarea name="privacyVo.privacyContent" class="content ckeditor" id="privacyVo.privacyContent" cols="" rows="" style="height:300px;">
    <c:out value="${resultVo.privacyVo.privacyContent }" />
  </textarea>
	```

  - 비동기로 처리하기 위해 JS작성

	```javascript
function surveyPrivacy(cnt){
	let sur_seq = $("#seq").val();
	if(sur_seq == undefined){
		sur_seq = 0;
	}
	if(cnt == '1'){
		$.ajax({
			type : 'GET'
			,async : false
			,url : `${SURVEY_BASE_URL}/privacy?surSeq=${sur_seq}` // 해당 URI의 컨트롤러로 이동
			,error  : function(xhr) {
				alert("error");
			},success : function(result){
				$("#surveyPrivacy").html(result);
                CKEDITOR.replace('privacyVo.privacyContent', {
                    height: 150,
                    width: 1400,
                    padding : 10,
                });
			}
		})
	}else{
		$("#surveyPrivacy").html('');
	}
}
	```

  CKEDITOR.replace를 이용해, `미출력` 라디오 버튼을 클릭했다가, 다시 `출력` 라디오 버튼을 클릭해도 나오게끔 만들어 준다.

  - 컨트롤러에서 비동기 처리

	뒤에 .xxx방식을 붙이지 않은 것은 Tiles를 타지 않고 직접 찾아가기 위함, Tiles를 거쳐 갈 경우, 해당 페이지의 전체 형식이 중복으로 나타나게됨.

	 - Tiles 방식 : `/survey/privacy.xxx`
	 - 직접 View를 찾아가는 방식 : `/xxx/survey/privacy`


	```java
	@GetMappng("/privacy")
	public String privacy(@ModelAttribute("surveyVo") SurveyPrivacyVo surveyVo, Model model) {
		SurveyPrivacyVo resultVo = privacyService.findSurveyPrivacy(surveyVo);
		model.addAttribute("resultVo", resultVo);

		return "XXX/survey/privacy";
	}
	```
