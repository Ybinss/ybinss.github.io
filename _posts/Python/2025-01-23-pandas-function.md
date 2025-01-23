---
layout: post
title:  "[Python] 판다스 주요 함수" 
#subtitle: 
tags: [pandas]
categories: Python
---

이번 포스팅에서는  Pandas에서 자주 사용되는 주요 함수들을 다룰 것 이다.
# 데이터 생성 및 변환
- `pd.DataFrame()` : DataFrame 객체를 생성
- `pd.Series()` : Series 객체를 생성
- `pd.read_csv()` : CSV 파일을 읽어 DataFrame으로 변환
- `pd.read_excel()` : Excel 파일을 읽어 DataFrame으로 변환
- `pd.to_datetime()` : 문자열을 날짜 형식으로 변환
- `pd.concat()` : 여러 DataFrame을 이어붙임
- `pd.merge()` : 두 개 이상의 DataFrame을 병합

# 데이터 정보 확인
- `df.head()` : 상위 n개의 데이터를 반환
- `df.tail()` : 하위 n개의 데이터를 반환
- `df.info()` : DataFrame의 요약 정보를 출력
- `df.describe()` : 수치형 데이터의 통계 요약 정보를 제공
- `df.shape` : DataFrame의 행과 열 개수를 반환
- `df.columns` : 열 이름 확인
- `df.index` : 인덱스 확인
-  `df.dtypes` : 열의 데이터 유형 확인

# 데이터 선택 및 필터링
- `df['column']` : 특정 열을 선택
- `df[['column']]` : 하나의 열만 선택하는데 데이터프레임으로 출력
- `df[['column1', 'column2']]` : 여러 열을 선택
- `df.loc[]` : 행 이름을 기준으로 행 추출
- `df.iloc[]` : 행 번호(행 위치)를 기준으로 행 추출
- `df[df['column'] > 값]` : 조건을 만족하는 데이터를 필터링
- `df.sample()` : 임의의 샘플 데이터를 반환

# 결측값 처리
- `df.isnull()` : 결측값 여부를 Boolean 형식으로 반환
- `df.notnull()` : 결측값이 아닌 데이터 여부를 반환
- `df.dropna()` : 결측값이 포함된 행이나 열을 삭제
- `df.fillna()` : 결측값을 특정 값으로 대체

# 데이터 그룹화 및 요약
- `df.groupby('column')` : 특정 열을 기준으로 데이터를 그룹화
- `df.agg()` : 여러 통계 함수를 한 번에 적용
- `df.mean()` : 평균을 계산
- `df.sum()` : 합계를 계산
- `df.count()` : 값의 개수를 계산
- `df.median()` : 중앙값을 계산
- `df.min()` : 최솟값 계산
- `df.max()` : 최댓값 계산

# 데이터 변형
- `df.apply()` : 특정 함수를 각 행 또는 열에 적용
- `df.map()` : Series 객체에 함수를 적용하거나 매핑
- `df.applymap()` : DataFrame의 각 요소에 함수를 적용
- `df.pivot_table()` : 피벗 테이블을 생성
- `df.melt()` : 여러 열을 하나로 합쳐서 긴 형태로 변환
- `df.crosstab()` : 교차 테이블을 만듦

# 파일 입출력
- `df.to_csv()` : DataFrame을 CSV 파일로 저장
- `df.to_excel()` : DataFrame을 Excel 파일로 저장
- `df.to_json()` : DataFrame을 JSON 형식으로 저장
- `df.to_sql()` : DataFrame을 SQL 데이터베이스에 저장

# 시간 관련 데이터 처리
- `pd.to_datetime()` : 문자열을 날짜 형식으로 변환
- `df.resample()` : 시간 데이터를 재샘플링
- `df.dt.year`, `df.dt.month`, `df.dt.day` : 날짜/시간 데이터를 연도, 월, 일 단위로 분리

참고 자료 : <https://wikidocs.net/258503>

