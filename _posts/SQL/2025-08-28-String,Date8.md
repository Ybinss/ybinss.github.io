---
layout: post
title:  "[SQL] LV.3 대여 기록이 존재하는 자동차 리스트 구하기(String, Date)" 
subtitle: programmers
tags: [MySQL]
categories: SQL
---

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/157341>

## 테이블 구조

1. `CAR_RENTAL_COMPANY_CAR`테이블
    - `CAR_ID`: 자동차 ID
    - `CAR_TYPE`: 자동차 종류
    - `DAILY_FEE`: 일일 대여 요금(원)
    - `OPTIONS`: 자동차 옵션 리스트
2. `CAR_RENTAL_COMPANY_RENTAL_HISTORY`테이블
    - `HISTORY_ID`: 자동차 대여 기록 ID
    - `CAR_ID`: 자동차 ID
    - `START_DATE`: 대여 시작일
    - `END_DATE`: 대여 종료일

## 답안


```sql
SELECT DISTINCT(C.CAR_ID)
FROM CAR_RENTAL_COMPANY_CAR C
JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY H ON C.CAR_ID = H.CAR_ID
WHERE C.CAR_TYPE = '세단' AND MONTH(H.START_DATE) = 10
ORDER BY CAR_ID DESC
```

## 쿼리 설명


```sql
SELECT DISTINCT(C.CAR_ID)
```

- 조인 결과, 같은 차량이 10월에 여러 번 대여되었다면 중복된 `CAR_ID`가 여러 행으로 나온다.
- `DISTINCT`를 사용해 중복된 `CAR_ID`를 하나로 합쳐서 고유한 차량 ID만 남긴다.
