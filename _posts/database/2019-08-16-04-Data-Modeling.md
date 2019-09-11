---
layout: post
title: 04. Data Modeling
tags: [Introduction To Database]
categories: [database]
---

## Data Modeling과 Data Model

현실 세계에 존재하는 데이터를 컴퓨터의 DB로 옮기는 변환 과정을 **Data Modeling**이라고 합니다. 실재 정보를 옮기기 쉽지 않으므로 **추상화(Abstraction)**을 이용해 DB에 저장하여 관리할 만한 가치가 있는 중요 데이터만 저장합니다.

현실 세계에서 중요한 데이터를 추출하여 개념 세계로 옮기는 작업을 **개념적 Modeling**이라고 합니다. 또한 개념 세계에서의 데이터를 DB에 저장할 구조로 결정하고 표현하는 작업을 **논리적 Modeling**이라고 합니다.

data model은 data modeling의 결과물을 표현하는 도구로, 개념적 data model과 논리적 data model이 있습니다.

* 개념적 data model : 사람의 머리로 이해할 수 있도록 현실 세계를 개념적 모델링하여 DB의 개념적 구조로 표현하는 도구
* 논리적 data model : 개념적 구조를 논리적 modeling하여 DB의 논리적 구조로 표현하는 도구

일반적으로 data model은 **데이터 구조 / 연산 / 제약조건** 으로 구성되어 있습니다.

* 데이터 구조(Data Structure) : 보통 자주 변하지 않고 정적이라는 특징을 지닙니다.
  * 개념적 data model : 현실 세계를 개념 세계로 추상화했을 때 어떤 요소로 이루어져 있는지를 표현하는 개념적 구조
  * 논리적 data model : 데이터를 어떤 모습으로 저장할 것인지를 표현하는 논리적 구조
* 연산(Operation) : 데이터 구조에 따라 표현된 값들을 처리하는 작업, 값이 연산에 의해 계속 변경될 수 있으므로 동적이라는 특징을 지닙니다.
* 제약조건(Constraint) : 구조적 측면의 제약 사항과 의미적 측면의 제약 사항이 존재합니다.

## 개체 - 관계 모델 (Entity - Relationship Model)

### Entity

개체는 현실 세계에서 조직을 운영하는 데 구별되는 모든 것을 의미합니다. 즉, 개체는 저장할 만한 가치가 있는 중요 데이터를 지니고 있으며, 개념적 모델링을 하는데 가장 중요한 요소입니다.

### Attribute

속성은 개체가 가지고 있는 고유의 특성입니다. 속성은 그 자체만으로는 의미가 없지만 관련 있는 속성들을 모아 개체를 구성하면 하나의 중요한 의미를 표현할 수 있습니다.

속성은 다음과 같이 다양한 기준으로 분류할 수 있습니다.

* 속성 값의 개수
  * 단일 값 속성 (Single-Valued Attribute) ex) 이름
  * 다중 값 속성 (Multi-Valued Attribute) ex) 연락처
* 의미의 분해 가능성
  * 단순 속성 (Simple Attribute) ex) 아이디
  * 복합 속성 (Composite Attribute) ex) 생년월일
* 기존 속성 값에서 유도
  * 유도 속성 (Derived Attribute) ex) 판매가격 = 가격 * 할인율

NULL 속성 : 0과 Blank와는 다른 속성입니다. unknown value를 의미합니다. 또한 존재하지 않는 값의 경우도 포함합니다.

Key 속성 : 모든 개체 Instance의 Key 속성이 다르므로 Key 속성은 개체 집합에 존재하는 각 개체 Instance들을 식별하는 데 사용합니다. 어떤 경우에는 Key를 둘 이상의 속성들로 구성하기도 합니다.

### Relationship

관계는 개체와 개체가 맺고 있는 의미 있는 연관성으로, E-R Model의 중요한 요소입니다. 관계는 개체 집합들 사이의 대응 관계(Correspondence) 즉, Mapping을 의미합니다.

