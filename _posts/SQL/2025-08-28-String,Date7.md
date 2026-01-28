---
layout: post
title:  "[SQL] LV.3 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기(String, Date)" 
subtitle: programmers
tags: [MYSQL]
categories: SQL
---

# 문제
<https://school.programmers.co.kr/learn/courses/30/lessons/164671>

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
2. `USED_GOODS_FILE`테이블
    - `FILE_ID`: 파일 ID
    - `FILE_EXT`: 파일 확장자
    - `FILE_NAME`: 파일 이름
    - `BOARD_ID`: 게시글 ID

## 답안


```sql
SELECT CONCAT('/home/grep/src/', F.BOARD_ID, '/', F.FILE_ID, F.FILE_NAME, F.FILE_EXT) AS FILE_PATH
FROM USED_GOODS_BOARD B
JOIN USED_GOODS_FILE F ON B.BOARD_ID = F.BOARD_ID
WHERE B.VIEWS = (SELECT MAX(VIEWS) 
                 FROM USED_GOODS_BOARD)
ORDER BY F.FILE_ID DESC
```

## 쿼리 설명


```sql
WHERE VIEWS = (SELECT MAX(VIEWS)
               FROM USED_GOODS_BOARD)
```

- 서브쿼리: 전체 게시판에서 가장 큰 조회수를 한 번 계산한다.
- `WHERE VIEWS = ...`: 최대 조회수를 가진 게시글만 남긴다.


```sql
SELECT CONCAT('/home/grep/src/', F.BOARD_ID, '/', F.FILE_ID, F.FILE_NAME, F.FILE_EXT) AS FILE_PATH
```

- 문자열 결합으로 파일의 절대경로를 만든다.
