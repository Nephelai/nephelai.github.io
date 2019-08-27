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

Key는 크게 5가지로 구분한 할 수 있다.

#### SuperKey (슈퍼키)

**유일한 특성을 만족하는 속성 또는 속성들의 집합**입니다. 유일성(Uniqueness)은 슈퍼키가 갖추어야 하는 기본적인 특성으로 Key 값이 같은 Tuple이 존재할 수 없습니다.

#### CandidateKey(대체키)

**유일성과 최소성을 만족하는 속성 또는 속성들의 집합**입니다. 최소성(Minimality)은 최소한의 속성들로만 Key를 구성하는 특성입니다. 즉, 하나의 속성으로 구성된 key는 당연히 최소성을 만족합니다.

superKey 가운데 최소성을 만족하는 것이 candidateKey가 됩니다. candidateKey의 특성은 새로운 Tuple이 삽입되거나 기존 Tuple의 속성 값이 바뀌어도 유지되어야 합니다. 그래서 candidateKey를 선정할 때는 현재의 Relation만 고려하는 것이 아닌 속성의 본래의 의미를 파악하여 선택해야 합니다.

#### PrimaryKey(기본키)

**DB 설계자나 관리자는 여러 candidateKey 가운데 기본적으로 사용할 Key를 의미**합니다. 만약 candidateKey가 하나인 경우 그 Key를 primaryKey로 해야하지만 여러 개의 경우 적합한 Key 하나를 선택해야 합니다. 그렇다면 primaryKey에 적합한 기준이 어떻게 될까요?

* NULL Value를 가질 수 있는 속성이 포함된 candidateKey는 primaryKey로 부적합합니다.

primaryKey가 NULL 값이 Tuple은 다른 Tuple들과 구별하여 접근하기 어려우므로 매우 중요합니다. 

* 값이 자주 변경될 수 있는 속성이 포함된 candidateKey는 primaryKey로 부적합합니다.

값이 자주 변경되는 속성으로 구성된 candidateKey를 primaryKey로 선택하면 속성 값이 바뀔 때마다 primaryKey 값으로 적합한지 여부를 판단해야 하므로 번거롭습니다.

* 단순한 candidateKey를 primaryKey로 선택합니다.

단순한 cadidateKey는 자리수가 적은 정수나 단순 문자열인 속성으로 구성되거나, 구성하는 속성의 개수가 적은 candidateKey 입니다. DB User뿐 아니라 DB를 처리하는 System도 단순 값처리를 선호합니다.

#### AlternateKey(대체키)

**primaryKey로 선택되지 못한 candidateKey들**입니다. alternateKey는 primaryKey를 대신할 수 있습니다.

#### ForeignKey(외래키)

**어떤 relation에 소속된 속성 또는 속성 집합이 다른 relation의 primaryKey가 되는 Key**입니다. 다시 말하면 다른 relation의 primaryKey를 그대로 참조하는 속성의 집합이 foreignKey입니다. **foreignKey는 relation들 사이의 관계를 올바르게 표현하기 위해서 필요**합니다.

## Constraint of Relational Data Model

relational data model에서 기본적으로 key와 관련된 제약 사항(Constraint)는 **무결성 제약조건(Integrity Constraint)**입니다.

무결성은 데이터가 정확하고 유효하게 유지된 상태를 말합니다. 즉, 무결성 제약조건은 DB의 데이터에 무결성을 보장하고, DB의 상태를 일관되게 유지하는 것을 의미합니다. 

내부의 데이터를 보호한다는 의미에서 무결성과 보안은 비슷합니다. 하지만 2가지의 차이가 있습니다.

* 보안 : 권한이 없는 사용자로부터 데이터를 보호
* 무결성 : 권한이 있는 사용자의 잘못된 요구에 의해 데이터가 부정확해지지 않게 보호

RDM에서 기본으로 지니고 있는 무결성 제약조건은 크게 2가지가 있습니다. 이는 개체 무결성 제약조건과 참조 무결성 제약조건입니다.

### 개체 무결성 제약조건(Entity Integrity Constraint)

**PrimaryKey를 구성하는 모든 속성은 NULL 값을 가지면 안 된다는 규칙**입니다. primaryKey를 구성하는 속성 전체나 일부가 NULL 값이 되면 Tuple의 유일성을 판단할 수 없어 primaryKey의 본래 목적을 상실하게 됩니다.

기본 무결성 제약조건을 만족시키려면 새로운 Tuple이 삽입되거나 primaryKey 속성 값을 변경하는 연산이 수행될 때, primaryKey에 NULL 값이 포함되는 상황에서는 연산을 거부하면 됩니다.

### 참조 무결성 제약조건(Reference Integrity Constraint)

개체 무결성 제약조건이 primaryKey에 대한 규칙으로 각 relation 마다 적용된다면, 참조 무결성 제약조건은 foreignKey에 대한 규칙으로 연관된 relation들에 적용됩니다.

참조 무결성 제약조건은 **ForeignKey는 참조할 수 없는 값을 가질 수 없다는 규칙**입니다. foreignKey가 자신이 참조하는 relation의 기본키와 상관이 없는 값을 가지게 되면 두 relation을 연관시킬 수 없으므로 foreignKey 본래의 의미가 없어집니다. 

DB의 상태가 변경되는 경우에도 참조 무결성 제약조건을 확인해 주어야 합니다. 두 relation의 data를 확인해 보면서 제약조건을 검사해야 합니다.