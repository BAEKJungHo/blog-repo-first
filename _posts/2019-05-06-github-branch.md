---
title: "Git branch란?"
layout: post
category: GitHub
tags: [Github]
excerpt: "Git branch 사용방법에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## branch

  여러 개발자들이 동시에 다양한 작업을 할 수 있게 만들어 주는 기능이 바로 `브랜치(Branch)` 입니다.
  각자 독립적인 작업 영역(저장소) 안에서 마음대로 소스코드를 변경할 수 있습니다.

  각각의 브랜치는 다른 브랜치에게 영향을 주지 않습니다. 또한 완성된 브랜치는 다른 브랜치와
  `병합(Merge)`하여 합칠 수 있습니다.

  여러 명이서 동시에 작업을 할 때에 다른 사람의 작업에 영향을 주거나 받지 않도록, 먼저 메인 브랜치에서 자신의 작업 전용 브랜치를 만듭니다. 그리고 각자 작업을 진행한 후, 작업이 끝난 사람은 메인 브랜치에 자신의 브랜치의 변경 사항을 적용합니다.

  저장소(repository)를 처음 만들면 `master branch`가 생깁니다. master에서 새로운 브랜치를 사용하기 위해 선언하지
  않는 이상은 모든 작업은 master branch에서 이루어집니다.

  Git에서는 작업에 따라 자유롭게 브랜치를 만들 수 있습니다. 그러나 이것을 효과적으로 관리하기 위해서는, 어떠한 방식으로 브랜치를 만들고 통합할 것인지 미리 정해두는 것이 좋습니다.

  > 브랜치 이름 규칙, 어떤 상황에서 브랜치를 만들지, 통합할지 등 규칙 정하기

## 브랜치의 종류

### 통합 브랜치

  통합 브랜치(Integration branch)는 언제든지 배포할 수 있는 버전을 만들 수 있어야 하는 브랜치 입니다.
  만약에 어떤 프로그램(어플리케이션)에 대해 버그 수정 혹은 새로운 기능을 추가 해야 할 때에는
  `토픽 브랜치(Topic branch) = 피처 브랜치(Feature branch)`를 만들어 사용하면 됩니다.

### 피처 브랜치

  피처 브랜치는 토픽 브랜치라고도 하며 `기능 추가나 버그 수정`과 같은 단위 작업을 위한 브랜치 입니다. 여러 개의 작업을 동시에 진행할 때에는, 그 수만큼 토픽 브랜치를 생성할 수 있습니다.

## 브랜치 생성/전환

  깃 브랜치를 만드는 방식은 다음과 같습니다.

  > git branch < branchName >

  `브랜치 전환(선택)`은 우리가 A라는 브랜치를 만들고 A 브랜치에서 어떠한 작업을 수행하기 위해서는 이 브랜치를 사용하겠다고 명시적으로 지정해주어야 합니다. 브랜치 전환하는 방법은 다음과 같습니다.

  > git checkout < branchName >
  >
  > git checkout -b < branch >  -b옵션을 주면 브랜치 생성과 체크아웃을 한꺼번에 실행

  만약에 test라는 브랜치를 생성한 후 이 브랜치의 변경사항을 master 브랜치에 병합하기 위해서는 `merge` 명령어를
  사용하면 됩니다.

  이 명령어에 병합할 커밋 이름을 넣어 실행하면, 지정한 커밋 내용이 `HEAD`가 가리키고 있는 브랜치에 넣어집니다.
  _HEAD는 현재 사용중인 브랜치에 위치_ 하게 됩니다. 따라서 test 브랜치에서 작업을 실행 한 상태이니 HEAD는
  test 브랜치를 가리키게 되고, 마스터 브랜치에 커밋내용을 병합하기 위해서는 HEAD가 master를 가리키게 해야 합니다.
  따라서 checkout 명령어를 통해 다음과 같이 입력 해줍니다.

  > git checkout master

  그리고 다음 명령어를 통해 병합 할 수 있습니다.

  > git merge < branch >

  master브랜치가 가리키는 커밋이 test 브랜치의 커밋과 같은 위치에 있게 됩니다. 이것을 `fast-forward (빨리감기) 병합`

