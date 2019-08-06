---
title: "JS로 정규식 처리하는 방법"
layout: post
category: Javascript
tags: [Javascript]
excerpt: "JS로 정규식 처리하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > JS로 정규식 처리하는 방법에 대해서 배워 봅시다.

## JS로 정규식 처리하는 방법

```javascript
const accnt = {

  /** 검색 폼 **/
  searchForm: document.searchForm,

  /** 등록, 수정, 삭제 폼 **/
  form: document.querySelector('#accntVo'),

  frm () {
    let admId = $("#admId").val();
    if (!verifyId(admId)) {
        alert('올바른 아이디 형식이 아닙니다.');
        $("input[name='admId']").focus();
        return false;
    } else {
        accnt.form.action = `${ACCNT_URL}`;
        accnt.form.submit();
    }
   }
}

function verifyId(targetValue) {
  console.log('targetValue = ', targetValue);
  let regExp = /^[a-z0-9+]{4,100}$/;
  if(targetValue.match(regExp)) {
    return true;
  }
  return false;
}
```
