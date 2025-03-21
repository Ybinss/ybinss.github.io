---
layout: post
title:  "[SQL] DATE_FORMAT 함수" 
# subtitle: programmers
tags: [MySQL]
categories: SQL
---

# `DATE_FORMAT`함수 정리
`DATE_FORMAT`함수는 날짜(DateTime) 데이터를 원하는 형식으로 변환할 때 사용한다.

## I. 기본 문법


```sql
DATE_FORMAT(날짜_컬럼, '형식')
```

- `날짜_컬럼`: 변환할 날짜 데이터
- `'형식'`: 출력할 날짜 및 시간 형식

## II. 날짜 포맷 코드

|형식 코드|설명|예제(2024-03-15 14:30:45)|
|:---|:---|:---|
|`%Y`|연도(4자리)|2024|
|`%y`|연도(2자리)|24|
|`%m`|월(2자리)|03|
|`%c`|월(1~12)|3|
|`%d`|일(2자리)|15|
|`%e`|일(1~31)|15|
|`%W`|요일(전체)|Friday|
|`%a`|요일(약어)|Fri|
|`%H`|시간(24시간제, 2자리)|14|
|`%h`|시간(12시간제, 2자리)|02|
|`%i`|분(2자리)|30|
|`%s`|초(2자리)|45|
|`%p`|AM/PM|PM|

## III. 사용 예제

(1) 날짜를 `YYYY-MM-DD`형식으로 변환


```sql
SELECT DATE_FORMAT('2024-03-15 14:30:45', '%Y-%m-%d') AS formatted_date;
-- 결과: 2024-03-15
```

(2) 날짜를 `YYYY년 MM월 DD일`형식으로 변환


```sql
SELECT DATE_FORMAT('2024-03-15', '%Y년 %m월 %d일') AS formatted_date;
-- 결과: 2024년 03월 15일
```

(3) 날짜와 시간을 `YYYY/MM/DD HH:MM:SS`형식으로 변환


```sql
SELECT DATE_FORMAT('2024-03-15 14:30:45', '%Y/%m/%d %H:%i:%s') AS formatted_datetime;
-- 결과: 2024/03/15 14:30:45
```

(4) 요일을 포함한 날짜 출력


```sql
SELECT DATE_FORMAT('2024-03-15', '%Y-%m-%d (%W)') AS formatted_date;
-- 결과: 2024-03-15 (Friday)
```

(5) 12시간제(AM/PM)시간 표시


```sql
SELECT DATE_FORMAT('2024-03-15 14:30:45', '%Y-%m-%d %h:%i:%s %p') AS formatted_time;
-- 결과: 2024-03-15 02:30:45 PM
```
