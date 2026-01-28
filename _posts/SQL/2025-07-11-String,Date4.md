---
layout: post
title:  "[SQL] LV.2 연도 별 평균 미세먼지 농도 조회하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/284530>

## 테이블 구조

1. `AIR_POLLUTION`테이블
    - `LOCATION1`: 지역구분1
    - `LOCATION2`: 지역구분2
    - `YM`: 측정일
    - `PM_VAL1`: 미세먼지 오염도
    - `PM_VAL2`: 초미세먼지 오염도

## 1. 답안(HAVING 절)


```sql
SELECT YEAR(YM) AS YEAR, ROUND(AVG(PM_VAL1), 2) AS PM10, ROUND(AVG(PM_VAL2), 2) AS 'PM2.5'
FROM AIR_POLLUTION
GROUP BY YEAR(YM), LOCATION1, LOCATION2
HAVING LOCATION2 = '수원'
ORDER BY YEAR
```

## 쿼리 설명


```sql
ROUND(AVG(PM_VAL2), 2) AS 'PM2.5'
```

- `'PM2.5'`에 따옴표 쓰는 이유
    + `.`(점)이 포함되어 있음 → SQL에서 테이블.컬럼처럼 쓰는 문법 요소


```sql
GROUP BY YEAR(YM), LOCATION1, LOCATION2
```

- 연도, 지역1, 지역2 단위로 그룹을 묶는다.


```sql
HAVING LOCATION2 = '수원'
```

- `GROUP BY`이후의 결과에 대해 필터링
- `WHERE`절이 아닌 `HAVING`절을 쓴 이유는 `LOCATION2`가 `GROUP BY`의 집계 기준이기 때문

## 2. 답안(WHERE 절)


```sql
SELECT YEAR(YM) AS YEAR, ROUND(AVG(PM_VAL1), 2) AS PM10, ROUND(AVG(PM_VAL2), 2) AS 'PM2.5'
FROM AIR_POLLUTION
WHERE LOCATION2 = '수원'
GROUP BY YEAR
ORDER BY YEAR
```
