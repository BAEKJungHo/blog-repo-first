---
title: "[AJAX] Image를 비동기 방식으로 업로드"
layout: post
category: JQurey
tags: [JQuery, Ajax]
excerpt: "Image를 비동기 방식으로 업로드하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Image를 비동기 방식으로 업로드하는 방법에 대해서 배워 봅시다.

## Image를 비동기 방식으로 업로드

### JSP

  Jsp에서 파일과, 이미지에 따라 버튼을 다르게 보여주기 위해서, FileTypes ENUM 상수로 비교, c:choose 사용

  ```html
  <div class="controls">
      <s:eval expression="T(XXX/fileManageHistory.FileTypes).IMAGE == type" var="isImage"/>
      <c:choose>
          <c:when test="${isImage}">
              <a href="#update" class="btn ty_1" onclick="fileManage.imageUpdate();">이미지 수정</a>
              <input type="file" id="files" />
          </c:when>
          <c:otherwise>
              <a href="#update" class="btn ty_1" onclick="fileManage.update();">파일 수정</a>
          </c:otherwise>
        </c:choose>
    </div>
  ```

### JS

  파일 업로드를 위한 비동기 처리

  ```javascript
const requestImageUpdate = (filePath, type) => {
    return new Promise((resolve, reject) => {
      const formData = new FormData();
      formData.append('filePath', filePath);
      formData.append("file", document.querySelector('#files').files[0]);
      console.dir(document.querySelector('#files').files[0]);
      $.ajax({
         url: `${CONTEXT_PATH}/fileManage/${type}/api`, // 컨트롤러 타는 url
         processData: false,
         contentType: false,
         enctype: 'multipart/form-data', // 파일 업로드를 위해 enctype 지정
         method: METHOD.POST,
         data: formData, // const formData 상수명
      }).done(res => resolve(res))
        .fail(err => reject(err));
    });
};
  ```

### Controller

```java
@PostMapping
  public ResponseEntity modImg(@PathVariable FileTypes type, MultipartFile file, String filePath) throws Exception {
      if (type != FileTypes.IMAGE && file.getSize() <= 0) {
          return ResponseEntity.badRequest().body(Message.BAD_REQUEST.getMsg());
      }
      String realPath = type.getPath() + filePath.replaceFirst(filePath.split("/")[0], "");

      fileManageService.overWriteImage(realPath, file);
      return ResponseEntity.ok().body(Message.UPDATED.getMsg());
  }
```

### ServiceImpl

- 서비스 overWriteImage MultipartFile의 getBytes()메서드 이용, Apache Commons IO의 FileCopyUtils.copy() 이용
original객체에 MultipartFile로 가져온 파일의 내용을 바이트로 씁니다. 이미 존재하는 경우 덮어씁니다.

```java
@Override
  public void overWriteImage(String realPath, MultipartFile file) {
      File original = getFileByPathToFile(realPath);
      try {
          FileCopyUtils.copy(file.getBytes(), original);
      } catch (IOException e) {
          throw new RuntimeException("이미지 수정중 오류 발생");
      }
  }
```
