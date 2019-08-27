---
layout: post
title: 10. Recovery & Concurrency Control
tags: [Introduction To Database]
categories: [database]

---

## Transaction

### Concept of Transaction

DB는 **다수의 사용자가 동시에 사용하더라도 항상 정확한 데이터를 유지**해야 합니다. 또한  **DB에 장애가 발생하여도 빠른 시간 내에 원래의 상태로 복구**할 수 있어야 합니다. 이와 같이 일관된 상태를 유지할 수 있도록 DB가 Transaction을 관리합니다.

**Transaction은 하나의 작업을 수행하는 데 필요한 DB의 연산들을 모아놓은 것**입니다. DB에서는 논리적인 작업의 단위가 됩니다. 또한 DB에 장애가 발생하였을 때 복구하는 작업의 단위가 되기도 합니다. 간단하게 SQL문들의 모임이라고 생각할 수도 있습니다.

Transaction의 모든 Query가 완벽하게 처리되거나 하나도 처리되지 않아야 DB가 일관된 상태를 유지할 수 있습니다. 따라서 DB의 무결성과 일관성을 보장하기 위해서 작업을 수행하는데 필요한 연산들을 하나의 Transaction으로 제대로 정의하고 관리해야 합니다.

### Transaction의 특성

ACID 특성이라고도 불리며 4가지 특성입니다.

#### 원자성(Atomicity)

**Transaction을 구성하는 연산들이 모두 정상적으로 실행되거나 하나도 실행되지 않아야 한다(All or Nothing)**는 것을 의미합니다. 만약 Transaction을 수행하다가 장애가 발생하여 작업을 완료하지 못했다면, 지금까지 실행한 연산 처리를 모두 취소하고 DB를 Transaction 작업 전의 상태로 되돌려(회복) Transaction의 Atomicity를 보장해야 합니다.

#### 일관성(Consistency)

**Transaction이 성공적으로 수행된 후에도 DB가 일관된 상태를 유지해야 한다**는 것을 의미합니다. 즉, Transaction이 수행되기 전에 DB가 일관된 상태였다면 Transaction이 수행이 완료된 후 결과를 반영한 DB도 또 다른 일관된 상태가 되어야 한다는 의미입니다. Transaction이 수행되는 동안에는 일시적으로 일관된 상태가 아닐 수 있지만 Transaction 수행이 완료된 이후에는 일관된 상태를 유지해야 합니다.

#### 격리성(Isolation)

**현재 수행중인 Transaction이 완료될 때까지 Transaction이 생성한 중간 연산 결과에 다른 Transaction들이 접근할 수 없다**는 것을 의미합니다. 일반적으로 DB System에서는 여러 Transaction이 동시에 수행되지만 각 Transaction이 독립적으로 수행될 수 있도록 다른 Transaction의 중간 연산 결과에 서로 접근하지 못하게 합니다. 즉 같은 Data에 대해서는 동시에 접근할 수 없도록 제한한다는 의미입니다.

#### 지속성(Durability)

**Transaction이 성공적으로 완료된 후 DB에 반영한 수행 결과는 어떤 경우에도 손실되지 않고 영구적이어야 한다**는 것을 의미합니다. 즉, System에 장애가 발생하더라도 Transaction 작업 결과는 없어지지 않고 DB에 그대로 남아 있어야 한다는 것을 의미합니다. Transaction의 지속성을 보장하기 위해서는 System에 장애가 발생했을 때 원래 상태로 복구하는 회복 기능이 필요합니다.

Transaction의 각각의 특성은 DBMS의 기능와 연관되어 있습니다.

* Atomicity ⇒ Recovery
* Consistency ⇒ Concurrency Control
* Isolation ⇒ Concurrency Control
* Durability ⇒ Recovery

### Transaction의 연산

Transaction의 수행과 관련하여 주로 사용하는 연산에는 **작업 완료를 의미하는 commit**과 **작업 취소를 의미하는 rollback**이 있습니다.

* commit : Transaction 수행이 성공적으로 완료되었음을 선언하는 연산 (작업 완료)
* rollback : Transaction 수행이 실패했음을 선언하는 연산 (작업 취소)

### Transaction의 상태

![Transcation States](https://nephelai.github.io/images/posts/transaction_states.jpg)

Transaction은 다음과 같이 5가지의 상태를 지닙니다.

* 활동 상태(Active) : **현재 수행중인 상태**
* 부분 완료 상태(Partially Committed) : **Transaction의 마지막 연산이 실행된 직후의 상태**, Transaction의 모든 연산을 처리한 상태입니다. 모든 연산의 처리는 끝났지만 Transcation이 수행된 최종 결과를 DB에 아직 반영하지 않은 상태입니다.
* 완료 상태(Commited) : **Transcation이 성공적으로 완료되어 commit 연산을 실행한 상태**, Transaction이 수행한 최종 결과를 DB에 반영하고 DB가 새로운 일관성을 지닌 상태가 되며 Transaction 종료됩니다.
* 실패 상태(Failed) : HW나 SW문제, Transaction 내부의 오류 등 **여러 이유로 인해 장애가 발생하여 Transaction의 수행이 중단되 상태**, Transaction이 더 이상 정상적으로 수행될 수 없습니다.
* 철회 상태(Aborted) : **Transaction을 수행하는 데 실패하여 rollback 연산을 실행한 상태**, 지금까지 실행한 Transaction의 연산을 모두 취소하고 Transaction이 수행되기 전의 DB 상태로 되돌리면서 Transaction이 종료됩니다.

## Failure & Recovery



