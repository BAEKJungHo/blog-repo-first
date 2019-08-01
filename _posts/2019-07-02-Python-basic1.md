---
title: "Python Basic 1"
layout: post
category: Python
tags: [Python]
excerpt: "Python 기초1에는 개발환경구축, 변수, 자료형 등에 대해 나옵니다."
author: BAEKJungHo
---

* content
{:toc}

파이썬 공부를 하면서 참고한 서적입니다. _파이썬 Version 3.x_

_데이터 분석을 위한 파이썬 입문_

## Python Basic 1

  Python 기초1에는 개발환경구축, 변수, 자료형 등에 대해 배웁니다.

## 개발환경구축

  [아나콘다(Anaconda) 배포판 설치](https://www.anaconda.com/distribution/)

  `아나콘다`를 사용하면 좋은점이 기본 라이브러리 이외의 외부 라이브러리 지원과 각종 패키지 및 통합개발환경을
  한꺼번에 설치 할 수 있습니다.

  ![an1](/images/posts/201903/an1.jpg)

  자신의 알맞은 운영체제를 클릭하고 64비트와 / 32비트 중 선택해서 다운받으면 됩니다.

  Downlodas 버튼 클릭 시 기본적으로 64비트로 다운로드가 됩니다.

  다운로드가 완료되면 `Next --> I Agree`를 클릭합니다.

  ![an2](/images/posts/201903/an2.jpg)

  다음 화면에서 `All Users`를 클릭하고 Next를 누릅니다.

  ![an3](/images/posts/201903/an3.jpg)

  다음 화면에서 아래 `Register Anaconda as the system Python 3.7`을 누릅니다.

  그리고 설치를 완료해줍니다.

  - Anaconda Prompt 실행

  Anaconda Prompt를 실행 후 명령 프롬프트가 나오면 `python`을 입력하면 아래와 같은 프롬프트가 나옵니다.

  `>>>(인터프리터 프롬프트, 파이썬 프롬프트)`표시가 나오면 파이썬 코드를 입력할 준비가 완료 된 것입니다.

  - Hello World

  어떤 프로그래밍 언어를 배우던 간에 가장 먼저 입력하는 코드는 `Hello World`가 아닐까 합니다.

  > print('Hellow World') 입력

  __파이썬은 인터프리터 방식이라서 한 줄 입력후 바로 출력문이 나옵니다.__

  -----------------------------------------------------------------------------

  - 파이썬 콘솔 프로그램 종료 방법

  1. Ctrl + Z
  2. exit() 입력

  - 파이썬 코드를 텍스트 파일로 저장및 실행하는 방법

  편집기(ex) 메모장)을 열어서 파이썬 코드를 입력한 후에 확장자를 `py`로 지정해주면 됩니다.

  > 파일명.py

  저장한 파이썬 파일을 실행하기 위해서 `python 파일명.py`를 입력하면 됩니다.

## Spyder

  Spyder는 아나콘다 배포판에 있는 `통합 개발 환경(IDE : Integrated development environment)`입니다.
  Spyder에는 `IPython(Interactive Python)` 콘솔과 내장 편집기(editor)가 있습니다.

  > IPython : python 개발 환경을 개선한 것, 데이터 시각화 하는데 매우 유용하다.

  - PYTHONPATH 환경 변수 설정 방법

    1. Spyder - Tools - PYTHONPATH Manager 클릭  
    2. Add path 클릭
    3. 원하는 폴더 선택하여 넣기
    4. Synchronize 클릭 후 Yes 클릭
    5. close 클릭 후 Spyder 재시작

## Jupyter Notebook

  주피터 노트북도 Spyder와 같은 개발 환경 입니다. 코드 작성, 실행, 코드, 수식, 시가고하 자료 및
  텍스트로 이루어진 문서를 생성하고 공유 할 수 있습니다.

  주피터 노트북에서는 40가지 이상의 프로그래밍 언어를 이용할 수 있도록 지원합니다.

  - Jupyter Notebook 실행

    `Anaconda Prompt`를 실행하고 원하는 디렉터리로 이동 후에 Jupyter notebook 입력
