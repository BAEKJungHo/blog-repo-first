---
title: "Feature Branch Workflow"
layout: post
category: GitHub
tags: [Github]
excerpt: "GitHub로 협업하는 방법 중 하나인 Feature Branch Workflow에 대해 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

git branch 관련 글은 [KWON heejeong](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html) 블로그 글을
인용하였습니다. 더 자세한 내용은 해당 블로그 접속!

## Feature Branch Workflow

  GitHub에서 Feature Branch Workflow를 이용하여 협업하는 방법에 대해 배워 보겠습니다.
  `Feature Branch Workflow`의 핵심은 __기능별 브랜치__ 를 만들어서 작업하는 것입니다. 다수의 팀 구성원이
  메인 코드 베이스(master)를 기준으로 안전하게 새로운 기능을 개발 할 수 있습니다.
  또한 Feature Branch Workflow와 Pull Request를 결합하면 팀 구성원간에 변경 내용에 대한 소통을 촉진해서 코드 품질을 높일 수 있습니다.

  Feature Branch Workflow는 `소규모 인원 프로젝트`에서 사용하는 협업방법 입니다.

  > [Feature Branch Workflow로 협업하기](https://medium.com/@lyoungh2570/gitlab-%ED%8C%80-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-cda74702b334)

## 저장소 개념

  - __중앙 원격 저장소__

    - 여러 명이 같은 프로젝트를 관리하는데  사용하는 그룹 계정의 중립된 원격 저장소
    - Organization을 만드는 방법은 GitHub에서 “+” 아이콘을 클릭하고 메뉴에서 “New organization”을 선택
    - Organization의 사용자와 저장소는 팀(Team)으로 관리되며, 저장소의 권한 설정도 팀으로 관리

  - __원격 저장소__

    - remote repository, 파일이 깃허브 전용서버에서 관리되는 원격저장소(즉, 깃허브 저장소)

  - __로컬 저장소__

    - local repository, 내 PC에 파일이 저장되는 개인 전용 저장소

## 프로젝트 협업 과정

### 기본 세팅

  중앙 원격 저장소(Organization)와 Project를 만들어야 합니다. Project를 만들때 중앙 원격 저장소의 리퍼지토리를
  연결(Link) 시켜줘야 합니다.

  - Create Organization
    - Github 상단 페이지 상단에서 + 아이콘 클릭 하여 New organization 선택
    - 생성 완료 후 화면
    - ![or1](/images/posts/201905/or1.jpg)

  - Project 생성
    - Oragnization에 들어가 상단에 있는 Project를 클릭하고 생성
    - repository와 연결(Link)
    - 생성 완료 후 화면
    - ![or2](/images/posts/201905/or2.jpg)

### 1. git clone

  프로젝트 참여자는 `git clone`명령어를 통해 중앙 원격 저장소를 복제 해와서 자신의 로컬 저장소에서 작업할 수 있습니다.

  > git clone [중앙원격저장소 리퍼지토리 URL]

  ![clone](/images/posts/201905/clone.jpg)

### 2. Feature Branch 생성

  새로운 기능 개발을 위해 피처 브랜치(Feature Branch)를 생성 해야 합니다.
  브랜치 생성 > 브랜치에 Commit > 중앙 원격 저장소에 Push

  - git branch feature/login (로그인 기능 개발을 위한 브랜치 생성)

  ```java
  git checkout -b feature/login
  ```

  ![check](/images/posts/201905/check.jpg)

  - feature/login 브랜치에 로그인 기능에 대한 커밋(commit)을 추가

  ```java
  $ git commit -a -m "Write commit message"

  # 위의 명령어는 아래의 두 명령어를 합한 것
  $ git add . # 변경된 모든 파일을 스테이징 영역에 추가
  $ git add [some-file] # 스테이징 영역에 some-file 추가
  $ git commit -m "Write commit message" # local 작업폴더에 history 하나를 쌓는 것
  ```

  - 커밋 완료 후 중앙 원격 저장소에 push 하기

  ![push](/images/posts/201905/push.jpg)

  ```java
  # -u 옵션: 새로운 기능 브랜치와 동일한 이름으로 중앙 원격 저장소의 브랜치로 추가한다.

  // 로컬의 기능 브랜치를 중앙 원격 저장소 (origin)에 올린다.
  $ git push -u origin feature/login

  // -u 옵션으로 한 번 연결한 후에는 옵션 없이 아래의 명령만으로 기능 브랜치를 올릴 수 있다.
  $ git push -origin feature/login
  ```

### 3. Pull requests

  프로젝트 관리자에게 풀 리퀘스트(풀리퀘)를 던져야 합니다. 만약 feature/storageModel이라는 브랜치를 만들고
  StorageDAO, DTO를 생성하여 git push 까지 마친 상태면 아래와 같이 나올 것입니다.

  - ![pl1](/images/posts/201905/pl1.jpg)
  - compare & pull request 클릭 후 내용을 적고 create pull request 클릭
  - ![pl2](/images/posts/201905/pl2.jpg)

  프로젝트 관리자(소규모 팀에서는 모두가 관리자가 될 수 있음)에게 자신의 기여분을 중앙 원격 코드 베이스에 반영해 달라고 요청해야 한다. 새로 만든 기능 개발용 브랜치도 중앙 저장소에 올려서 팀 구성원들과 개발 내용에 대한 의견(코드 리뷰 등)을 나눌 수 있다.

  - 장점
    - master 브랜치를 손대지 않기 때문에 다른 기능 개발 브랜치를 얼마든지 올려도 된다.
    - 일종의 로컬 저장소 백업 역할을 하기도 한다

  > 풀 리퀘스트(Pull Request) 란?
  >
  > 기능 개발을 끝내고 바로 master에 merge하는 것이 아니라 branch를 중앙 원격 저장소에 올리고 master에 merge 해달라고 요청하는 것

  Github Page에서 `Pull requests` 버튼을 사용하면 어떤 branch를 제출할 것인지 정할 수 있습니다.

### 4. Merge

  프로젝트 관리자는 변경 내용을 확인한 후에 중앙 원격 코드 베이스에 병합(Merge)을 해야 합니다.

  ![merge](/images/posts/201905/merge.jpg)

  이후에는 모든 팀원이 변경한 코드 내용을 확인하고 마지막으로 확인한 팀원이 변경 내용을 중앙 원격 코드 베이스에 병합(merge)하는 작업을 한다.

  GitHub 페이지에서 `Pull requests` 버튼을 누른 후, File changed 탭에서 변경 내용을 확인한다.
  Conversation 탭으로 이동하여 `Confirm merge`를 하면 중앙 원격 코드 베이스에 병합(merge)된다.
  충돌이 일어난 경우는 팀원들고 합의 하에 충돌 내용을 수정한 후 병합을 진행한다.

### 5. 중앙 원격 저장소와 자신의 로컬 저장소 동기화

  중앙 원격 저장소와 자신의 로컬 저장소 동기화를 위해 branch를 master로 이동(즉, HEAD 이동)

  ![pull1](/images/posts/201905/pull1.jpg)

  ```java
  // 마스터 브랜치로 이동하여 git pull로 당겨온다.
  // Oragnization에 있는 저장소와 동기화를 위해(최신화)
  $ git checkout master
  $ git pull
  ```

  ![pull2](/images/posts/201905/pull2.jpg)

### 6. 중앙 원격 저장소의 코드 베이스에 새로운 커밋이 있다면 가져오기

  중앙 원격 저장소(origin)의 메인 코드 베이스(master)가 변경되었으므로, 프로젝트 참여하는 모든 개발자가 자신의 로컬 저장소를 동기화해서 최신 상태로 만들어야 한다.

  > git pull origin master

### 7. 새로운 기능 개발을 위해 master로 이동해 새로운 branch 생성

  중앙 원격 저장소와 동기화가 완료된 자신의 로컬 저장소 master branch에서 새로운 기능 개발을 위한 branch 생성

  > git branch feature/register

  local repository에서 완성한 이전 작업 브랜치를 삭제해도 자신의 remote에는 남아 있습니다.

## 요약

  1. 중앙 원격 저장소(Organization) 생성
  2. Oragnization에서 repository 생성
  3. Oragnization에서 Project 생성(repository와 연결-Link)
  4. 자신을 포함한 각 팀원들이 git clone으로 해당 리퍼지토리 복제
  5. 각자 자신의 Local repository에서 branch를 생성
  6. 기능 개발(ex) 자바 파일 추가 및 프로그래밍 등)
  7. 기능 개발 완료되면 아래와 같이 진행
  7. git add *
  8. git commit -m "기능에 대한 설명"
  9. git push -u origin [branch name]
  10. Github 페이지에서 pull request 생성
  11. 관리자가 Confilct 해결하고, merge
  12. 자신을 포함 팀원들이 저장소와 Local repository를 최신으로 동기화 하기위해 다음 과정 진행
  13. git checkout master (마스터 브랜치로 이동)
  14. git pull (동기화)
  15. git branch -d [branch name] (브랜치 삭제)
  16. 새로운 기능 개발을 위한 브랜치 생성
  17. 위 과정을 반복 진행

## 참고

  https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html

  https://gmlwjd9405.github.io/2018/05/12/how-to-collaborate-on-GitHub-3.html
