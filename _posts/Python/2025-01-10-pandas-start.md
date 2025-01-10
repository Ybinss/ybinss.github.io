---
layout: post
title:  "[Python] 판다스 시작하기" 
subtitle: 판다스 입문
tags: [pandas]
categories: Python
---
<span style="color:gray">
**By Yong Bin Kwon, with Comments by Jung In Seo**
</span>

<details open>
    <summary> Reference </summary>
        
<img width = "30%" alt = "fig1" src = "https://github.com/user-attachments/assets/59dee25b-108e-4f51-9c93-7425b38f9351" />
</details>

# I. 판다스가 왜 필요할까?
- 판다스 : 데이터 분석용 오픈소스 파이썬 라이브러리
    - **데이터프레임(Datafram)** 과 **시리즈(Series)** 라는 두 가지 새로운 자료형을 제공하며, 스프레드시트 형태의 데이터를 불러와 빠르게 조작,정렬,병합할 수 있다.
        - **데이터프레임** : 전체 스프레드시트 또는 직사각형 형태의 데이터
        - **시리즈** : 데이터프레임의 한 열
 - 판다스 같은 도구를 사용해야 하는 이유
   1. 여러 **데이터셋**에 같은 분석 과정을 적용해야 할 때 일련의 작업을 자동화할 수 있다.
    <br>✓ **데이터셋** : 데이터 집합
   2. 데이터 작업을 수행할 때 데이터에 적용한 모든 실행 단계를 기록할 수 있다는 장점, 즉 재현성이 있다.

# II. 데이터셋 불러오기

## II-1. 데이터셋 불러오기
- 데이터셋 출처 : <https://www.gapminder.org/> , <https://github.com/jennybc/gapminder/>


```python
# 라이브러리 불러오기
import pandas

# gapminder.tsv 불러오기
df = pandas.read_csv('/Users/kwon/Desktop/gapminder.tsv', sep = '\t') # sep = '\t'는 구분 문자가 탭임을 알림

# 불러온 데이터셋 출력
print(df)

```

              country continent  year  lifeExp       pop   gdpPercap
    0     Afghanistan      Asia  1952   28.801   8425333  779.445314
    1     Afghanistan      Asia  1957   30.332   9240934  820.853030
    2     Afghanistan      Asia  1962   31.997  10267083  853.100710
    3     Afghanistan      Asia  1967   34.020  11537966  836.197138
    4     Afghanistan      Asia  1972   36.088  13079460  739.981106
    ...           ...       ...   ...      ...       ...         ...
    1699     Zimbabwe    Africa  1987   62.351   9216418  706.157306
    1700     Zimbabwe    Africa  1992   60.377  10704340  693.420786
    1701     Zimbabwe    Africa  1997   46.809  11404948  792.449960
    1702     Zimbabwe    Africa  2002   39.989  11926563  672.038623
    1703     Zimbabwe    Africa  2007   43.487  12311143  469.709298
    
    [1704 rows x 6 columns]



```python
# 판다스 별칭 부여
import pandas as pd

df = pd.read_csv('/Users/kwon/Desktop/gapminder.tsv', sep = '\t')
```

## II-2. 데이터 프레임 이해하기
- 데이터프레임은 엑셀의 시트와 같고, 시리즈는 그 시트의 1개 열이라 볼 수 있다.


```python
# type()을 사용하여 실행 결과 dfdml 자료형이 무엇인지 확인
print(type(df))
```

    <class 'pandas.core.frame.DataFrame'>



```python
# shape 속성을 사용하여 행과 열의 개수 확인
print(df.shape)

```

    (1704, 6)


> - 첫 번째 값이 행 개수 두 번째 값이 열 개수인 튜플 반환
> - `shape`는 데이터프레임 객체의 함수나 메서드가 아닌 속성, 따라서 이름뒤에 소괄호가 없다.


```python
# columns 속성을 사용하여 데이터프레임의 열 이름 확인
print(df.columns)
```

    Index(['country', 'continent', 'year', 'lifeExp', 'pop', 'gdpPercap'], dtype='object')



