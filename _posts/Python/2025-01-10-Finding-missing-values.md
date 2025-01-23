---
layout: post
title:  "[Python] 그룹으로 묶어 연산하기" 
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

<details >
    <summary> Versions </summary>
 <pre>       
    Python = 3.11.11
    numpy = 1.24.0
    pandas = 2.2.3
 </pre>   
</details>

# I. 결측값이란?
- 판다스의 NaN은 넘파이의 결측값에서 유래했으며 결측값은 NaN, NAN 또는 nan과 같이 표시할 수 있다.


```python
import numpy as np
print(np.__version__)
```

    1.24.0



```python
from numpy import NaN, NAN, nan
```

- 결측값은 말 그대로 데이터 자체가 없다는 것을 의미한다.


```python
print(NaN == True)
print(NaN == 0)
print(NaN == '')
print(NaN == NaN)
print(NaN == NAN)
print(NaN == nan)
print(nan == NAN)
```

    False
    False
    False
    False
    False
    False
    False



```python
# 결측값 확인
import pandas as pd

print(pd.isnull(NaN))
print(pd.isnull(nan))
print(pd.isnull(NAN))
```

    True
    True
    True



```python
# 결측값이 아닌지 검사
print(pd.notnull(NaN))
print(pd.notnull(42))
print(pd.notnull('missing'))
```

    False
    True
    True


# II. 결측값은 왜 생길까?

## II-1. 데이터를 불러올 때 생기는 결측값
- 데이터를 불러올 때 판다스는 자동으로 빠진 데이터를 NaN으로 대체한 데이터프레임을 반환한다. `read_csv()` 함수에는 결측값을 읽어 오는 방법을 결정하는 세 가지 매개변수 na_values, keep_default_na, na_filter가 있다.


```python
# survey_visited.csv 데이터 불러오기
visited_file = './data/survey_visited.csv'
print(pd.read_csv(visited_file))
```

       ident   site       dated
    0    619   DR-1  1927-02-08
    1    622   DR-1  1927-02-10
    2    734   DR-3  1939-01-07
    3    735   DR-3  1930-01-12
    4    751   DR-3  1930-02-26
    5    752   DR-3         NaN
    6    837  MSK-4  1932-01-14
    7    844   DR-1  1932-03-22


> - 데이터셋에 NaN값이 포함되어 있다.


```python
# keep_default_na를 False로 설정하여 결측값으로 보는 값을 처리하지 않도록 설정
print(pd.read_csv(visited_file, keep_default_na = False))
```

       ident   site       dated
    0    619   DR-1  1927-02-08
    1    622   DR-1  1927-02-10
    2    734   DR-3  1939-01-07
    3    735   DR-3  1930-01-12
    4    751   DR-3  1930-02-26
    5    752   DR-3            
    6    837  MSK-4  1932-01-14
    7    844   DR-1  1932-03-22



```python
# na_values로 결측값으로 처리할 값으로 빈 문자열 지정
print(
    pd.read_csv(visited_file, na_values = [""], keep_default_na = False)
)
```

       ident   site       dated
    0    619   DR-1  1927-02-08
    1    622   DR-1  1927-02-10
    2    734   DR-3  1939-01-07
    3    735   DR-3  1930-01-12
    4    751   DR-3  1930-02-26
    5    752   DR-3         NaN
    6    837  MSK-4  1932-01-14
    7    844   DR-1  1932-03-22


## II-2. 데이터를 연결할 때 생기는 결측값


```python
visited = pd.read_csv('./data/survey_visited.csv')
survey = pd.read_csv('./data/survey_survey.csv')
print(visited)
```

       ident   site       dated
    0    619   DR-1  1927-02-08
    1    622   DR-1  1927-02-10
    2    734   DR-3  1939-01-07
    3    735   DR-3  1930-01-12
    4    751   DR-3  1930-02-26
    5    752   DR-3         NaN
    6    837  MSK-4  1932-01-14
    7    844   DR-1  1932-03-22



```python
print(survey)
```

        taken person quant  reading
    0     619   dyer   rad     9.82
    1     619   dyer   sal     0.13
    2     622   dyer   rad     7.80
    3     622   dyer   sal     0.09
    4     734     pb   rad     8.41
    5     734   lake   sal     0.05
    6     734     pb  temp   -21.50
    7     735     pb   rad     7.22
    8     735    NaN   sal     0.06
    9     735    NaN  temp   -26.00
    10    751     pb   rad     4.35
    11    751     pb  temp   -18.50
    12    751   lake   sal     0.10
    13    752   lake   rad     2.19
    14    752   lake   sal     0.09
    15    752   lake  temp   -16.00
    16    752    roe   sal    41.60
    17    837   lake   rad     1.46
    18    837   lake   sal     0.21
    19    837    roe   sal    22.50
    20    844    roe   rad    11.25



