---
title: "Branch의 종류"
layout: post
category: GitHub
tags: [Github]
excerpt: "Git branch의 종류에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

git branch 관련 글은 [KWON heejeong](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html) 블로그 글을
인용하였습니다. 더 자세한 내용은 해당 블로그 접속!

## Branch의 종류

  Branch의 종류에 대해서 배워 보겠습니다.

  -----------------------------------------------------------------------------

### Master branch

  master branch란 배포(Release) 이력을 관리하기 위해 사용되며 __즉, 배포 가능한 상태만을 관리__ 한다.

  ![master](/images/posts/201905/master.jpg)

  -----------------------------------------------------------------------------

### Develop branch

  develop branch란 __다음 출시 버전을 개발하는 브랜치__ 를 의미합니다.

  기능 개발을 위한 브랜치들(Feature branches)을 병합하기 위해 사용되며 즉, 모든 기능이 추가되고
  버그가 수정되어 배포 가능한 안정적인 상태라면 develop branch를 `master branch`에 merge합니다.
  평소에는 이 브랜치를 기반으로 개발을 진행 합니다.

  ![develop](/images/posts/201905/develop.jpg)

  -----------------------------------------------------------------------------

### Feature branch

  feature branch란 기능 개발이나 버그 수정등을 하기 위해 __develop 브랜치로부터 분기__ 합니다.

  feature branch에서의 작업은 팀원들과 공유할 필요가 없기 때문에 자신의 local repository에서 관리합니다.
  개발이 완료되면 develop branch로 merge 하여 다른 사람들과 공유합니다.

  > feature branch Naming
  >
  > feature/기능요약 - 다음과 같은 형식을 사용 ex) feature/register

  ![feature](/images/posts/201905/feature.jpg)

  - __과정__
    - develop 브랜치에서 새로운 기능에 대한 feature 브랜치를 분기한다.
    - 새로운 기능 및 버그 수정에 대한 작업 수행한다.
    - 작업이 끝나면 develop 브랜치로 병합(merge)한다.
    - 더 이상 필요하지 않은 feature 브랜치는 삭제한다.
    - 새로운 기능에 대한 feature 브랜치를 중앙 원격 저장소에 올린다.(push)

    ```java
    // feature 브랜치(feature/login)를 'develop' 브랜치에서 분기
    // 'master' 브랜치에서 따는 것이 아닙니다.
    $ git checkout -b feature/login develop

    /* ~ 새로운 기능에 대한 작업 수행 ~ */

    /* feature 브랜치에서 모든 작업이 끝나면 */
    // 'develop' 브랜치로 이동한다.
    $ git checkout develop
    // 'develop' 브랜치에 feature/login 브랜치 내용을 병합(merge)한다.
    $ git merge --no-ff feature/login
    // -d 옵션: feature/login에 해당하는 브랜치를 삭제한다.
    $ git branch -d feature/login
    // 'develop' 브랜치를 원격 중앙 저장소에 올린다.
    $ git push origin develop
    ```

  - __--no-ff 옵션__

    - feature 브랜치에 존재하는 모든 커밋 이력을 하나로 합쳐 새로운 커밋(Commit) 객체를 만들어 develop 브랜치에 merge 한다.

    ![feature2](/images/posts/201905/feature2.jpg)

  -----------------------------------------------------------------------------

