Link

- [W3School](https://www.w3schools.com/mysql/)
- [HackerRank](https://www.hackerrank.com/domains/sql)
- [KhanAcademy](https://www.khanacademy.org/)
- [TutorialsPoint](https://www.tutorialspoint.com/sql/)

1. Hr Database

2. North wind Database

3. Sakila database

---

# MYSQL

- 특정 하드웨어에 종속되지 않음
- 크로스 플랫폼
- ANSI SQL 표준을 준수
- RDBMS
  - 관계형 데이터베이스
  - SQL 쿼리를 사용해 DB의 데이터 엑세스
- record(행) / column(열)
- 키워드 대소문자 구분 ❌

# 중요 키워드

    - SELECT: 데이터베이스에서 데이터를 추출합니다.
    - UPDATE: 데이터베이스의 데이터를 업데이트합니다.
    - DELETE: 데이터베이스에서 데이터를 삭제합니다.
    - INSERT INTO: 데이터베이스에 새로운 데이터를 삽입합니다.
    - CREATE DATABASE: 새로운 데이터베이스를 생성합니다
    - ALTER DATABASE: 데이터베이스를 수정합니다.
    - CREATE TABLE: 새 테이블을 생성합니다.
    - ALTER TABLE: 테이블을 수정합니다
    - DROP TABLE: 테이블을 삭제합니다
    - CREATE INDEX: 인덱스(검색 키)를 생성합니다.
    - DROP INDEX: 인덱스를 삭제합니다

---

# SELECT / DISTINCT

- 데이터 선택하는데 사용
- 반환된 데이터는 result-set이라는 result table에 저장됨

```sql
SELECT ////
FROM table_name;
```

- SELECT DISTINCT는 고유한 값만 반환
- 대부분의 DB SYSTEM에서는 "", '' 둘다 허용

```sql
SELECT DISTINCT ////
FROM table_name;
```

```sql
-- 다음 SQL은 Country의 고유값만
SELECT DISTINCT Country FROM Customers;

-- 다음 SQL은 Country의 고유값의 수를 반환
SELECT COUNT(DISTINCT Country) FROM Customers;
```

---

# WHERE

- 레코드를 필터링하는데 사용

```sql
SELECT ///
FROM table_name
WHERE condition;
```

> SELECT 뿐만 아니라 UPDATE, DELETE에서도 사용

| 연산자       | 설명                            |
| ------------ | ------------------------------- |
| =            | Equal                           |
| <, >, <=, >= | 크기 비교                       |
| <>, !=       | Not Equal                       |
| BETWEEN      | 범위                            |
| LIKE         | 패턴 찾기                       |
| IN           | 열에 가능한 여러 값을 지정할 때 |

---

# ORDER BY

- result-set을 ASC, DESC 로 정렬
- default는 오름차순

```sql
SELECT ///
FROM table_name
ORDER BY c1, c2 , ... ASC | DESC;
```

```sql
-- 이런 것도 됨
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

---

# INSERT INTO

- 새 레코드를 삽입하는데 사용
- 두가지 방법으로 작성

```sql
-- column을 모두 지정
INSERT INTO table_name (c1, c2 ...)
VALUES (v1, v2, ....);

-- 모든 열에 데이터를 추가할 경우 이름을 지정할 필요없음
INSERT INTO table_name
values (v1, v2 ...);
```

---

# NULL

- 비교 연산자를 사용하여 NULL 테스트를 할 수 없음
- IS NULL, IS NOT NULL을 사용해야함

> WHERE 없으면 전부 업데이트되버림

```sql
SELECT c1
FROM table_name
WHERE c1, IS NULL;
WHERE c1, IS NOT NULL;
```

---

# UPDATE

- 기존 레코드를 수정

```sql
UPDATE table_name
SET c1 = v1, c2 = v2, ...
WHERE condition;
```

```sql
-- Country = 'Mexico'인 PostalCode를 00000 으로
UPDATE Customers
SET PostalCode = 00000
WHERE Country = 'Mexico';
```

---

# DELETE

- 기존 레코드 삭제

```sql
DELETE FROM table_name WHERE condition;
```

- 테이블을 삭제하지 않고도 모든 행을 삭제할 수 있음
  - 이는 테이블의 구조, 속성 및 인덱스가 그대로 유지됨

```sql
DELETE FROM table_name;
```

---

# FUNCTION

## LIMIT

- 반환할 레코드 수를 지정하는 데 사용

```sql

SELECT ///
FROM table_name
WHERE condition
-- LIMIT num : 처음부터 num 만큼
-- LIMIT num1, num2 : num1에서 num2만큼
-- LIMIT num2 OFFSET num1 : num1에서 num2만큼
```

## MIN() && MAX()

- MIN() : 선택한 column의 가장 작은 값
- MAX() : 선택한 column의 가장 큰 값

```sql
SELECT
-- MAX(column_name)
MIN(column_name)
FROM table_name
WHERE condition;
```

## COUNT(), AVG(), SUM()

- COUNT() : 지정된 기준과 일치하는 row의 수를 반환
- AVG() : column의 평균값을 반환
- SUM() : column의 총합을 반환

---

# LIKE (검색)

- column에서 지정된 패턴을 검색하기 위해 WHERE 절에서 사용
- 백분율 기호(%)는 0, 하나 또는 여러 문자를 나타냄
- 밑줄 기호(\_)는 하나의 단일 문자를 나타낸다

```sql
SELECT c1, c2, ...
FROM table_name
WHERE columnN LIKE pattern
```

> regexp 도 찾아볼 것

---

# Wildcards

- 문자열에서 하나 이상의 문자를 대체하는데 사용
- 와일드 카드는 LIKE 와 함께 사용
- LIKE 연산자는 열에서 지정된 패턴을 검색하기 위해 WHERE 절에서 사용

- % : 0자 이상의 문자
- \_ : 단일 문자

---

# IN

- IN에 여러 값을 사용하면 WHERE 절에 여러 값을 지정할 수 있음
- 여러 OR 조건의 약어

```sql
SELECT ///
FROM ///
WHERE c1 IN (v1,v2,v3)

-- 둘이 같은 query

SELECT ///
FROM ///
WHERE c1 IN (SELECT STATEMENT)
```

# BETWEEN

- 주저인 범위 내의 값을 선택 // 값은 숫자, 텍스트 또는 날짜
- 시작과 끝값을 포함

```sql
SELECT //
FROM ///
WHERE c1 BETWEEN v1 AND v2;
```

---

# AS (alias)

- table이나 column의 임시 이름일 지정하는데 사용
- 가독성을 위해 자주 사용
- alias는 해당 쿼리 기간 동안에만 존재ㅏ

```sql
SELECT column_name AS alias_name
FROM table_name;

SELECT column_name(s)
FROM table_name AS alias_name;
```

---

# 조인 유형

1. INNER JOIN : 두 테이블 모두에서 일치하는 값이 있는 record를 반환
2. LEFT JOIN : 왼쪽 테이블의 모든 레코드를 오른쪽 테이블의 일치하는 레코드를 반환
3. RIGHT JOIN : 오른쪽 테이블의 모든 레코드와 왼쪽 테이블의 일치하는 레코드를 반환
4. CROSS JOIN : 두 테이블의 모든 레코드를 반환
   > JOIN ~ ON

# JOIN (INNER JOIN)

- 2개 이상의 테이블 사이의 관련 column을 기반으로 두 개 이상의 테이블에서 행을 결합할 때 사용

## INNER JOIN

- 두 테이블 모두에서 일치하는 값이 있는 record를 선택
- 일치하는 항목이 없는 record는 표시되지 않음

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2 -- JOIN과 같음
ON table1.column_name = table2.column_name; -- 조건절
```

## LEFT JOIN

- 왼쪽 테이블의 모든 record를 반환하고 오른쪽 테이블에서 일치하는 record(있는 경우) 반환

## RIGHT JOIN

- 오른쪽 테이블의 모든 레코드와 왼쪽 테이블의 일치하는 레코드(있는 경우) 반환

## CROSS JOIN

- 두 테이블의 모든 레코드를 반환

```sql
SELECT column_name(s)
FROM table1
CROSS JOIN table2;  -- ON 조건절이 없음
-- WHERE은 가능
```

> 잠재적으로 매우 큰 결과 집합을 반환할 수 있다.
> 일치 여부 관계없이 모두에서 일치하는 레코드를 반환한다

## SELF JOIN

- 일반 조인이지만 테이블이 자체적으로 조인됨

```sql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

---

# UNION

- 두 개 이상의 statement의 reset-set을 결합하는데 사용한다
  - UNION 내의 모든 select statement에는 동일한 수의 column이 있어야 함
  - column의 데이터 유형도 유사해야함
  - 모든 select 문의 column 순서도 동일해야함
  - 기본적으로 고유한 값만 선택
    - DISTINCT()가 내부적으로 일어남

# UNION ALL

- 중복 값을 허용하려할 떄
- result-set의 column 이름은 일반적으로 첫번째 select 문의 열 이름과 동일하다

```sql
SELECT column_name(s) FROM table1
UNION
-- UNION ALL
SELECT column_name(s) FROM table2;
```

```sql
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION ALL
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
```

> WHERE 절은 각 SELECT 문에 적용되고
>
> UNION 된 테이블에 직접적으로 WHERE 적용이 불가능해 서브쿼리나 중첩쿼리로 후처리 해야함

---

# GROUP BY

- 동일한 값을 가진 ROW를 summary row으로 그룹화
- 집계 함수(`COUNT(), MAX(), SUM(), AVG()`)와 사용되어 result-set을 하나 이상의 열로 그룹화하는 경우가 많음
- DISTINCT()가 내부적으로 일어남

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

# HAVING

- GROUP BY의 조건
  - 집계 함수와 WHERE를 같이 사용할 수 없어 추가됨

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

# EXISTS

- 하위 쿼리에 레코드가 있는지 테스트
- 하나 이상 record 반환하면 TRUE

---

> 비교 연산자 써야함

# ANY

- return bool
- 하나라도 만족할 시

# ALL

- return bool
- 모두 만족할 시

[여기](https://www.w3schools.com/mysql/mysql_insert_into_select.asp)

---

# INSERT INTO SELECT

- 한 테이블의 데이터를 복사하여 다른 테이블에 삽입
- 소스 테이블과 대상 테이블의 데이터 유형이 일치해야함

```sql
-- 한 테이블의 모든 열을 다른 테이블로 복사
INSERT INTO table2
SELECT * FROM table1
WHERE condition;

한 테이블의 일부 열만 다른 테이블로 복사
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;
```

---

# CASE

- 조건을 거쳐 첫 번째 조건이 충족될 때 값을 반환(IF-THEN-ELSE와 유사)
- TRUE면 읽기를 중지하고 결과를 반환
- 조건이 TRUE가 아니면 ELSE를 반환
- ELSE 부분이 없고 조건이 모두 참이면 NULL을 반환

```sql
CASE
  WHEN c1 THEN r1
  WHEN c2 THEN r2
  WHEN c3 THEN r3
  ELSE result
END;


-- 정렬에 CASE문 사용
SELECT CustomerName, City, Country
FROM Customers
ORDER BY
(CASE
    WHEN City IS NULL THEN Country
    ELSE City
END);
```

--

# NULL FUNCTION ( IFNULL() / COALESCE() )

- 값이 NULL일 경우 값을 대체
- IFNULL()은 하나의 인자만 받으며 COALESCE()는 여러 인자를 받고 첫 번째로 NULL이 아닌 인자를 받으면 그 값을 반환한다

```sql
SELECT IFNULL(NULL, 'Not Null'); -- 결과: 'Not Null'
SELECT IFNULL('Hello', 'Not Null'); -- 결과: 'Hello'

SELECT COALESCE(NULL, 'Not Null', 'Value'); -- 결과: 'Not Null'
SELECT COALESCE(NULL, NULL, NULL, 'Value'); -- 결과: 'Value'
SELECT COALESCE(NULL, NULL, NULL); -- 결과: NULL
```
