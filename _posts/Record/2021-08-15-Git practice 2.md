---
title: "[Tip] Git commit 가지고 놀기"
date: 2021-06-15 16:59:00
author: Leechanhyuk
categories: Git
tags:	Record
use_math: true
cover: "/assets/instacode.png"
toc: true
---
# 시작하기전에

 - 본 포스팅은 '2021 오픈소스 컨트리뷰션 아카데미'의 일환으로 제공된 Git 교육을 개인 공부 및 기록용으로 제작한 포스팅입니다.
 
## commit 되감기 (rebase)

 - commit을 잘못 추가했을 경우, 새 커밋을 기존 커밋사이에 넣어야 할 때, 이 기능을 사용한다.

 - git rebase -i --root

 - 아래 사진은 위 명령어를 타이핑했을 때 나오는 화면으로, 기존 commit들을 의미한다.

  <img src="/assets/image/git_practice2/3.png" width="400px" height="300px" title="MAE" alt="MAE"> 

 - 가장 위에 commit 부터 옛날 커밋이다.

 - pick을 edit으로 변경 후 :w로 저장 후 :qa로 빠져나오기. (내 경우 editor가 vim)

 - edit으로 변경하면, 변경한 줄의 그 시점으로 commit들이 되감아지는 것이다.

 - git rebase --continue는 되감은걸 푸는 역할을 한다.

 - 즉 git rebase해서 되감고, 작업할 것 하고, 다시 git rebase --continue로 돌려 놓는 식으로 진행함.

 - git log --oneline으로 확인하면서 실습 가능.


## 예전 commit 뒤에 새로 commit 추가

 - git rebase -i --root로 제일 위에서 두개째 commit을 edit으로 변경하면, 그 시점까지 되감아진다.

 - 그 이후에 add - commit 으로 신규 commit을 추가하고

 - 그 후에 git rebase --continue로 되돌리면

 - 예전 commit 사이에 신규 commit이 추가된다.

  <img src="/assets/image/git_practice2/1.png" width="400px" height="300px" title="MAE" alt="MAE"> 

  <img src="/assets/image/git_practice2/2.png" width="400px" height="300px" title="MAE" alt="MAE"> 

## Rebase 취소

 - git rebase --abort

## 중간에 낀 3개의 commit을 하나로 합치기

 - git rebase -i --root를 해서 3개 중 제일 마지막 commit으로 간다.

 - git reset --soft HEAD~1

 - 위 command는 최신 HEAD에서 앞쪽으로 하나만 앞에 commit을 제거한다.

 - 만약 git reset --hard HEAD~1 이면 add한 파일까지 없애버리는데, soft는 commit만 없애고 add는 남겨둔 상태인 것이다.

  <img src="/assets/image/git_practice2/4.png" width="400px" height="300px" title="MAE" alt="MAE"> 

 - 진행한 결과, test3이 없어졌다. 그렇지만 add 했던 파일들은 그대로 남아있다.

 - 여기서 git의 commit을 병합하기 위해서

 - git commit --amend

 - 를 사용해서 기존에 남아있던 test2 commit message를 변경하고, add 내역 또한 병합한다.

  <img src="/assets/image/git_practice2/5.png" width="600px" height="450px" title="MAE" alt="MAE"> 

 - 이제 git rebase --continue로 되돌린 후에, test2와 test1까지 병합해보겠다.

 - 똑같은 방식으로 진행한다.

 - 역시 git rebase -i --root으로 test3 and test2 commit까지 돌아간다.

 - 그 후, 최근꺼 하나를 git reset --soft HEAD~1로 최근 commit을 삭제하고

 - git commit --amend로 'test1' commit에 'test2 and test1' commit을 병합하고, 메세지 또한 test1 and test2 and test3으로 변경한다.

  <img src="/assets/image/git_practice2/6.png" width="600px" height="450px" title="MAE" alt="MAE"> 

## 소스 코드 변경 사항 추적

 - 특정 소스라인에 누가 마지막으로 수정을 했는지 commit 추적이 가능함.

 - git blame filename으로 접근

  <img src="/assets/image/git_practice2/7.png" width="600px" height="450px" title="MAE" alt="MAE">

 - 특정 정보는 /info 같은 식으로 찾을 수 있다.

 - 최초 이 코드를 만든 사람을 찾는 방법은

 - git page 내에서 찾는 방법이 있다.

 - 해당 코드가 코드 파일과 같이 탄생했을 가능성이 있다면

 - git log --oneline --reverse node_http_parser.cc | HEAD -1과 같이 찾을 수 있다.

## Pull-request시 기존 repo가 updated 됐다면?

  - git remote add upstream url 으로 upstream을 등록하고

  - **되감은 내역 외에 기존 커밋이 수정된 경우가 아니라면** 이 상태에서 수정사항을 git fetch upstream url로 반영하고

  - git status / git dff로 충돌 사항을 확인한 후에

  - git rebase upstream/master로 내 코드에 반영한다.

  - 그 다음에 add하고, git rebase --continue로 가져온 후에

  - git push로 올리면 된다.