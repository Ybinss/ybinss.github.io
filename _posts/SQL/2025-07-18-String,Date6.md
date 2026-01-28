---
layout: post
title:  "[SQL] LV.1 특정 옵션이 포함된 자동차 리스트 구하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/157343>

## 테이블 구조

1. `CAR_RENTAL_COMPANY_CAR`테이블
    - `CAR_ID`: 자동차 ID
    - `CAR_TYPE`: 자동차 종류
    - `DAILY_FEE`: 일일 대여 요금(원)
    - `OPTIONS`: 자동차 옵션 리스트

## 답안


```sql
SELECT CAR_ID, CAR_TYPE, DAILY_FEE, OPTIONS
FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%네비게이션%'
ORDER BY CAR_ID DESC
```

## 쿼리 설명


```sql
WHERE OPTIONS LIKE '%네비게이션%'
```

- `OPTIONS`컬럼에 `"네비게이션"`이라는 단어가 포함된 행만 필터링한다.
    + 이 조건은 컬럼이 문자열(`VARCHAR`)이고, 여러 옵션이 쉼표(,)로 연결되어 있을 때 자주 사용
