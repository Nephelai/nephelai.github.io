---
layout: post
title: index using B+ Tree
tags: [database, algorithm]
categories: [InternshipLine]
---

## Index란?

RDBMS에서 검색속도를 높이기 위해서 사용하는 하나의 기술입니다. 해당 Table의 Column을 Index를 이용하여 검색시 검색속도를 빠르게 합니다. Index는 Tree 구조로 구성되어있습니다. 실제 RDBMS에서 사용되는 Index는 B-Tree에서 파생된 B+ Tree로 구성되어 있습니다.

### Index 원리

Index를 해당 Column에 주게 되면 초기 Table 생성 시 만들어진 MYD, MYI, FRM 3개의 파일 중에서 MYI에 해당 Column을 색인화하여 저장합니다. Index를 사용안할시에는 MYI 파일은 비어있습니다. 그래서 Index를 해당 Column에 만들게 되면 해당 Column을 따로 index를 이용해 MYI 파일에 입력합니다. 그래서 사용자가 SELECT Query로 Index를 사용하는 Query를 사용시 해당 Table을 검색하는 것이 아니라 빠른 Tree로 정리해둔 MYI 파일의 내용을 검색합니다.

만일 Index를 사용하지 않는 SELECT Query 라면 해당 Table을 full scan 하여 모두 검색합니다. 

#### 실질적인 DB Data 정보

실질적인 파일은 Table 마다 3가지의 파일명으로 만들어 진다.

* [Table명].frm / [Table명].MYD / [Table명].MYI
* .frm 파일 : Table의 구조가 저장되어 있습니다.
* .MYD 파일 : 실제 data가 저장되어 있습니다.
* .MYI 파일 : Index 정보가 저장되어 있습니다.

MyISAM의 경우 3가지의 파일이 같은 위치에 생성되고, InnoDB의 경우는 .frm은 원래 위치에 생성되고, 나머지 2개의 파일은 다른 위치에 생성된다.

### Index의 장점

* key 값을 기준으로 Table에서의 Search와 Sort 속도를 향상시킵니다.
* Query나 Grouping 작업의 속도를 향상시킵니다.
* Index를 사용하면 Table Row의 고유성을 강화시킬 수 있습니다.
* Table의 Primary Key는 자동으로 Index 됩니다.
* Field 중에는 Data Type으로 인해 Index가 될 수 없는 field도 있습니다.

### Index의 단점

* Index를 만들면 .mdb(.MYI) 파일 크기가 늘어납니다.
* 여러 User가 Application에서의 동시에 수정할 수 있는 Concurrency가 감소합니다.
* Index된 field에서 data를 update 하거나, record를 추가 또는 삭제할 때, Performance가 떨어집니다.
* Index를 생성하는데 시간이 많이 소요될 수 있습니다.
* data update 작업이 자주 일어날 경우 index를 재작성해야할 필요가 있기에 성능에 영향을 끼칠 수 있습니다.

index를 적용하는 data에 따라서 효율이 나쁠 수도 있습니다.

### index의 목적

index의 목적은 해당 RDBMS의 검색 속도를 높이는데 있습니다. 적용시 검색속도를 고려하여 사용해야합니다. 따라서 사용하지 않는 index를 제거해 주는것이 좋습니다.

## B-Tree

검색을 위한 자료구조 중에서 Binary Tree는 비록 하나의 부모가 2개의 자식밖에 가지질 못하고 자칫 균형(balance)이 맞지 않으면 검색 효율이 선형 검색 급으로 떨어지지만 잠재력이 가장 크다는 장점이 있습니다. 잠재력이란 Binary Tree의 간결함과 균형이 맞다면 검색(Search, SELECT), 삽입(INSERT), 삭제(DELETE) 모두 O(logn) 의 성능을 보여줍니다.

B-Tree는 Binary Tree를 확장하여 더 많은 수의 자식(child)을 가질 수 있게 일반화하였습니다. 또한 하나의 Level에 더 많이 저장하는 것 뿐만 아니라 Tree의 균형을 자동으로 맞추는 logic까지 갖추었습니다. 게다가 이 균형을 맞추는 로직은 단순하면서 효율적입니다. 

한 노드에 M개의 자료가 배치된 경우 M차 B-Tree라고 합니다. M이 짝수냐 홀수냐에 따라서 알고리즘이 다릅니다. 여기에서는 3차 이상의 홀수라고 가정합니다.

B-Tree의 경우 다음의 규칙(Rules)을 지니고 있습니다.

