---
layout: post
title: Isolation Level
tags: [database, acid]
categories: [InternshipLine]
---

Isolation Level 이란 Transaction 에서 일관성이 없는 데이터를 허용하는 수준을 이야기합니다.

여기서 Transaction 이란 정보의 교환이나 database의 갱신 등 연관되는 작업들에 대한 일련의 연속을 의미합니다.

기본적으로 4가지의 Level이 존재합니다.

* Read Uncommitted
* Read Committed
* Repeatable Read
* Serializable

## Read Uncommitted

Transaction 이 끝나지 않은 상황에서 각기 다른 Transaction 이 변경한 내용에 대한 조회(Select)가 가능합니다. Database로써의 일관성(consistency)를 유지할 수 없습니다.

* dirty read, non-repeatable read, phantom read

## Read Committed

조회 시 data에 대한 Shared Lock이 됩니다. Commit이 이루어진 data가 조회됩니다. Commit이 이루어지지 않는 동안에는 대기하는 단점이 있습니다.

* non-repeatable read, phantom read

## Repeatable Read

Transaction 이 범위내에서는 조회한 데이터의 내용이 항상 동일함을 보장해줍니다. 조회가 들어간 시점에서 데이터의 내용이 일정하다는 것을 보장해줍니다. insert를 동작을 하지만 update는 조회다 모두 끝난 이후에 동작되는 것을 볼 수 있습니다.

* phantom read

## Serializable

모든 동작이 직렬화 되어 동작합니다. 따라서 Insert 또한 동작하지 않고 select가 다 끝난 이후에 동작하게 됩니다.

## Read 종류

* dirty read : commit을 기다리지 않고 값을 읽어온다.
* unrepeatable read
  * SELECT 경우 Shared Lock
  * 이 외 연산(INSERT, UPDATE, DELETE)의 경우 Exclusive Lock
  * 공유 잠금의 데이터가 변경 중인 경우에는 읽을 수 없다.
* phantom read : 데이터가 변경시키지 못할 뿐 새로운 데이터를 추가 / 삭제하는 것은 가능하다.

## 출처

[Medium]([https://medium.com/@wonderful.dev/isolation-level-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-94e2c30cd8c9](https://medium.com/@wonderful.dev/isolation-level-이해하기-94e2c30cd8c9))