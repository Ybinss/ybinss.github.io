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

# I. 데이터 집계하기
- 수집한 데이터를 바탕으로 평균이나 합계 등을 구하여 의미 있는 값을 도출해 내는 것을 '집계'라고 한다. 데이터를 집계하면 전체 데이터를 요약, 정리하여 볼 수 있으므로 데이터 분석이 훨씬 편해진다.

### groupby() 메서드로 데이터 집계하기


```python
# 갭마인더 데이터셋 불러오기
import pandas as pd
df = pd.read_csv('./data/gapminder.tsv', sep='\t')
```


```python
# year 열을 기준으로 데이터를 그룹화하고 lifeExp 열의 평균 구하기
avg_life_exp_by_year = df.groupby('year')["lifeExp"].mean()
print(avg_life_exp_by_year)
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
# unique()로 확인한 year 열의 고윳값
years = df.year.unique()
print(years)
```

    [1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 2002 2007]



```python
# 1952년 데이터 확인
y1952 = df.loc[df.year == 1952, :]
print(y1952)
```

                     country continent  year  lifeExp       pop    gdpPercap
    0            Afghanistan      Asia  1952   28.801   8425333   779.445314
    12               Albania    Europe  1952   55.230   1282697  1601.056136
    24               Algeria    Africa  1952   43.077   9279525  2449.008185
    36                Angola    Africa  1952   30.015   4232095  3520.610273
    48             Argentina  Americas  1952   62.485  17876956  5911.315053
    ...                  ...       ...   ...      ...       ...          ...
    1644             Vietnam      Asia  1952   40.412  26246839   605.066492
    1656  West Bank and Gaza      Asia  1952   43.160   1030585  1515.592329
    1668         Yemen, Rep.      Asia  1952   32.548   4963829   781.717576
    1680              Zambia    Africa  1952   42.038   2672000  1147.388831
    1692            Zimbabwe    Africa  1952   48.451   3080907   406.884115
    
    [142 rows x 6 columns]



```python
# lifeExp 열의 평균
y1952_mean = y1952["lifeExp"].mean()
print(y1952_mean)
```

    49.057619718309866


- groupby() 메서드는 year 열의 모든 고윳값에 대해 이 작업을 반복한다. 고윳값을 기준으로 하위 집합을 구하고(데이터 분할), 평균값을 계산(함수 연산 결과 적용)한 결과를 하나의 데이터프레임에 담아(데이터 결합) 반환한다.

- 다음은 판다스에 내장된 집계 메서드를 정리한 표

|판다스 메서드|넘파이/사이파이 함수|설명
|:---|:---|:---|
|count()|np.count_nonzero()|NaN값을 제외한 데이터 개수|
|size()| |NaN값을 포함한 데이터 개수|
|mean()|np.mean()|평균|
|std()|np.std()|표준편차|
|min()|np.min()|최솟값|
|quantile(q=0.25)|np.percentile(q=0.25)|25%|
|quantile(q=0.50)|np.percentile(q=0.50)|50%|
|quantile(q=0.75)|np.percentile(q=0.75)|75%|
|max()|np.max()|최댓값|
|sum()|np.sum()|합계|
|var()|np.var()|비편향 분산|
|sem()|scipy.stats.sem()|평균의 비편향 표준편차|
|describe()|scipy.stats.describe()|개수, 평균, 표준편차, 최솟값, 25%, 50%, 75%, 최댓값|
|first()| |첫 번째 행 반환|
|last()| |마지막 행 반환|
|nth()| |n번째 행 반환|

## I-1. groupby() 메서드와 함께 사용하는 집계 메서드


```python
# describe()로 대륙별 요약 통계 확인
continent_describe = df.groupby('continent')["lifeExp"].describe()
print(continent_describe)
```

               count       mean        std     min       25%      50%       75%  \
    continent                                                                     
    Africa     624.0  48.865330   9.150210  23.599  42.37250  47.7920  54.41150   
    Americas   300.0  64.658737   9.345088  37.579  58.41000  67.0480  71.69950   
    Asia       396.0  60.064903  11.864532  28.801  51.42625  61.7915  69.50525   
    Europe     360.0  71.903686   5.433178  43.585  69.57000  72.2410  75.45050   
    Oceania     24.0  74.326208   3.795611  69.120  71.20500  73.6650  77.55250   
    
                  max  
    continent          
    Africa     76.442  
    Americas   80.653  
    Asia       82.603  
    Europe     81.757  
    Oceania    81.235  


