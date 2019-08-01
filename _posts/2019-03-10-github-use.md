---
title: "GitHub 사용방법"
layout: post
category: GitHub
tags: [Github]
excerpt: "1.Git 설치 https://www.git-scm.com / 2. GitHub 회원가입 : https://github.com"
author: BAEKJungHo
---

* content
{:toc}

GitHub 사용방법 <br /> ☞ 기본 세팅 : Git 설치 [https://www.git-scm.com](https://www.git-scm.com) / GitHub 회원가입 : [https://github.com](https://github.com)

## How to use GitHub ?

  각종 Git 명령어와 Github 사용법에 대해 알아봅시다.

## Github Process

  GitHub에 파일을 올리고 꺼내는 과정은 다음과 같습니다.

  ![process](/images/posts/201903/pro.jpg)

  깃허브에 파일/디렉터리를 올리고, 빼내는 과정이 위 사진과 같은 방식으로 이루어진다.

  `< User ☞ Index ☞ Head ☞ Github >`

  아래 글을 읽으면서 그림을 같이보기 바랍니다. 위 그림이 완벽히 이해가 되야 합니다.

  가장먼저 `Working Directory`는 작업하려는 컴퓨터 내의 디렉토리이다.

  예를들어 지금 사용하고 있는 컴퓨터에서 작업하고자 하는 디렉터리`[D:\Workspace\Web]`를 만들고 cmd(커맨드)를 열어 해당 디렉토리로 이동한다. 해당 디렉터리로 이동 한 후에 다음과 같은 명령어를 쳐준다 `[ git --version ]`을 입력하고 git명령어가 제대로 작동 하는지 확인한다.

  만약에 git(깃)을 설치 했음에도 제대로 작동하지 않는다면 `[ 내컴퓨터 > 속성 > 고급 > 환경변수 ]`로 들어가서 맨위에 있는 환경변수 `[ path ]`에서 `[ c:\windows\system32 ]` 이 경로로 수정해주면 작동한다.

  이제 제대로 작동할 것이다. 이제 해야 할 일은 자신의 깃허브 닉네임, 이메일을 등록해야한다. 닉네임 등록 방법과 이메일 등록 방법은 아래와 같다.

  `[ git config --global user.name "닉네임" ]`

  `[ git config --global user.email "이메일" ]`

  닉네임과 이메일 등록을 마치면 이제 저 위에 있는 그림에서 1번 ~ 4번 과정을 거칠 것이다. 1번부터 4번까지의 과정이 작업디렉토리에서 깃허브에 파일을 올리는 과정이다. 가장먼저 1번을 입력한다.

  `[ ① git init ]`

  git init은 작업디렉토리 밑에 ' .git ' 깃 저장소를 만들어 준다.  / 만약 git init 을 취소하고 싶으면 `[ del .git ]`을 입력한다.

  다음으로 2번 과정을 들어가기 전에 자신의 깃허브로 들어가서 `Remote Repository(원격 저장소)`를 만들어 주어야 한다.

### Remote Repository 만들기

  `[ 깃허브 접속 > 오른쪽 상단 프로필 클릭 > your repositories > New ]`

  ![pro1](/images/posts/201903/pro1.jpg)

  저장소 이름을 설정한다음, 저장소에 대한 설명(Description)을 적고 Public을 선택해준다. Private는 돈을 지불해야한다. 그리고 Initialize this respository with a README는 체크를 해제 해준다.

  README.md 파일은 `마크다운` 형식으로 작성한 파일인데 나중에 저장소를 생성하고나서, READM.md 파일을 만들고 추가 해주면 됩니다.

  다음 자신의 원격저장소 주소를 복사한다음 `[ git remote add origin 주소 ]`를 입력해준다.

  이제 자신의 원격 저장소가 생성되었을 것이다. 이제 2번과정을 진행 해야한다. 다시 cmd창에서 Index(Staging Area)에 보낼 파일이나 디렉터리를 입력한다.

  `[ ② git add 파일/ 디렉터리명 ]`

  입력 후에 `[ git status ]`를 입력하고 상태를 확인해준다.  초록색 부분은 완료 / 빨간색 부분은 아직 add가 안된 파일이나 디렉터리이다.

  만약에 자신이 add명령을 입력하고나서 취소하고 싶으면 `[ git reset HEAD 파일 / 디렉터리명 ]` 을 입력해준다. 그러면 add명령을 취소 할 수 있다.

  다음으로 HEAD(Local Repository)로 보내기 위해 3번을 입력 해준다.

  `[ ③ git commit -m "확정본에 대한 설명" ]`

  마지막으로 GitHub(Remote Repository)에 파일/디렉터리를 올리기 위해서 4번 과정을 진행한다.

  `[ ④ git push -u origin master ]`

  위 글을 천천히 읽고 빠짐 없이 따라 했다면, 쉽게 GitHub에 파일 올리는 방법을 알게됬을 것이다. 이제 첫번째 파일/디렉토리를 등록하고나서 두번째, 세번째 등등 계속해서 파일/디렉터리를 GitHub에 등록하기 위해서 `[ ② → ③ → ④ ]` 과정만 반복해주면된다.

  (단, 처음에 git push - u origin master를 했으면, 다음부터는 git push만 해주면 된다.)

  `[ git reset ]` 을 입력하면 Staging Area에서 Working Directory로 작업을 취소 할 수 있다.

## Atom

  아톰에서 자신이 GitHub에 올렸던 HTML파일을 수정하고 다시 올리고 싶을 경우에는 파일을 수정한 후에 `[ git status ]`를 입력하면 modify라는 단어를 볼 수 있을 것이다. 수정 됬다는 뜻이다.  이제 다시 `[ ② → ③ → ④ ]` 과정을 반복해주면 된다.

## GitHub를 이용한 팀 프로젝트

  팀프로젝트에 있어서 GitHub를 많이 사용할 것이다. 먼저 PL(Project Leader)이 GitHub(원격저장소)에 프로젝트를 올릴 것이다. 그러면 팀원들은 자신의 컴퓨터(Working Directory)에서 작업을 해야할 것이다. 그리고 자신의 작업물을 다시 `[ add > commit > push ]` 순서로 GitHub에 올릴 것이고 다른사람들이 올린 작업물을 갱신하면서 작업 하기위해서 `[ pull ]`로 자신의 디렉터리로 파일을 갱신할 것이다.

  따라서, 어떤 작업을 하기 전에는 git pull을 사용하고, 작업을 마치면 git push로 꼭 올려주어야 합니다.

  이때 원격저장소에서 Working Directory로 가져오는 과정은 다음과 같다.

## GitHub에서 파일을 Working Directory로 가져오는 과정

  `[ ① git fetch > git merge ] [ ② git pull (fetch + merge) ] [ ③ git clone "원격 저장소" ]`

  첫 번째는 fetch로 로컬 저장소(Local Directory)로 받고 merge로 Working Directory로 가져온다.

  두 번째는 pull명령어를 사용한다. fetch + merge라고 생각하면된다.

  세 번째는 자신이 다른 사람 GitHub를 보고 그 소스를 받고 싶을때 clone명령으로 받으면 된다.

## git fork

  `git fork`는 자신이 복제 하고싶은 상대방의 remote repository(깃허브)에 가서 fork라는 버튼을 누르게되면 자신의 새로운 remote repository가 생기며 그 새로 생긴 원격저장소에 상대방 원격저장소의 내용이 담겨있다. 저는 이 GitHub Blog를 만들때 git fork를 사용했습니다.

## git clone

  `git fork`와 `git clone`의 동작은 다음 그림과 같다.

  ![clone](/images/posts/201903/clone.jpg)

  git clone은 git fork후에 자신의 remote repository의 URL을 복사하여 `git clone URL` 방식으로 작성해주면된다. 원격저장소의 내용을 그대로 복제해오는 과정이며 대신 자기의 작업디렉토리에 아무것도 없어야 한다. 또한 git clone 사용 후에는 `.git`이 자동으로 생성되어 git init 작업이 필요가 없다.
  따라서 git clone 후에는 git add -> git commit -> git push순서로 다시 작업하면된다.

## git pull

  `git pull`과 `git push`의 동작은 다음 그림과 같다.

  ![pull](/images/posts/201903/pull.jpg)

  git pull은 User A가 컴퓨터에서 작업한 결과물을 git push하여 올려 놓고, User B가 자신의 Working Directory에서 작업하고자 할때 사용한다. 즉, 원격저장소의 데이터를 자신의 Working Directory와 동일하게 갱신하고자 할 때 사용한다.
  User B가 작업이 끝나고 `git add -> git commit -> git push`를 통해 원격저장소(Remote Repository)에 올려놓고 User C가 다시 자신의 Working Directory에서 작업하고자 하면 git pull을 사용하여 가져와야 한다.

  > git pull이나 git push 작업시 error: RPC failed; curl 56 OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054 fatal: the remote end hung up unexpectedly 다음과 같은 에러가 날 경우 아래와 같이 작성해주면 됩니다.
  >
  > **git config --global http.postBuffer 524288000**
  >
  > 위 에러가 나는 이유는 용량이 너무 커서 생기는 문제이기 때문에 위 코드만 입력해주면 해결됩니다.
  >
  > _git pull = git fetch(가져오기) + git merge(병합)_

## 취소와 삭제명령

  add명령 후 취소방법이랑, commit명령 후 취소방법을 알아봅시다.

### git reset, git revert

  1. `git reset`을 이용하면 add명령을 취소할 수 있습니다.

  2. `git reset --merge`을 이용하면 merge명령을 취소 할 수 있습니다.

  3. `git reset --soft HEAD^`을 이용하면 commit만 취소하고 코드는 살릴 수 있습니다.

  4. `git reset --hard HEAD^`을 이용하면 commit하기 이전의 코드로 돌아갈 수 있습니다.
  > git reset --hard HEAD~2 : 마지막 2개의 커밋을 취소합니다. HEAD 뒤에 숫자를 적어준 만큼 취소.

  5. `git reset --hard HEAD`을 이용하면 HEAD를 기준으로 이전의 코드로 돌아갈 수 있습니다.

  6. `git reset HEAD^`을 이용하면 최종 커밋을 취소할 수 있습니다.(push 하기 전에 사용하면 유용합니다.)
  > git reset HEAD 파일명 : 해당 파일을 unstaged 상태로 변경 할 수 있습니다. git add 취소명령

  7. `git reset --hard ORIG_HEAD`을 이용하면 merge 한 것을 커밋했을 때, 그 커밋을 취소합니다. (잘못된 머지를 이미 커밋한 경우 유용합니다.)

  8. `git revert HEAD`을 이용하면 HEAD에서 변경한 내역을 취소하는 새로운 커밋 발행(undo commit). (커밋을 이미 push 해버린 경우 유용합니다.)

  > git reset 옵션
  >
  > --soft : index 보존(add한 상태, staged 상태), 워킹 디렉터리의 파일 보존. 즉 모두 보존.
  >
  > --mixed : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 보존 (기본 옵션)
  >
  > --hard : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 삭제. 즉 모두 취소.


### 커밋 수정 방법

  `git commit --amend`를 사용해서 커밋 메세지를 수정할 수 있다.
  만약 Staging Area에 보낼 파일을 깜빡하고 빠트렸으면 다음과 같이 수정하면 된다.

  > git commit -m fisrt file // 첫 번째 파일 커밋
  >
  > git add second file // 빠트린 두 번째 파일 Staging Area에 보내기
  >
  > git commit --amend // 빠트린 파일 포함해서 커밋

### untracked 파일 삭제 방법

  `git clean`을 통해서 untracked 파일을 삭제 할 수 있다.

  > git clean -f // 파일만 삭제
  >
  > git clean -f -d // 파일 + 디렉터리 삭제
  >
  > git clean -n -f -d // -n옵션은 가상으로 실험하는 옵션이다.
  >
  > git clean -f -d를 입력했을때 어떤 파일들이 사라질지 보여준다.

### git reset 복원 방법

  `git reflog`를 입력하면 다음과 같이 나온다.

  ![reflog](/images/posts/201903/reflog.jpg)

  여기서 HEAD@{숫자} 이런식으로 표시가 되는데, 자신이 실행한 명령어의 위치를 보여준다.
  만약 git reset 후에 다시 원래대로 복원을 하고 싶으면 `git reflog`를 입력하여 자신이 복원하고자하는 위치를 확인하고
  `git reset --hard HEAD@{숫자}`를 입력하면 된다.

  > 위치는 HEAD@{숫자} 이다.

## git config

  git config에 관해 user.name, user.email삭제 방법과 변경 방법을 배워봅시다.

  `[ git config --global user.name "닉네임" ]`

  `[ git config --global user.email "이메일" ]`

  --global을 붙이게 되면 시스템 전체에서 닉네임과, 이메일을 사용할 수 있습니다.

  만약에, 프로젝트마다 다른 닉네임과 이메일을 사용하고 싶으면 --global을 빼고 입력하면 됩니다.

### git config user 삭제방법

  git config로 user.name과 user.email을 등록하고나서 삭제하는 방법을 알아봅시다.

  1. global을 사용 안하고 등록했던 경우
  > git config --unset user.name
  >
  > git config --unset user.email

  2. global을 사용했던 경우
  > git config --unset --global user.name
  >
  > git config --unset --global user.email

  삭제를 하고나서 삭제가 제대로 되었는지 `git config --list`로 확인하면됩니다.
  삭제가 된 경우에는 내용이 출력되지 않습니다.

### git config user 변경방법

  다른 계정으로 등록된 user.name, user.email을 변경하고싶을때 사용하면 됩니다.

  ![config](/images/posts/201903/config.jpg)

  제어판 -> 사용자 계정 -> 자격 증명 관리자 -> Windows 자격 증명에서 Github 정보를 수정하면 됩니다.

## LF will be replaced by CRLF

  git을 사용하다 파일을 add시킬때 위 처럼 `LF WILL BE REPLACED BY CRLF`이런 경고창을 본 적이 있을 겁니다.
  LF와 CRLF는 라인피드 방식에 대한건데 이 문구는 크게 신경쓰지 않아도 됩니다.

  이 기능을 끄기 위해서는 다음과 같이 입력하면 됩니다.

  > git config --global core.safecrlf false

## 원격저장소 주소 변경

  처음에 원격 저장소를 생성하고 파일을 올리기 위해서는 우리가 `git remote add origin URL`을 이용하여
  원격저장소의 주소를 등록 할 것입니다.

  만약, 이 주소를 바꾸고 싶을때는 아래와 같이 하시면 됩니다.

  > git remote -v // 현재 저장소 확인
  >
  > git remote set-url origin URL // 원격저장소 주소 변경
