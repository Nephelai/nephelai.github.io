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

### 장애의 유형

**System이 제대로 동작하지 않는 상태**를 장애(Failure)라고 합니다. 장애가 발생하는 원인은 매우 다양하지만 크게 3가지의 유형으로 분리할 수 있습니다.

* Transaction Failure : Transaction의 수행 중 오류가 발생하여 문제가 생긴 상태
* System Failure : HW의 결함으로 인해 문제가 생긴 상태
* Media Failure : Disk Device의 결함으로 인해 Disk에 저장된 DB의 일부 혹은 전체가 손상된 상태

### DB의 저장 장치 종류

DB의 data는 기본적으로 저장장치에 저장됩니다. 저장 장치에 따라 3가지로 분리할 수 있습니다.

* 비휘발성(non-volatile) 저장 장치 : 장애가 발생하여도 저장된 데이터가 손실되지 않습니다. 단, 저장 장치에 이상이 발생하면 데이터가 손실될 수 있습니다.
* 휘발성(volatile) 저장 장치 : 장애가 발생하면 저장된 데이터가 손실됩니다.
* 안정(stable) 저장 장치 : non-volatile 저장 장치를 이용해 데이터 복사본 여러개를 만드는 방법으로 어떤 장애가 발생해도 데이터가 손실되지 않고 데이터를 영구적으로 저장합니다.

### 회복 기법

회복은 **DB에 장애가 발생했을 때 장애가 발생하기 전의 모순이 없고 일관된 상태로 복구시키는 것**으로, DB 관리 시스템에 있는 recovery manager가 담당합니다. 회복 관리자는 장애 발생을 탐지하고, 장애가 탐지되면 DB 복구 기능을 제공합니다.  대부분 장애가 일어난 DB를 복구하는 동안에는 DB에 접근하여 업무를 처리할 수 없으므로, DB를 회복시키는 작업은 빠른 시간 내에 이루어져야 합니다.

#### 회복을 위한 연산

DB 회복의 핵심 원리는 **데이터 중복**입니다. 데이터를 별도의 장소에 미리 복사해두고, 장애가 발생했을 때 복사본을 이용해 원래의 상태로 복원하는 것입니다. 이때 사용하는 2가지 방법이 존재합니다.

* 덤프(dump) : DB 전체를 다른 저장 장치에 주기적으로 복사하는 방법
* 로그(log) : DB에서 변경 연산이 수행될 떄마다 데이터를 변경하기 이전 값과 변경한 이후 값을 별도의 파일에 기록하는 방법

장애가 발생하였을 때, dump나 log 방법으로 중복 저장한 데이터를 이용해 DB를 복구하는 가장 기본적인 방법은 redo와 undo 연산을 수행하는 방법입니다.

* 재실행(redo) : 가장 최근에 저장한 DB 복사본을 가져온 후 log를 이용해 복사본이 만들어진 이후에 실행된 모든 변경 연산을 재실행하여 장애가 발생하기 직전의 DB 상태로 복구합니다.
* 취소(undo) : 로그를 이용해 지금까지 실행된 모든 변경 연산을 취소하여 DB를 원래의 상태로 복구합니다.

#### 로그 회복 기법

로그 회복 기법은 DB 회복 기법 중에서 일반적으로 많이 사용되는 방법으로 **데이터를 변경한 연산 결과를 DB에 반영하는 시점에 따라 즉시 갱신 회복 기법과 자연 갱신 회복 기법으로 나뉩니다.** 

##### 즉시 갱신(Immediate Update) 회복 기법 

Transaction 수행 중에 data를 변경한 연산의 결과를 DB에 즉시 반영하고 장애 발생에 대비하기 위해 데이터 변경에 대한 내용을 로그 파일에도 기록합니다. 장애가 발생하면 로그 파일에 기록된 내용을 참조하여, 장애 발생 시점에 따라 redo나 undo 연산을 실행하여 DB를 복구합니다.

* Transaction이 완료되기 전 장애가 발생한 경우 : undo 연산
* Transaction이 완료된 후 장애가 발생한 경우 : redo 연산

