---
title: "[AJAX] 코드로 비동기 이해하기"
layout: post
category: JQurey
tags: [JQuery, Ajax]
excerpt: "스프링에서 적용된 AJAX가 어떤 방식으로 진행되는지 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > 스프링에서 적용된 AJAX가 어떤 방식으로 진행되는지 배워 봅시다.

## 코드로 이해하는 AJAX 동작 방식

  파일관리시스템을 개발하면서 jstree 라이브러리를 사용하여 비동기로 처리하는 코드를 구현해야 했습니다.

  먼저 비동기 사용을 위해 JSP에 Javascript를 Import 시켜야 합니다.

  ```html
  <!-- jstree css를 사용하기 위해 추가 -->
  <link rel="stylesheet" href="<c:url value="/js/jstree/dist/themes/default/style.css"/>"/>
  <!-- jstree 스크립트를 사용하기 위해 추가 -->
  <script src="<c:url value="/js/jstree/dist/jstree.js"/>"></script>
  <script type="text/babel" data-presets="es2015,stage-2" charset="UTF-8">
      const SEARCH_TYPE = '${type}'; // ENUM 상수를 자바스크립트에서 사용하려고 추가
  </script>
  ```

  `data-presets="es2015,stage-2" charset="UTF-8"` 이걸 추가하는 이유는 최신문법을 es2015 이전으로 컴파일해서 구형 브라우저에서도 실행이 되도록하는건데 이 역할을 하는게 `babel`입니다.

  처음 네비게이션 메뉴에 있는 CSS/JS/JSP 등 목록을 클릭하면 해당 TYPE의 문자열이 자바스크립트로 타게 됩니다.

  ```javascript
  $(() => {
      fileManage.init();
  });
  ```

  위 함수를 먼저 실행해서 fileManage.init()을 찾아 실행시킵니다.

  ```javascript
  const fileManage = {
  	$fileTree: $('#treeRoute'),
  	init () {
  		const that = this;
  		// js
  		requestFileTree(SEARCH_TYPE)
  			.then(res => {
  				// 성공
  				that.$fileTree.jstree({
  					"core": {
  						"animation": 5,
  						"check_callback": true,
  						"multiple": false,
  						"data" :  res, // data 를 json형식으로 ... { id: 고유값, parent: 부모키값, text: 출력될이름 }
  						// { id: 고유값, parent: 부모키, 부모가없다면 '#', sort: 순서, text: 출력될 이름 }
  					},
  					"plugins": ["dnd", "wholerow", "types", "contextmenu"],
  				}).on('changed.jstree',that.view);
  			})
  			// 에러
  			.catch(err => console.error(err));
  	},
  	view (e, data) {
  		const seq = data.node.original.id;
  		userMenu.form.seq.value = seq;
  		getMenu(seq)
  			.then(renderData)
  			.catch(err => {
  				console.error(err);
  			});
  		$menuGroup.addClass('on');
  	},
  };
  ```

  그리고 requestFileTree(SEARCH_TYPE) 함수를 타고 들어갑니다.

  ```javascript
  const requestFileTree = (type) => {
    // 대리자 역할 , 콜백지옥 해소
    // Promise 객체 리턴
    // 성공시 resolve() 호출,
    // 실패시 reject() 호출
    // /mec/fileManage/js
    return new Promise((resolve, reject) => {
      $.ajaxGet(`${CONTEXT_PATH}/xxx/fileManage/${type}/api`)
          .done(res => resolve(res))
          .fail(err => reject(err));
      });
  };
  ```

  requestFileTree 상수에서 type에 따라 `${CONTEXT_PATH}/mec/fileManage/${type}/api`로 요청을 보냅니다.
  요청 성공과 실패에 따라 const fileManage로 가서 성공과 실패에 따라 다르게 작동합니다.

  비동기 처리를 할 때 컨트롤러에서는 @RequestMapping("/xxx/xxx/api") 마지막에 api를 적어서, 이 컨트롤러는 비동기 처리를 하는 컨트롤러임을 명시해줍니다.

  ```java
  @RestController
  @RequestMapping("/xxx/fileManage/{type}/api")
  @RequiredArgsConstructor
  public class FileManageRestController { }
  ```

  일반적인 URL요청에 따른 스프링 작동 방식은 다음과 같습니다. (요청 -> GET /xxx/fileManage/js/api)

  - DispatcherServlet -> 요청 분석
  - HandlerMapping이 적합한 컨트롤러를 찾음
  - HandlerAdapter가 적함한 메서드를 찾음
  - Adapter로 HandlerMethod를 실행해서 요청을 처리
  - returnType을 가지고 ViewResolver가 prefix와 return suffix를 조합해서 View를 보내준다.

  반면, 비동기처리를 하기 위해서는 `@ResponseBody` 어노테이션을 사용하며(@RestController 어노테이션 사용시 생략 가능), @ResponseBody 어노테이션이 View를 리턴하는게 아니라 `응답 본문`으로 리턴을합니다.

  ContentsNegotiationViewResolver -> accept-header 어떤 응답을 원하는지 분석

    - text/html
      - InternalResourceViewResolver, Tiles...

    - application/json, xml 등
      - ViewResolver가 없으므로 알맞은 `MessageConverter`를 사용
      - Jackson, JAXb2 ..
      - MessageConverter로 응답 변환해서 응답 본문으로 쏴준다.

  `ContentsNegotiationViewResolver`가 text/html 혹은 application/json 등 InternalResourceViewResolver, TilesViewResolver 등을 탈지 MessageConverter를 사용할지 정합니다.

  > ContentsNegotiationViewResolver는 스프링에서는 따로 web.xml에 설정을 해야하는데, 스프링 부트에서는 자동으로 설정을 해줍니다.

  자바스크립트를 통해 HTTP 요청으로 type 변수를 받아왔습니다. 하지만, 받아온 type 변수는 String이지만, 이것을 Enum으로 변환해야 하므로
  Formatter를 이용합니다.

  __그리고 비동기 처리 방식이므로 HandlerMethod에서 return 값이 view가아닌 JSON으로 변환된 객체여야 합니다.__

  원래라면 JSON 형식을 사용하기 위해서는 라이브러리를 사용해야하는데 GSON, JACKSON 등이 있습니다.

  스프링은 JSON Format을 사용하기 위해서는 `JACKSON 라이브러리`를 사용하는데, 사용하기 위해서는 스프링에서는 수동으로 등록해야 하며, 스프링 부트는 기본으로 포함 되어 있습니다.

  `import com.fasterxml.jackson.databind.ObjectMapper;` JACKSON 라이브러리에서 `ObjectMapper`를 제공합니다.

  ```java
  @Autowired
    private ObjectMapper objectMapper;
  ```

  __JACKSON라이브러리에서 제공하는 ObjectMapper는 객체를 JSON 문자열로 변환 시키기 위해서 사용합니다.__

  스프링 부트에서 컨트롤러에서는 ObjectMapper를 사용하지 않아도 되는데, 그 이유는 ContentsNegotiationViewResolver가 application/json, xml등의 요청이 오면 MessageConverter를 사용하여 json일 경우 JACKSON등의 라이브러리를 사용해서 ObjectMapper를 사용하여 자동으로 JSON으로 변환해줍니다. 따라서, 스프링 부트의 컨트롤러에서는 JSON으로 Parsing 하기위한 코드를 작성하지 않아도 됩니다.

## String to Json

  문자열을 json으로 바꾸려면 객체에 담아서 바꿔야 합니다.

  Json은 key, value로 되어있기 때문에, key, value로 되어있는 객체의 타입만 변환이 가능합니다.

  따라서 문자열을 DTO에 String 속성을 만들어 담아서 XXXDTO 객체를 사용하여 json으로 변환해야합니다.

  ```java
  @Getter
  @Setter
  public class XXXDTO {
    private String parseJson;
  }
  ```

  ```java
  @Controller
  @RequestMapping("/xxx/fileManage/{type}/api")
  @RequiredArgsConstructor
  public class FileManageRestController {

    private final FileManage fileManage;

    @GetMapping
    public FileManage ajaxMethod(.....) {
        .....
        return XXXDTO;
    }

  }
  ```
