---
title: "추가Button 클릭 시 태그요소를 추가"
layout: post
category: JSP
tags: [JSP]
excerpt: "추가 Button 클릭 시 태그요소를 추가하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 추가Button 클릭 시 태그요소를 추가하는 방법에 대해서 배워 봅시다.

## 추가Button 클릭 시 태그요소를 추가

  ![cm1](/images/posts/201908/cm1.jpg)

  위 처럼 추가버튼을 클릭했을 때 제목과 주소 input text가 나오기 위해서는 js를 사용해야 합니다.

  먼저 jsp파일을 보겠습니다.

  ```html
  <tr>
      <th scope="row">제목/주소<span class="red">*</span></th>
      <td class="left">
      <div id="juso24">
          <!--JavaScript를 통해 append()함수 호출 시 이 div내부에 input text 태그가 삽입됨 -->
      </div>
      <li class="addrbox">
          <div class="txts">
              <a href="#" onclick="attachAddr(); return false;" class="btn ty_2 small">추가</a>
          </div>
      </li>
      </td>
  </tr>
  ```

  위에서 attachAddr()이라는 자바 스크립트 function함수를 호출합니다. 아래 자바스크립트를 보겠습니다.

  - Javascript

  ```javascript
function attachAddr(){
  const str = `<li>
        제목 <input type="text" name="urlTitle" id="urlTitle" maxlength="80" />
                  주소 <input type="text" name="url" id="url" maxlength="900" />
        <a href="#delete" class="btn ty_3 small" onclick="attach.deleteEmpty(this);">삭제</a>
        </li>`;
  $("#juso24").append(str); // JQuery를 이용해서 juso24라는 id값을 가져와서 그곳에 append 시킨다.
}
  ```
