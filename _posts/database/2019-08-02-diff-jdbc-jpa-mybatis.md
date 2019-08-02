---
layout: post
title: Difference JDBC, JPA and Mybatis
tags: []
categories: [database]
---

## 영속성(Persistence)

데이터를 생성한 프로그램이 종료가 되더라도 사라지지 않는 데이터의 특성을 말합니다. 이런 영속성을 지니지 않은 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 데이터를 모두 잃어버리게 됩니다.

이런 속성 때문에 File System, Relation DataBase, Object DataBase 등을 활용하여 데이터를 영구적으로 저장하여 영속성을 부여합니다.



## 출처

[[JDBC] JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)