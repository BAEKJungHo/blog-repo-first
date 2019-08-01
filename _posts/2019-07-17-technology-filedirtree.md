---
title: "디렉터리와 파일을 계층구조로 나타내기"
layout: post
category: Technology
tags: [Java]
excerpt: "디렉터리 내에 있는 파일과 디렉터리를 계층구조로 표현하는 방법에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 디렉터리 내에 있는 파일과 디렉터리를 계층구조로 표현하는 방법에 대해서 배워 봅시다.

## Directory와 File을 TREE(계층구조)로 나타내기

  ![rs2](/images/posts/201907/rs2.jpg)

  위 와같은 디렉터리와 파일구조가 있다고 할 경우, 이 구조를 웹에서 보여주기 위해서 어떻게 로직을 짜야하는지에
  대해서 배워봅시다.

  위 형식을 `JSON` 구조로 나타내면 다음과 같습니다.

  ```json
  {
    "name":"root"
    "children":[{"name":"DirectoryA" "children":[{"name":"DirectoryA-1" "children":...}]}]
  }
  ```

  가장 바깥이 `{ } = 객체`로 되어있고, 객체 내부에는 name이라는 `String` 변수명과 children이라는
  `배열(리스트)`를 가지고 있으며, 배열안에는 다시 가장 바깥 객체를 가지고 있습니다. 이 개념을 자바 코드로 나타내기 위해서
  DTO를 생성하면 다음과 같습니다.

  ```java
  @Getter @Setter
  public class FileManageDto {
    private String name;
    private List<FileManageDto> fileManageDto;
  }
  ```

  객체 내부에는 문자열 타입의 name변수와 자기 자신 객체를 리스트로 가지고 있는 fileManageDto 리스트 참조 변수를 가지며,
  Getter와 Setter를 가지고 있습니다.

  위 처럼 DTO안에 List속성을 가지고 있어도 되며, 메서드 파라미터로 리스트를 주입받는 형식으로 로직을 구성해도 됩니다.

  여기서 중요한 특징이 리스트의 제네릭 타입이 자기 자신인 FileManageDto 객체이므로, 로직을 구성할 때 `재귀(Recursion)`가 들어감을 알 수 있습니다.
  디렉터리안에는 파일과 디렉터리가 존재할 수 있지만, 파일은 자기자신이 마지막이므로, 디렉터리를 만나게 되면 재귀를 호출하는 것입니다.

  __재귀의 특성 중 하나는 스택(Stack)의 특성을 들고 다닙니다.__

  ![rs3](/images/posts/201907/rs3.jpg)

  DirectoryA를 만나면 스택 바닥에 함수 A가 실행되는 채로 들어갑니다. 그리고 다시 디렉터리A-1을 만나면, 함수 A가 실행되는 채로 바닥에 들어갑니다.
  다시 DirectoryA-1-1을 만나면 함수 A가 실행되는 채로 스택에 들어가며, __파일을 만나게 될 경우 재귀함수가 종료됩니다.__ 종료가 될 때에는 DirectoryA-1-1이 먼저 종료되고 DirectoryA-1, DirectoryA 순으로 함수가 종료됩니다. 이 개념을 자바 코드로 구현하면 다음과 같습니다.

  루트경로를 파라미터로 받고, 파일과 디렉터리 존재를 확인하면서, 디렉터리를 만날경우 재귀함수를 호출하여 리스트에 넣는 방식입니다.

  파일을 TREE처럼 나타내기 위해서는 `jsTree` 라이브러리를 사용해야하며, jsTree 라이브러리를 사용하기 위해서는 JavaScript에서 "data" : res,의 res 값으로 `id : 고유값, parent : 부모키값, sort : 순서, text : 출력될 이름` 을 전달해야합니다.

  > jsTree Library : [https://www.jstree.com/demo/](https://www.jstree.com/demo/)

## CODE

```java

/** Recursion + Stack */
public FileManageDto getDirFileList(List<FileManageDto> result, String root, Integer parent, Integer sort) {
  File dir = new File(root);
  File[] dirAndFileList = dir.listFiles();

  int parentId = autoIncrementId();
  FileManageDto parentDto = new FileManageDto(parentId, parent == null ? "#" : String.valueOf(parent), sort++, dir.getName(), null);

  try{
    Integer childSort = 1;
    for(int i=0; i<dirAndFileList.length; i++){
      File file = dirAndFileList[i];
      if(file.isFile()) {
        FileManageDto subDto = new FileManageDto(autoIncrementId(), String.valueOf(parentId), childSort++, file.getName(), "jstree-file");           result.add(subDto);
      } else if(file.isDirectory()) {
        // 서브디렉토리가 존재하면 재귀적 방법으로 다시 탐색
        getDirFileList(result, file.getCanonicalPath(), parentId, childSort++);
      }
    }
  }catch(IOException e){
    throw new RuntimeException(e);
  }
  result.add(parentDto);
  return parentDto;
}
```
