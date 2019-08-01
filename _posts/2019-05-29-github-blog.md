---
title: "GitHub로 블로그 만들기"
layout: post
category: GitHub
tags: [Github]
excerpt: "Github로 블로그를 만드는 방법을 배워봅시다."
author: BAEKJungHo
---

* content
{:toc}

## GitHub Blog

  > GitHub : [https://github.com/](https://github.com/)

  - 위 깃허브 사이트로 들어가서 회원가입
    - 회원가입 후에 오른쪽 상단 프로필 사진 클릭
    - Your repositories 클릭
    - ![gh1](/images/posts/201905/gh1.jpg)  

  - Repository name에 `자신의 닉네임.github.io` 방식으로 적어줍니다.
    - 꼭 이 방식으로 적어야 블로그를 만들 수 있습니다.
    - 저는 이미 생성했기 때문에 경고 문구가 뜹니다.
    - Repository name을 적고나서 Create repository를 클릭해서 생성합니다.
    - ![gh2](/images/posts/201905/gh2.jpg)  

  - cmd창을 열고 Remote Repository를 받을 디렉터리까지 이동합니다.
    - 예를들어 `D:\workspace\자신이 생성한 깃허브 저장소 폴더` 이렇게 저장하고 싶은 경우
    - D:\workspace까지 이동합니다.
    - 그리고 빨간 네모박스를 클릭하여 `URL`을 복사하고 다음 명령어를 입력해줍니다.
    - `git clone 붙여넣기(URL)` (git clone http://baekjungho.github.io) 이런 방식입니다.
    - ![gh3](/images/posts/201905/gh3.jpg)  

  > Jekyll : [http://jekyllthemes.org/](http://jekyllthemes.org/)

  - 위 지킬 테마 사이트에 들어가서 원하는 테마를 하나 고릅니다.
    - ![gh4](/images/posts/201905/gh4.jpg)  

  - 테마를 클릭하고 Download를 클릭 합니다.
    - ![gh5](/images/posts/201905/gh5.jpg)

  - 다운 받은 알집 파일을 자신이 git clone으로 다운 받은 `WorkingDirectory`
  - 즉 `.git폴더`가 있는 곳으로 가서 알집을 풀어줍니다.

  > 알집을 풀 때 알집을 푼 폴더가 A일 경우 A안에 있는 각종 파일, 폴더들을 밖으로 꺼내야 합니다.

  - ![gh6](/images/posts/201905/gh6.jpg)

  - 마지막으로 다음 명령어를 순서대로 입력하여 마무리를 해줍니다.
      - git add *
      - git commit -m "자신이 적고싶은 메시지"
      - git push

  블로그 테마를 적용시키고 나서는 자신이 `Jupyter Notebook 또는 Atom Editor`등을 사용하여
  `Markdown`문법으로 블로그 내용을 작성하면 됩니다.

  그리고 기존에 적혀있는 내용을 자신에 알맞게 수정해주면 됩니다.
  하나의 깃허브 계정당 하나의 블로그만 만들 수 있습니다. 하나의 계정에 2개 이상의 개인 블로그를 만들 수는
  없습니다. 또한, 자신이 테마를 변경하고 싶을때 초반에는 상관 없지만, 포스트 내용이 많아 질 수록 변경이 힘들고
  귀찮습니다. 따라서 테마를 선정할 때 가급적 `블로그에 기능이 많은 테마`를 고르는게 좋습니다.

  물론 자신이 여러 기능을 만드는데 익숙하고 실력이 좋다면, 뭐든 상관 없을 것입니다.

  GitHub로 블로그를 만드는 방법에 `Fork`를 사용하여 만드는 방식도 있는데 이 방식을 사용하게 되면
  자신이 `commit`을 해도 블로그에는 적용이 되지만, Overview에 있는 커밋이력에는 남지 않습니다.

  따라서 Overview에도 commit을 할 때마다 숫자가 증가되도록 `온전히 자신의 블로그`를 만들고 싶을 경우에는
  위 방식으로 진행해주시면 됩니다.

  > 수정 해야 할 내용 : config.yml 수정, disqus 적용, favicon 수정

## Github 사용법

  > Github 사용법 : [https://baekjungho.github.io/github-use/](https://baekjungho.github.io/github-use/)

## Markdown 사용법

  > Markdown 사용법 : [https://baekjungho.github.io/markdown-syntax/](https://baekjungho.github.io/markdown-syntax/)
