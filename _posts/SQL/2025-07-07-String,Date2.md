---
layout: post
title:  "[SQL] LV.1 자동차 대여 기록에서 장기/단기 대여 구분하기(String, Date)" 
subtitle: programmers
tags: [Oracle]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />


# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/151138>

## 테이블 구조

1. `CAR_RENTAL_COMPANY_RENTAL_HISTORY`테이블
    - `HISTORY_ID`: 자동차 대여 기록 ID
    - `CAR_ID`: 자동차 ID
    - `START_DATAE`: 대여 시작일
    - `END_DATE`: 대여 종료일

## 답안


```sql
SELECT 
    HISTORY_ID, 
    CAR_ID,
    DATE_FORMAT(START_DATE, '%Y-%m-%d') AS START_DATE,
    DATE_FORMAT(END_DATE, '%Y-%m-%d') AS END_DATE,
    CASE
        WHEN DATEDIFF(END_DATE,START_DATE) >= 29 THEN '장기 대여'
        ELSE '단기 대여'
    END AS RENT_TYPE
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE START_DATE LIKE '2022-09%'
ORDER BY HISTORY_ID DESC
```

## 쿼리 설명


```sql
    CASE
        WHEN DATEDIFF(END_DATE,START_DATE) >= 29 THEN '장기 대여'
        ELSE '단기 대여'
    END AS RENT_TYPE
```

- `DATEDIFF(END_DATE, START_DATE)`
    + 대여일수 차이를 계산(END_DATE - START_DATE)


```sql
WHERE START_DATE LIKE '2022-09%'
```

- `START_DATE`가 2022년 9월에 해당하는 데이터만 가져옴
