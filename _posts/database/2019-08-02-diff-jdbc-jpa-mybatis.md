---
layout: post
title: Difference JDBC, JPA and Mybatis
tags: []
categories: [database]
---

## 영속성(Persistence)

데이터를 생성한 프로그램이 종료가 되더라도 사라지지 않는 데이터의 특성을 말합니다. 이런 영속성을 지니지 않은 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 데이터를 모두 잃어버리게 됩니다.

이런 속성 때문에 File System, Relation DataBase, Object DataBase 등을 활용하여 데이터를 영구적으로 저장하여 영속성을 부여합니다.

### Persistence Layer

프로그램의 구조(Architecture)에서 데이터에 영속성을 부여해주는 계층을 의미합니다. JDBC를 이용하여 직접 구현할 수 있지만 **Persistence Framework**를 이용한 개발이 많이 이루어집니다.

![Backend Architecture](https://nephelai.github.io/images/posts/backend_architecture.jpg)

### Persistence Framework

JDBC Programming의 복잡함이나 번거로움이 없이 간단한 작업만으로 DB와 연동되는 System을 빠르게 개발할 수 있으며 안정적인 구동을 보장합니다.

Persistence Framework는 크게 2가지로 나뉠 수 있습니다.

* SQL Mapper
* ORM (Object-Relational Mapping)

## SQL Mapper와 ORM

SQL Mapper의 경우, SQL을 직접 명시해주어야 하지만, ORM의 경우 DB 객체를 Java의 객체로 Mapping 하면서 객체 간의 관계를 바탕으로 SQL을 자동으로 생성해 줍니다.

SQL Mapper는 단순히 Field를 Mapping하는 것이 목적이지만, ORM의 경우 Relational DB의 'Relation'을 Object에 반영하는 것이 목적입니다.

### SQL Mapper

* SQL (Mapping) Object Field

SQL 문장으로 직접 DB의 data를 다룹니다. 즉, SQL 명시가 필요합니다.

예를 들어 Mybatis, JdbcTemplate이 있습니다.

### ORM(Object-Relational Mapping)

* Database data (Mapping) Object Field

객체를 통해 간접적으로 DB data를 다룹니다. ORM을 이용하면, SQL Query가 아닌 직관적인 코드(Method)를 통해 데이터 조작을 할 수 있습니다. 또한 객체 간의 관계를 바탕으로 SQL이 자동으로 생성됩니다. Persistant API라고도 할 수 있습니다.

예를 들어 JPA, Hibernate 등이 있습니다.

### ORM의 장단점

#### 장점

**객체 지향적인 코드로 인해 더욱 직관적으로 Business Logic에 더욱 집중할 수 있게 도와줍니다.**

선언문, 할당, 종료 같은 부수적인 코드가 급격히 감소하며 각종 객체에 대한 코드를 별도로 작성하기 때문에 코드의 가독성이 높아집니다. 또한 SQL의 절차적이고 순차적인 접근이 아닌 객체 지향적인 접근으로 인해 생산성이 증가합니다.

**재사용 및 유지보수의 편리성이 증가합니다.**

ORM은 독립적으로 작성되어 있고, 해당 객체들을 재활용 할 수 있습니다. 그래서 Model에서 가공된 data를 Controller에 의해 View와 합쳐지는 형태로 구성하는데 유리합니다. 또한 Mapping 정보가 명확하여, ERD를 보는 것에 대한 의존도를 낮출 수 있습니다.

**DBMS에 대한 종속성이 줄어듭니다.**

대부분의 ORM Solution은 DB에 종속적이지 않습니다. 예를 들어 DBMS를 교체하는 거대한 작업이 있다고 하면, 비교적 적인 Risk와 시간이 소요됩니다. 또한 간결하고 빠른 data 가공이 가능합니다.

#### 단점

**완벽히 ORM으로만 서비스를 구현하기가 어렵습니다.**

사용하기에는 편리하지만 설계를 매우 신중하게 해야합니다. 프로젝트의 복잡성이 커질경우 난이도 또한 올라가며, 잘못 구현이 된 경우 속도 저하 및 심각할 경우 Consistency 가 무너지는 문제점이 발생합니다. 

**Procedure가 많은 System에서는 ORM의 객체 지향적인 장점을 활용하기 어렵다.**

이미 Procedure가 많은 System에서는 다시 객체로 바꾸어야 합니다. 그 과정에서 생산성 저하나 Risk가 많이 발생할 수 있습니다.

## JDBC (Java DataBase Connectivity)

![Data Access Layaer](https://nephelai.github.io/images/posts/data_access.jpg)

JDBC는 DB에 접근할 수 있도록 Java에서 제공하는 API 입니다. 모든 Java의 Data Access의 기본이며, 모든 Presistemce Framework는 내부적으로 JDBC API를 이용합니다. 또한 JDBC는 DB에서 자료를 관리(읽기, 쓰기, 수정, 삭제)하는 방법 제공합니다.

## JPA(Java Persistance API)

JAVA ORM 기술에 대한 API Standard Specification으로 JAVA에서 제공하는 API입니다. 즉, JPA는 ORM을 사용하기 위한 표준 Interface를 모아둔 것입니다. 기존의 EJB에서 제공되었던 Entity Bean을 대체하는 기술입니다.

JPA의 구성 요소는 3가지로 구성됩니다.

1. javax.persistance package로 정의된 API 그 자체
2. JPQL(Java Persistence Query Language), 보통의 Query와 비슷합니다.
3. Object - Relation Meta Data

User가 원하는 JPA 구현체를 선택하여 사용할 수 있습니다. JPA의 대표적인 구현체로는 Hibernate, DataNucleus, OpenJPA, TopLink Essentials 등이 있습니다. 이런 구현체들을 ORM Framework 라고 합니다.

## 출처

[[JDBC] JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)