## 브랜치 삭제

  test 브랜치의 내용이 master에 모두 병합되어 test 브랜치가 필요 없게 된 경우 다음 명령어를 통해 test 브랜치를
  삭제 할 수 있습니다.

  > git branch -d < branchName >

## 브랜치 확인

  브랜치 확인 명령어는 다음과 같이 입력하면됩니다.

  > git branch

## 두 개 이상의 브랜치를 만들고 작업하기

  예를들어 ABC라는 브랜치와 DEF라는 브랜치를 만들어 보겠습니다. 그리고 이 두 브랜치는
  testBranch.txt라는 파일을 다룬다고 가정 해보겠습니다.

  > git branch ABC , git branch DEF

  그리고 HEAD를 ABC를 가리키도록 checkout 명령어를 통해 이동해 보겠습니다.

  > git checkout ABC

  그리고 testBranch.txt파일을 수정하고 commit 내용을 "객체지향에 대한 설명 추가"라고 하겠습니다.

  > git commit -m "객체지향에 대한 설명 추가"

  그리고 이번에는 DEF 브랜치로 이동하겠습니다.

  > git checkout DEF

  그리고 testBranch.txt에 "컬렉션에 대한 설명 추가"를 하고 commit 을 해보겠습니다.

  > git commit -m "컬렉션에 대한 설명 추가"

  이제 ABC브랜치와 DEF브랜치에서 변경한 작업을 모두 master 브랜치에 통합 해보겠습니다.
  이떄 `충돌(conflict)`이 일어나는 현상까지 해결해 보겠습니다.

  먼저 HEAD가 마스터 브랜치를 가리키도록 이동 해보겠습니다.

  > git checkout master

  그리고 ABC 브랜치와 DEF브랜치를 차례대로 merge 해보겠습니다.

  > git merge ABC
  >
  > git merge DEF

  이때 DEF를 merge하는 과정에서 `충돌(conflict)`이 일어나는데 앞서 각각의 브랜치에서 변경한 내용이 testBranch.txt 의 같은 행에 포함되어 있기 때문입니다. 이때 파일을 열어 해당 오류를 일일이 직접 수정해주어야 합니다. 그리고 다시 add, commit을 실행 해주면 됩니다.

  > git add testBranch.txt
  >
  > git commit -m "conflict 해결 및 DEF 병합"

  ABC커밋과 DEF커밋을 병합할때 충돌되는 부분을 수정했으므로 그 변화를 기록하는 새로운 커밋이 생성되어 다음과 같이 이루어지게 됩니다. 이것을 `non fast-forward 병합` 이라고 합니다.

  ![nff](/images/posts/201905/nff.jpg)

  > 마지막으로 진행했던 병합명령 취소 : git reset --hard HEAD~

## rebase로 병합하기

  위에서 두 개의 브랜치를 병합할 때에는 두 개의 줄기로 브랜치가 분기 되었다가 다시 하나로 합쳐지는 것을 확인 했었습니다.
  이번에는 rebase를 이용해서 DEF를 병합할때 rebase 를 먼저 실행한 후 병합을 시도한다면 그 이력을 하나의 줄기로 만들 수도 있습니다.

  먼저 checkout 명령을 통해 DEF 브랜치로 이동 후 rebase명령으로 master 브랜치 실행

  > git checkout DEF
  >
  > git rebase master

  이 때 충돌(conflict)이 발생하면 똑같이 해결을 해주어야 합니다. rebase 의 경우 충돌 부분을 수정 한 후에는 commit 이 아니라 rebase 명령에 `--continue 옵션`을 지정하여 실행해야 합니다.  `--abort` 옵션을 주면 rebase 자체를 취소 할 수 있습니다.

  > git add testBranch.txt
  >
  > git rebase --continue

  rebase만 실행한 경우에는 DEF가 앞으로만 이동하고 master브랜치는 아직 DEF 브랜치의 변경사항이 적용되지 않았습니다.

  ![rebase1](/images/posts/201905/rebase1.jpg)

  이제 checkout으로 master로 이동하여 merge를 해주겠습니다.

  > git checkout master
  >
  > git merge DEF

  ![rebase2](/images/posts/201905/rebase2.jpg)


## 참고

  https://backlog.com/git-tutorial/kr/stepup/stepup1_1.html
