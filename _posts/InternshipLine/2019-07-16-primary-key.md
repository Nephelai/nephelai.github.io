---
layout: post
title: Primary Key 종류
tags: [database]
categories: [InternshipLine]

---

### Surrogate Key Vs Natural Key

Surrogate key는 business적인 의미가 전혀 없이 단순히 해당 row를 나타내기 위해서 생성한 key입니다. 대표적인 예로 id가 있습니다. id는 단순한 정수 값으로 아무런 관련이 없는 정보이지만 table의 각 row를 대표하기 위한 id로 쓰입니다.

Natural Key는 Surrogate Key와는 다르게 실제 domain에 관련한 정보를 key로 쓰는 개념입니다. id가 아닌 하나의 field를 primary key로 사용하게 됩니다.

### Natural Key의 단점

위의 단순한 예제를 보면 Natural Key가 더 합리적이라고 생각할 수 있습니다. 어차피 고유한 값을 지니고 있는 field를 갖고 primary key를 사용하는 방법이 새로운 field를 추가하여 사용하는 것보다 더 효율적이라고 생각될 수 있습니다. 

하지만 natural key의 가장 큰 문제점은 아무리 고정 값의 field를 natural key로 사용한다고 하더라도 대부분의 natural key들이 언젠가는 값이 변할 수 있다는 점입니다. 회사가 다른 회사와 병합 된다던가 하는 등의 흔치는 않지만 충분히 일어날 수 있는 이벤트에 의해 field가 변할 수 있습니다. 이렇게 변하게 된다면 그에 따른 관리를 해주는 것이 굉장히 복잡해지고 어려워질 수 있습니다. 

예를 들어 고유하다고 생각되었던 어떤 값이 변하게 되면 해당 field와 연관된 foreign key가 걸려 있는 다른 table들의 row들도 전부 동시에 변경해주어야 합니다. 큰 규모의 실제 시스템에서는 DB 구조가 이보다 훨씬 더 복잡한 경우가 많으므로 모든 foreign key를 update 하는 일은 굉장히 어려워질 수 있습니다.

하지만 surrogate key의 경우는 변경되지 않기 때문에 예상치 못하게 변경될 이유가 없는 것입니다.

### Surrogate Key

Surrogate Key의 가장 큰 단점은 DB Query가 복잡해진다는 점을 들 수 있습니다. Business 적으로 의미가 전혀 없는 key를 사용하기 때문에 간단한 query라도 join이 들어가게 되어 복잡해지게 됩니다. 따라서 Query에 따른 Cost가 많이 부여됩니다.

### Surrogate Key Or Natural Key?

Surrogate Key가 query를 복잡하게 하는 단점을 지니고 있지만 값이 변할 가능성이 전혀 없기에 대부분은 Natural Key보다는 Surrogate Key를 사용합니다. 값이 절대 안바뀌는 Natural Key는 굉장히 드물다고 할 수 있습니다. 심지어 책의 ISBN이나 주민번호도 바뀔 수 있는 가능성이 있습니다. 대부분의 경우 Surrogate Key를 사용하는 것이 합리적입니다.

## 출처

[Primary Key Blog](https://rampart81.github.io/post/surrogate_key_vs_natural_key/)