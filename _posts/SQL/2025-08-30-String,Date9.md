---
layout: post
title:  "[SQL] LV.3 조건에 맞는 사용자 정보 조회하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/164670>

## 테이블 구조

1. `USED_GOODS_BOARD`테이블
    - `BOARD_ID`: 게시글 ID
    - `WRITER_ID`: 작성자 ID
    - `TITLE`: 게시글 제목
    - `CONTENTS`: 게시글 내욜
    - `PRICE`: 가격
    - `CREATED_DATE`: 작성일
    - `STATUS`: 거래상태
    - `VIEWS`: 조회수
2. `USED_GOODS_USER`테이블
    - `USER_ID`: 회원 ID
    - `NICKNAME`: 닉네임
    - `CITY`: 시
    - `STREET_ADDRESS1`: 도로명 주소
    - `STREET_ADDRESS2`: 상세 주소
    - `TLNO`: 전화번호

## 답안


```sql
SELECT 
    U.USER_ID, 
    U.NICKNAME,
    CONCAT_WS(' ', U.CITY, U.STREET_ADDRESS1, U.STREET_ADDRESS2) AS `전체주소`,
    CONCAT(SUBSTR(U.TLNO, 1, 3), '-', SUBSTR(U.TLNO, 4, 4), '-', SUBSTR(U.TLNO, 8, 4))  AS '전화번호'
FROM USED_GOODS_BOARD B
JOIN USED_GOODS_USER U ON B.WRITER_ID = U.USER_ID
GROUP BY U.USER_ID
HAVING COUNT(*) >= 3
ORDER BY USER_ID DESC
```

## 쿼리 설명


```sql
CONCAT_WS(' ', U.CITY, U.STREET_ADDRESS1, U.STREET_ADDRESS2) AS `전체주소`,
```

- `CONCAT_WS(' ', ...)`를 쓰면 NULL을 자동 무시하고 공백도 중복 없이 넣어준다.


```sql
CONCAT(SUBSTR(U.TLNO, 1, 3), '-', SUBSTR(U.TLNO, 4, 4), '-', SUBSTR(U.TLNO, 8, 4))  AS '전화번호'
```

- 예: 01012345678 -> 010-1234-5678