## I-2. agg() 메서드와 groupby() 메서드 조합하기
- 판다스에 내장된 메서드가 아닌 다른 라이브러리의 집계 함수를 사용할 수도 있다. `agg()` 또는 `aggregate()` 메서드에 원하는 함수를 전달하면 된다.

  ✓ `agg()` 메서드는 `aggregate()`를 줄인 것이다. 판다스 공식 문서에서는 `agg()`를 사용하라고 권장한다.

### 다른 라이브러리의 집계 함수 사용하기


```python
# np.mean()으로 continet별 평균 수명 구하기
import numpy as np

cont_le_agg = df.groupby('continent')["lifeExp"].agg("mean")
print(cont_le_agg)
```

    continent
    Africa      48.865330
    Americas    64.658737
    Asia        60.064903
    Europe      71.903686
    Oceania     74.326208
    Name: lifeExp, dtype: float64


### 사용자 집계 함수 사용하기
- 판다스나 다른 라이브러리에서 제공하는 집계 메서드로는 원하는 값을 계산할 수 없다면 직접 함수를 만들어서 사용한다. 즉, 넘파이 예제처럼 사용자 정의 함수를 `agg()`에 전달한다.

- 평균을 구하는 함수를 직접 만들어 보자. 숫자 개수를 n, 각 숫자를 $x_1, x_2,\dots, x_n$이라고 할 때 평균을 구하는 식은 다음과 같다.

$$평균 = \bar{x} = \frac{1}{n}\sum_{i=1}^{10} t_i$$


```python
# 평균을 구하는 함수
def my_mean(values):
    n = len(values) # 숫자 개수를 구하기
    sum = 0 # 합계를 0으로 초기화
    for value in values:
        sum += value # 각 값을 더하기
    return sum / n # 합계를 숫자 개수로 나눈 값을 반환
```

> - 직접 구현한 평균 함수는 매개변수를 하나만 받는데, 매개변수 values는 평균을 구하고자 하는 값을 모두 포함한 시리즈를 인수로 받는다. 따라서 값의 전체 합을 구하려먼 for문으로 각 값을 순회하여 더한다.


```python
# year 열의 lifeExp 평균 구하기
agg_my_mean = df.groupby('year')["lifeExp"].agg(my_mean) # 사용자 함수로 평균 집계
print(agg_my_mean)
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


- 다음은 연도나 대륙과 상관없이 전체 기대 수명의 평균인 diff_value와 각 그룹 평균 수명의 차이를 계산하는 `my_mean_diff()` 함수이다.


```python
def my_mean_diff(values, diff_value): # 첫 번째 매개변수는 시리즈여야 한다.
    n = len(values)
    sum = 0
    for value in values:
        sum += value
    mean = sum / n
    return(mean - diff_value)
```


```python
# 모든 기대 수명의 평균 구하기
global_mean = df["lifeExp"].mean()
print(global_mean)
```

    59.474439366197174



```python
# my_mean_diff() 함수를 agg()에 전달
agg_mean_diff = (
    df
    .groupby("year")
    ["lifeExp"]
    .agg(my_mean_diff, diff_value = global_mean) # 첫 번째 매개변수는 함수 이름, 두 번째부터는 함수의 매개변수 이름과 함께 전달
)

