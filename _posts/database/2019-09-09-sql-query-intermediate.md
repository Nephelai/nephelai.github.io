---
ayout: post
title: SQL Query Intermediate
tags: [query]
categories: [database]

---

## Intermediate Query

* Aggregate Functions
* GROUP BY
* HAVING
* DISTINCT
* CASE / WHEN / THEN / ELSE / END
* JOIN / ON
* UNION

## Aggregate Functions

### COUNT

말 그대로 Table에서 갯수를 찾아내는 함수입니다.

```sql
SELECT COUNT(*)
  FROM tutorial.aapl_historical_stock_price
```

하지만 COUNT 안에 특정 column이 주어지는 경우 column의 값이 NULL이 아닌 값의 갯수를 파악합니다.

```sql
SELECT COUNT(high)
  FROM tutorial.aapl_historical_stock_price
```

### SUM / MIN / MAX / AVG

## GROUP BY

데이터들을 원하는 그룹으로 나누고자 할 때 이용합니다. 나누고자 하는 Group의 Column 명을 SELECT 절과 GROUP BY 절 뒤에 있어야 합니다.

```sql
SELECT year,
       COUNT(*) AS count
  FROM tutorial.aapl_historical_stock_price
  GROUP BY year
```

만일 2개를 이용한다면 2개의 Column을 이용하여 Grouping이 됩니다.

GROUP BY의 Column은 항상 SELECT에 존재해야 합니다.

## HAVING

Aggregate Function은 WHERE 절에서 사용할 수 없습니다. 이 Aggregation을 이용해 조건 비교를 하기 위해서 HAVING을 이용합니다. **HAVING은 GROUP BY와 함께 사용해야 합니다.**

```sql
SELECT year,
       month,
       MAX(high) AS month_high
  FROM tutorial.aapl_historical_stock_price
 GROUP BY year, month
HAVING MAX(high) > 400
```

HAVING의 내용은 항상 SELECT에 존재해야 합니다.

## Query 절의 순서

1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY

다음과 같은 순서로 Query를 정의해야 합니다.

## DISTINCT

SELECT 하려는 Column의 중복을 제거하고 보여집니다.

```sql
SELECT DISTINCT month
  FROM tutorial.aapl_historical_stock_price
```

DISTINCT는 한번 정의하면 모든 Column에 대해서 동작합니다.

```sql
SELECT DISTINCT year, month
  FROM tutorial.aapl_historical_stock_price
```

## Condition

```sql
SELECT player_name,
       year,
       CASE WHEN year = 'SR' THEN 'yes'
            ELSE NULL END AS is_a_senior
  FROM benn.college_football_players
```

year가 `'SR'`인 경우 column `is_a_senior`에 대해서 `yes`나 `NULL`을 기록합니다.

## JOIN

Relational Database를 구축하기 위해서 사용합니다. RDB의 기본은 데이터 정보를 쪼개 여러개의 테이블에 나누어서 저장하는데, 이는 데이터의 중복되는 현상이나 데이터의 일관성을 유지하기 어렵습니다.

하지만 JOIN을 이용하여 데이터의 중복을 막고, 데이터의 UPDATE시 변경 연산을 줄여 처리 속도를 향상시킬 수 있습니다.

![SQL JOINS](https://nephelai.github.io/images/posts/SQL_JOINS.png)

### INNER JOIN

JOIN의 Default로 `ON` 조건에 맞는 정보를 합쳐서 보여줍니다.

```sql
SELECT *
  FROM benn.college_football_players players
  JOIN benn.college_football_teams teams
    ON teams.school_name = players.school_name
```

쉽게 2개의 Table 간에서 교집합으로 생각할 수 있습니다.

### OUTER JOIN

OUTER JOIN은 크게 3가지로 나뉩니다.

* LEFT JOIN
* RIGHT JOIN
* FULL JOIN

각각의 다이어그램 모양의 위의 모습과 같습니다.

### SELF JOIN

같은 테이블에서 2가지 이상의 정보가 필요한 경우에 이용합니다.

```sql
SELECT *
  FROM benn.college_football_players AS players, benn.college_football_teams AS teams
  WHERE teams.school_name = players.school_name AND players.position = 'RB'
```

### NATURAL JOIN

테이블에서 같은 Column을 추출하여 이를 바탕으로 INNER JOIN이 발생합니다.

```sql
SELECT *
  FROM benn.college_football_players players
  NATURAL JOIN benn.college_football_teams teams
```

위의 예시에서 두 테이블에 Column이 일치하는 것은 school_name 과 id 입니다. 즉, school_name과 id가 일치하는 data를 붙여서 보여주게 됩니다.

## UNION

UNION 연산은 2가지의 SELECT된 결과를 합쳐주는 연산입니다. 즉, ROW 기준으로 상하로 붙여주는 역할을 합니다.

```sql
SELECT *
  FROM tutorial.crunchbase_investments_part1

 UNION ALL

 SELECT *
   FROM tutorial.crunchbase_investments_part2
```

UNION은 중복을 제거하지만 UNION ALL은 중복이 제가가 되지 않고 다 등장합니다.