##### 지연 갱신(Deferred Update) 회복 기법

Transaction이 수행되는 동안에는 데이터 변경 연산의 결과를 DB에 즉시 반영하지 않고 로그 파일에만 기록해 두었다가, Transaction이 부분 완료된 후에 로그에 기록된 내용을 이용해 DB에 한 번에 반영합니다. Transaction이 수행되는 동안 장애가 발생할 경우 로그에 기록된 내용을 버리기만 하면 DB가 원래 상태를 그대로 유지하게 됩니다. 지연 갱신 회복 기법에서는 undo 연산은 필요 없고, redo 연산만 필요하므로 로그 레코드에 변경 이전 값을 기록할 필요가 없습니다.

* Transaction이 완료되기 전 장애가 발생한 경우 : 로그 내용을 무시하고 버립니다.
* Transaction이 완료된 후 장애가 발생한 경우 : redo 연산

#### 검사 시점 회복 기법

로그를 이용한 회복 기법은 로그 전체를 분석하여 로그에 기록된 모든 Transaction을 대상으로 redo나 undo 중에서 적용할 회복 연산을 결정해야 합니다. 하지만 로그 전체를 대상으로 회복 기법을 적용하면 DB 회복에 너무 많은 시간이 걸리고 redo 연산을 수행할 필요가 없는 Transaction에도 redo 연산을 실행하는 일이 발생하기도 합니다. 이러한 비효율성을 해결하기 위해 제안된 방법이 검사 시점 회복 기법입니다.

검사 시점 회복 기법은  **로그 기록을 이용하되, 일정 시간 간격으로 검사 시점(CheckPoint)을 만들어 주는 기법**입니다. 장애가 발생하면 가장 최근 검사 시점 이전의 Transaction에는 회복 작업을 수행하지 않고, 이후의 Transaction에만 회복 작업을 수행합니다.

회복 작업의 범위가 검사 시점으로 정해지므로 불필요한 회복 작업을 수행하지 않아 DB 회복 시간이 단축된다는 장점이 있습니다.

#### 미디어 회복 기법

디스크의 경우 메모리보다 장애가 드물게 발생하지만 디스크 Header의 고장과 같은 원인으로 장애가 발생할 수 있습니다. **디스크에 발생할 수 있는 장애에 대비한 회복 기법**을 미디어 회복 기법이라고 합니다.

미디어 회복 기법은 전체 DB의 내용을 일정 주기마다 다른 안전한 저장 장치에 복사해두는 덤프(dump)를 이용합니다. 디스크 장애가 발생하면 가장 최근에 복사해둔 dump를 이용해 장애 발생 이전의 일관된 DB 상태로 복구합니다. 그 다음 필요에 따라 로그 내용을 바탕으로 redo 연산을 수행합니다.

전체 DB를 다른 저장 장치에 복사하는 방법은 비용이 많이 들고, 복사하는 동안 Transaction 수행을 중단해야 하므로 CPU 낭비가 많이 발생합니다.

## Concurrency Control

DBMS의 경우 여러 사용자가 동시에 DB를 사용할 수 있도록 여러 Transaction이 동시에 수행될 수 있도록 *Concurrency*를 지원합니다. 실제로 Concurrency는 여러 Transaction이 번갈아 가면서 수행되는 *Interleaving* 방식으로 진행됩니다. 여러 Transaction이 서로 영향을 받지 않고 정화간 수행 결과를 얻을 수 있도록 제어가 이루어져야 합니다. 이렇게 Concurrency를 제어하는 것을 병행 제어(Concurrency Control)이라고 합니다.

### Concurrency의 문제

Control 없이 수행되는 병행 수행은 여러 문제를 야기합니다.

#### 갱신 분실(Lost Update)

**하나의 Transaction이 수행한 data 변경 연산의 결과를 다른 Transaction이 덮어써 Update 연산이 무효되는 것**입니다.

#### 모순성(Inconsistency)

