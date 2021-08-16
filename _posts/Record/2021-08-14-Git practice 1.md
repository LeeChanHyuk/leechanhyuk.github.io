---
title: "[Tip] Github tips for contributing opensource"
date: 2021-08-14 16:59:00
author: Leechanhyuk
categories: Record
tags:	Record
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# 시작하기전에

 - 본 포스팅은 '2021 오픈소스 컨트리뷰션 아카데미'의 일환으로 제공된 Git 교육을 개인 공부 및 기록용으로 제작한 포스팅입니다.

# Groom

 - 웹 브라우저에서 리눅스 기반의 개발 환경을 구축하고, 사용이 가능하다.

# Git 기능 정리

  - 본 포스트의 예시는 pytorch 공식 git page 및 pytorch-example git page를 분석하여 포스팅하였습니다.

  - ## 기여도 체크

    - 기여도는 Git page에 들어가서 GUI로 체크할 수도 있지만, 이런 방법들을 사용하면 folder별로, 기간 별로 (상반기에 누가 어떤 부분에 Contribute를 많이 했는지) 정리할 수 있다.

    - **commit 기여도 랭킹**

      - git shortlog -sn | nl

      <img src="/assets/image/git_practice/one.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **개발자별 commit 개수 요약**

      - git shortlog --summary (-s)

      <img src="/assets/image/git_practice/two.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **개발자별 commit 개수 순위 정리**

      - git shortlog --number (-n)
      
      <img src="/assets/image/git_practice/three.png" width="200px" height="300px" title="MAE" alt="MAE"> 

  - ## Commit 내역 체크

    - **commit 내역 최근 순으로 출력**

      - git log

      <img src="/assets/image/git_practice/4.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **commit 내역 한 줄로 요약해서 출력**

      - git log --oneline (commit 내역 한 줄로 요약해서 출력)

      <img src="/assets/image/git_practice/5.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **한 commit의 세부 commit 내역 출력**

      - git show 3f43a8b9a3(commit hash) (한 commit의 세부 내역 출력)

      <img src="/assets/image/git_practice/6.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **특정 키워드가 포함된 commit 내역 출력**
    
      - git show 3f43a8b9a3 | grep "diff --git" 
    
      - grep은 filtering 해주는 역할을 담당. 여기서는 git show로 출력되는 내용 중에서 "diff --git"이 포함된 애들을 출력해주는 것. (diff는 파일을 변경했기에 추가되는 것.)

      <img src="/assets/image/git_practice/7.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **특정 폴더의 commit 내역 출력**

      - git log --oneline -- foldername

      <img src="/assets/image/git_practice/8.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **특정 날짜기준 commit 내역 출력**

      - git log --oneline --after=2020-01-01 --before=2020-06-30

      <img src="/assets/image/git_practice/9.png" width="200px" height="300px" title="MAE" alt="MAE"> 

  - ## Branch

    - Branch는 말 그대로, 원본 코드에서 가지를 뻗어 나가 듯이, 안전한 수정을 위해 새로운 버전의 코드를 만들고, 수정하는 것을 의미한다.

    - 일반적으로는 branch의 명칭은 내가 작업하고자 하는 종류에 따라서 결정한다.

    - **Branch 생성**

      - git branch branch_name

      <img src="/assets/image/git_practice/11.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **Branch 변경**

      - git checkout branch_name

      <img src="/assets/image/git_practice/12.png" width="200px" height="300px" title="MAE" alt="MAE"> 
    
    - **Branch 생성 및 변경**

      - git checkout -b branch_name

      <img src="/assets/image/git_practice/10.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **Branch 삭제**

      - master branch로 변경 후

      - git branch -D branch_name

      <img src="/assets/image/git_practice/13.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - **Branch 목록 확인**

      - git branch

      <img src="/assets/image/git_practice/14.png" width="200px" height="300px" title="MAE" alt="MAE"> 

  - ## pull request

    - 원하는 Repository를 fork한다.

    - 새 branch를 만든다.

    - 새 branch에서 작업을 하고, add, commit, push를 한다.

    - 내 git 내 fork한 repository에서 pull-request를 누른다.

      <img src="/assets/image/git_practice/15.png" width="200px" height="300px" title="MAE" alt="MAE"> 

  - ## 다른 함수 모음

    - **git diff**

      - 기존의 파일이 변경되면 그 정보를 출력.

      - 나는 주석 하나를 추가하였다.

      <img src="/assets/image/git_practice/18.png" width="200px" height="300px" title="MAE" alt="MAE">

    - **git stash**

      - 내가 수정한 내용을 임시저장하고, 원래 코드로 돌린다.

      - 내가 만약에 버그 수정을 하고 있었다고 할 때, 다 고치고나서 버그에 대해서 다시 한번 확인이 필요하다고 생각해보자.

      - 그럴때 before - after 점검이 필요할 때 사용한다.

      <img src="/assets/image/git_practice/19.png" width="200px" height="300px" title="MAE" alt="MAE">

      - 위 그림처럼, stash하면 원 상태로 돌아가기 때문에 diff로도 변경사항을 확인할 수가 없고, stash pop으로 다시 변경 상태로 돌아갈 수 있다. 그 상태에서 diff하면 다시 변경 상태로 돌아간 것을 확인할 수 있다.

      - git stash는 여러번 중첩해서 사용 가능하다.

    - **git checkout**

      - git checkout은 내가 변경한 부분을 원상복구하고싶을 때 사용한다.

      - stash와는 다르게 임시저장되지 않는다.

    - **git commit --amend**

      - commit을 수정하고 싶을 때

      - 일단 코드 수정하고, 위 라인을 타이핑하면 된다.

      - 코드 수정은 물론이고, commit message 또한 변경 가능하다.

      - 가장 최근의 commit이 수정된다.

      - 그렇다면 중간에 있는 commit을 수정하고 싶다면?

      - git rewind를 사용하면 된다. (추가 예정)

  - ## 코드 개발 도중 오픈 소스 repository의 상태가 변경되었을 경우 (Rebase가 필요한 경우)

    - 내가 fork떠서 Branch 만들어서 수정하는 도중에, 기존의 코드가 수정되는 경우.

    - pull-request 했을 때, rebase 요청이 들어올 수 있다.

    - Rebase 과정에 들어가기전에, 내 프로젝트에서 오픈 소스 프로젝트에 접근할 수 있도록 remote 저장소를 추가해줘야한다.

    - 내 경우 기존 pytorch의 저장소가 기존 저장소이므로, 이 저장소를 remote로 추가하도록 하겠다.

    - git remote add upstream https://github.com/pytorch/pytorch

    - git remote -v

    - git remote -v 명령어로 내 remote 저장소를 모두 확인할 수 있다.

    <img src="/assets/image/git_practice/16.png" width="200px" height="300px" title="MAE" alt="MAE"> 

    - 여기서 upstream이란, 기존 오픈소스 저장소를 일컫는다.

    - **Rebase 과정**

      - git fetch upstream master

      - git rebase upstream/master

      - git push --force origin fix-mnist

      <img src="/assets/image/git_practice/17.png" width="200px" height="300px" title="MAE" alt="MAE"> 

      - 실제로 나는 작업도중 rebase가 필요하지 않아서 up-to-date 메세지가 출력됐지만, rebase가 필요한 경우에는 작동했을 것이다