print(agg_mean_diff)
```

    year
    1952   -10.416820
    1957    -7.967038
    1962    -5.865190
    1967    -3.796150
    1972    -1.827053
    1977     0.095718
    1982     2.058758
    1987     3.738173
    1992     4.685899
    1997     5.540237
    2002     6.220483
    2007     7.532983
    Name: lifeExp, dtype: float64


## I-3. 여러 개의 집계 함수 한 번에 사용하기
- 여러 개의 집계 함수를 한 번에 사용하고 싶다면 어떻게 해야 할까? 이럴 때는 집계 함수를 리스트 형식으로 `agg()` 또는 `aggregate()` 메서드에 전달하면 된다.


```python
# 넘파이 함수 몇 가지를 리스트로 묶어 agg() 메서드로 넘기기
gdf = (
    df
    .groupby("year")
    ["lifeExp"]
    .agg([np.count_nonzero, "mean", "std"]) # 함수 이름을 리스트로 만들어 전달
)

print(gdf)
```

          count_nonzero       mean        std
    year                                     
    1952            142  49.057620  12.225956
    1957            142  51.507401  12.231286
    1962            142  53.609249  12.097245
    1967            142  55.678290  11.718858
    1972            142  57.647386  11.381953
    1977            142  59.570157  11.227229
    1982            142  61.533197  10.770618
    1987            142  63.212613  10.556285
    1992            142  64.160338  11.227380
    1997            142  65.014676  11.559439
    2002            142  65.694923  12.279823
    2007            142  67.007423  12.073021


### agg()나 aggregate() 메서드에 딕셔너리 사용하기
- 딕셔너리에 함수 정보를 담아 `agg()`나 `aggregate()` 메서드에 전달하는 방법도 있다. 단, 데이터프레임에 바로 적용하는지 시리즈에 적용하는지에 따라 결과가 다르다.

1. 데이터프레임에 사용하기
    - 그룹화된 데이터프레임에 딕셔너리를 활용할 때는 데이터프레임의 열을 키로, 집계 함수를 값으로 설정한다. 이 방법을 사용하면 하나 이상의 변수를 그룹화하고 열별로 서로 다른 집계 함수를 적용할 수 있다.


```python
# year 열을 기준으로 그룹화하여 lifeExp의 평균, pop의 중앙값, gdpPercap의 중앙값 구하기
gdf_dict = df.groupby("year").agg(
    {
        "lifeExp": "mean",
        "pop": "median",
        "gdpPercap": "median"
    } # 데이터프레임이라면 {"열 이름": "함수"} 형식의 딕셔너리를 전달할 수 있다.
)

print(gdf_dict)
```

            lifeExp         pop    gdpPercap
    year                                    
    1952  49.057620   3943953.0  1968.528344
    1957  51.507401   4282942.0  2173.220291
    1962  53.609249   4686039.5  2335.439533
    1967  55.678290   5170175.5  2678.334740
    1972  57.647386   5877996.5  3339.129407
    1977  59.570157   6404036.5  3798.609244
    1982  61.533197   7007320.0  4216.228428
    1987  63.212613   7774861.5  4280.300366
    1992  64.160338   8688686.5  4386.085502
    1997  65.014676   9735063.5  4781.825478
    2002  65.694923  10372918.5  5319.804524
    2007  67.007423  10517531.0  6124.371108


2. 시리즈에 사용하기
    - 시리즈에서는 열 이름을 지정한 딕셔너리를 `agg()`로 전달할 수 없다. 그 대신 이름을 변경하려면 원하는 함수 목록을 `agg()`에 전달하고 나서 `rename()` 메서드로 결과 열의 이름을 변경한다.


```python
gdf = (
    df
    .groupby("year")
    ["lifeExp"]
    .agg(
        [
            np.count_nonzero,
            "mean",
            "std",
        ] # 시리즈에서 먼저 집계
    )
    .rename(
        columns = {
            "count_nonzero": "count",
            "mean": "avg",
            "std": "std_dev",
        } # 집계하고 나서 열 이름 바꾸기
    )
    .reset_index() # 평탄화한 데이터프레임 반환
)

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


# II. 데이터 변환하기
- 데이터 변환 메서드는 데이터와 메서드를 일대일로 대응하여 계산하므로 데이터양은 줄지 않는다. 말 그대로 데이터를 변환하는 데 사용한다.

## II-1. 표준점수 계산하기
- 통계 분야에서는 평균과의 차이를 표준펴차로 나눈 값을 표준점수라고 한다. 표준점수를 구하면 변환한 데이터의 평균은 0이 되고 표준편차는 1이 된다.