**하나의 Transaction이 여러 개의 data 변경 연산을 실행할 때 일관성 없는 상태의 DB에서 데이터를 가져와 연산을 실행하므로써 모순된 결과가 발생하는 것**입니다. Transaction 내에서 분리되어 수행되는 경우, 만일 Transaction안에 연산이 여러 개일 경우 연산 하나 하고 Transaction이 다른 연산을 수행한다면 일관성 유지가 깨지는 현상이 발생합니다.

#### 연쇄 복귀(Cascading Rollback)

**Transaction이 완료되기 전에 장애가 발생하여 rollback 연산을 수행하면, Transaction이 장애 발생 전에 변경한 데이터를 가져가 변경 연산을 실행한 또 다른 Transaction에도 rollback 연산을 연쇄적으로 수행하는 것**을 의미합니다. 즉, rollback 수행 시 이에 영향을 받는 다른 Transaction에서도 rollback이 수행되어야 합니다. 하지만 장애가 발생한 Transaction이 rollback 연산을 실행하기 전에 변경한 data를 가져가 사용한 다른 Transaction이 수행을 완료해버리면 rollback 연산을 수행할 수 없서져 문제가 발생합니다.

### Transaction Schedule

**Transaction에 포함되어 있는 연산들을 수행하는 순서**를 Transaction Schedule이라고 합니다. 이는 크게 3가지 방식으로 나누어 볼 수 있습니다.

#### 직렬 스케줄 (Serial Schedule)

**Interleaving 방식을 이용하지 않고 각 Transaction 별로 연산들을 순차적으로 실행**시키는 것입니다. 이 경우 모든 Transaction이 완료될 때까지 다른 Transaction이 방해받지 않고 독립적으로 수행됩니다. 따라서 직렬 스케줄에 따라 Transaction이 수행되는 경우 항상 모순이 없는 정확한 결과를 얻습니다.

하지만 직렬 스케줄의 경우는 각각의 Transaction을 독립적으로 수행하기 때문에 비효율적이며 성능이 떨어집니다.

#### 비직렬 스케줄(Non-Serial Schedule)

**Interleaving 방식을 이용하여 Transaction을 병행해서 수행시키는 것**입니다. 비직렬 스케줄은 Transaction이 돌아가면서 연산들을 실행하기 때문에 하나의 Transaction이 완료되기 전에 다른 Transaction의 연산이 수행될 수 있습니다. 여러 Tansaction이 Concurrency를 발생할 수 있으므로 *갱신 분실, 모순성, 연쇄 복귀* 등의 문제를 발생할 수 있어 결과의 정확성을 보장할 수 없습니다.

#### 직렬 가능 스케줄(Serializable Schedule)

**직렬 스케줄에 따라 수행한 것과 같이 정확한 결과를 생성하는 비직렬 스케줄**입니다. 모든 비직렬 스케줄이 직렬 가능한 것은 아닙니다. 하지만 이는 InterLeaving 방식을 이용하여 여러 Transaction을 Concurrency 하게 수행하면서도 정확한 결과를 얻을 수 있습니다.

직렬 가능 스케줄을 이용해 Transaction을 병행 수행을 하면 좋지만 이를 판별하는 일이 쉽지 않습니다. 다수의 Transaction을 대상으로 비직렬 스케줄을 찾아내는 것이 어려울 뿐만 아니라, 하나씩 수행하면서 직렬 스케줄과 같은 결과가 나오는지 비교해야하는 일이 힘들기 때문입니다. 따라서 대부분의 DBMS에서는 직렬 가능 스케줄인가를 검사하기 보다는 **직렬 가능성을 보장하는 병행 제어(Concurrency Control) 기법을 사용합니다.**

### 병행 제어 기법

**여러 Transaction을 병행 수행하면서도 정확한 결과를 얻을 수 있는 직렬 가능성을 보장 받기 위해서 사용**합니다. 기본 원리는 모든 Transaction이 따르면 직렬 가능성이 보장되는 나름의 규약(Policy)을 정의하고, Transaction들이 이 규약을 따르도록 하는 것입니다.

 