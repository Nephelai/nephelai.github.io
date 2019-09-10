---
layout: post
title: SQL Query Basic
tags: [query]
categories: [database]
---

## Basic Query

* SELECT / INSERT / UPDATE / DELETE
* FROM / WHERE / LIMIT
* Logical Operators
* ORDER BY

## SELECT

**SELECT {보여줄 Column} FROM {찾고자 하는 DB} WHERE {조건문}**

### AS

보여질 Column명을 바꿉니다.

```sql
SELECT year,
       month,
       west,
       south,
       (west + south)/2 AS south_west_avg
  FROM tutorial.us_housing_units
```

(west + south) / 2를 Column으로 보여지는 것이 아니라 **south_west_avg**로 보이게 됩니다.

#### 만일 쓰지 않는다면?

Aggregation 연산으로 Column명이 명확히 나오지 않습니다.

## Logical Operators

### LIKE

지정된 패턴을 찾습니다.

ex) `김%` 는 `김`으로 시작하는 값을 검색합니다.

ex) `김_` 은 `김`으로 시작하면서 길이가 2인 값을 검색합니다.

``` sql
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist LIKE 'Snoop%'
```

#### WHERE 절에서 'group'과 같이 예약어가 사용될 경우

`"group"` 을 이용해서 접근할 수 있습니다.

### IN

LIKE의 확장된 모습으로 여러 개의 패턴을 지정할 때 이용합니다.

```sql
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist IN ('Taylor Swift', 'Usher', 'Ludacris')
 -- Taylor %
```

### BETWEEN

### IS NULL

### AND, OR, NOT

## ORDER BY

순서를 보장하여 결과를 보여줄 때 사용합니다.

```sql
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 2013
 ORDER BY year_rank DESC
```

### DESC

Descent (내림차순) : 큰거부터 보여집니다.

### ASC

Ascent (오름차순) : 작은거부터 보여집니다.

### 여러개의 Column 지정

```sql
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank <= 3
 ORDER BY year_rank, year DESC
```

year_rank로 먼저 비교하고 year로 비교하게 됩니다. year_rank는 ASC이므로 rank가 작은 것부터 나오나 rank_year를 비교하여 큰 것(최근)부터 보여집니다.

#### ORDER BY에 숫자가 들어가는 경우

이는 Column의 위치를 이야기하는 것입니다. 여기서 2는 year_rank, 1은 year을 의미합니다.

```sql
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank <= 3
 ORDER BY 2, 1 DESC
```



## 출처

[Mode - SQL Tutorial](https://mode.com/resources/sql-tutorial/introduction-to-sql)