$$z = \frac{x - \mu}{\sigma}$$

- $x$는 데이터셋 하나의 값이다.
- $\mu$는 데이터셋의 평균이다.
- $\sigma$는 표준편차로, 구하는 식은 다음과 같다.

$$\sigma = \sqrt{\frac{1}{n}\sum_{i=1}^{n} (x_i - \mu)^2}$$

### 표준점수 계산 함수 만들기


```python
# 표준점수 계산하는 함수 생성
def my_zscore(x):
    return((x - x.mean()) / x.std())
```

> - 매개변수 x는 값의 시리즈 또는 벡터를 의미한다.


```python
# transform() 사용하여 my_zscore() 함수로 year 열의 lifeExp를 변환
transform_z = df.groupby('year')["lifeExp"].transform(my_zscore)
print(transform_z)
```

    0      -1.656854
    1      -1.731249
    2      -1.786543
    3      -1.848157
    4      -1.894173
              ...   
    1699   -0.081621
    1700   -0.336974
    1701   -1.574962
    1702   -2.093346
    1703   -1.948180
    Name: lifeExp, Length: 1704, dtype: float64


> - `my_zscore()` 함수는 데이터를 표준화할 뿐 집계는 하지 않는다. 즉, 데이터양은 줄지 않는다.

- 다음은 원본 데이터프레임 df의 크기와 변환한 데이터프레임 transform_z의 크기를 비교한 것이다. 데이터의 행 개수가 줄지 않음을 알 수 있다.


```python
print(df.shape)
```

    (1704, 6)



```python
print(transform_z.shape)
```

    (1704,)


- scipy 라이브러리는 `zscore()` 함수를 제공한다. `groupby()`와 `transform()`에 `zscore()`를 적용하고 그룹화하지 않고 특정 열에 `zscore()`를 적용한 결과를 비교하자.


```python
from scipy.stats import zscore
```

- year 열로 그룹화하고 lifeExp 열에 `transform()` 메서드로 `zscore()`를 적용한 결과를 sp_z_grouped에 저장한다.


```python
sp_z_grouped = df.groupby('year')["lifeExp"].transform(zscore)
```

- 데이터를 그룹화하지 않고 데이터프레임의 lifeExp 열에 `zscore()`를 적용한 결과를 sp_z_nogroup에 저장


```python
sp_z_nogroup = zscore(df["lifeExp"])
```

- transform_z와 sp_z_grouped, sp_z_nogroup의 결과를 비교하면 값이 다르다는 것을 알 수 있다.


```python
print(transform_z.head())
```

    0   -1.656854
    1   -1.731249
    2   -1.786543
    3   -1.848157
    4   -1.894173
    Name: lifeExp, dtype: float64



```python
print(sp_z_grouped.head())
```

    0   -1.662719
    1   -1.737377
    2   -1.792867
    3   -1.854699
    4   -1.900878
    Name: lifeExp, dtype: float64



```python
print(sp_z_nogroup[:5])
```

    0   -2.375334
    1   -2.256774
    2   -2.127837
    3   -1.971178
    4   -1.811033
    Name: lifeExp, dtype: float64


> - 그룹화한 데이터프레임에 `my_zscore()` 함수를 적용한 결과와 scipy의 `zscore()` 함수를 적용한 결과는 비슷하다. 이와 달리 `groupby()`로 그룹으로 묶어 계산한 `zscore()`와 전체 데이터셋에 `zscore()`를 적용한 결과는 차이가 크다.

## II-2. 평균값으로 결측값 채우기
- 에볼라 데이터셋을 이용하여 `interpolat()` 메서드로 결측값을 채우거나 앞쪽이나 뒤쪽으로 데이터를 채우는 방법이다.

### 평균값으로 결측값 채우기


```python
# tips 데이터셋에서 10개 행 추출
import seaborn as sns
import numpy as np

np.random.seed(42) # 실행할 때마다 같은 결과를 얻도록 설정한다.

tips_10 = sns.load_dataset("tips").sample(10)
```


