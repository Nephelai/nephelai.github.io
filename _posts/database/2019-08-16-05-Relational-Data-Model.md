---
layout: post
title: 05. Relational Data Model
tags: [Introduction To Database]
categories: [database]

---

## Concepts of Relational Data Model

### Relation Data Model의 기본 용어

일반적으로 Relational Data Model(RDM)에서는 하나의 Entity에 관한 data를 Relation 하나에 담아 DB에 저장합니다.

#### Attribute

Relation의 열(Column)을 속성(Attribute)이라 부릅니다. 각 속성은 서로 다른 이름을 이용해 구별합니다.

#### Tuple

Relation의 행(Row)을 Tuple이라 부릅니다.

#### Domain

Domain은 속성 하나가 가질 수 있는 모든 값의 집합을 의미합니다. RDM에서는 속성값으로 더는 나눌 수 없는 원자 값만 사용할 수 있습니다. Domain을 특정 속성이 가질 수 있는 모든 원자 값의 모음이라고도 합니다.

#### NULL Value

Relation에 있는 특정 Tuple의 속성 값을 모르거나 적합한 값이 없는 경우에는 특별한 값 NULL을 사용할 수 있습니다. NULL은 특정 속성에 해당하는 값이 없음을 나타내므로 0이나 Blank와는 다릅니다.

#### Degree

하나의 Relation에서 속성 전체의 개수를 Relation's Degree라고 합니다. 모든 Relation은 최소 1 이상의 Degree를 유지해야 하며 Degree는 일반적으로 자주 변하지 않는 정적인 특징을 지닙니다.

#### Cardinality

하나의 Relation에서 Tuple 전체 개수를 Relation's Cardinality라고 합니다. Tuple이 없을 수도 있으므로 Cardinality가 0일 수도 있으며, Tuple은 자주 변경되므로 Cardinality 또한 자주 변하는 동적인 특징을 지닙니다.

### Relation과 Database 구성

#### Relation Schema

Relation Schema는 Relation의 이름과 Relation에 포함된 모든 속성의 이름으로 정의하는 Relation의 논리적 구조입니다. Relation Schema는 DBMS이 내부적으로 DDL을 이용해 정의합니다. Relation Schema를 보면 Relation의 이름과, 어떤 속성들로 구성이 되어 있는지 전체 구조를 쉽게 파악할 수 있습니다.

#### Relation Instance

Relation Instance는 어느 한 시점에 Relation에 존재하는 Tuple들의 집합입니다. Relation Instance에 포함된 Tuple은 Relation Schema에서 정의한 각 속성에 대응하는 실제 값으로 구성되어 있습니다.Relation Instance를 이용하면 실제 Relation의 내용을 파악할 수 있습니다.

### Relation의 특징

Relation Data Model의 Relation에는 4가지의 주요한 특징을 지니고 있습니다.

1. Tuple의 유일성 : 하나의 Relation에는 동일한 Tuple이 존재할 수 없다.
2. Tuple의 무순서 : 하나의 Relation에서 Tuple 사이의 순서는 무의미하다.
3. Attribute의 무순서 : 하나의 Relation에서 Attribute 사이의 순서는 무의미하다.
4. Attribute의 원자성 : Attribute값으로 원자 값만 사용할 수 있다.



### Key의 종류

