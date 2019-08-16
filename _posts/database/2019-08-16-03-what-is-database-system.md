---
layout: post
title: 03. What is Database System
tags: [Introduction To Database]
categories: [database]

---

## Database System 이란

Database System은 데이터 베이스에 데이터를 저장하고, 저장된 데이터를 관리하여 필요한 정보를 생성해주는 시스템입니다. 따라서 다양한 목적의 정보를 처리 시스템을 구축하는 데 필요한 핵심 요소가 됩니다.

![Database System](https://nephelai.github.io/images/posts/database_system.jpg)

database system은 다음 그림과 같이 데이터 베이스와 DBMS와 User, 그리고 사용자가 DB에 접근하기 위해서 사용하는 데이터 언어로 구성되어 있습니다.

## Database 구조

### Schema(스키마)

Schema는 Database에 저장되는 데이터 구조와 제약조건을 정의한 것입니다.

또한 정의된 Schema에 따라 DB에 실제로 저장된 값이 Instance 입니다. 보통 Schema는 한번 정의되면 자주 변경되지는 않지만, Instance는 계속 변하는 특성이 있습니다.

### 3단계 Database 구조 (3-level database architecture)

3단계 database 구조는 하나의 database를 3단계로 나누어 이해합니다. 개별 사용자 관점에서 바라보는 **외부 단계(External Level)**, 조직 전체의 관점에서 바라보는 **개념 단계(Conceptual Level)**, 물리적인 저장 장치의 관점에서 바라보는 **내부 단계(Internal Level)**로 나뉩니다.

위와 같이 3단계로 나누고, 각 단계별로 다른 추상화(Abstraction)를 제공하면 데이터 베이스를 더욱 효과적으로 관리할 수 있습니다. 일반적으로 내부 단계에서 외부 단계로 갈수록 추상화 level이 높아집니다. 즉, 늦은 단계에서 데이터의 저장·유지와 관련된 복잡한 내용을 숨기고 필요한 데이터만 단순화한 외부 단계의 관점을 사용자들에게 제공할 수 있습니다.

1. 외부 단계(External Level)

개별 사용자(User) 관점에서 데이터 베이스를 이해하고 표현합니다. 하나의 DB를 여러 User가 함께 사용하지만 각 User가 전체 DB에 관심이 있지는 않습니다. 오직 자신과 관련된 데이터에만 관심을 갖습니다. 즉, User마다 생각하는 DB의 구조가 다릅니다. 이처럼 외부 단계에서 User에게 필요한 DB를 정의한 것을 External Schema라고 합니다. External Schema는 각 User가 생각하는 DB의 모습을 표현한 논리적인 구조로 User마다 다릅니다.

2. 개념 단계(Conceptual Level)

데이터 베이스를 이용하는  User들의 관점을 통합하여 데이터 베이스를 조직 전체의 관점에서 이해하고 표현합니다. Database Management System이나 Database Administer 같은 경우 DB의 일부분이 아닌 전체 DB에 관심을 갖습니다. 개념 단계에서는 이와 같이 DBMS나 DBA의 관점에서 모든 User에게 필요한 데이터를 통합하여 전체 DB의 논리적 구조를 정의합니다. 이를 개념 스키마(Conceptual Schema)라고 합니다.

개념 스키마의 경우 전체 DB에 어떤 Data가 저장되는지, data 간에는 어떤 관계가 존재하고 어떤 제약조건이 있는지 정의할 뿐만 아니라, 데이터에 대한 보안 정책이나 접근 권한에 대한 정의도 포함합니다. 하지만 데이터를 물리적으로 저장하는 방법이나 데이터 저장 장치와는 독립적입니다. 각 User는 이 개념 스키마의 일부분을 사용합니다. 즉, 외부 스키마는 개념 스키마를 기초로 하여 User의 목적에 맞게 만들어 집니다. **일반적으로 언급하는 Schema는 이 개념 Schema 입니다.**

3. 내부 단계(Internal Level)

DB를 Disk나 Tape 같은 저장 장치의 관점에서 이해하고 표현합니다. 즉, 내부 단계에서는 전체 DB가 저장 장치에 실제로 저장되는 방법을 정의하며 이를 내부 스키마(Internal Schema)라고 불립니다. DB는 저장 장치 내에서 파일 형태로 저장되는데 내부 Schema는 파일에 데이터를 저장하는 Record 구조, Record를 구성하는 Field의 크기, Index를 이용한 Record 접근 경로 등을 정의합니다. 이는 물리적인 저장구조를 표현하고 있으므로 DB에 하나만 존재합니다.

### 데이터 독립성

실제 데이터는 물리적 저장 장치에 저장된 DB에만 존재하므로 사용자가 자신의 외부 Schema를 통해 원하는 데이터를 얻으려면 내부 Schema에 따라 저장된 DB에 접근해야 합니다. 그러므로 **3가지 Schema 사이에는 유기적인 대응관계가 성립해야 합니다.**

이런 Schema들 사이에 대응 관계를 **사상** 또는 **매핑(Mapping)**이라고 합니다. 이렇게 나누고 Mapping을 하는 궁극적인 목표는 **데이터 독립성(data independency)** 때문입니다. 여기서 데이터 독립성이란, 하위 Schema를 변경하여도 상위 Scema가 영향을 받지 않는 것을 의미합니다.

![3-level Database Architecture](https://nephelai.github.io/images/posts/3-level_database_architecture.jpg)

1. 논리적 데이터 독립성 : Logical data independency

개념 Schema가 변경되더라도 외부 Schema가 영향을 받지 않는 것을 의미합니다. 이를 Application Interface 라도고 합니다. 즉, 외부 Schema의 User가 전체 DB의 논리적 구조가 변경되었음을 알 필요가 없다는 것을 의미합니다.

2. 물리적 데이터 독립성 : Physical data independency

내부 Schema가 변경되더라도 개념 Schema는 영향을 받지 않는 것을 의미합니다. 이를 Storage Interface 라고도 합니다. 내부 Schema에 새로운 Index가 추가되거나 기존 Index가 삭제되는 경우에도 개념 Schema는 영향을 받지 않습니다.

### 데이터 사전

데이터 베이스는 실제 데이터를 저장하지만 **저장된 데이터를 올바르게 관리하고 이용하기 위해서 부가 정보도 저장해야합니다.** 대표적인 부가 정보로는 Schema와 Mapping 정보가 있습니다.

이런 부가 정보를 저장하는 곳을 데이터 사전(data dictionary) 또는 data catalog 라고 합니다. 이런 데이터에 관한 정보를 데이터에 대한 데이터, meta data라고 합니다.

data dictionary로 DB의 일종이기 때문에 사용자가 사용하는 DB를 **User Database**라 하고, meta data를 저장하는 DB를 **System Database**라 분리해서 부르기도 합니다. 이 둘의 차이는 접근 권한에 있습니다. User Database의 경우 사용자가 접근할 수 있지만, System Database의 경우는 System만 접근할 수 있다는 차이가 있습니다.

## Database User

User는 DB를 이용하기 위해서 접근하는 모든 사람을 의미합니다. 이를 이용 목적에 따라 분리할 수 있는데, 크게 3가지로 분류할 수 있습니다.

1. DBA : DB 관리자로 DB System을 운영하고 관리합니다.
2. User : DB에 접근하여 Data를 조작합니다. Data INSERT, UPDATE 등.
3. Application Programmer : data language를 이용하여 Application을 작성합니다. 데이터 조작어(DML)를 직접 삽입합니다. 

## Data Language

data language는 사용자가 DB를 구축하고 이에 접근하기 위해 DBMS와 통신하는 수단입니다. 데이터 언어를 사용 목적에 따라 데이터 정의어(DDL), 데이터 조작어(DML), 데이터 제어어(DCL)로 나눌 수 있습니다.

### 데이터 정의어(DDL : Data Definition Language)

새로운 데이터 베이스를 구축하기 위해 Schema를 정의하거나 기존 Schema의 정의를 삭제 또는 수정하기 위해 사용하는 데이터 언어입니다. DDL로 정의된 Schema는 data dictionary에 저장되고, 삭제나 수정이 발생하면 이 내용도 data dictionary에 반영됩니다.

### 데이터 조작어(DML : Data Manipulation Language)

사용자가 데이터의 INSERT·DELETE·UPDATE·SELECT 등의 처리를 DBMS에 요구하기 위해 사용하는 데이터 언어입니다. 사용자가 실제 데이터 값을 활용하기 위해 사용하는 것이 DML입니다. DML은 설명 방식에 따라 절차적 DML과 비절차적 DML로 나뉩니다.

* 절차적 DML (procedural DML) : User가 어떤(What) 데이터를 원하고 해당 데이터를 얻으려면 어떻게(How) 처리해야 하는지 설명한다.
* 비절차적 DML (nonprocedural DML) : User가 어떤(What) 데이터를 원하는지만 설명한다. 즉, 어떻게(How) 부분은 DBMS에게 맡긴다. DBMS에게 어떤 데이터를 원하는지만 알려주므로 선언적 언어(declarative language)라고도 한다.

### 데이터 제어어(DCL : Data Control Language)

데이터 베이스에 저장된 데이터를 여러 사용자가 무결성과 일관성을 유지하며 문제없이 공유할 수 있도록, 내부적으로 필요한 규칙이나 기법을 정의하는 데 사용하는 데이터 언어입니다. User는 DB를 올바르게 관리하기 위해 필요한 규칙과 기법을 DCL을 이용해 DBMS에게 설명합니다. 이를 이용해 DBMS가 이 규칙과 기법에 따라 DB를 제어하고 보호합니다.

다음과 같은 DBMS의 장점인 특성들을 보장하기 위해서 규칙이나 기법을 정의합니다.

* 무결성(Integrity) : DB에 정확하고 유요한 데이터만 유지
* 보안(Security) : 데이터에 접근하는 것을 차단, 권한을 부여
* 회복(Recovery) : 장애가 발생해도 데이터의 일관성을 유지
* 동시성(Concurrency) : 여러 사용자가 같은 데이터에 동시 접근하여 처리
* 

