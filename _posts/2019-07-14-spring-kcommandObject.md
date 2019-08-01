---
title: "커맨드 객체(Command Object)"
layout: post
category: Spring
tags: [Spring]
excerpt: "커맨드 객체(Command Object)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 커맨드 객체(Command Object)에 대해서 배워 봅시다.

## 커맨드 객체(Command Object)

  커맨드 객체란? 쉽게 말해서 VO(DTO)와 같다고 생각하셔도 됩니다. 즉, 커맨드 객체가 되기 위한 조건은
  `Getter와 Setter`가 필수적으로 있어야 합니다.

  커맨드 객체의 역할에는 크게 3가지로 나뉩니다.

  1. 컨트롤러에서 View로 바인딩 : View단에서 `form:form 태그`를 사용하는 경우
  2. View에서 컨트롤러로 바인딩 : View 단에서 `input type="text" 혹은 input type="hidden"` 으로 값을 컨트롤러로 전송하는 경우
  3. 컨트롤러에서 Mapper.xml로 바인딩 : Mapper.xml에서 title = #{title}, contents = #{contents}처럼 사용하는 경우, 커맨드 객체를 통해 #{변수명}과 커맨드 객체의 필드명을 통해 바인딩 해주는 경우

### 컨트롤러에서 View로 바인딩

  컨트롤러에서 View로 바인딩를 해주는 경우는 View에서 `form:form(form 커스텀 태그)`를 사용하는 경우 입니다.

  - 커맨드 객체(VO, DTO)

  ```java
  public class BoardDTO {
    private String title;
    private String contents;

    public String getTitle() {
      return this.title;
    }

    public void setTitle(String title) {
      this.title = title;
    }

    public String getContents() {
      return this.contents;
    }

    public void setContents(String contents) {
      this.contents = contents;
    }
  }
  ```

  위 처럼 커맨드 객체가 있을때 View단에서 form:form을 사용하는경우 model.addAttribute로
  커맨드 객체를 담아주지 않으면, form이 생성되지 않습니다. 그 이유는 __form:form은 커맨드 객체의
  Property(속성, 필드)로 View를 생성__ 하기 때문입니다.

  form:form을 사용하기 위한 커맨드 객체를 만들었고, 그다음 Controller단에서 model.addAttribute로 커맨드 객체를 담아서 보내야 합니다.

  - Controller

  ```java
/** form 커스텀 태그 사용 : 바인딩을 위해 boardDTO 객체를 Model로 담아서 넘겨줌 */
@RequestMapping(value="/boardWrite", method=RequestMethod.GET)
public String boardWrite(Model model) {
  model.addAttribute("boardDTO", new BoardDTO());
  return "boardWrite";
}
  ```

  Controller에서 View로 보낼때는 `폼 생성을 위한 바인딩`이므로 Setter를 통한 실제 값이 담겨있지 않아도 됩니다.
  단, form:form태그에서 __commandName="model.addAttribute의 키값", path="바인딩될 커맨드 객체의 속성(필드)명"__ 이 부분을 정확히
  맞춰줘야 합니다.

  ```html
  <form:form commandName="boardDTO" mehtod="post" enctype="multipart/form-data">
  <table border="1">
    <tr>
      <th><form:label path="title">제목</form:label></th>
      <td>
        <form:input path="title" />
        <form:errors path="title" />
      </td>
    </tr>
    <tr>
      <th><form:label path="contents">내용</form:label></th>
      <td>
        <form:input path="contents" />
        <form:errors path="contents" />
      </td>
    </tr>
    <tr>
      <!-- 다중 업로드 버전 -->
      <td><input multiple="multiple" type="file" name="file" /></td>
    </tr>
  </table>
  <div>
    <input type="submit" value="등록">
    <a href="<c:url value="/boardSearchList" />"> 목록</a>
  </div>
</form:form>
  ```

  위 폼에서 input을 통해 입력을 하고, 등록(submit) 버튼을 클릭하게 되면, 이제 View에서 Controller로 바인딩이 됩니다.

### View에서 컨트롤러로 바인딩

  ```java
@RequestMapping(value="/boardWrite", method=RequestMethod.POST)
public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
  if(bindingResult.hasErrors()) {
    return "boardWrite"; // ViewResolver로 보냄
  } else {
    return "redirect:/boardSearchList";
  }
}
  ```

  등록 버튼을 누르게 되면 POST 방식으로 지정된 boardWrite 컨트롤러로 오게 됩니다.

  boarsWrite의 파라미터를 보면 `BoardDTO boardDTO`라고 커맨드 객체가 명시 되어있습니다.

  그 이유는 __View의 폼에서 입력한 값을, 파라미터(boardDTO)를 통해 알아서 Setter를 불러와 바인딩을 시켜줍니다.__

  즉, 파라미터 boardDTO의 title 필드와, contents 필드에는 폼에서 입력한 값이 담겨 있습니다.

### 상위 클래스를 상속받은 하위 클래스의 커맨드 객체 사용

  예를들어 게시판 페이지 기능을 구현할 때에 상위 클래스인 Criteria(page와 perPageNum 속성을 가지고 있음)와
  이를 상속받은 검색 기능을 구현하기 위한 SearchCriteria(searchType과 keyword 속성을 가지고 있음)가 있을 경우

  SearchCriteria searchCriteria와 같이 하위 클래스를 커맨드 객체로 사용하는 경우에 SearchCriteria는 Criteria의 필드(속성)까지
  받아서 바인딩 시켜줄 수 있습니다.

  즉, View단에서 page, perPageNum, searchType, keyword에 대한 값을 컨트롤러로 넘겨 받을 때에, 컨트롤러에서 메소드 파라미터로
  SearchCriteria 커맨드 객체만 있으면, Criteria 커맨드 객체를 명시하지 않아도 알아서 바인딩 시켜줍니다.

### 컨트롤러에서 Mapper.xml로 바인딩

  View에서 입력한 값을 컨트롤러에서 커맨드 객체를 사용하여 값을 바인딩 하고, 커맨드 객체에 저장된 값을 Service와 DAO를 통해 Mapper.xml로
  값을 전송해야 하는경우, 커맨드 객체를 사용하면 알아서 바인딩 해줍니다.

  - DAO Interface

  ```java
  // DAO Interface
  public void insert(BoardDTO boardDTO);
  ```

  DAO Interface를 선언할 때에 insert를 통해서 여러 값을 넣어야 하는데, 그 속성들이 커맨드 객체에 전부 포함되는 속성이므로
  파라미터로 커맨드 객체를 선언해서 받습니다.

  만약 public void insert(String title, String contents)와 같이 받는 경우는 코드도 길어지고 비효율적이므로, 가급적 커맨드 객체를 사용하는것이
  낫습니다.

  - boardMapper.xml

  ```xml
<insert id="insert" parameterType="boardDTO">
  INSERT INTO b_board (title, contents)
  VALUES (#{title},#{contents})
</insert>
  ```

### 실무에서는 가급적 커맨드 객체 사용

  위에서 커맨드 객체의 역할에 대해서 배웠습니다. 정말 어마무시한 역할을 가지고 있고,
  가급적 커맨드 객체에 대한 속성을 파라미터로 지정하고, 나열하여 Mapper.xml까지 보내지말고, 커맨드 객체를 사용하여 보내는 것이 좋습니다.