* 노드의 자료수가 N이라면 자식의 수는 N+1 입니다.
* 각 노드의 자료는 정렬된 상태입니다.
* 노드의 자료(D_k)의 왼쪽 SubTree는 D_k보다 작은 값들이고, 오른쪽 SubTree는 D_k보다 큰 값들입니다.
* Root 노드는 적어도 2개 이상의 자식을 갖습니다.
* Root 노드를 제외한 모든 노드는 적어도 ​​M/2개의 자료를 가지고 있습니다.
  * ex) 5차의 경우 모든 노드는 적어도 2개 이상의 자료를 갖습니다.
* 외부 노드(Terminal Node)로 가는 경로의 길이는 모두 같습니다. 즉, 외부노드는 모두 같은 Level에 있습니다. Balance 된 모양입니다.
* 입력 자료는 중복이 될 수 없습니다.

### 구현 (github 확인)

### 분석

* M차 B-Tree의 높이는 $$ log_{M/2} N $$ 이하입니다.
  * 각 노드는 최소 M/2개의 자료를 갖습니다.
* 검색 시 각 노드당 최대 M번의 순차검색이 일어납니다.
* 삽입 / 삭제 / 검색 성능 : T(N) < c * M * $ log_{M/2} N $, 즉 O(logN) 입니다.
* B-Tree는 외부 검색에 적합합니다.
  * 하나의 노드 크기를 Disk I/O의 단위 크기로 설정합니다. ex) 8KB로 읽는데, 한 자료가 500byte 라면 15-16차 B-Tree로 구현하면 유리합니다.
  * 순차 검색과 트리 검색의 장점을 지니고 있습니다.
    * B-Tree는 Balanced Tree로 최악의 경우가 없습니다.
    * QuickSort에서 소구간에 대한 삽입정령 성능이 좋습니다.
    * Node내에서 순차검색 대신 Binary Search 이용도 가능합니다. ex) 99차 101차 B-Tree를 고려시에

## B+ Tree

B-Tree는 특성상 순회(Traversal) 작업이 상당히 난감합니다. B+ Tree는 Index 구조에서 순차접근에 대한 문제의 해결책으로 제시되었습니다. B-Tree에서는 특정 key값이 하나의 노드에서만 존재할 수 있지만, B+ Tree에서는 leaf 노드와 leaf의 부모 노드에서 공존할 수 있습니다. **B+ Tree의 비단말노드(not leaf)들은 data의 빠른 접근을 위한 Index 역할만 하기 떄문입니다.** 그리고 leaf 노드들은 Linked List 형태로 서로 연결되어 있고 이를 순차집합(squence set)으로 오름차순으로 정렬이 되어있다.

Index 노드들과 data 노드(record)로 구성되어 있습니다.

* Index node : leaf node를 제외한 나머지 노드들
* data node : leaf node

data node는 Linked List로 형성되어 있고, data node 하나의 크기는 Index node 하나의 크기와 똑같지 않아도 됩니다. 또한 Index node에 존재하는 key는 자신의 오른쪽 SubTree의 data node의 가장 첫번째 위치에 존재합니다.

B+ Tree의 높이는 B Tree보다 낮게 구성되므로 검색시간과 DIsk에 접근하는 횟수가 줄어듭니다.

### 정리

B-Tree의 변형구조로 index 부분과 leaf 노드로 구성된 sequenec data 부분으로 이루어집니다. index 부분의 key 값은 leaf에 있는 key 값을 직접 찾아 가는데 사용하고 모든 key 값은 leaf 노드에 나열됩니다. 즉, index 부분의 key 값도 leaf 노드에 다시 한 번 나열됩니다. Leaf 노드는 순차적으로 linked list를 구성하고 있어서 순차적 처리가 가능합니다.

B-Tree와 달리 Tree의 최하위 Level의 leaf node에만 data들이 정렬되어 있습니다. 나머지 노드들은 key 값만 가지고 있습니다. 그래서 파일시스템 같은 Block 기반 Storage에서 저장 데이터의 효율적인 검색에 유용합니다.

## 출처

[후회하기 싫으면 그렇게 살지 말고, 그렇게 살거면 후회하지 마라](https://lalwr.blogspot.com/2016/02/db-index.html)

[잉구블로그](https://wangin9.tistory.com/entry/B-tree-B-tree)

[백인감자](https://potatoggg.tistory.com/174)

[Digital Dynamics](http://ddmix.blogspot.com/2015/01/cppalgo-18-b-tree-search.html)