관계도 개체처럼 속성을 가질 수 있습니다. 관계를 맺음으로써 발생하는 중요한 data들이 관계의 속성이 됩니다.

#### 관계의 유형

관계도 다양한 기준에 따라 분류할 수 있습니다. 여기서 살펴볼 분류 기준은 **매핑 원소의 수, 즉 Mapping Cardinality**입니다. Mapping Cardinality는 관계를 맺는 두 개체 집합에서, 각 개체 Instance가 연관성을 맺고 있는 상대 개체 집합의 Instance 개수를 의미합니다.

* 1 : 1 관계

개체 A의 각 객체 Instance가 개체 B의 개체 Instance 하나와 관계를 맺을 수 있고, 개체 B의 각 객체 Instance가 개체 A의 개체 Instance 하나와 관계를 맺을 수 있다면 두 개체의 관계를 일대일 관계입니다. 예를 들어 대한민국의 부부관계가 있습니다. 남편에 아내는 일대일 관계입니다.

* 1 : N 관계

개체 A의 각 객체 Instance가 개체 B의 개체 Instance 여러 개와 관계를 맺을 수 있지만, 개체 B의 각 객체 Instance가 개체 A의 개체 Instance 하나와만 관계를 맺을 수 있다면 두 개체의 관계를 일대다 관계입니다. 예를 들어 부서와 사원의 소속관계가 있습니다. 일반적으로 한 부서에는 사원이 여러명이 소속될 수 있지만, 사원 한 명은 부서 하나에만 소속되기 때문에 일대다 관계입니다.

* N : M 관계

개체 A의 각 객체 Instance가 개체 B의 개체 Instance 여러 개와 관계를 맺을 수 있고, 개체 B의 각 객체 Instance가 개체 A의 개체 Instance 여러 개와 관계를 맺을 수 있다면 두 개체의 관계를 다대다 관계입니다. 예를 들면 고객과 책에 구매 관계를 들 수 있습니다. 책은 여러권으로 여러 고객에게 팔릴 수 있고, 고객 입장에서도 여러 책을 구매할 수 있으므로 다대다 관계입니다.

#### 관계의 참여 특성

개체 A와 B의 관계에서 개체 A의 모든 개체 Instance가 관계에 반드시 참여해야 된다면 **개체 A가 관계에 필수적 참여한다**고 한다. 또한 개체 A의 Instance 중 일부만 관계에 참여해도 되면 **개체 A가 관계에 선택적 참여한다**고 한다.

#### 관계의 종속성

개체 B가 독자적으로는 존재할 수 없고 다른 개체 A의 존재 여부에 의존적이라면, 개체 B가 개체 A에 종속되어 있다고 합니다. 개체 B가 개체 A에 종속되면, 이는 개체 A가 존재해야 개체 B가 존재할 수 있고 개체 A가 삭제되면 개체 B도 함께 삭제되어야 함을 의미합니다. 이러한 종속을 **존재 종속(Existence Dependence)**라고 합니다. 여기 두 개체에 대해서 의존적인 개체 B를 **Weak Entity**, 다른 개체의 존재 여부를 결정하는 개체 A를 **Strong Entity**라고 합니다.

## 논리적 Data Model

선택한 DBMS에 따라 User 입장에서 E-R Diagram으로 표현된 개념적 구조를 DB에 저장할 형태로 표현한 논리적인 구조를 논리적 Data Model이라고 합니다. 쉽게 말해서 논리적 Data Model은 논리적 Data Modeling의 결과물이고, User가 생각하는 DB의 모습 또는 구조입니다. 또한 논리적 Data Model로 표현된 DB의 논리적 구조가 바로 DB의 Schema입니다.

대표적인 논리적 Data Model로는 **관계 데이터 모델**이 있습니다. 또한 **Hierarchical Data Model**과 **Network Data Model**이 존재합니다.