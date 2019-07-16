---
layout: post
title: Hibernate는 어떻게 generate Key 전략을 결정할까?
tags: [database, hibernate]
categories: [InternshipLine]

---

JPA(Java Persistence API)는 기본적으로 DB Table Surrogate Key로 Primary Key를 자동 생성하는 기능을 지원합니다.

사용법은 Entity Class에 Id Annotation(@)를 함께 결합하여 GeneratedValue Annotation을 추가합니다.

여기 GeneratedValue Annotation에는 AutoIncrease 전략을 4개로 지정해 줄  수 있습니다.

| 생성 전략     | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| AUTO(default) | JPA 구현체가 자동으로 생성 전략을 결정                       |
| IDENTITY      | 기본키 생성을 Database에 위임한다. 예를 들어 Mysql의 경우 AUTO_INCREMENT를 사용하여 PK를 생성 |
| SEQUENCE      | DataBase의 특별한 Object Sequence를 사용하여 PK를 생성       |
| TABLE         | DataBase에 Key 생성 전용 Table을 하나 만들고 이를 참조하여 기본키를 생성 |

그렇다면 Hibernate AUTO는 어떤 과정으로 전략을 선택할까?

![Hibernate Interpreting AUTO](https://nephelai.github.io/images/posts/Hibernate_Interpreting_AUTO.png)

가장 먼저 @GeneratedValue가 사용된 Java Data Type인 `UUID`를 검사합니다. 그리고 `UUID` 이면 UUID Generator가 됩니다.

#### UUID(Universally Unique Identifier)란?

Java 5부터 UUID Class를 사용해서 유익한 식별자를 생성할 수 있습니다. 32개의 16진수의 수와 4개의 '-'로 구성된 36자리 문자열을 return 한다. 여기에 생성 시간을 더해서 키를 생성시 중복 방지할 수 있습니다.



Data Type이 숫자 (Integer, Long)면 Hibernate Property인 **hibernate.id.new_generator_mappings** 를 확인합니다.

* False의 경우 : 'Native Generator'가 됩니다. 이는 다시 MySQL5Dialect로 결정됩니다.
* True의 경우 : SequenceStyleGenerator를 사용하게 됩니다. 이는 다시 DataBase가 Sequence를 지원하게 되면, Sequence Generator로 결정되고, 지원하지 않는다면, Table Generator로 결정됩니다.

#### 참고

Mysql의 경우 Sequence Object를 지원하지 않습니다.

## 출처

[Medium - 하이버네이트는 어떻게 자동키 생성 전략을 결정하는가?](https://nephelai.github.io/images/posts/Hibernate_Interpreting_AUTO.png)

[THOUGHTS ON JAVA - jpa-generate-primary-keys](https://thoughts-on-java.org/jpa-generate-primary-keys/)

