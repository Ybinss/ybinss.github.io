---
layout: post
title:  "[SQL] LV.2 카테고리 별 상품 개수 구하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---
<img width="100%" alt="Image" src="https://github.com/user-attachments/assets/576a3402-5d36-4141-8911-03a2e9c1a015" />

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/131529>

## 테이블 구조

1. `PRODUCT`테이블
    - `PRODUCT_ID`: 상품 ID
    - `PRODUCT_CODE`: 상품코드
    - `PRICE`: 판매가

## 답안


```sql
SELECT SUBSTRING(PRODUCT_CODE, 1, 2) AS CATEGORY, COUNT(*) AS PRODUCTS 
FROM PRODUCT
GROUP BY CATEGORY
ORDER BY CATEGORY
```

## 쿼리 설명


```sql
SUBSTRING(PRODUCT_CODE, 1, 2) AS CATEGORY
```

- `SUBSTRING(PRODUCT_CODE, 1, 2)`
    + SUBSTRING(문자열, 시작 위치, 추출할 문자 개수)
    + 예)
        * `'A1000045'` → `'A1'`
        * `'C3000002'` → `'C1'` 
