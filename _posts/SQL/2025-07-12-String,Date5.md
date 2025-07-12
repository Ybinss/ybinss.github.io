---
layout: post
title:  "[SQL] LV.2 분기별 분화된 대장균의 개체 수 구하기(String, Date)" 
subtitle: programmers
tags: [MySQL]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/299308>

## 테이블 구조

1. `ECOLI_DATA`테이블
    - `ID`: 대장균 개체의 ID
    - `PARENT_ID`: 부모 개체의 ID
    - `SIZE_OF_COLONY`: 개체의 크기
    - `DIFFERENTIATION_DATE`: 분화되어 나온 날짜
    - `GENOTYPE`: 개체의 형질

## 1. 답안(CASE WHEN 문)


```sql
SELECT
    CASE
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '01' AND '03' THEN '1Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '03' AND '06' THEN '2Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '06' AND '09' THEN '3Q'
        ELSE '4Q'
    END AS QUARTER,
    COUNT(*) AS ECOLI_COUNT
FROM ECOLI_DATA
GROUP BY QUARTER
ORDER BY QUARTER
```

## 쿼리 설명


```sql
    CASE
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '01' AND '03' THEN '1Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '04' AND '06' THEN '2Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '07' AND '09' THEN '3Q'
        ELSE '4Q'
    END AS QUARTER,
```

- `DIFFERENTIATION_DATE`의 월(month) 값에 따라 분기(Quarter)를 분류한다.

## 2. 답안(CASE WHEN 문)


```sql
SELECT
    CASE
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '01' AND '03' THEN '1Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '03' AND '06' THEN '2Q'
        WHEN DATE_FORMAT(DIFFERENTIATION_DATE, '%m') BETWEEN '06' AND '09' THEN '3Q'
        ELSE '4Q'
    END AS QUARTER,
    COUNT(*) AS ECOLI_COUNT
FROM ECOLI_DATA
GROUP BY QUARTER
ORDER BY QUARTER
```

## 3. 답안(CONCAT 함수) ✓


```sql
SELECT
    CONCAT(QUARTER(DIFFERENTIATION_DATE), 'Q') AS QUARTER,
    COUNT(*) AS ECOLI_COUNT
FROM ECOLI_DATA
GROUP BY QUARTER
ORDER BY QUARTER
```

## 쿼리 설명


```sql
CONCAT(QUARTER(DIFFERENTIATION_DATE), 'Q') AS QUARTER,
```

- `QUARTER(DIFFERENTIATION_DATE)`
    + `QUARTER()`함수는 이 날짜가 몇 분기(Quarter)에 해당하는지를 반환한다.
- `CONCAT(..., 'Q')`
    + 위의 결과에서 `'Q'`를 결합 문자열 형태 분기 라벨을 만든다.