```python
# dtypes 속성으로 데이터셋의 각 열이 어떤 자료형인지 확인
print(df.dtypes)
```

    country       object
    continent     object
    year           int64
    lifeExp      float64
    pop            int64
    gdpPercap    float64
    dtype: object


> - object는 문자 자료형
> - int64, float64는 정수 또는 소수로, 모두 숫자 자료형


```python
# 데이터와 관련된 다양한 정보를 함께 확인
print(df.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1704 entries, 0 to 1703
    Data columns (total 6 columns):
     #   Column     Non-Null Count  Dtype  
    ---  ------     --------------  -----  
     0   country    1704 non-null   object 
     1   continent  1704 non-null   object 
     2   year       1704 non-null   int64  
     3   lifeExp    1704 non-null   float64
     4   pop        1704 non-null   int64  
     5   gdpPercap  1704 non-null   float64
    dtypes: float64(2), int64(2), object(2)
    memory usage: 80.0+ KB
    None


- [x] 판다스의 자료형은 파이썬과 다르다.

|판다스|파이썬|설명|
|:---|:---|:---|
|object|string|문자열, 가장 일반적인 자료형|
|int64|int|정수|
|float64|float|소수점이 있는 숫자|
|datetime64|datetime|표준 라이브러리 datetime에서 제공하는 자료형|

# III. 데이터 추출하기


```python
# head()는 가장 앞 5개 행을 확인
print(df.head())
```

           country continent  year  lifeExp       pop   gdpPercap
    0  Afghanistan      Asia  1952   28.801   8425333  779.445314
    1  Afghanistan      Asia  1957   30.332   9240934  820.853030
    2  Afghanistan      Asia  1962   31.997  10267083  853.100710
    3  Afghanistan      Asia  1967   34.020  11537966  836.197138
    4  Afghanistan      Asia  1972   36.088  13079460  739.981106


## III-1. 문자열로 열 데이터 추출하기


```python
# 데이터프레임 df에서 country 열 데이터를 추출하고 그 결과를 country_df 변수에 저장
country_df = df['country']
```


```python
# country_df의 첫 5개 데이터 확인
print(country_df.head())
```

    0    Afghanistan
    1    Afghanistan
    2    Afghanistan
    3    Afghanistan
    4    Afghanistan
    Name: country, dtype: object



```python
# country_df의 마지막 5개 데이터 확인
print(country_df.tail())
```

    1699    Zimbabwe
    1700    Zimbabwe
    1701    Zimbabwe
    1702    Zimbabwe
    1703    Zimbabwe
    Name: country, dtype: object


## III-2. 리스트로 열 데이터 추출하기
 - 열 이름으로 여러 열의 데이터를 추출하고 싶다면 대괄호 안에 열 이름 리스트를 전달한다. 이 때 대괄호는 [[]]와 같이 이중으로 중첩한다. 바깥쪽 대괄호는 데이터프레임에서 열을 추출한다는 뜻이며 안쪽 대괄호는 선택할 열 목록을 지정한 파이썬 리스트이다.
    <br>✓ 파이썬의 리스트 : 여러 요소의 모음으로, 각 요소에는 순서가 있다.


```python
# df에서 country, continent, year 등 3개 열 데이터를 추출하고 그 결과를 subset 변수에 저장
subset = df[['country', 'continent', 'year']]

