---
title: "Forward vs Redirect"
layout: post
category: JSP
tags: [JSP]
excerpt: "Forward 방식의 화면이동과 Redirect 방식의 화면이동에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Forward

  forward를 사용하기 위해서는 보통 다음과 같은 형식을 가집니다.

  ```java
  RequestDispatcher rd = null;
  // 자바코드
  // ArrayList<StorageDTO> supplierSearchList = pDao.getTitleProducts(categoryNum, curPage);
  // request.setAttribute("supplierSearchList", supplierSearchList);
  rd = request.getRequestDispatcher("../XXX/XXX/XXX.jsp");
  rd.forward(request, response);
  ```

  즉 `RequestDispatcher` 클래스를 사용하는데 RequestDispatcher를 사용하여 forward 방식으로
  화면을 이동하게 될 경우 위에서 주석처리한 자바코드 즉 setAttribute값을 jsp파일에서 가져와
  사용할 수 있습니다.

  > forward : 데이터 값 전달(setAttribute 사용가능) + 화면이동

  Web Container 차원에서 페이지 이동만 있고, 실제로 웹 브라우저는 다른 페이지로 이동했음을 알 수 없습니다.
  따라서, 웹 브라우저에는 최초에 호출한 URL이 표시되고 이동한 페이지의 URL 정보는 볼 수 없게되며, 동일한 웹 컨테이너에 있는 페이지로만 이동할 수 있습니다. 현재 실행중인 페이지와 Forwad에 의해 호출될 페이지는 request와 response 객체를 공유합니다.

## Redirect

  redirect를 사용할 때에는 보통 다음과 같은 형식을 지닙니다.

  ```java
  // 자바코드
  // ArrayList<StorageDTO> supplierSearchList = pDao.getTitleProducts(categoryNum, curPage);
  // request.setAttribute("supplierSearchList", supplierSearchList);
  response.sendRedirect("bbsServlet?action=list&page=" + curPage);
  ```

  redirect는 forward와 달리 화면으로 이동하게 되면 위에서 주석처리한 자바코드 값들을
  jsp파일에서 사용할 수 없습니다. 만약 데이터를 넘기고 싶은 경우 setAttribute를 사용하지 않고
  위 코드처럼 url에 `?변수=값&변수=값` 형식으로 사용하여 데이터를 넘길 수 있습니다.

  > redirect : 데이터 값 전달(setAttribute 사용X) + 화면이동

  Web Container는 Redirect 명령이 들어오면 웹 브라우저에게 다른 페이지로 이동하라고 명령을 내리며, 웹 브라우저는 URL을 지시된 주소로 바꾸고 그 주소로 이동합니다. 즉, 다른 웹 컨테이너에 있는 주소로 이동이 가능합니다. 새로운 페이지에서는 request와 response객체가 새롭게 생성됩니다.
