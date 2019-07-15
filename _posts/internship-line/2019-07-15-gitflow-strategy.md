---
layout: post
title: GitFlow Strategy
tags: [github]
categories: [internship-line]
---

Git-flow 에는 5가지 종류의 branch가 존재합니. 항상 유지되는 Main branch(master, develop)과 일정 기간동안만 유지되는 Sub branch(feature, release, hotfix)가 있습니다.

* master : 제품으로 출시될 수 있는 branch
* develop : 다음 출시 버전을 개발하는 branch
* feature : 기능을 개발하는 branch
* release : 이번 출시 버전을 준비하는 branch
* hotfix : 출시 버전에서 발새한 bug를 수정하는 branch

![git-flow_overall](../../images\posts\git-flow_overall_graph.png)

처음에는 master와 develop branch 가 존재하고, develop branch에는 상시로 bug를 수정한 commit 들이 추가됩니다. 

새로운 기능을 추가 작업이 있는 경우 develop에서 feature branch를 생성되고, 기능 추가 작업이 완료시 feature branch는 develop branch로 merge 됩니다.

모든 기능이 추가되면, QA를 위해 develop branch에서부터 release branch를 생성합니다. QA를 진행하면서 생기는 bug들은 release branch에서 수정하게 됩니다.

QA를 무사히 통과하게 되면 release branch를 develop과 master branch에 merge를 하게 됩니다.

이외에 hotfix branch의 경우는 이미 출시된 product에서 발생된 버그들을 빠르게 수정하기 위한 branch입니다. 만일 여기서 수정되었다면, develop, master에도 적용하게 되고, 만일 release도 존재한다면 마찬가지로 적용하게 됩니다.

git-flow에서 branch는 개발 process의 각 단계가 독립적으로 진행될 수 있도록 하기 위해서 이용됩니다. 각 branch가 자신의 역할에 맞게 규칙을 두어 변경점을 제어함으로써 자신만의 형상을 관리하고 있습니다. 하지만 중요한 것은 git이 제공하는 branch를 활용하는 방식에 있습니다.

### Git-Flow를 Customize 하자.

현재에는 git-flow는 너무 복잡하다며, github flow와 gitlab flow 등의 새로운 Model이 등장했습니다.

먼저 대부분의 회사에서는 QA 부서가 따로 존재하지 않으며, 개발단계에서 발견되지 않은 bug가 실제 필드에서 치명적으로 작용할 일 없는 product를 개발할 때에는 굳이 release branch가 필요 없습니다. 즉, 작은 프로젝트에서의 git-flow는 너무 비효율적이 됩니다. 또한 사소한 bug들을 즉시 수정하여 배포해야 할 필요가 없는 경우에는 hotfix branch도 필요 없습니다. 그리고, 한 두사람이 담당하고 있는 module의 경우에도 feature branch가 필요 없을 수 있습니다.

즉, 변화하는 프로젝트에 따라서 git branch를 전략적으로 구성해야 합니다.



### 출처 

[우아한 형제들 - 기술 블로그](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)

[티몬의 개발이야기](https://tmondev.blog.me/220763012361)

