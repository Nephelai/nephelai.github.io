---
layout: post
title: Pull과 Fetch의 차이
tags: [git]
categories: [git]
---

### Pull

* 원격 저장소로부터 필요한 파일을 다운 + 병합(Merge)
* 지역 branch와 원격 저장소 origin/master가 같은 위치를 가르킵니다.

### Fetch

* 원격 저장소로부터 필요한 파일을 다운(Merge는 따로 필요)
* 지역 branch는 원래 가지고 있던 지역 저장소의 최근 commit 위치를 가르키고, 원격 저장소 origin/master는 가져운 최신 commit을 가르킵니다.
* 신중할 때 사용한다. 확인이 필요한 경우
* 사용하는 이유?
  * 원래 내용과 바뀐 내용의 차이를 알 수 있습니다 : git diff HEAD origin/master
  * commit이 얼마나 됐는지 알 수 있습니다 : git log --decorate --all --oneline
  * 이런 세부 내용을 확인 후 git merge origin/master 하면 git pull 상태와 같아집니다. Merge까지 완료.

## 출처

[유자콩의 개발아지트](https://yuja-kong.tistory.com/60)