```python
# visited 데이터프레임의 ident 열과 survey 데이터프레임의 taken 열을 기준으로 병함
vs = visited.merge(survey, left_on = 'ident', right_on = 'taken')
print(vs)
```

        ident   site       dated  taken person quant  reading
    0     619   DR-1  1927-02-08    619   dyer   rad     9.82
    1     619   DR-1  1927-02-08    619   dyer   sal     0.13
    2     622   DR-1  1927-02-10    622   dyer   rad     7.80
    3     622   DR-1  1927-02-10    622   dyer   sal     0.09
    4     734   DR-3  1939-01-07    734     pb   rad     8.41
    5     734   DR-3  1939-01-07    734   lake   sal     0.05
    6     734   DR-3  1939-01-07    734     pb  temp   -21.50
    7     735   DR-3  1930-01-12    735     pb   rad     7.22
    8     735   DR-3  1930-01-12    735    NaN   sal     0.06
    9     735   DR-3  1930-01-12    735    NaN  temp   -26.00
    10    751   DR-3  1930-02-26    751     pb   rad     4.35
    11    751   DR-3  1930-02-26    751     pb  temp   -18.50
    12    751   DR-3  1930-02-26    751   lake   sal     0.10
    13    752   DR-3         NaN    752   lake   rad     2.19
    14    752   DR-3         NaN    752   lake   sal     0.09
    15    752   DR-3         NaN    752   lake  temp   -16.00
    16    752   DR-3         NaN    752    roe   sal    41.60
    17    837  MSK-4  1932-01-14    837   lake   rad     1.46
    18    837  MSK-4  1932-01-14    837   lake   sal     0.21
    19    837  MSK-4  1932-01-14    837    roe   sal    22.50
    20    844   DR-1  1932-03-22    844    roe   rad    11.25


> - 여러 개의 결측값 NaN이 생겼다.

## II-3. 직접 입력한 결측값


```python
num_legs = pd.Series({'goat': 4, 'amoeba': nan})
print(num_legs)
```

    goat      4.0
    amoeba    NaN
    dtype: float64



```python
scientists = pd.DataFrame(
    {
        "Name": ["Rosaline Franklin", "William Gosset"],
        "Occupation": ["Chemist", "Statistician"],
        "Born": ["1920-07-25", "1876-06-13"],
        "Died": ["1958-04-16", "1937-10-16"],
        "missing": [NaN, nan],
    }
)

print(scientists)
```

                    Name    Occupation        Born        Died  missing
    0  Rosaline Franklin       Chemist  1920-07-25  1958-04-16      NaN
    1     William Gosset  Statistician  1876-06-13  1937-10-16      NaN



```python
print(scientists.dtypes)
```

    Name           object
    Occupation     object
    Born           object
    Died           object
    missing       float64
    dtype: object


> - scientists 데이터프레임에 결측값을 입력한 missing 열의 데이터형을 보면 float64이다. 이것은 넘파이의 NaN 결측값이 부동 소수점이기 때문이다.


```python
# 모든 값이 NaN인 열 할당
scientists = pd.DataFrame(
    {
        "Name": ["Rosaline Franklin", "William Gosset"],
        "Occupation": ["Chemist", "Statistician"],
        "Born": ["1920-07-25", "1876-06-13"],
        "Died": ["1958-04-16", "1937-10-16"],
        "missing": [NaN, nan],
    }
)

scientists["missing"] = nan # missing 열을 결측값으로 채우기
print(scientists)
```

                    Name    Occupation        Born        Died  missing
    0  Rosaline Franklin       Chemist  1920-07-25  1958-04-16      NaN
    1     William Gosset  Statistician  1876-06-13  1937-10-16      NaN


## II-4. 인덱스를 다시 설정할 때 생기는 결측값


```python
# 갭마인더 데이터셋 불러와서 연도별 평균 기대 수명 확인
gapminder = pd.read_csv('./data/gapminder.tsv', sep = '\t')

life_exp = gapminder.groupby(['year'])['lifeExp'].mean()
print(life_exp)
```

    year
    1952    49.057620
    1957    51.507401
    1962    53.609249
    1967    55.678290
    1972    57.647386
    1977    59.570157
    1982    61.533197
    1987    63.212613
    1992    64.160338
    1997    65.014676
    2002    65.694923
    2007    67.007423
    Name: lifeExp, dtype: float64



```python
# 2000년 이후의 데이터만 추출
y2000 = life_exp[life_exp.index > 2000]
print(y2000)
```

    year
    2002    65.694923
    2007    67.007423
    Name: lifeExp, dtype: float64



```python
# reindex()를 사용하여 2000부터 2010까지의 범위로 연도를 다시 인덱싱
print(y2000.reindex(range(2000, 2010)))
```

    year
    2000          NaN
    2001          NaN
    2002    65.694923
    2003          NaN
    2004          NaN
    2005          NaN
    2006          NaN
    2007    67.007423
    2008          NaN
    2009          NaN
    Name: lifeExp, dtype: float64


