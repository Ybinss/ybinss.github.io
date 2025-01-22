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

<details >
    <summary> Versions </summary>
 <pre>       
    Python = 3.11.11
    numpy = 1.24.0
    pandas = 2.2.3
 </pre>   
</details>

# I. 깔끔한 데이터란?
- 깔끔한 데이터는 R 커뮤니티의 해들리 위컴이 한 [논문](http://vita.had.co.nz/papers/tidy-data.pdf)에서 처음 소개한 개념으로, 데이터셋을 구조화하는 프레임워크이다. 이 프레임워크를 사용하면 데이터셋을 분석하고 시각화하기 쉽다. 즉, 데이터를 정리하는 가장 이상적인 목표라고 볼 수 있다. 이를 이해하면 데이터 분석과 시각화, 그리고 데이터 수집이 훨씬 쉬워진다. 논문을 보면 깔끔한 데이터는 다음 조건을 만족해야 한다.


    1. 행은 관측값을 나타내야 한다.
    2. 열은 변수를 나타내야 한다.
    3. 관측 단위별로 데이터 표를 구성해야 한다.   

- 해들리 위컴은 저서 \<R을 활용한 데이터 과학>에서 하나의 데이터셋, 즉 표에 초점을 두고 깔끔한 데이터를 다음과 같이 다시 정의했다.

    + 변수는 열로 나타내야 한다.
    + 관측값은 행으로 나타내야 한다.
    + 값은 셀로 나타내야 한다.

# II. 열 이름이 값일 때
- 변수가 아닌 값 자체를 열 이름으로 표현할 떄가 있다. 데이터를 수집하고 구성하기 편리한 형식이기 때문이다. 그러나 데이터를 쉽게 분석하고 시각화하려면 깔끔한 데이터로 만들어야 한다.

## II-1. 하나의 열만 남기기

### 넓은 데이터 확인하기


```python
# pew.csv 데이터셋 불러오기
import pandas as pd

pew = pd.read_csv('./data/pew.csv')
print(pew)
```

                       religion  <$10k  $10-20k  $20-30k  $30-40k  $40-50k  \
    0                  Agnostic     27       34       60       81       76   
    1                   Atheist     12       27       37       52       35   
    2                  Buddhist     27       21       30       34       33   
    3                  Catholic    418      617      732      670      638   
    4        Don’t know/refused     15       14       15       11       10   
    5          Evangelical Prot    575      869     1064      982      881   
    6                     Hindu      1        9        7        9       11   
    7   Historically Black Prot    228      244      236      238      197   
    8         Jehovah's Witness     20       27       24       24       21   
    9                    Jewish     19       19       25       25       30   
    10            Mainline Prot    289      495      619      655      651   
    11                   Mormon     29       40       48       51       56   
    12                   Muslim      6        7        9       10        9   
    13                 Orthodox     13       17       23       32       32   
    14          Other Christian      9        7       11       13       13   
    15             Other Faiths     20       33       40       46       49   
    16    Other World Religions      5        2        3        4        2   
    17             Unaffiliated    217      299      374      365      341   
    
        $50-75k  $75-100k  $100-150k  >150k  Don't know/refused  
    0       137       122        109     84                  96  
    1        70        73         59     74                  76  
    2        58        62         39     53                  54  
    3      1116       949        792    633                1489  
    4        35        21         17     18                 116  
    5      1486       949        723    414                1529  
    6        34        47         48     54                  37  
    7       223       131         81     78                 339  
    8        30        15         11      6                  37  
    9        95        69         87    151                 162  
    10     1107       939        753    634                1328  
    11      112        85         49     42                  69  
    12       23        16          8      6                  22  
    13       47        38         42     46                  73  
    14       14        18         14     12                  18  
    15       63        46         40     41                  71  
    16        7         3          4      4                   8  
    17      528       407        321    258                 597  


> 데이터셋을 살펴보면 일부 열 이름이 변수가 아닌 값을 나타낸다는 것을 알 수 있다. religion을 제외한 모든 열은  <\$10k, \$10-20k와 같이 소득 범위를 나타내고 각 소득 범위에 해당하는 사람 수를 값으로 설정했다. 즉, 소득이라는 변수 하나를 여러 범위로 나뉘어 여러 열로 분산했다.


```python
print(pew.iloc[:, 0:5])
```

                       religion  <$10k  $10-20k  $20-30k  $30-40k
    0                  Agnostic     27       34       60       81
    1                   Atheist     12       27       37       52
    2                  Buddhist     27       21       30       34
    3                  Catholic    418      617      732      670
    4        Don’t know/refused     15       14       15       11
    5          Evangelical Prot    575      869     1064      982
    6                     Hindu      1        9        7        9
    7   Historically Black Prot    228      244      236      238
    8         Jehovah's Witness     20       27       24       24
    9                    Jewish     19       19       25       25
    10            Mainline Prot    289      495      619      655
    11                   Mormon     29       40       48       51
    12                   Muslim      6        7        9       10
    13                 Orthodox     13       17       23       32
    14          Other Christian      9        7       11       13
    15             Other Faiths     20       33       40       46
    16    Other World Religions      5        2        3        4
    17             Unaffiliated    217      299      374      365


- 판다스의 데이터프레임에는 깔끔한 데이터 형식으로 변환하는 `melt()`메서드가 있다. 이 메서드에는 다음과 같은 매개변수를 지정한다.

|매개변수|설명|
|:---|:---|
|id_vars|그대로 유지할 변수를 나타내는 컨테이너(리스트, 튜플, ndarray)입니다.
|value_vars|피벗 되돌리기 할 열을 나타냅니다. 기본적으로 매개변수 id_vars로 지정하지 않은 모든 열이 피벗 되돌리기 대상으로 설정됩니다.|
|var_name|value_vars의 열을 피벗 되돌리기 하여 구성할 새 열의 이름으로 설정할 문자열입니다. 피벗 되돌리기 한 열의 이름을 값으로 하는 새 열의 이름을 나타내며 기본값은 'variable' 입니다.|
|value_name|var_name 열의 값을 나타내는 새로운 열 이름 문자열입니다. 기본값은 'value' 입니다.|

### 긴 데이터로 만들기


```python
# religion 열을 제외한 나머지 모든 열을 피벗 되돌리기
pew_long = pew.melt(id_vars = 'religion')
print(pew_long)
```

                      religion            variable  value
    0                 Agnostic               <$10k     27
    1                  Atheist               <$10k     12
    2                 Buddhist               <$10k     27
    3                 Catholic               <$10k    418
    4       Don’t know/refused               <$10k     15
    ..                     ...                 ...    ...
    175               Orthodox  Don't know/refused     73
    176        Other Christian  Don't know/refused     18
    177           Other Faiths  Don't know/refused     71
    178  Other World Religions  Don't know/refused      8
    179           Unaffiliated  Don't know/refused    597
    
    [180 rows x 3 columns]


- [x] 데이터프레임과 판다스의 `melt()`메서드 이해하기
    - 데이터프레임의 `melt()`메서드와 마찬가지로 판다스의 `pd.melt()`함수를 사용할 수도 있다. 


```python
# melt() 메서드
pew_long = pew.melt(id_vars = 'religion')

# pd.melt() 함수
pew_long = pd.melt(pew, id_vars = 'religion')
```


```python
# var_name으로 "income"을, value_name으로 "count" 설정
pew_long = pew.melt(
    id_vars = "religion", var_name = "income", value_name = "count"
)
print(pew_long)
```

                      religion              income  count
    0                 Agnostic               <$10k     27
    1                  Atheist               <$10k     12
    2                 Buddhist               <$10k     27
    3                 Catholic               <$10k    418
    4       Don’t know/refused               <$10k     15
    ..                     ...                 ...    ...
    175               Orthodox  Don't know/refused     73
    176        Other Christian  Don't know/refused     18
    177           Other Faiths  Don't know/refused     71
    178  Other World Religions  Don't know/refused      8
    179           Unaffiliated  Don't know/refused    597
    
    [180 rows x 3 columns]


## II-2. 여러개의 열 남기기
- religion이라는 하나의 열만 유지했던 이전 실습과 다르게 2개 이상의 열을 유지해야 한다면 어떻게 해야 할까?

### 여러 개 열 유지하기


```python
# billobard.csv 데이터셋을 불러와 행과 열 확인
billboard = pd.read_csv('./data/billboard.csv')
print(billboard.iloc[0:5, 0:16])
```

       year        artist                    track  time date.entered  wk1   wk2  \
    0  2000         2 Pac  Baby Don't Cry (Keep...  4:22   2000-02-26   87  82.0   
    1  2000       2Ge+her  The Hardest Part Of ...  3:15   2000-09-02   91  87.0   
    2  2000  3 Doors Down               Kryptonite  3:53   2000-04-08   81  70.0   
    3  2000  3 Doors Down                    Loser  4:24   2000-10-21   76  76.0   
    4  2000      504 Boyz            Wobble Wobble  3:35   2000-04-15   57  34.0   
    
        wk3   wk4   wk5   wk6   wk7   wk8   wk9  wk10  wk11  
    0  72.0  77.0  87.0  94.0  99.0   NaN   NaN   NaN   NaN  
    1  92.0   NaN   NaN   NaN   NaN   NaN   NaN   NaN   NaN  
    2  68.0  67.0  66.0  57.0  54.0  53.0  51.0  51.0  51.0  
    3  72.0  69.0  67.0  65.0  55.0  59.0  62.0  61.0  61.0  
    4  25.0  17.0  17.0  31.0  36.0  49.0  53.0  57.0  64.0  


> - wk는 한 주를 뜻한다. 즉, 각 주를 열로 표현했다. 이렇게 데이터를 관리하면 새로운 주의 데이터를 입력하기 쉽고 직관적으로 이해할 수 있다는 장점이 있다. 그러나 피벗 되돌리기 해야 하는 때도 있다. 예를 들어 주간 평가 결과를 패싯 그래프로 나타내야 한다면 패싯 변수를 데이터프레임의 열로 표현해야 한다.


```python
# 빌보드 데이터셋을 피벗 되돌리기
billboard_long = billboard.melt(
    id_vars = ["year", "artist", "track", "time", "date.entered"], # 유지할 열을 리스트로 지정
    var_name = "week",
    value_name = "rating",
)
print(billboard_long)
```

           year            artist                    track  time date.entered  \
    0      2000             2 Pac  Baby Don't Cry (Keep...  4:22   2000-02-26   
    1      2000           2Ge+her  The Hardest Part Of ...  3:15   2000-09-02   
    2      2000      3 Doors Down               Kryptonite  3:53   2000-04-08   
    3      2000      3 Doors Down                    Loser  4:24   2000-10-21   
    4      2000          504 Boyz            Wobble Wobble  3:35   2000-04-15   
    ...     ...               ...                      ...   ...          ...   
    24087  2000       Yankee Grey     Another Nine Minutes  3:10   2000-04-29   
    24088  2000  Yearwood, Trisha          Real Live Woman  3:55   2000-04-01   
    24089  2000   Ying Yang Twins  Whistle While You Tw...  4:19   2000-03-18   
    24090  2000     Zombie Nation            Kernkraft 400  3:30   2000-09-02   
    24091  2000   matchbox twenty                     Bent  4:12   2000-04-29   
    
           week  rating  
    0       wk1    87.0  
    1       wk1    91.0  
    2       wk1    81.0  
    3       wk1    76.0  
    4       wk1    57.0  
    ...     ...     ...  
    24087  wk76     NaN  
    24088  wk76     NaN  
    24089  wk76     NaN  
    24090  wk76     NaN  
    24091  wk76     NaN  
    
    [24092 rows x 7 columns]


# III. 열 이름에 변수가 여러 개 일 때

## III-1. 열 이름이 여러 가지 뜻일 때
- 열에 여러개의 변수를 포함할 때도 있다. 예를 들어 건강 데이터를 다룰 때 이러한 형식을 자주 사용한다.

### 깔끔한 데이터 만들기①


```python
# country_timeseries.csv 데이터셋 열 확인
ebola = pd.read_csv('./data/country_timeseries.csv')
print(ebola.columns)
```

    Index(['Date', 'Day', 'Cases_Guinea', 'Cases_Liberia', 'Cases_SierraLeone',
           'Cases_Nigeria', 'Cases_Senegal', 'Cases_UnitedStates', 'Cases_Spain',
           'Cases_Mali', 'Deaths_Guinea', 'Deaths_Liberia', 'Deaths_SierraLeone',
           'Deaths_Nigeria', 'Deaths_Senegal', 'Deaths_UnitedStates',
           'Deaths_Spain', 'Deaths_Mali'],
          dtype='object')



```python
#행과 열 확인
print(ebola.iloc[:5, [0, 1, 2, 10]])
```

             Date  Day  Cases_Guinea  Deaths_Guinea
    0    1/5/2015  289        2776.0         1786.0
    1    1/4/2015  288        2775.0         1781.0
    2    1/3/2015  287        2769.0         1767.0
    3    1/2/2015  286           NaN            NaN
    4  12/31/2014  284        2730.0         1739.0


> - 열 이름 Cases_Guinea와 Deaths_Guinea는 사실 변수 2개를 포함한다. Guinea는 국가 이름을 나타내고 Case는 확진 사례를, Deaths는 사망자 수를 나타낸다. 또한 데이터가 넓은 데이터 형식이므로 `melt()` 메서드로 재구성해야 한다.


```python
# melt()사용하여 Date와 Day 열을 제외한 열을 정리(피벗 되돌리기)
ebola_long = ebola.melt(id_vars = ['Date', 'Day'])
print(ebola_long)
```

                Date  Day      variable   value
    0       1/5/2015  289  Cases_Guinea  2776.0
    1       1/4/2015  288  Cases_Guinea  2775.0
    2       1/3/2015  287  Cases_Guinea  2769.0
    3       1/2/2015  286  Cases_Guinea     NaN
    4     12/31/2014  284  Cases_Guinea  2730.0
    ...          ...  ...           ...     ...
    1947   3/27/2014    5   Deaths_Mali     NaN
    1948   3/26/2014    4   Deaths_Mali     NaN
    1949   3/25/2014    3   Deaths_Mali     NaN
    1950   3/24/2014    2   Deaths_Mali     NaN
    1951   3/22/2014    0   Deaths_Mali     NaN
    
    [1952 rows x 4 columns]


> - Cases_Guinea와 같은 variable값은 사망자 또는 확진 사례를 나타내는 질병 현황과 국가 정보를 모두 포함한다. 따라서 밑줄(_)을 기준으로 분할하는 것이 좋아 보인다.

## III-2. 열 이름 분할하고 새로운 열로 할당하기
- str 접근자를 사용하여 `split()` 메서드를 호출해보자.

### 깔끔한 데이터 만들기②


```python
# ebloa_long 데이터프레임의 variable 열에 접근해서 split()로 구분 문자_을 기준으로 열 이름 분할
variable_split = ebola_long.variable.str.split('_') # str.split()로 문자열 분할
print(variable_split[:5])
```

    0    [Cases, Guinea]
    1    [Cases, Guinea]
    2    [Cases, Guinea]
    3    [Cases, Guinea]
    4    [Cases, Guinea]
    Name: variable, dtype: object


> - `split()` 메서드는 분할한 문자열을 리스트로 반환한다.
> - 분할 결과의 자료형이 리스트인지 확인하는 방법
>   + 기본 파이썬 문자열 객체의 `split()` 메서드 공식 [문서](https://docs.python.org/3/library/stdtypes.html#str.split) 참조
>   + variable 열에 `split()` 메서드를 적용한 출력 결과의 대괄호([])
>   + 시리즈의 한 요소에 대해 `type()` 확인


```python
# split()을 적용한 열의 값 유형 확인
print(type(variable_split))
```

    <class 'pandas.core.series.Series'>



```python
# type() 확인
print(type(variable_split[0]))
```

    <class 'list'>


- 값을 분할했다면 다음 단계는 분할한 값을 새로운 열로 할당하는 것이다. 분할한 문자열 리스트의 각 값에 접근하려면 `get()` 메서드를 사용해야 한다.


```python
# str 속성과 get()를 사용하여 사례(status_values)와 국가(country_values)로 저장
status_values = variable_split.str.get(0)
country_values = variable_split.str.get(1)

# status_values 확인
print(status_values)
```

    0        Cases
    1        Cases
    2        Cases
    3        Cases
    4        Cases
             ...  
    1947    Deaths
    1948    Deaths
    1949    Deaths
    1950    Deaths
    1951    Deaths
    Name: variable, Length: 1952, dtype: object



```python
# 사례는 status 열로, 국가는 country 열로 데이터프레임에 추가
ebola_long['status'] = status_values
ebola_long['country'] = country_values
print(ebola_long)
```

                Date  Day      variable   value  status country
    0       1/5/2015  289  Cases_Guinea  2776.0   Cases  Guinea
    1       1/4/2015  288  Cases_Guinea  2775.0   Cases  Guinea
    2       1/3/2015  287  Cases_Guinea  2769.0   Cases  Guinea
    3       1/2/2015  286  Cases_Guinea     NaN   Cases  Guinea
    4     12/31/2014  284  Cases_Guinea  2730.0   Cases  Guinea
    ...          ...  ...           ...     ...     ...     ...
    1947   3/27/2014    5   Deaths_Mali     NaN  Deaths    Mali
    1948   3/26/2014    4   Deaths_Mali     NaN  Deaths    Mali
    1949   3/25/2014    3   Deaths_Mali     NaN  Deaths    Mali
    1950   3/24/2014    2   Deaths_Mali     NaN  Deaths    Mali
    1951   3/22/2014    0   Deaths_Mali     NaN  Deaths    Mali
    
    [1952 rows x 6 columns]


## III-3. 한 번에 분할하고 합치기
- 여러 단계에 걸쳐 수행한 작업을 한 번에 수행하는 방법이 있다. `str.split()` 메서드의 공식 [문서](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.split.html#pandas.Series.str.split)를 보면 expand라는 매개변수가 있다. 이 매개변수의 기본값은 False인데 이 값을 True로 설정하면 리스트 시리즈 대신 분할 결과를 새로운 열로 만든 데이터프레임을 반환한다.

### 깔끔한 데이터 한 번에 만들기


```python
# melt()를 사용하여 피벗 되돌리기 상태로 초기화
ebola_long = ebola.melt(id_vars = ['Date', 'Day'])

# expand에 인수 True 전달
variable_split = ebola_long.variable.str.split('_', expand = True)
print(variable_split)
```

               0       1
    0      Cases  Guinea
    1      Cases  Guinea
    2      Cases  Guinea
    3      Cases  Guinea
    4      Cases  Guinea
    ...      ...     ...
    1947  Deaths    Mali
    1948  Deaths    Mali
    1949  Deaths    Mali
    1950  Deaths    Mali
    1951  Deaths    Mali
    
    [1952 rows x 2 columns]



```python
# 새로운 두 열로 구성된 variable_split 데이터프레임을 ebola_long 데이터프레임에 새로운 두 열로 할당
ebola_long[['status', 'country']] = variable_split
print(ebola_long)
```

                Date  Day      variable   value  status country
    0       1/5/2015  289  Cases_Guinea  2776.0   Cases  Guinea
    1       1/4/2015  288  Cases_Guinea  2775.0   Cases  Guinea
    2       1/3/2015  287  Cases_Guinea  2769.0   Cases  Guinea
    3       1/2/2015  286  Cases_Guinea     NaN   Cases  Guinea
    4     12/31/2014  284  Cases_Guinea  2730.0   Cases  Guinea
    ...          ...  ...           ...     ...     ...     ...
    1947   3/27/2014    5   Deaths_Mali     NaN  Deaths    Mali
    1948   3/26/2014    4   Deaths_Mali     NaN  Deaths    Mali
    1949   3/25/2014    3   Deaths_Mali     NaN  Deaths    Mali
    1950   3/24/2014    2   Deaths_Mali     NaN  Deaths    Mali
    1951   3/22/2014    0   Deaths_Mali     NaN  Deaths    Mali
    
    [1952 rows x 6 columns]


> - 파이썬과 판다스의 다중 할당 기능을 사용하여 새로 분할한 열을 원본 데이터프레임에 바로 할당할 수도 있다.

# IV. 변수가 행과 열 모두에 있을 때
- 변수가 행과 열 모두에 있는 데이터도 있다. 즉, 앞서 살펴본 유형이 모두 나타난 형태이다. 이 절에서는 날씨 데이터셋을 사용하여 열이 하나가 아닌 두 개의 변수를 나타낼 때는 어떻게 처리하는지 살펴본다. 이때는 변수를 별도의 열로 피벗해야 한다. 즉, 긴 데이터에서 넓은 데이터로 변환해야 한다.

### 행과 열 모두에 있는 변수 정리하기


```python
# weather.csv 데이터셋 불러오기
weather = pd.read_csv('./data/weather.csv')
print(weather.iloc[:5, :11])
```

            id  year  month element  d1    d2    d3  d4    d5  d6  d7
    0  MX17004  2010      1    tmax NaN   NaN   NaN NaN   NaN NaN NaN
    1  MX17004  2010      1    tmin NaN   NaN   NaN NaN   NaN NaN NaN
    2  MX17004  2010      2    tmax NaN  27.3  24.1 NaN   NaN NaN NaN
    3  MX17004  2010      2    tmin NaN  14.4  14.4 NaN   NaN NaN NaN
    4  MX17004  2010      3    tmax NaN   NaN   NaN NaN  32.1 NaN NaN


> - 날씨 데이터셋은 월(month)별 날짜(d1, d2, ..., d3)의 최저 기온(tmin)과 최고 기온(tmax)을 포함한다. element 열은 값이 2가지이므로 넓게 피벗을 적용할 수 있으며 날짜 변수는 값으로 피벗 되돌리기 해야 한다.   
    넓은 데이터 자체는 아무런 문제가 없다. 직관적으로 읽기에는 정말 좋은 구성이지만 데이터를 분석하기 쉬운 형태가 아니므로 변환이 필요하다.


```python
# 날짜 정리
weather_melt = weather.melt(
    id_vars = ["id", "year", "month", "element"], # 날짜 열을 제외한 열을 리스트에 담아 id_vars에 전달
    var_name = "day", # 일자 열을 행의 값으로 갖는 새로운 열의 이름은 "day"
    value_name = "temp", # 피벗 되돌리기 한 열의 값은 "temp"
)
print(weather_melt)
```

              id  year  month element  day  temp
    0    MX17004  2010      1    tmax   d1   NaN
    1    MX17004  2010      1    tmin   d1   NaN
    2    MX17004  2010      2    tmax   d1   NaN
    3    MX17004  2010      2    tmin   d1   NaN
    4    MX17004  2010      3    tmax   d1   NaN
    ..       ...   ...    ...     ...  ...   ...
    677  MX17004  2010     10    tmin  d31   NaN
    678  MX17004  2010     11    tmax  d31   NaN
    679  MX17004  2010     11    tmin  d31   NaN
    680  MX17004  2010     12    tmax  d31   NaN
    681  MX17004  2010     12    tmin  d31   NaN
    
    [682 rows x 6 columns]



```python
# pivot_table() 사용하여 element 열에 저장된 변수 피벗
weather_tidy = weather_melt.pivot_table(
    index = ['id', 'year', 'month', 'day'], # 그대로 유지할 나머지 열
    columns = 'element', # 넓게 피벗할 열
    values = 'temp' # 새로운 열
)
print(weather_tidy)
```

    element                 tmax  tmin
    id      year month day            
    MX17004 2010 1     d30  27.8  14.5
                 2     d11  29.7  13.4
                       d2   27.3  14.4
                       d23  29.9  10.7
                       d3   24.1  14.4
                 3     d10  34.5  16.8
                       d16  31.1  17.6
                       d5   32.1  14.2
                 4     d27  36.3  16.7
                 5     d27  33.2  18.2
                 6     d17  28.0  17.5
                       d29  30.1  18.0
                 7     d3   28.6  17.5
                       d14  29.9  16.5
                 8     d23  26.4  15.0
                       d5   29.6  15.8
                       d29  28.0  15.3
                       d13  29.8  16.5
                       d25  29.7  15.6
                       d31  25.4  15.4
                       d8   29.0  17.3
                 10    d5   27.0  14.0
                       d14  29.5  13.0
                       d15  28.7  10.5
                       d28  31.2  15.0
                       d7   28.1  12.9
                 11    d2   31.3  16.3
                       d5   26.3   7.9
                       d27  27.7  14.2
                       d26  28.1  12.1
                       d4   27.2  12.0
                 12    d1   29.9  13.8
                       d6   27.8  10.5


> - 피벗 표를 보면 element 열의 각 값이 새로운 열 이름으로 설정된 것을 확인할 수 있다. 하지만 열의 이름을 보면 두 줄로 계층이 생긴 것을 확인할 수 있다.


```python
# reset_index()를 사용하여 계층 열을 평면화
weather_tidy_flat = weather_tidy.reset_index()
print(weather_tidy_flat)
```

    element       id  year  month  day  tmax  tmin
    0        MX17004  2010      1  d30  27.8  14.5
    1        MX17004  2010      2  d11  29.7  13.4
    2        MX17004  2010      2   d2  27.3  14.4
    3        MX17004  2010      2  d23  29.9  10.7
    4        MX17004  2010      2   d3  24.1  14.4
    5        MX17004  2010      3  d10  34.5  16.8
    6        MX17004  2010      3  d16  31.1  17.6
    7        MX17004  2010      3   d5  32.1  14.2
    8        MX17004  2010      4  d27  36.3  16.7
    9        MX17004  2010      5  d27  33.2  18.2
    10       MX17004  2010      6  d17  28.0  17.5
    11       MX17004  2010      6  d29  30.1  18.0
    12       MX17004  2010      7   d3  28.6  17.5
    13       MX17004  2010      7  d14  29.9  16.5
    14       MX17004  2010      8  d23  26.4  15.0
    15       MX17004  2010      8   d5  29.6  15.8
    16       MX17004  2010      8  d29  28.0  15.3
    17       MX17004  2010      8  d13  29.8  16.5
    18       MX17004  2010      8  d25  29.7  15.6
    19       MX17004  2010      8  d31  25.4  15.4
    20       MX17004  2010      8   d8  29.0  17.3
    21       MX17004  2010     10   d5  27.0  14.0
    22       MX17004  2010     10  d14  29.5  13.0
    23       MX17004  2010     10  d15  28.7  10.5
    24       MX17004  2010     10  d28  31.2  15.0
    25       MX17004  2010     10   d7  28.1  12.9
    26       MX17004  2010     11   d2  31.3  16.3
    27       MX17004  2010     11   d5  26.3   7.9
    28       MX17004  2010     11  d27  27.7  14.2
    29       MX17004  2010     11  d26  28.1  12.1
    30       MX17004  2010     11   d4  27.2  12.0
    31       MX17004  2010     12   d1  29.9  13.8
    32       MX17004  2010     12   d6  27.8  10.5



```python
# 메서드 체인을 이용하여 element 피벗 단계를 한 번에 수행
weather_tidy = (
    weather_melt
    .pivot_table(
        index = ['id', 'year', 'month', 'day'],
        columns = 'element',
        values = 'temp')
    .reset_index() # 메서드 체인으로 피벗한 테이블을 바로 평면화
)
print(weather_tidy)
```

    element       id  year  month  day  tmax  tmin
    0        MX17004  2010      1  d30  27.8  14.5
    1        MX17004  2010      2  d11  29.7  13.4
    2        MX17004  2010      2   d2  27.3  14.4
    3        MX17004  2010      2  d23  29.9  10.7
    4        MX17004  2010      2   d3  24.1  14.4
    5        MX17004  2010      3  d10  34.5  16.8
    6        MX17004  2010      3  d16  31.1  17.6
    7        MX17004  2010      3   d5  32.1  14.2
    8        MX17004  2010      4  d27  36.3  16.7
    9        MX17004  2010      5  d27  33.2  18.2
    10       MX17004  2010      6  d17  28.0  17.5
    11       MX17004  2010      6  d29  30.1  18.0
    12       MX17004  2010      7   d3  28.6  17.5
    13       MX17004  2010      7  d14  29.9  16.5
    14       MX17004  2010      8  d23  26.4  15.0
    15       MX17004  2010      8   d5  29.6  15.8
    16       MX17004  2010      8  d29  28.0  15.3
    17       MX17004  2010      8  d13  29.8  16.5
    18       MX17004  2010      8  d25  29.7  15.6
    19       MX17004  2010      8  d31  25.4  15.4
    20       MX17004  2010      8   d8  29.0  17.3
    21       MX17004  2010     10   d5  27.0  14.0
    22       MX17004  2010     10  d14  29.5  13.0
    23       MX17004  2010     10  d15  28.7  10.5
    24       MX17004  2010     10  d28  31.2  15.0
    25       MX17004  2010     10   d7  28.1  12.9
    26       MX17004  2010     11   d2  31.3  16.3
    27       MX17004  2010     11   d5  26.3   7.9
    28       MX17004  2010     11  d27  27.7  14.2
    29       MX17004  2010     11  d26  28.1  12.1
    30       MX17004  2010     11   d4  27.2  12.0
    31       MX17004  2010     12   d1  29.9  13.8
    32       MX17004  2010     12   d6  27.8  10.5



```python

```