# subset 출력
print(subset)
```

              country continent  year
    0     Afghanistan      Asia  1952
    1     Afghanistan      Asia  1957
    2     Afghanistan      Asia  1962
    3     Afghanistan      Asia  1967
    4     Afghanistan      Asia  1972
    ...           ...       ...   ...
    1699     Zimbabwe    Africa  1987
    1700     Zimbabwe    Africa  1992
    1701     Zimbabwe    Africa  1997
    1702     Zimbabwe    Africa  2002
    1703     Zimbabwe    Africa  2007
    
    [1704 rows x 3 columns]


- [x] 대괄호 표기법을 사용할 때 주의!!
    - 데이터프레임에 대괄호를 덧붙이는 대괄호 표기법에 열 이름이 아닌 열 위치를 전달하면 오류가 발생한다.

## III-3. 열 데이터를 추출하는 두 가지 방법의 차이점 이해하기


```python
# country 열 데이터 추출 후 country_df에 저장
country_df = df['country']
print(type(country_df))
```

    <class 'pandas.core.series.Series'>



```python
# country_df를 print()로 출력
print(country_df)
```

    0       Afghanistan
    1       Afghanistan
    2       Afghanistan
    3       Afghanistan
    4       Afghanistan
               ...     
    1699       Zimbabwe
    1700       Zimbabwe
    1701       Zimbabwe
    1702       Zimbabwe
    1703       Zimbabwe
    Name: country, Length: 1704, dtype: object


> - 데이터프레임을 출력했을 때와는 다르게 위가 아닌 아래에 열 이름을 출력하며 마지막 줄은 행과 열 개수가 아닌 열 이름, 길이, 자료형 정보이다.


```python
# country 열만 포함하는 요소가 하나인 리스트로 열 데이터를 추출
country_df_list = df[['country']]

# 결과를 country_df_list에 저장하고 type()함수를 사용하여 자료형 확인
print(type(country_df_list))
```

    <class 'pandas.core.frame.DataFrame'>


> - country_df_list의 자료형은 country_df과 다르게 데이터프레임 객체임을 알 수 있다. 이와 같이 대괄호에 리스트를 전달하면 항상 DataFrame 객체를 반환한다.


```python
# country_df_list의 출력 결과
print(country_df_list)
```

              country
    0     Afghanistan
    1     Afghanistan
    2     Afghanistan
    3     Afghanistan
    4     Afghanistan
    ...           ...
    1699     Zimbabwe
    1700     Zimbabwe
    1701     Zimbabwe
    1702     Zimbabwe
    1703     Zimbabwe
    
    [1704 rows x 1 columns]


 - [x] 열 데이터 추출하는 두 가지 방법


```python
# 대괄호 표기법
print(df['country'])

# 점 표기법
print(df.country)
```

    0       Afghanistan
    1       Afghanistan
    2       Afghanistan
    3       Afghanistan
    4       Afghanistan
               ...     
    1699       Zimbabwe
    1700       Zimbabwe
    1701       Zimbabwe
    1702       Zimbabwe
    1703       Zimbabwe
    Name: country, Length: 1704, dtype: object
    0       Afghanistan
    1       Afghanistan
    2       Afghanistan
    3       Afghanistan
    4       Afghanistan
               ...     
    1699       Zimbabwe
    1700       Zimbabwe
    1701       Zimbabwe
    1702       Zimbabwe
    1703       Zimbabwe
    Name: country, Length: 1704, dtype: object


## III-4. 행 데이터 추출하기

|속성|설명|
|:---|:---|
|loc|행 이름을 기준으로 행 추출|
|iloc|행 번호(행 위치)를 기준으로 행 추출|

### 행 이름으로 행 데이터 추출하기


```python
# 데이터셋 확인
print(df)
```

              country continent  year  lifeExp       pop   gdpPercap
    0     Afghanistan      Asia  1952   28.801   8425333  779.445314
    1     Afghanistan      Asia  1957   30.332   9240934  820.853030
    2     Afghanistan      Asia  1962   31.997  10267083  853.100710
    3     Afghanistan      Asia  1967   34.020  11537966  836.197138
    4     Afghanistan      Asia  1972   36.088  13079460  739.981106
    ...           ...       ...   ...      ...       ...         ...
    1699     Zimbabwe    Africa  1987   62.351   9216418  706.157306
    1700     Zimbabwe    Africa  1992   60.377  10704340  693.420786
    1701     Zimbabwe    Africa  1997   46.809  11404948  792.449960
    1702     Zimbabwe    Africa  2002   39.989  11926563  672.038623
    1703     Zimbabwe    Africa  2007   43.487  12311143  469.709298
    
    [1704 rows x 6 columns]


> - 출력 결과를 살펴보면 데이터프레임의 가장 왼쪽에 있는 행 번호를 확인할 수 있다. 이것이 판다스가 자동으로 매긴 DataFrame의 인덱스로, 칼럼 없는 행 번호이자 행 이름이다.


```python
# loc 속성의 대괄호에 0을 넣어 첫 번쨰 행 데이터 추출
print(df.loc[0])
```

    country      Afghanistan
    continent           Asia
    year                1952
    lifeExp           28.801
    pop              8425333
    gdpPercap     779.445314
    Name: 0, dtype: object


> - 파이썬의 인덱스가 0부터 시작하는 것처럼 행 번호로 설정된 판다의 행 번호도 0부터 시작한다. 즉, 첫 번째 행 데이터의 인덱스는 0이다.


```python
# loc 속성의 100번 째 행 데이터 추출
print(df.loc[99])
```

    country      Bangladesh
    continent          Asia
    year               1967
    lifeExp          43.453
    pop            62821884
    gdpPercap    721.186086
    Name: 99, dtype: object



```python
# shape 속성을 사용하여 행의 개수 구하기
number_of_rows = df.shape[0]

