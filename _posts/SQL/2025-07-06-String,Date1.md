---
layout: post
title:  "[SQL] LV.2 자동차 평균 대여 기간 구하기(String, Date)" 
subtitle: programmers
tags: [Oracle]
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
SELECT CAR_ID , ROUND(AVG(DATEDIFF(END_DATE, START_DATE) + 1), 1) AS AVERAGE_DURATION
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
HAVING AVERAGE_DURATION >= 7
ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC
```

## 쿼리 설명


```sql
ROUND(AVG(DATEDIFF(END_DATE, START_DATE) + 1), 1)
```

- `DATEDIFF(END_DATE, START_DATE)`
    + `END_DATE`와 `START_DATE`의 일수 차이를 계산
- `+ 1`
    + 대여일을 포함하기 위해 1을 더함
    + 예) 2022-09-01 ~ 2022-09-01 → 1일 대여
- `ROUND(..., 1)`
    + 평균값을 소수점 첫째 자리까지 반올림


```sql
HAVING AVERAGE_DURATION >= 7
```

- 평균 대여 기간이 7일 이상인 자동차만 결과에 포함
- `HAVING`은 그룹화된 집계 결과에 조건을 걸 때 사용
