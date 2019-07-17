---
layout: post
title: Database의 종류
tags: [database]
categories: [InternshipLine]
---

DataBase는 **작성된 목록으로써 여러 응용 시스템들의 통합된 정보들을 저장하여 운영할 수 있는 공용 데이터들의 묶음** 이라고 정의합니다. 즉, 데이터들의 접근하고 관리할 수 있게 제공해주는 시스템입니다.

Database의 종류는 크게 4가지의 종류로 나눌 수 있습니다.

1. 계층형 Database : data의 관계를 Tree구조로 정의하고, parent - child 형태를 갖는 구조입니다. 하지만 data의 중복이 문제가 생깁니다.
2. 네트워크형 Database : 계층형 Database의 data 중복 문제를 해결하고 Record 간의 다양한 관계를 그물처럼 갖는 구조입니다. 하지만 복잡한 구조 때문에 추후에 구조를 변경한다면 많은 어려움이 따릅니다.
3. 관계형 Database : 흔히 표현하는 Column과 Record로 구성된 Table간의 관계를 나타낼 때 사용합니다. 이렇게 표현된 Data를 SQL(Structured Query Language)을 사용하여 data 관리 및 접근을 합니다.
4. NoSQL Database : 관계형 Database보다 덜 제한적인 일관성 모델을 이용한다. Key-Value 형태로 저장되고, Key를 이용해 data 관리 및 접근을 한다.

이 중에서 우리가 흔히 사용하는 Database는 관계형 데이터베이스(RDB)와 NoSQL 데이터베이스이다. 

### 관계형 데이터베이스 (SQL)

#### 장점

* 다양한 용도로 사용이 가능하고, 일반적으로 높은 성능을 보여주고 있습니다.(범용적 / 고성능)
* 데이터의 일관성을 보증합니다. 무결성 유지
* 정규화에 따른 갱신 비용 최소화
* 복잡한 형태의 query가 가능하여 원하는 data를 추출할 수 있습니다. (join 등의 연상을 지원)
* Scale-up에 유리 : 한 대의 머신을 중심으로 확장

#### 단점

* 대량의 데이터 입력, 조회시 성능히 저하됩니다.
* 갱신이 발생한 테이블의 Index 생성 및 Schema 변경, 많은 비용이 발생합니다.
* Table 간의 관계를 맺는 과정이 굉장히 복잡합니다.

#### 주요 제품

Mysql, Oracle, SQLite, PostgreSQL, MSSQL

### NoSQL 데이터베이스 (Not Only SQL)

#### 장점

* 대용량 data 처리에 용이합니다.
* data 분산 처리에 용이합니다.
* Cloud Computing
* 빠른 Read / Write 속도
* 유연한 Data 모델링
* Scale-out 에 유리 : 여러 대의 서버를 중심으로 확장

#### 단점

* 다양하고 복잡한 data query는 불가능합니다.
* 기존의 data를 update 하는데 오랜 시간이 소요됩니다.
* 데이터의 일관성이 보장되지 않습니다.

#### 종류

* Key - Value 
  * 휘발성 / 영속성
  * Memchached, Redis 등이 존재합니다.
* Document
  * Schema의 정의가 없습니다.
  * MonogoDB, CouchDB
* Big Table(Column 형)
  * 뛰어난 확장성, 검색에 유리합니다.
  * Hbase, Casandara
* Graph 형

예를 들어 SNS를 개발시에는 User, Feed, Comment 등의 다양한 data가 존재하고 data 간의 관계가 존재하기 때문에 SQL을 사용하면 보다 빠르게 데이터를 접근할 수 있습니다. 만약 이 내용을 NoSQL로 구현을 한다면, 모든 내용을 하나의 Document로 저장하면 되겠지만, 그렇게 될 경우 data의 중복이 엄청나기 때문에 올바르지 못합니다. 중복을 피하기 위해서 Key-Value로 저장하고 있다면 그 과정의 비용이 상당합니다.





## 출처 

[DBMS - 데이터베이스 종류와 장/단점](https://ourcstory.tistory.com/30)

[Medium - 조영하](https://medium.com/@duddk1551/db-rdb-관계형데이터베이스-와-nosql-adbd21f6f9f1)