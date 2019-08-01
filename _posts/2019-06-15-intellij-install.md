---
title: "IntelliJ Install"
layout: post
category: Java
tags: [Java]
excerpt: "IDE(개발툴)인 IntelliJ를 설치하는 방법을 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## IntelliJ

  ![i1](/images/posts/201906/i1.jpg)

  자바 개발툴(IDE)에는 3가지가 있습니다.

  1. Eclipse
  2. IntelliJ(JetBrains사의 IntelliJ IDEA)
  3. NetBeans(Oracle)

### 장단점

  - __장점__
    - 상당한 IDE의 안정성
      - 이클립스를 사용하다보면 점점 프로그램이 무거워지고 특히나 플러그인 설치 충돌이 일어나거나 호환성에 문제가 간혹 발생하기도 합니다.
    - Java 개발 퍼포먼스 상승
      - Java 개발을 보통 준비시간이 상당한 시간을 차지한다고 합니다. 하지만 인텔리 J에서는 편하게 VS처럼 단계별로 설정후 프로젝트를 거의 바로 시작해도 될만큼 초기 준비시간이 단축됩니다. 또한 이클립스에 비해 Code Assist가 안정적으로 느껴집니다. (이클립스는 처음 Ctrl + Space로 Code Assist 시 렉이 발생)
    - Plugin 지원
      - 이클립스의 최대장점은 플러그인을 설치하여 편리하게 많은 확장이 가능합니다. 인텔리J에서도 동일하게 지원합니다. 제가 이것을 장점으로 둔 이유는 이클립스는 플러그인 조합도 신경써야하고 충돌이 일어날 가능성이 비교적 높은 편입니다. (최근에는 많이 줄었습니다.) 더구나 필요한 플러그인들만 있는 느낌에 설치를 해도 많이 느려지지 않는 것이 장점이라고 생각합니다.

  - __단점__
    - 프로젝트 기반의 워크스페이스 (다른 폴더구조)
      - 부연설명을 하자면 한 IDE의 창에 한개의 프로젝트만 열리는 구조입니다. VS와 같은 구조입니다.어찌보면 저는 처음 프로그래밍을 배웠을때 VS의 노예여서 그런지 크게 거부감은 없는편입니다. 하지만 웹 개발을 하면서 느낀것은 MVC 패턴을 프로젝트별로 나눌때 인텔리J에서는 정말 난감합니다. 어찌보면 Java에서는 이클립스를 많이 사용하다보니 나오는 형태인것 같기도 합니다.
    - 유료
      - 유료버전인 `Ultimate`가 있고 무료버전인 `Community`가 있습니다.

### 설치

  > [IntelliJ Install Community](https://www.jetbrains.com/idea/download/#section=windows)

  ![i2](/images/posts/201906/i2.jpg)

  위 링크를 통해서 무료버전인 Community 버전을 설치합니다. 다운로드받은 exe실행파일을 실행하면 설치가 진행되고 계속 Next를 눌러 진행하면 됩니다.

  ![i3](/images/posts/201906/i3.jpg)

  다음 화면에서 추가적인 설정 없이 `OK`를 클릭합니다.

  ![i4](/images/posts/201906/i4.jpg)

  테마를 선택하는 화면에서 원하는 테마를 클릭합니다.

  ![i5](/images/posts/201906/i5.jpg)

  설치만 할 것이기 때문에 그냥 넘어갑니다.

  ![i6](/images/posts/201906/i6.jpg)

  인텔리J가 실행되는 화면입니다.

### 폰트 설정

  - 메뉴에서 File-Settings 선택
    - 단축키 : `Ctrl + Alt + S`
  - 왼쪽 탐색창에서 Editor – Colors&Fonts - Font 선택하여 수정

  ![font1](/images/posts/201906/font1.jpg)

## 참조

  https://altkeycode.tistory.com/14
