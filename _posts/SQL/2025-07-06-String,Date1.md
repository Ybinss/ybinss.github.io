---
layout: post
title:  "[SQL] LV.2 자동차 평균 대여 기간 구하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/157342>

## 테이블 구조

1. `CAR_RENTAL_COMPANY_RENTAL_HISTORY`테이블
    - `HISTORY_ID`: 자동차 대여 기록 ID
    - `CAR_ID`: 자동차 ID
    - `START_DATAE`: 대여 시작일
    - `END_DATE`: 대여 종료일

## 답안


```sql
SELECT CAR_ID, ROUND(AVG(DATEDIFF(END_DATE, START_DATE) + 1), 1) AS AVERAGE_DURATION
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID 
HAVING AVG(DATEDIFF(END_DATE, START_DATE) + 1) >= 7
ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC
```

## 쿼리 설명


```sql
(END_DATE - START_DATE) + 1)
```

- `DATEDIFF(end, start)`는 두 날짜의 차이 일수를 반환한다.
    + 같은 날이면 0
    + 하루 차이면 1
- `+ 1`을 하는 이유는 시작일과 종료일을 포함한 "포함일수"로 계산하려는 것이다.
