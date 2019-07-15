---
layout: post
title: Transaction & ACID
tags: [database, acid]
categories: [InternshipLine]
---

## Tansaction

질의(query)를 하나의 묶음 처리하여 만약 중간에 실행이 중단될 경우, 처음부터 다시 실행하는 Rollback을 수행하고, 오류없이 실행을 마치면 commit을 실행하는 단위를 의미합니다.

즉, 한 번 질의가 실행되면 질의가 모두 수행되거나 모두 수행되지 않는 작업 수행의 논리적 단위를 의미합니다.

### Transaction 을 사용하는 이유

Transaction은 DB 서버에 여러 개의 Client가 동시에 Access 하거나 App에서 Update를 처리하는 과정에서 중단될 수 있는 경우 등 data 신뢰성(reliability)을 보장하기 위해서 사용합니다.

부정합이 발생하지 않으려면 process를 병렬로 처리하지 않도록 하여 한 번에 하나의 process만 처리하도록 하면 되지만, 이는 효율이 너무 떨어집니다.

즉, 병렬로 처리할 수 밖에 없는 상황으로 인해 신뢰성을 얻기 위해서 transaction을 이용합니다.

## Transcation의 특성

4가지의 특성이 존재 (ACID)

### Atomicity ( 원자성 )

Transaction이 부분적으로 실행되거나 중단되지 않는 것을 보장하는 것을 말합니다.

All or Nothing의 개념으로써 작업 단위를 일부분만 실행하지 않는다는 것을 의미합니다.

### Consistency ( 일관성 )

Transaction이 성공적으로 완료되면 일관적인 DB 상태를 유지하는 것을 말합니다.

### Isolation ( 격리성 )

Transaction 수행시 다른 Transaction이 끼어들지 못하도록 보장하는 것을 말합니다.

Transaction 끼리는 서로를 간섭할 수 없습니다.

### Durability( 지속성 )

성공적으로 수행된 Transaction의 경우 영원히 반영되는 것을 의미합니다.

Commit 시 현재 상태를 영원히 보장됩니다.

## 각 특성을 유지하는 방법

### Atomicity 보장

수행하고 있는 Transaction에 의해 변경된 내용을 유지하면서, 이전에 Commit된 상태를 임시 영역에 따로 저장함으로서 보장합니다.

현재 수행하고 있는 Transaction에서 Error 발생 시, 현재 내역을 날려버리고 임시 영역에 저장했던 상태로 rollback을 합니다.

이전 data들이 임시로 저장되는 영역을 rollback segment 라고 하며, 현재 수행하고 있는 Transaction에 의해 새롭게 변경되는 내역을 database table이라고 합니다.

### Consistency 보장

Transaction에서 Consistency은 Transaction 수행 전, 후에 data model의 모든 제약조건(Primary Key, Foreign Key, Domain Constraints)을 만족하는 것을 통해 보장합니다.

또한 Transcation의 일관성은 이벤터와 조건이 발생했을 때, Trigger를 통해 보장합니다.

### Isolation 보장

병행 Transaction에 대해서 먼저 알아야 합니다.

#### 병행 처리 ( Concurrent Processing )

Transaction에 정해진 시간을 할당해서 작업을 하다가 부여된 시간이 끝나면, 다른 Transaction을 실행하는 방식으로 CPU 병렬처리랑 비슷합니다.

이렇게 여러 Transaction을 동시에 처리하다가 공통의 data를 다른 Transaction에 의해 방해 받을 수도 있습니다.

이를 위해 Transaction의 Isolation은 보장되어야 합니다.

이를 위해서 Shared_Lock과 Exclusive Lock을 이용합니다. 구체적인 방법은 

[Isolation Level](https://nephelai.github.io/internshipline/2019/07/15/isolation-level/)을 참조하면 됩니다.

## 출처

[victolee Blog](https://victorydntmd.tistory.com/129)

[위키백과](https://ko.wikipedia.org/wiki/ACID)