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



**6) 인덱스를 생성해야 하는 경우와.... 부터 시작**





## 출처

[후회하기 싫으면 그렇게 살지 말고, 그렇게 살거면 후회하지 마라](https://lalwr.blogspot.com/2016/02/db-index.html)