---
layout: post
title: Pull과 Fetch의 차이
tags: [git]
categories: [git]

---

## 용어 정리

저장소 관련

* remote : 현재 project에 등록된 remote repository를 알 수 있습니다. remote repository는 Internet이나 Network 어딘가에 있는 repository로써 여러개가 존재할 수 있습니다. 간단히 말해서 다른 사람들과 함께 일한다는 것은 remote repository를 관리하면서 data를 거기에 Push하고 Pull 하는 것을 의미합니다.
* local : 내 PC에 파일이 저장되는 개인 전용 repository 입니다. 평소에는 내 PC의 local repository에서 작업하다가 작업한 내용을 Network에 저장하기 위해서 remote repository에 upload 합니다. 물론 remote repo에서 다른 사람이 파일을 자신의 local repo로 가져올 수도 있습니다.
* init : Git 저장소(repo)를 만드는 방법은 2가지입니다. 기존 Project를 git repo로 만드는 방법과 다른 서버에 있는 repo를 clone을 통해 가져오는 방법입니다. 그 중 기존 Directory를 git repository로 만드는 방법이 `init`입니다. 이 명령을 통해 `.git`이라는 하위 directory를 생성합니다. 이 directory에는 repo에 필요한 Skeleton 파일이 들어있습니다.
* clone : 다른 프로젝트에 참여하거나(Contribute) Git Repo를 복사하고 싶을때 `clone`을 이용합니다. `clone`을 실행하면 project의 history를 전부 받아옵니다. 실제로 서버의 disk가 망가져도 client repo에서 아무거나 하나 가져다가 복구하면 됩니다. 

Git은 다양한 protocol을 지원합니다. 이제까지 `git://`protocol을 사용했지만, `http(s)://`를 사용할 수도 있고, SSH protocol을 사용할 수도 있습니다.