```python
# 추출한 10개 행 중에 4개의 total_bill값을 무작위로 선택하여 결측값 np.NaN으로 변경
tips_10.loc[
    np.random.permutation(tips_10.index)[:4],
    "total_bill"
] = np.nan

print(tips_10)
```

         total_bill   tip     sex smoker   day    time  size
    24        19.82  3.18    Male     No   Sat  Dinner     2
    6          8.77  2.00    Male     No   Sun  Dinner     2
    153         NaN  2.00    Male     No   Sun  Dinner     4
    211         NaN  5.16    Male    Yes   Sat  Dinner     4
    198         NaN  2.00  Female    Yes  Thur   Lunch     2
    176         NaN  2.00    Male    Yes   Sun  Dinner     2
    192       28.44  2.56    Male    Yes  Thur   Lunch     2
    124       12.48  2.52  Female     No  Thur   Lunch     2
    9         14.78  3.23    Male     No   Sun  Dinner     2
    101       15.38  3.00  Female    Yes   Fri  Dinner     2


> - 단순하게 total_bill의 평균으로 결측값을 채울 수도 있다. 하지만 성별(sex)에 따라 지출 습관이 다를 수도 있고 시간(time)이나 일행 수(size)에 따라 totla_bill 값이 다를 수도 있으므로 이럴 때를 고려하여 결측값을 채워 보자.


```python
# sex 열의 각 값에서 결측값이 아닌 값의 개수 확인
count_sex = tips_10.groupby('sex', observed = False).count()
print(count_sex)
```

            total_bill  tip  smoker  day  time  size
    sex                                             
    Male             4    7       7    7     7     7
    Female           2    3       3    3     3     3


> - Male은 결측값이 3개이고 Female은 1개다.


```python
# 그룹화된 평균을 계산하고 결측값 채우기
def fill_na_mean(x):
    avg = x.mean()
    return x.fillna(avg)

total_bill_group_mean = (
    tips_10
    .groupby("sex", observed = False)
    .total_bill
    .transform(fill_na_mean)
)

# 원본 데이터에 fill_total_bill이라는 새로운 열을 할당하여 결측값을 채운 결과를 추가
tips_10["fill_total_bill"] = total_bill_group_mean
```


```python
# total_bill 열과 fill_total_bill 비교
print(tips_10[['sex', 'total_bill', 'fill_total_bill']])
```

            sex  total_bill  fill_total_bill
    24     Male       19.82          19.8200
    6      Male        8.77           8.7700
    153    Male         NaN          17.9525
    211    Male         NaN          17.9525
    198  Female         NaN          13.9300
    176    Male         NaN          17.9525
    192    Male       28.44          28.4400
    124  Female       12.48          12.4800
    9      Male       14.78          14.7800
    101  Female       15.38          15.3800


# III. 원하는 데이터 걸러 내기
- 그룹화한 데이터에서 원하는 데이터를 걸러 내고 싶다면 어떻게 해야 할까? 이럴 때는 데이터 필터링을 사용한다. 데이터 필터링을 사용하면 기준에 맞는 데이터만 걸러낼 수 있다.

### 데이터 필터링하기


```python
# tips 데이터셋을 불러와 데이터 크기 확인
tips = sns.load_dataset('tips')
print(tips.shape)
```

    (244, 7)



```python
# size 열 각 값의 빈도수 확인
print(tips['size'].value_counts())
```

    size
    2    156
    3     38
    4     37
    5      5
    1      4
    6      4
    Name: count, dtype: int64


> - 출력 결과를 보면 일행이 1, 5, 6명인 데이터가 적다는 점을 알 수 있다.


```python
# 관측값이 30개 이상인 데이터만 필터링
tips_filltered = (
    tips
    .groupby("size")
    .filter(lambda x: x["size"].count() >= 30) # 람다 함수로 데이터 필터링
)
```


```python
# shape 확인
print(tips_filltered.shape)
```

    (231, 7)



```python
print(tips_filltered['size'].value_counts())
```

    size
    2    156
    3     38
    4     37
    Name: count, dtype: int64

