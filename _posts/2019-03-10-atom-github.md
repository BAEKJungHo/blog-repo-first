---
title: "Atom과 GitHub 연동하기"
layout: post
category: GitHub
tags: [Github, Atom]
excerpt: "윈도우 커맨드로 깃허브에 올리기 & Atom 에디터로 깃허브에 올리기"
author: BAEKJungHo
---

* content
{:toc}

윈도우 커맨드(cmd)로 github에 파일을 올리는 방법을 모르는 사람은 먼저 전 글에 올려둔 [github 사용법](https://baekjungho.github.io/github-use)을 보고오면 더 확실히 이해가 될 것입니다. 그럼 이번에는 아톰(Atom)과 깃허브(Github)를 연동해서 ATOM EDITOR로 github에 올리는 방법을 배울 것입니다.

윈도우 커맨드로 github에 올릴때 `[ add - commit - push ]` 순서로 했던것처럼 Atom에도 똑같이 적용된다. 어떤 버튼이 add역할이고 어떤 버튼이 commit과 push버튼인지 확인해봅시다. Github에 새로운 리퍼지토리를 생성한 상태라고 가정하고 시작하겠습니다.


# cmd로 깃허브에 올리기

  `[ ① Github에 New remote repository 생성 ]`

  `[ ② 자신의 Working Directory 생성 ] ☞ 자신의 컴퓨터에서 작업할 공간(폴더)을 만들기`

  `[ ③ cmd로 git init을 입력하여 자신의 Working Directory에 깃저장소를 만들기 ] ☞ git init 취소 ☞ del .git`

  `[ ④ remote repository의 주소를 복사하여 cmd로 git remote add origin "URL" 입력하여 연동하기 ]`

  `[ ⑤ 자신이 깃허브에 올릴 파일을 작업하고 Working Directory 안에 저장하기 ]`

  `[ ⑥ git add 폴더/디렉터리명 입력하여 staging area에 보내기 ]`

  `[ ⑦ git commit -m "확정본에 대한 내용" 입력하여 Local Directory에 보내기 ]`

  `[ ⑧ git push -u origin master를 입력하여 Remote repository, 즉 깃허브에 등록 ]`

  `[ git push -u origin master를 한 번 수행한 상태이면 다음 부터는 git push만 입력해도 된다 ]`


  여기서 가장 중요한 부분은 3번(git init)은 자신이 새로운 깃 저장소를 만들때 사용한다. 즉, 자신이 A라는 폴더에서 작업하여 1번부터 8번까지의 과정을 한번 수행했으면 다음부터는 5번부터 8번까지의 과정을 수행하면 된다. 하지만 자신이 B라는 폴더에서 작업하여 기존에 있던 혹은 새로운 원격저장소에 파일/디렉터리를 등록하고 싶으면 3번부터 8번까지의 과정을 수행하면 된다.

# Atom Editor를 이용한 깃허브 연동

  이제 ATOM EDITOR로 GITHUB에 올리는 방법을 배워봅시다.

  아톰 에디터로 깃허브에 파일을 등록하기 위해서는 먼저 cmd에서 1번부터 8번까지의 과정을 윈도우 커맨드로 한번 수행 해야한다.


  `[ ① File - settings - git plus 설치 ]`

  `[ ② View - Toggle git tab 열기 ]`

  ![pic](/images/posts/201903/pic.jpg)


  가장 먼저 파일을 수정하면 unstaged changed라는 프레임 위에 파일이 올라가져 있다.


  `[ ③ stage all을 눌러서 Staging Area에 보내기 ] ☞ 즉, stage all = git add`

  `[ ④ Commit message 에 확정본에 대한 메시지 적고 Commit to master 클릭 ] ☞ 즉, Commit to master = git commit`

  `[ ⑤ 마지막으로 3번그림 밑에 있는 push 버튼 클릭 ] ☞ 즉, push = git push`


  여기서 중요한 부분은 아톰 에디터의 push버튼을 사용하려면 먼저 cmd로 git push -u origin master를 한 번 수행해야한다.

  만약 Working Directory, Staging Area, Local Directory, Remote repository의 의미를 잘모르겠다면, github 사용법의 글에있는 그림을 이해하고 오기바랍니다. 이 부분을 확실히 이해해야 github가 쉬워집니다.
