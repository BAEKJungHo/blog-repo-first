---
title: "Atom 에디터 설치방법"
layout: post
category: HTML5
tags: [Javascript, HTML5, CSS3]
excerpt: "Atom 에디터 설치방법과, 한글언어 설정 방법에 대해 알아봅시다."
author: BAEKJungHo
---

* content
{:toc}

Atom 에디터 설치방법과, 유용한 패키지 및 한글언어 설정 방법에 대해 알아봅시다.

[Atom Homepage에 접속하여 설치](https://atom.io)

## 다운로드 및 설치

  ![a1](/images/posts/201903/a1.jpg)

## Naver D2 coding 설치

   naver d2 coding 알집 파일을 다운받고, 열어서 일일이 모든 폰트를 설치 해준다. 파워포인트/한글/엑셀등 OFFICE를 열고, 폰트(Font)설정 부분에서 Naver d2 coding
   복사 후 → Atom → file → settings → editor → FontFamily 부분에 붙여넣는다. 폰트 변경 후에 아톰을 다시 실행 시켜야, Naver d2 ligature 폰트가 적용된다.

   ![a2](/images/posts/201903/a2.jpg)

## 유용한 패키지 설치

  ▶ atom-beautify
  ▶ file-icons
  ▶ indent-guide-improved
  ▶ atom-live-server
  ▶ minimap
  ▶ emmet
  ▶ todo
  ▶ pigments
  ▶ ask-stack
  ▶ highlight-selected
  ▶ linter
  ▶ stack-overflow

  intall → 설치할 패키지 이름 검색 → 다운로드 횟수가 가장 많은 패키지 다운로드 → 다운로드가 완료되면 파란색 부분처럼 표시가 된다.
  → 설치한 패키지를 사용하는 방법은 상단에 있는 MenuBar에서 Packages 클릭 후 사용

  ![a3](/images/posts/201903/a3.jpg)

## Emmet 패키지 설치 후 한글언어 설정

  - 설치한 emmet → settings 클릭
  - viewcode 클릭    

  ![a4](/images/posts/201903/a4.jpg)

  - emmet → node_modules → lib → snippets.json 클릭

  ![a5](/images/posts/201903/a5.jpg)

  - 빨간색 박스로 표시된 부분에서 “lang” : “en” 부분을 “ko”로 수정하고 “locale” : “en-UR”로 표시된 부분을 “ko-KR”로 수정하면 완료된다.  

  ![a6](/images/posts/201903/a6.jpg)