### Release branch

  release branch는 __이번 출시 버전을 준비하는 브랜치__ 입니다.

  배포를 위한 전용 브랜치를 사용함으로써 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 계속할 수 있습니다. 즉, 딱딱 끊어지는 개발 단계를 정의하기에 아주 좋습니다.
  예를 들어, ‘이번 주에 버전 1.3 배포를 목표로 한다!’라고 팀 구성원들과 쉽게 소통하고 합의할 수 있다는 말입니다.

  ‘develop’ 브랜치에서 배포할 수 있는 수준의 기능이 모이면 또는 정해진 배포 일정이 되면, release 브랜치를 분기합니다.
  release 브랜치를 만드는 순간부터 배포 사이클이 시작됩니다.
  release 브랜치에서는 배포를 위한 최종적인 버그 수정, 문서 추가 등 릴리스와 직접적으로 관련된 작업을 수행합니다.
  직접적으로 관련된 작업들을 제외하고는 release 브랜치에 새로운 기능을 추가로 병합(merge)하지 않습니다.
  ‘release’ 브랜치에서 배포 가능한 상태가 되면(배포 준비가 완료되면), ‘master’ 브랜치에 병합한다. (이때, 병합한 커밋에 Release 버전 태그를 부여!)

  > 배포 가능한 상태: 새로운 기능을 포함한 상태로 모든 기능이 정상적으로 동작 하는 상태

  배포를 준비하는 동안 release 브랜치가 변경되었을 수 있으므로 배포 완료 후 ‘develop’ 브랜치에도 병합한다.
  이때, 다음 번 배포(Release)를 위한 개발 작업은 ‘develop’ 브랜치에서 계속 진행해 나간다.

  > release branch Naming
  >
  > release-RB_* 또는 release-* 또는 release/* 처럼 이름 짓는 것이 일반적인 관례
  >
  > [release-* ] 형식을 추천 EX) release-1.2

  ![release](/images/posts/201905/release.jpg)

  - __release branch 생성 및 종료 과정__

  ```java
  // feature 브랜치(feature/login)를 'develop' 브랜치에서 분기
  // 'master' 브랜치에서 따는 것이 아닙니다.
  $ git checkout -b release-1.2 develop

  /* ~ 배포 사이클이 시작 ~ */

  /* release 브랜치에서 배포 가능한 상태가 되면 */
  // 'master' 브랜치로 이동한다.
  $ git checkout master
  // 'master' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
  $ git merge --no-ff release-1.2
  // 병합한 커밋에 Release 버전 태그를 부여한다.
  $ git tag -a 1.2

  /* 'release' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
  // 'develop' 브랜치로 이동한다.
  $ git checkout develop
  // 'develop' 브랜치에 release-1.2 브랜치 내용을 병합(merge)한다.
  $ git merge --no-ff release-1.2
  // -d 옵션: release-1.2에 해당하는 브랜치를 삭제한다.
  $ git branch -d release-1.2
  ```

  -----------------------------------------------------------------------------

### Hotfix branch

  Hotfix branch란 __출시 버전에서 발생한 버그를 수정하는 브랜치__ 를 말합니다.

  > Hotfix branch는 master branch를 부모로 하는 임시 브랜치

  배포한 버전에서 긴급하게 수정해야할 부분이 있을 경우 master 브랜치에서 분기하는 브랜치입니다.
  ‘develop’ 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어려우므로 바로 배포가 가능한 ‘master’ 브랜치에서 직접 브랜치를 만들어 필요한 부분만을 수정한 후 다시 ‘master’브랜치에 병합하여 이를 배포해야 하는 것입니다.

  - 배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, ‘master’ 브랜치에서 hotfix 브랜치를 분기한다. (‘hotfix’ 브랜치만 master에서 바로 딸 수 있다.)

  - 문제가 되는 부분만을 빠르게 수정한다.
    - 다시 ‘master’ 브랜치에 병합(merge)하여 이를 안정적으로 다시 배포한다.
    - 새로운 버전 이름으로 태그를 매긴다.

  - hotfix 브랜치에서의 변경 사항은 ‘develop’ 브랜치에도 병합(merge)한다.

  ![hotfix](/images/posts/201905/hotfix.jpg)

  > hotfix branch Naming
  >
  > [hotfix-* ] 형식을 추천 EX) hotfix-1.2.1

  - __hotfix branch 생성 및 종료 과정__

  ```java
  // release 브랜치(hotfix-1.2.1)를 'master' 브랜치(유일!)에서 분기
  $ git checkout -b hotfix-1.2.1 master

  /* ~ 문제가 되는 부분만을 빠르게 수정 ~ */

  /* 필요한 부분을 수정한 후 */
  // 'master' 브랜치로 이동한다.
  $ git checkout master
  // 'master' 브랜치에 hotfix-1.2.1 브랜치 내용을 병합(merge)한다.
  $ git merge --no-ff hotfix-1.2.1
  // 병합한 커밋에 새로운 버전 이름으로 태그를 부여한다.
  $ git tag -a 1.2.1

  /* 'hotfix' 브랜치의 변경 사항을 'develop' 브랜치에도 적용 */
  // 'develop' 브랜치로 이동한다.
  $ git checkout develop
  // 'develop' 브랜치에 hotfix-1.2.1 브랜치 내용을 병합(merge)한다.
  $ git merge --no-ff hotfix-1.2.1
  ```

  -----------------------------------------------------------------------------

### Branch의 전체 흐름

  ![branch](/images/posts/201905/branch.jpg)


## 참고

  https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html

  https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