# 행의 개수에서 1을 뺀 값으로 마지막 행의 인덱스 구하기
last_row_index = number_of_rows - 1

# 마지막 행의 인덱스로 데이터 추출하기
print(df.loc[last_row_index])
```

    country        Zimbabwe
    continent        Africa
    year               2007
    lifeExp          43.487
    pop            12311143
    gdpPercap    469.709298
    Name: 1703, dtype: object



```python
# tail() 함수로 마지막 행의 데이터 추출하기
print(df.tail(n = 1))
```

           country continent  year  lifeExp       pop   gdpPercap
    1703  Zimbabwe    Africa  2007   43.487  12311143  469.709298


- [x] `tail()` 메서드와 loc 속성이 반환한 1행의 자료형은 어떻게 다를까?


```python
subset_loc = df.loc[0]
subset_head = df.head(n = 1) # tail()이 반환하는 결과는 head()와 같다.
```


```python
print(type(subset_loc)) # 출력 결과는 Series 객체
```

    <class 'pandas.core.series.Series'>



```python
print(type(subset_head)) # 출력 결과는 DataFrame 객체
```

    <class 'pandas.core.frame.DataFrame'>



```python
# 1번째, 100번째, 1,000번째 행의 데이터 추출
print(df.loc[[0,99,999]])
```

             country continent  year  lifeExp       pop    gdpPercap
    0    Afghanistan      Asia  1952   28.801   8425333   779.445314
    99    Bangladesh      Asia  1967   43.453  62821884   721.186086
    999     Mongolia      Asia  1967   51.253   1149500  1226.041130


## III-5. 행 번호로 행 데이터 추출하기
- iloc 속성은 행 데이터를 추출한다는 점과 대괄호를 쓴다는 점에서는 loc 속성과 같지만 행 번호(행 위치)를 사용한다는 점이 다르다.


```python
# iloc 속성을 사용하여 두 번째 행 데이터 추출
print(df.iloc[1])
```

    country      Afghanistan
    continent           Asia
    year                1957
    lifeExp           30.332
    pop              9240934
    gdpPercap      820.85303
    Name: 1, dtype: object



```python
# 100번 째 행 데이터 추출
print(df.iloc[99])
```

    country      Bangladesh
    continent          Asia
    year               1967
    lifeExp          43.453
    pop            62821884
    gdpPercap    721.186086
    Name: 99, dtype: object



```python
# 마지막 행 데이터 추출
print(df.iloc[-1])
```

    country        Zimbabwe
    continent        Africa
    year               2007
    lifeExp          43.487
    pop            12311143
    gdpPercap    469.709298
    Name: 1703, dtype: object


> - loc 속성에서는 마지막 요소를 선택할 때 -1을 사용할 수 없었지만 iloc 속성에서는 사용 할 수 있다. iloc 속성은 특정 값이 아닌 요소의 위치, 즉 행 번호로 작동하기 때문이다.


```python
# 1번째, 100번째, 1,000번째 행 데이터 추출
print(df.iloc[[0,99,999]])
```

             country continent  year  lifeExp       pop    gdpPercap
    0    Afghanistan      Asia  1952   28.801   8425333   779.445314
    99    Bangladesh      Asia  1967   43.453  62821884   721.186086
    999     Mongolia      Asia  1967   51.253   1149500  1226.041130


## III-6. loc와 iloc로 데이터 추출하기
- loc와 iloc를 사용하여 특정 행과 열을 선택할 수도 있다. `df.loc[[행], [열]]`, `df.iloc[[행], [열]]`과 같이 사용한다. 이때 loc에는 행 이름과 열 이름을 지정하고 iloc에는 행 위치와 열 위치를 정수로 지정한다.

### 슬라이싱 구문으로 데이터 추출하기
- loc와 iloc로 행이나 열 데이터를 추출하려면 슬라이싱 구문을 사용한다.
    - 슬라이싱 구문에서 콜론(:)은 해당 축의 모든 값을 선택한다는 뜻이다.
    - df.loc[:, [열]]은 특정 열의 모든 행 데이터를 뜻한다.


```python
# loc로 year와 pop열의 모든 행 데이터 추출
subset = df.loc[:, ['year', 'pop']]
print(subset)
```

          year       pop
    0     1952   8425333
    1     1957   9240934
    2     1962  10267083
    3     1967  11537966
    4     1972  13079460
    ...    ...       ...
    1699  1987   9216418
    1700  1992  10704340
    1701  1997  11404948
    1702  2002  11926563
    1703  2007  12311143
    
    [1704 rows x 2 columns]



```python
# iloc로 3,5번째와 마지막(-1) 열 데이터 추출
subset = df.iloc[:, [2, 4, -1]]
print(subset)
```

          year       pop   gdpPercap
    0     1952   8425333  779.445314
    1     1957   9240934  820.853030
    2     1962  10267083  853.100710
    3     1967  11537966  836.197138
    4     1972  13079460  739.981106
    ...    ...       ...         ...
    1699  1987   9216418  706.157306
    1700  1992  10704340  693.420786
    1701  1997  11404948  792.449960
    1702  2002  11926563  672.038623
    1703  2007  12311143  469.709298
    
    [1704 rows x 3 columns]


### range()로 데이터 추출하기
- 파이썬 내장 함수 `range()`에 시작값과 끝값을 입력하면 파이썬이 자동으로 시작값 이상, 끝값 미만의 값을 생성한다.
    <br>✓ `range()` : 리스트가 아닌 정해진 범위의 수를 반환하는 제너레이터
    <br>✓ 제너레이터 : 그때마다 값을 생성하며 한 번 사용한 값은 메모리에서 사라진다.


```python
# iloc 속성과 range()를 사용하여 열 추출
small_range = list(range(5))
print(small_range)
```

    [0, 1, 2, 3, 4]


> - `range(5)`를 호출하면 0이상 5미만의 정수를 반환하는 제너레이터가 생성된다. iloc 속성에 사용하고자 `list()` 함수로 변환하면 0부터 4까지 5개의 정수를 포함하는 리스트가 된다.


```python
# 리스트를 사용하여 데이터프레임에서 열 추출
subset = df.iloc[:, small_range]
print(subset)
```

              country continent  year  lifeExp       pop
    0     Afghanistan      Asia  1952   28.801   8425333
    1     Afghanistan      Asia  1957   30.332   9240934
    2     Afghanistan      Asia  1962   31.997  10267083
    3     Afghanistan      Asia  1967   34.020  11537966
    4     Afghanistan      Asia  1972   36.088  13079460
    ...           ...       ...   ...      ...       ...
    1699     Zimbabwe    Africa  1987   62.351   9216418
    1700     Zimbabwe    Africa  1992   60.377  10704340
    1701     Zimbabwe    Africa  1997   46.809  11404948
    1702     Zimbabwe    Africa  2002   39.989  11926563
    1703     Zimbabwe    Africa  2007   43.487  12311143
    
    [1704 rows x 5 columns]



```python
# 3 이상 6 미만의 숫자로 구성된 리스트 생성
small_range = list(range(3, 6))
print(small_range)
```

    [3, 4, 5]



```python
# 위 리스트로 데이터프레임에서 열 데이터 추출
subset = df.iloc[:, small_range]
print(subset)
```

          lifeExp       pop   gdpPercap
    0      28.801   8425333  779.445314
    1      30.332   9240934  820.853030
    2      31.997  10267083  853.100710
    3      34.020  11537966  836.197138
    4      36.088  13079460  739.981106
    ...       ...       ...         ...
    1699   62.351   9216418  706.157306
    1700   60.377  10704340  693.420786
    1701   46.809  11404948  792.449960
    1702   39.989  11926563  672.038623
    1703   43.487  12311143  469.709298
    
    [1704 rows x 3 columns]



```python
# range()함수로 0이상 6미만의 범위에서 2만큼의 간격으로 숫자 생성 후 리스트로 변환
small_range = list(range(0, 6, 2))
subset = df.iloc[:, small_range]
print(subset)
```

              country  year       pop
    0     Afghanistan  1952   8425333
    1     Afghanistan  1957   9240934
    2     Afghanistan  1962  10267083
    3     Afghanistan  1967  11537966
    4     Afghanistan  1972  13079460
    ...           ...   ...       ...
    1699     Zimbabwe  1987   9216418
    1700     Zimbabwe  1992  10704340
    1701     Zimbabwe  1997  11404948
    1702     Zimbabwe  2002  11926563
    1703     Zimbabwe  2007  12311143
    
    [1704 rows x 3 columns]


### 슬라이싱 구문과 range() 비교하기
- 파이썬의 슬라이싱 구문을 사용하면 `range()` 함수로 데이터를 추출하는 과정을 훨씬 단순하게 만들 수 있다.


```python
# 데이터셋 열 확인
print(df.columns)
```

    Index(['country', 'continent', 'year', 'lifeExp', 'pop', 'gdpPercap'], dtype='object')



```python
# range()와 :를 사용하여 열 데이터 추출
small_range = list(range(3))
subset = df.iloc[:, small_range]
print(subset)
```

              country continent  year
    0     Afghanistan      Asia  1952
    1     Afghanistan      Asia  1957
    2     Afghanistan      Asia  1962
    3     Afghanistan      Asia  1967
    4     Afghanistan      Asia  1972
    ...           ...       ...   ...
    1699     Zimbabwe    Africa  1987
    1700     Zimbabwe    Africa  1992
    1701     Zimbabwe    Africa  1997
    1702     Zimbabwe    Africa  2002
    1703     Zimbabwe    Africa  2007
    
    [1704 rows x 3 columns]



```python
# 슬라이싱 구문으로 추출
subset = df.iloc[:, :3]
print(subset)
```

              country continent  year
    0     Afghanistan      Asia  1952
    1     Afghanistan      Asia  1957
    2     Afghanistan      Asia  1962
    3     Afghanistan      Asia  1967
    4     Afghanistan      Asia  1972
    ...           ...       ...   ...
    1699     Zimbabwe    Africa  1987
    1700     Zimbabwe    Africa  1992
    1701     Zimbabwe    Africa  1997
    1702     Zimbabwe    Africa  2002
    1703     Zimbabwe    Africa  2007
    
    [1704 rows x 3 columns]


> - list(range(3))과 :3은 같은 결과를 출력한다.


```python
# 3 이상 6 미만의 범위에 해당하는 열 데이터 추출
small_range = list(range(3, 6))
subset = df.iloc[:, small_range]
print(subset)
```

          lifeExp       pop   gdpPercap
    0      28.801   8425333  779.445314
    1      30.332   9240934  820.853030
    2      31.997  10267083  853.100710
    3      34.020  11537966  836.197138
    4      36.088  13079460  739.981106
    ...       ...       ...         ...
    1699   62.351   9216418  706.157306
    1700   60.377  10704340  693.420786
    1701   46.809  11404948  792.449960
    1702   39.989  11926563  672.038623
    1703   43.487  12311143  469.709298
    
    [1704 rows x 3 columns]



```python
# 슬라이싱 구문으로 열 데이터 추출
subset = df.iloc[:, 3:6]
print(subset)
```

          lifeExp       pop   gdpPercap
    0      28.801   8425333  779.445314
    1      30.332   9240934  820.853030
    2      31.997  10267083  853.100710
    3      34.020  11537966  836.197138
    4      36.088  13079460  739.981106
    ...       ...       ...         ...
    1699   62.351   9216418  706.157306
    1700   60.377  10704340  693.420786
    1701   46.809  11404948  792.449960
    1702   39.989  11926563  672.038623
    1703   43.487  12311143  469.709298
    
    [1704 rows x 3 columns]



```python
# 0이상 6미만 범위에서 2 간격으로 숫자 생성
small_range = list(range(0, 6, 2))
subset = df.iloc[:, small_range]
print(subset)
```

              country  year       pop
    0     Afghanistan  1952   8425333
    1     Afghanistan  1957   9240934
    2     Afghanistan  1962  10267083
    3     Afghanistan  1967  11537966
    4     Afghanistan  1972  13079460
    ...           ...   ...       ...
    1699     Zimbabwe  1987   9216418
    1700     Zimbabwe  1992  10704340
    1701     Zimbabwe  1997  11404948
    1702     Zimbabwe  2002  11926563
    1703     Zimbabwe  2007  12311143
    
    [1704 rows x 3 columns]



```python
# range()에 전달한 세 개의 인수를 : 기호로 구분하여 슬라이싱 구문 적용
subset = df.iloc[:, 0:6:2]
print(subset)
```

              country  year       pop
    0     Afghanistan  1952   8425333
    1     Afghanistan  1957   9240934
    2     Afghanistan  1962  10267083
    3     Afghanistan  1967  11537966
    4     Afghanistan  1972  13079460
    ...           ...   ...       ...
    1699     Zimbabwe  1987   9216418
    1700     Zimbabwe  1992  10704340
    1701     Zimbabwe  1997  11404948
    1702     Zimbabwe  2002  11926563
    1703     Zimbabwe  2007  12311143
    
    [1704 rows x 3 columns]


## III-7. 행과 열 함께 지정하여 추출하기
- 특정 행과 열에 있는 데이터만 선택하려면 쉼표 왼쪽의 값을 변경하면 된다.


```python
# loc 속성으로 contry 열에서 행 이름이 42인 데이터 추출
print(df.loc[42, 'country'])
```

    Angola



```python
# iloc 속성으로 추출
print(df.iloc[42, 0])
```

    Angola


### 여러 행과 열 지정하여 데이터 추출하기


```python
# 1번째, 4번째, 6번째 열의 1번째, 100번째, 1,000번째 행 데이터 추출
print(df.iloc[[0, 99, 999], [0, 3, 5]])
```

             country  lifeExp    gdpPercap
    0    Afghanistan   28.801   779.445314
    99    Bangladesh   43.453   721.186086
    999     Mongolia   51.253  1226.041130



```python
# loc로 위 데이터 추출
print(df.loc[[0, 99, 999], ['country', 'lifeExp', 'gdpPercap']])
```

             country  lifeExp    gdpPercap
    0    Afghanistan   28.801   779.445314
    99    Bangladesh   43.453   721.186086
    999     Mongolia   51.253  1226.041130


- [x] loc는 이름을 기준으로 해석하고 iloc는 위치를 기준으로 해석한다.


```python
print(df.loc[10:13, :]) # 이름 기준이므로 13도 포함
```

            country continent  year  lifeExp       pop    gdpPercap
    10  Afghanistan      Asia  2002   42.129  25268405   726.734055
    11  Afghanistan      Asia  2007   43.828  31889923   974.580338
    12      Albania    Europe  1952   55.230   1282697  1601.056136
    13      Albania    Europe  1957   59.280   1476505  1942.284244



```python
print(df.iloc[10:13, :]) # 번호 기준이므로 13은 제외
```

            country continent  year  lifeExp       pop    gdpPercap
    10  Afghanistan      Asia  2002   42.129  25268405   726.734055
    11  Afghanistan      Asia  2007   43.828  31889923   974.580338
    12      Albania    Europe  1952   55.230   1282697  1601.056136

