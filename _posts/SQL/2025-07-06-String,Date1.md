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
SELECT CAR_ID , TO_CHAR(ROUND(AVG((END_DATE - START_DATE) + 1), 1), 'FM9999.0') AS AVERAGE_DURATION
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
HAVING ROUND(AVG((END_DATE - START_DATE) + 1), 1) >= 7
ORDER BY TO_NUMBER(AVERAGE_DURATION) DESC, CAR_ID DESC
```

## 쿼리 설명


```sql
(END_DATE - START_DATE) + 1)
```

- Oracle에서 `END_DATE - START_DATE`는 일(day) 단위 차이를 숫자로 반환한다.
    + 예) 9/10 - 9/01 = 9
- `+ 1`을 하는 이유는 시작일과 종료일을 포함한 "포함 일수"로 만들기 위해서다. 


```sql
TO_CHAR(ROUND(AVG((END_DATE - START_DATE) + 1), 1), 'FM9999.0')
```

- `TO_CHAR(..., 'FM9999.0')
    + `FM`(Fill Mode): 불필요한 공백 제거
        * `FM`이 없으면 자릿수 맞추려고 앞에 공백이 생길 수 있다.
        * 예) 7 -> 7.0
    + `'9999.0'`: 정수부 최대 4자리 + 소수 1자리 형태   


```sql
HAVING ROUND(AVG((END_DATE - START_DATE) + 1), 1) >= 7
```

- `WHERE`는 개별 행을 필터링
- `HAVING`은 **그룹 집계 결과**를 필터링
- 평균이 7일 이상인 차량만 필터링
