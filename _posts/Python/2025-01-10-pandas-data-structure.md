---
title: "판다스 자료구조 살펴보기"
format: 
  html:
    toc: true
    toc-depth: 4 # 목록 깊이
    # toc-expand: 3 # 목록 넓이
    toc-location: left 
    smooth-scroll: true 
    code-overflow: scroll
    number-sections: false
    self-contained: true # html 관련된 파일 하나만 만듦
---
<span style="color:gray">
**By Yong Bin Kwon, with Comments by Jung In Seo**
</span>

<details>
    <summary> Reference </summary>
        
<img src = "./../fig/fig1.png" width = 30%>
</details>

# I. 나만의 데이터 만들기

## I-1. 시리즈와 데이터프레임 만들기

### 시리즈 만들기
- 판다스의 시리즈는 파이썬의 기본 자료구조인 리스트와 비슷한 1차원 자료구조이며 데이터프레임의 각 열을 나타내는 자료형이다. 한 열에 있는 모든 값은 자료형(dtype)이 같아야 한다. 예를 들어 어떤 열에 숫자 1과 일련의 문자 'pizza'가 있다면 이 열의 전체 자료형은 문자열이 된다.
- 데이터프레임은 각 키가 열의 이름이고 값이 시리즈인 딕셔너리로 볼 수 있다. 시리즈는 모든 요소의 자료형이 같아야 한다는 점을 제외하면 파이썬의 리스트와 매우 닮았다.


```python
# 정수 42와 문자열 'banna'로 이루어진 리스트로 시리즈 생성
import pandas as pd

s = pd.Series(['banana', 42])
print(s)
```

    0    banana
    1        42
    dtype: object


> - 출력 결과를 보면 시리즈의 왼쪽에 행 번호가있다는 점을 확인할 수 있다. 이것이 시리즈의 인덱스이다.


```python
# 행 이름 인덱스 할당
s = pd.Series(data = ['Wes McKinney', 'Creator of Pandas'], 
              index = ['Person', 'Who']) # Person과 Who를 시리즈의 인덱스로 지정
print(s)
```

    Person         Wes McKinney
    Who       Creator of Pandas
    dtype: object


> - 위와 같이 Series를 생성할 때 매개변수 index에 파이썬 리스트를 지정하면 행 이름 인덱스를 할당할 수 있다.

### 데이터프레임 만들기
- 데이터프레임을 생성하는 가장 일반적인 방법은 딕셔너리를 사용하는 것이다.
- 딕셔너리의 키로 열 이름을 표현하고 값으로 열 내용을 설정한다.


```python
# 데이터프레임 scientists
scientists = pd.DataFrame({
    "Name" : ["Rosaline Franklin", "William gosset"],
    "Occupation" : ["Chemist", "Statistician"],
    "Born" : ["1920-07-25", "1876-06-13"],
    "Died" : ["1958-04-16", "1937-10-16"],
    "Age" : [37, 61],
})
print(scientists)
```

                    Name    Occupation        Born        Died  Age
    0  Rosaline Franklin       Chemist  1920-07-25  1958-04-16   37
    1     William gosset  Statistician  1876-06-13  1937-10-16   61



```python
scientists = pd.DataFrame(
    data = {
        "Occupation" : ["Chemist", "Statistician"],
        "Born" : ["1920-07-25", "1876-06-13"],
        "Died" : ["1958-04-16", "1937-10-16"],
        "Age" : [37, 61],
    },
    index = ["Rosaline Franklin", "William Gosset"], # 열 이름 인덱스로 사용
    columns = ["Occupation", "Born", "Died", "Age"], # 열 순서 지정
)
print(scientists)
```

                         Occupation        Born        Died  Age
    Rosaline Franklin       Chemist  1920-07-25  1958-04-16   37
    William Gosset     Statistician  1876-06-13  1937-10-16   61


# II. 시리즈 다루기

### 시리즈 추출하기


```python
# 매개변수 index에 행 이름 인덱스를 지정하여 scientists 데이터프레임 생성
scientists = pd.DataFrame(
    data = {
        "Occupation" : ["Chemist", "Statistician"],
        "Born" : ["1920-07-25", "1876-06-13"],
        "Died" : ["1958-04-16", "1937-10-16"],
        "Age" : [37, 61],
    },
    index = ["Rosaline Franklin", "William Gosset"],
    columns = ["Occupation", "Born", "Died", "Age"], 
)
print(scientists)
```

                         Occupation        Born        Died  Age
    Rosaline Franklin       Chemist  1920-07-25  1958-04-16   37
    William Gosset     Statistician  1876-06-13  1937-10-16   61



```python
# loc로 행 이름 인덱스 'William Gosset'을 지정하여 행 추출
first_row = scientists.loc['William Gosset']
print(type(first_row))
```

    <class 'pandas.core.series.Series'>



```python
# first_row 내용 확인
print(first_row)
```

    Occupation    Statistician
    Born            1876-06-13
    Died            1937-10-16
    Age                     61
    Name: William Gosset, dtype: object


> - 시리즈를 `print()`로 출력하면 첫 번째 열에 열 이름을, 두 번째 열에 값을 표시한다.


```python
# index 확인
print(first_row.index)
# values 확인
print(first_row.values)
```

    Index(['Occupation', 'Born', 'Died', 'Age'], dtype='object')
    ['Statistician' '1876-06-13' '1937-10-16' np.int64(61)]


|시리즈 속성|설명|
|:---|:---|
|loc|열 이름으로 데이터 추출|
|iloc|열 위치로 데이터 추출|
|dtype 또는 dtypes|시리즈에 저장된 값의 자료형
|T|시리즈의 전치|
|shape|데이터의 차원|
|size|시리즈의 요소 개수|
|values|시리즈의 ndarray 또는 ndarray와 같은 형태|

- 자세한 내용은 공식 [문서](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)를 참조

## II-1. 시리즈의 keys() 메서드
- 시리즈의 `keys()` 메서드는 index 속성과 같은 역할을 한다.


```python
print(first_row.keys())
```

    Index(['Occupation', 'Born', 'Died', 'Age'], dtype='object')


> - 속성은 객체의 특성이라 볼 수 있으며 메서드는 객체를 대상으로 수행하는 계산이나 연산이라 할 수 있다.
> - 속성 구문을 사용할 때는 소괄호 ()가 아닌 대괄호[]를 사용한다.


```python
# index 속성을 사용하여 첫 번째 인덱스 추출
print(first_row.index[0])

# keys()를 사용하여 첫 번째 인덱스 추출
print(first_row.keys()[0])
```

    Occupation
    Occupation


## II-2. 시리즈와 ndarray
- 판다스의 시리즈 자료구조는 넘파이의 ndarray(numpy.ndarray)와 매우 닮아있다. ndarray에서 사용할 수 있는 대부분의 메서드와 함수는 시리즈에도 사용할 수 있다. 한 특성에 대한 여러 가지 값이므로 시리즈를 벡터라고도 한다.

### 시리즈의 메서드 사용하기


```python
# scientists 데이터프레임에서 Age 열 시리즈 추출
ages = scientists['Age']
print(ages)
```

    Rosaline Franklin    37
    William Gosset       61
    Name: Age, dtype: int64


- 넘파이는 숫자 벡터를 다루는 유명한 과학 계산 라이브러리이다. 시리즈는 넘파이의 ndarray를 확장한 개념으로 생각할 수 있으며 많은 속성과 메서드를 그대로 사용할 수 있다.


```python
# 평균
print(ages.mean())

# 최솟값
print(ages.min())

# 최댓값
print(ages.max())

# 표준편차
print(ages.std())
```

    49.0
    37
    61
    16.97056274847714


|시리즈 메서드|설명|
|:---|:---|
|append()|2개 이상의 시리즈 연결|
|corr()|다른 시리즈와의 상관관계 계산(결측값은 자동으로 제외)|
|cov()|다른 시리즈와의 공분산 계산(결측값은 자동으로 제외)|
|describe()|요약 통계랑 계산(결측값은 자동으로 제외)|
|drop_duplicates()|중복값이 없는 시리즈 반환|
|equals()|시리즈에 주어진 값을 가진 요소가 있는지 확인|
|get_values()|시리즈 값 구하기(values 속성과 동일)|
|hist()|히스토그램 그리기|
|isin()|주어진 값이 시리즈에 포함되어 있는지 확인|
|min()|최솟값 반환|
|max()|최댓값 반환|
|mean()|산술 평균 반환|
|median()|중앙값 반환|
|mode()|최빈값 반환|
|quantile()|사분위수로 값을 반환|
|replace()|시리즈의 특정 값을 변환|
|sample()|시리즈에서 임의의 값을 반환|
|sort_values()|값 정렬|
|to_frame()|시리즈를 데이터프레임으로 변환|
|transpose()|시리즈의 전치 반환|
|unique()|고윳값만으로 이루어진 numpy.ndarray를 반환|

- 자세한 내용은 ndarray의 공식 [문서](https://numpy.org/doc/stable/reference/arrays.ndarray.html)를 참고

## II-3. 시리즈와 불리언

### 기술 통계량 계산하기


```python
scientists = pd.read_csv('./data/scientists.csv')

# scientists 데이터프레임에서 Age 열(나이)을 추출하고 ages에 저장
ages = scientists['Age']
print(ages)
```

    0    37
    1    61
    2    90
    3    66
    4    56
    5    45
    6    41
    7    77
    Name: Age, dtype: int64



```python
# 기술 통계량 계산
print(ages.describe())
```

    count     8.000000
    mean     59.125000
    std      18.325918
    min      37.000000
    25%      44.000000
    50%      58.500000
    75%      68.750000
    max      90.000000
    Name: Age, dtype: float64



```python
# 평균값 계산
print(ages.mean())
```

    59.125



```python
# 평균 나이보다 나이가 많은 과학자만 추출
print(ages[ages > ages.mean()])
```

    1    61
    2    90
    3    66
    7    77
    Name: Age, dtype: int64



```python
# ages > ages.mean() 알아보기
print(ages > ages.mean())
```

    0    False
    1     True
    2     True
    3     True
    4    False
    5    False
    6    False
    7     True
    Name: Age, dtype: bool



```python
# 자료형 확인
print(type(ages > ages.mean()))
```

    <class 'pandas.core.series.Series'>



```python
# 행 번호가 0, 1, 4, 5, 7인 데이터 추출
manual_bool_values = [
    True, # 0
    True, # 1
    False, # 2
    False, # 3
    True, # 4
    True, # 5
    False, # 6
    True, # 7
]
print(ages[manual_bool_values])
```

    0    37
    1    61
    4    56
    5    45
    7    77
    Name: Age, dtype: int64


## II-4. 시리즈와 브로드캐스팅
- 프로그래밍에 익숙한 독자라면 for 루프 없이 age > age.mean()이라는 조건문 하나로 해당하는 모든 데이터를 반환한다는 점이 이상해 보일 수도 있다. 이는 시리즈와 데이터프레임을 대상으로 사용하는 많은 메서드는 모든 데이터를 대상으로 연산, 즉 **브로드캐스팅**하기 때문이다. 이 방법을 사용하면 코드의 가독성을 높일 수 있고 일반적으로 계산 속도를 높이는 최적화 효과도 얻을 수 있다.

    ✓ **브로드캐스팅**은 벡터화라 부르기도 한다.

### 벡터와 벡터, 벡터와 스칼라 계산하기


```python
# 두 개의 시리즈를 대상으로 연산 수행
print(ages + ages)

print(ages * ages)
```

    0     74
    1    122
    2    180
    3    132
    4    112
    5     90
    6     82
    7    154
    Name: Age, dtype: int64
    0    1369
    1    3721
    2    8100
    3    4356
    4    3136
    5    2025
    6    1681
    7    5929
    Name: Age, dtype: int64



```python
# 스칼라와 벡터 연산
print(ages + 100)

print(ages *2)
```

    0    137
    1    161
    2    190
    3    166
    4    156
    5    145
    6    141
    7    177
    Name: Age, dtype: int64
    0     74
    1    122
    2    180
    3    132
    4    112
    5     90
    6     82
    7    154
    Name: Age, dtype: int64


### 길이가 서로 다른 벡터 연산하기
- 길이가 서로 다른 벡터로 연산할 때는 벡터의 `type()`에 따라 결과가 달라진다. 길이가 서로 다른 벡터를 연산할 때는 인덱스가 같은 요소끼리 연산을 수행한다. 결과 벡터에서 나머지는 결측값으로 채우고 숫자가 아님을 나타내는 NaN(Not a Number)으로 표시한다.


```python
# 길이가 8인 ages 시리즈와 길이가 2인 새로운 시리즈 더하기
print(ages + pd.Series([1, 100]))
```

    0     38.0
    1    161.0
    2      NaN
    3      NaN
    4      NaN
    5      NaN
    6      NaN
    7      NaN
    dtype: float64


> - 결과를 보면 인덱스가 일치하는 0, 1행만 계산했다는 것을 알 수 있다. 나머지 인덱스(2~7)는 계산할 수 없으므로 결측값(NaN)으로 처리한다.

### 인덱스가 같은 벡터 자동 정렬하기
- 판다스는 대부분의 데이터를 자동으로 정렬하므로 편리하다. 즉, 어떤 작업을 수행할 때 시리즈나 데이터프레임은 가능한 한 인덱스를 기준으로 데이터를 정렬한다.


```python
# ages 시리즈 내림차순
rev_ages = ages.sort_index(ascending = False)
print(rev_ages)
```

    7    77
    6    41
    5    45
    4    56
    3    66
    2    90
    1    61
    0    37
    Name: Age, dtype: int64



```python
# ages 스칼라 연산
print(ages * 2)
```

    0     74
    1    122
    2    180
    3    132
    4    112
    5     90
    6     82
    7    154
    Name: Age, dtype: int64



```python
# age와 rev_ages 더한 결과
print(ages + rev_ages)
```

    0     74
    1    122
    2    180
    3    132
    4    112
    5     90
    6     82
    7    154
    Name: Age, dtype: int64


# III. 데이터프레임 다루기
- 데이터프레임은 가장 일반적인 판다스 객체이다.

## III-1. 데이터프레임의 구성
- 판다스의 데이터프레임 객체에는 각각 행 이름, 열 이름, 데이터를 나타내는 index, cloumns, values의 3가지 주요 구성 요소가 있다.


```python
# index
scientists.index
```




    RangeIndex(start=0, stop=8, step=1)




```python
# columns
scientists.columns
```




    Index(['Name', 'Born', 'Died', 'Age', 'Occupation'], dtype='object')




```python
# values
scientists.values
```




    array([['Rosaline Franklin', '1920-07-25', '1958-04-16', 37, 'Chemist'],
           ['William Gosset', '1876-06-13', '1937-10-16', 61, 'Statistician'],
           ['Florence Nightingale', '1820-05-12', '1910-08-13', 90, 'Nurse'],
           ['Marie Curie', '1867-11-07', '1934-07-04', 66, 'Chemist'],
           ['Rachel Carson', '1907-05-27', '1964-04-14', 56, 'Biologist'],
           ['John Snow', '1813-03-15', '1858-06-16', 45, 'Physician'],
           ['Alan Turing', '1912-06-23', '1954-06-07', 41,
            'Computer Scientist'],
           ['Johann Gauss', '1777-04-30', '1855-02-23', 77, 'Mathematician']],
          dtype=object)



> - values는 모든 행 인덱스 정보를 보는 대신 기본 numpy 표현법으로 간단하게 데이터를 살펴볼때 유용하다.

## III-2. 데이터프레임과 불리언 추출
- True나 False와 같은 불리언으로 시리즈의 일부 데이터를 추출할 수 있었던 것처럼 데이터프레임에서도 불리언으로 데이터를 추출할 수 있다.


```python
# Age 열의 값이 평균값보다 큰 데이터만 추출
print(scientists.loc[scientists['Age'] > scientists['Age'].mean()])
```

                       Name        Born        Died  Age     Occupation
    1        William Gosset  1876-06-13  1937-10-16   61   Statistician
    2  Florence Nightingale  1820-05-12  1910-08-13   90          Nurse
    3           Marie Curie  1867-11-07  1934-07-04   66        Chemist
    7          Johann Gauss  1777-04-30  1855-02-23   77  Mathematician


- loc 외에도 데이터프레임에서 데이터를 추출하는 다양한 방법

|구문|추출 결과|
|:---|:---|
|df[column_name]|시리즈|
|df[[column1, column2, ...]]|데이터프레임|
|df.loc[row_label]|행 이름으로 추출한 행 데이터|
|df.loc[[label1, label2, ...]]|행 이름으로 추출한 여러 행 데이터|
|df.iloc[row_number]|행 번호로 추출한 행 데이터|
|df.iloc[[row1, row2, ...]]|행 번호로 추출한 여러 행 데이터|
|df[bool]|불리언으로 추출한 행 데이터|
|df[[bool1, bool2, ...]]|불리언으로 추출한 여러 행 데이터|
|df[start:stop:step]|슬라이싱 구문으로 추출한 여러 행 데이터|

## III-3. 데이터프레임과 브로드캐스팅
- 시리즈와 데이터프레임 객체는 넘파이 [라이브러리](https://numpy.org/) 기반으로 구현했으므로 브로드캐스팅을 지원하는 넘파이와 같이 판다스도 브로드캐스팅을 지원한다. 브로드캐스팅은 배열과 같은 객체 사이에 연산을 수행하는 원리를 의미하며 이는 객체 유형, 길이, 객체와 연결된 레이블 등에 따라 달라진다.

### 데이터프레임을 대상으로 연산하기


```python
# scientists 데이터프레임을 반씩 나누기
first_half = scientists[:4]
second_half = scientists[4:]
print(first_half)
```

                       Name        Born        Died  Age    Occupation
    0     Rosaline Franklin  1920-07-25  1958-04-16   37       Chemist
    1        William Gosset  1876-06-13  1937-10-16   61  Statistician
    2  Florence Nightingale  1820-05-12  1910-08-13   90         Nurse
    3           Marie Curie  1867-11-07  1934-07-04   66       Chemist



```python
print(second_half)
```

                Name        Born        Died  Age          Occupation
    4  Rachel Carson  1907-05-27  1964-04-14   56           Biologist
    5      John Snow  1813-03-15  1858-06-16   45           Physician
    6    Alan Turing  1912-06-23  1954-06-07   41  Computer Scientist
    7   Johann Gauss  1777-04-30  1855-02-23   77       Mathematician


- 데이터프레임과 스칼라를 연산하면 데이터프레임의 각 셀마다 스칼라와 연산을 수행한다. 이때 각 셀의 자료형에 따라 연산이 다르다. 예를 들어 scientists 데이터프레임에 스칼라 2를 곱하면 숫자 셀은 2배가 되고 문자열은 두 번 반복한다.


```python
print(scientists * 2)
```

                                           Name                  Born  \
    0        Rosaline FranklinRosaline Franklin  1920-07-251920-07-25   
    1              William GossetWilliam Gosset  1876-06-131876-06-13   
    2  Florence NightingaleFlorence Nightingale  1820-05-121820-05-12   
    3                    Marie CurieMarie Curie  1867-11-071867-11-07   
    4                Rachel CarsonRachel Carson  1907-05-271907-05-27   
    5                        John SnowJohn Snow  1813-03-151813-03-15   
    6                    Alan TuringAlan Turing  1912-06-231912-06-23   
    7                  Johann GaussJohann Gauss  1777-04-301777-04-30   
    
                       Died  Age                            Occupation  
    0  1958-04-161958-04-16   74                        ChemistChemist  
    1  1937-10-161937-10-16  122              StatisticianStatistician  
    2  1910-08-131910-08-13  180                            NurseNurse  
    3  1934-07-041934-07-04  132                        ChemistChemist  
    4  1964-04-141964-04-14  112                    BiologistBiologist  
    5  1858-06-161858-06-16   90                    PhysicianPhysician  
    6  1954-06-071954-06-07   82  Computer ScientistComputer Scientist  
    7  1855-02-231855-02-23  154            MathematicianMathematician  



```python
# add()메서드를 사용해 데이터프레임의 모든 값이 숫자이고 같은 셀끼리 값을 더하기
df1 = df2 = pd.DataFrame(data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]])
df_added = df1.add(df2)
print(df_added)
```

        0   1   2
    0   2   4   6
    1   8  10  12
    2  14  16  18


# IV. 시리즈와 데이터프레임 데이터 변환하기

### 열 추가하기


```python
print(scientists.dtypes)
```

    Name          object
    Born          object
    Died          object
    Age            int64
    Occupation    object
    dtype: object


> - 각 열의 유형을 보면 Born 열(출생일)과 Died 열(사망일)의 dtype은 object이다. 즉, 두 열의 값은 문자열 또는 일련의 문자이다.
> - 날짜와 시간을 나타내는 문자열은 datetime형으로 변환하는 것이 좋다. 날짜 차이를 계산하거나 사람의 나이를 계산하는 등 날짜와 시간 계산에 다양하게 활용할 수 있기 때문이다. 날짜 문자열이 다른 형식이라면 매개변수 format을 지정하여 원하는 형식의 datetime으로 변환할 수 있다.


```python
# Born 열을 문자열(object)에서 datetime형으로 변환
born_datetime = pd.to_datetime(scientists['Born'], format = '%Y-%m-%d')
print(born_datetime)
```

    0   1920-07-25
    1   1876-06-13
    2   1820-05-12
    3   1867-11-07
    4   1907-05-27
    5   1813-03-15
    6   1912-06-23
    7   1777-04-30
    Name: Born, dtype: datetime64[ns]



```python
# Died 열을 datetime형으로 변환
died_datetime = pd.to_datetime(scientists['Died'], format = '%Y-%m-%d')
```


```python
# born_datetime과 died_datetime을 각각 born_dt, died_dt라는 새로운 열로 추가
scientists['born_dt'], scientists['died_dt'] = (born_datetime, died_datetime)
```


```python
print(scientists.head())
```

                       Name        Born        Died  Age    Occupation    born_dt  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   37       Chemist 1920-07-25   
    1        William Gosset  1876-06-13  1937-10-16   61  Statistician 1876-06-13   
    2  Florence Nightingale  1820-05-12  1910-08-13   90         Nurse 1820-05-12   
    3           Marie Curie  1867-11-07  1934-07-04   66       Chemist 1867-11-07   
    4         Rachel Carson  1907-05-27  1964-04-14   56     Biologist 1907-05-27   
    
         died_dt  
    0 1958-04-16  
    1 1937-10-16  
    2 1910-08-13  
    3 1934-07-04  
    4 1964-04-14  



```python
print(scientists.shape)
```

    (8, 7)



```python
print(scientists.dtypes)
```

    Name                  object
    Born                  object
    Died                  object
    Age                    int64
    Occupation            object
    born_dt       datetime64[ns]
    died_dt       datetime64[ns]
    dtype: object


### 열 내용 변환하기


```python
# scientists 데이터프레임의 Age 열 확인
print(scientists['Age'])
```

    0    37
    1    61
    2    90
    3    66
    4    56
    5    45
    6    41
    7    77
    Name: Age, dtype: int64



```python
# sample() 사용하여 열을 무작위로 섞기
print(scientists["Age"].sample(frac = 1, random_state = 42))
```

    1    61
    5    45
    0    37
    7    77
    2    90
    4    56
    3    66
    6    41
    Name: Age, dtype: int64


> - 매개변수 frac에 원하는 추출 비율을 0~1 사이의 값을 지정할 수 있다.
> - random_state는 컴퓨터가 생성하는 난수의 기준값을 정하는 역할로, 같은 값을 사용하면 같은 결과를 얻는다.


```python
# 섞인 열을 Age 열에 할당
scientists["Age"] = scientists["Age"].sample(frac = 1, random_state = 42)
print(scientists['Age'])
```

    0    37
    1    61
    2    90
    3    66
    4    56
    5    45
    6    41
    7    77
    Name: Age, dtype: int64


> - 분명히 순서를 섞은 데이터를 할당했는데 Age 열의 값이 기존 순서대로 돌아왔다. 이것은 판다스가 대부분의 연산에서 index를 기준으로 자동 병합하기 때문이다. 즉, 열의 값을 섞을 때 각 값에 대응하는 인덱스는 변하지 않으면서 제자리를 찾아간 것이다. 이 문제를 해결하려면 열의 값을 할당할 때 index 정보를 제거해야 한다.


```python
# values를 사용하여 섞인 값 할당
scientists["Age"] = scientists["Age"].sample(frac = 1, random_state = 42).values
print(scientists['Age'])
```

    0    61
    1    45
    2    37
    3    77
    4    90
    5    56
    6    66
    7    41
    Name: Age, dtype: int64



```python
# datetime 연산을 사용하여 각 과학자의 실제 나이 구하기
scientists['age_days'] = (scientists['died_dt'] - scientists['born_dt'])
print(scientists)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  
    0 1920-07-25 1958-04-16 13779 days  
    1 1876-06-13 1937-10-16 22404 days  
    2 1820-05-12 1910-08-13 32964 days  
    3 1867-11-07 1934-07-04 24345 days  
    4 1907-05-27 1964-04-14 20777 days  
    5 1813-03-15 1858-06-16 16529 days  
    6 1912-06-23 1954-06-07 15324 days  
    7 1777-04-30 1855-02-23 28422 days  



```python
import numpy as np

# age_days에 저장된 날짜 수를 햇수로 변환
scientists['age_years'] = (scientists['age_days'].dt.days / 365).apply(np.floor)
print(scientists)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  age_years  
    0 1920-07-25 1958-04-16 13779 days       37.0  
    1 1876-06-13 1937-10-16 22404 days       61.0  
    2 1820-05-12 1910-08-13 32964 days       90.0  
    3 1867-11-07 1934-07-04 24345 days       66.0  
    4 1907-05-27 1964-04-14 20777 days       56.0  
    5 1813-03-15 1858-06-16 16529 days       45.0  
    6 1912-06-23 1954-06-07 15324 days       41.0  
    7 1777-04-30 1855-02-23 28422 days       77.0  


> - Age열은 엉망이 되었지만 age_years로 과학자의 진짜 나이를 다시 확인할 수 있게 되었다.

- [x] 매개변수 inplace를 사용할 때는 조심!!
    - 판다스 라이브러리의 많은 함수와 메서드는 매개변수 inplace를 제공한다. 기본적으로 판다스의 함수나 메서드는 기존의 데이터프레임을 그대로 두고 함수 호출로 수정된 데이터프레임을 새로운 데이터프레임으로 생성하여 반환한다. 이때 inplace를 True로 지정하면 수정된 데이터프레임이 아닌 None을 반환하고 기존 데이터프레임을 바로 수정한다. 그러므로 데이터프레임을 바로 수정하면 데이터를 원치 않게 덮어쓸 수 있으므로 매개변수 inplace는 설정하지 않는 것이 좋다.

### assign()으로 열 수정하기
- 열을 할당하고 수정하는 또 다른 방법으로 `assign()`메서드가 있다. 이 메서드를 사용하면 메서드 체이닝을 활용할 수 있다.
- `assign()`메서드를 사용하는 방법은 할당 연산자(=)의 왼쪽에 새로운 열의 이름을 넣고 오른쪽에 할당할 값을 계산하는 연산을 넣는다. 각 열은 쉼표(,)로 구분하여 작성한다.


```python
# assign() 사용해서 기존의 age_days와 age_year를 구한 연산을 각각 age_days_assign, age_year_assign에 할당
scientists = scientists.assign(
    age_days_assign = scientists['died_dt'] - scientists['born_dt'],
    age_year_assign = (scientists['age_days'].dt.days / 365)
                        .apply(np.floor))
print(scientists)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  age_years age_days_assign  age_year_assign  
    0 1920-07-25 1958-04-16 13779 days       37.0      13779 days             37.0  
    1 1876-06-13 1937-10-16 22404 days       61.0      22404 days             61.0  
    2 1820-05-12 1910-08-13 32964 days       90.0      32964 days             90.0  
    3 1867-11-07 1934-07-04 24345 days       66.0      24345 days             66.0  
    4 1907-05-27 1964-04-14 20777 days       56.0      20777 days             56.0  
    5 1813-03-15 1858-06-16 16529 days       45.0      16529 days             45.0  
    6 1912-06-23 1954-06-07 15324 days       41.0      15324 days             41.0  
    7 1777-04-30 1855-02-23 28422 days       77.0      28422 days             77.0  


- [x] 다른 방법으로 나이 계산하기
    - `assign()`을 사용한 예제를 보면 age_year_assign의 값을 계산할 때 age_days_assign 대신 age_days를 사용한 것을 확인할 수 있다. 그럼 age_days가 없다면 어떻게 해야 할까? 앞서 본 것처럼 날짜 수를 햇수로 변환하면 된다.


```python
# 람다 함수를 사용하여 age_your_assign 구하기
scientists = scientists.assign(
    age_days_assign = scientists["died_dt"] - scientists["born_dt"],
    age_year_assign = lambda df_:
    (df_["age_days_assign"].dt.days / 365).apply(np.floor) # 날짜 수를 햇수로 변환하는 람다 함수
)
print(scientists)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  age_years age_days_assign  age_year_assign  
    0 1920-07-25 1958-04-16 13779 days       37.0      13779 days             37.0  
    1 1876-06-13 1937-10-16 22404 days       61.0      22404 days             61.0  
    2 1820-05-12 1910-08-13 32964 days       90.0      32964 days             90.0  
    3 1867-11-07 1934-07-04 24345 days       66.0      24345 days             66.0  
    4 1907-05-27 1964-04-14 20777 days       56.0      20777 days             56.0  
    5 1813-03-15 1858-06-16 16529 days       45.0      16529 days             45.0  
    6 1912-06-23 1954-06-07 15324 days       41.0      15324 days             41.0  
    7 1777-04-30 1855-02-23 28422 days       77.0      28422 days             77.0  


- 다양한 예제는 `assign()`공식 [문서](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.assign.html)를 참고

### 열 삭제하기
- 열을 삭제하려면 열 추출 기법을 사용하여 남기고 싶은 모든 열을 선택하거나 데이터프레임의 `drop()`메서드로 특정 열만 없애면 된다.


```python
# colums 속성을 사용하여 scientists 데이터프레임에 있는 모든 열 확인
print(scientists.columns)
```

    Index(['Name', 'Born', 'Died', 'Age', 'Occupation', 'born_dt', 'died_dt',
           'age_days', 'age_years', 'age_days_assign', 'age_year_assign'],
          dtype='object')



```python
# Age 열 삭제
scientists_dropped = scientists.drop(['Age'], axis = "columns")

print(scientists_dropped.columns)
```

    Index(['Name', 'Born', 'Died', 'Occupation', 'born_dt', 'died_dt', 'age_days',
           'age_years', 'age_days_assign', 'age_year_assign'],
          dtype='object')


# V. 데이터 저장하고 불러오기

## V-1. 피클로 저장하고 불러오기
- 피클은 이진 형식(바이너리)으로 직렬화한 데이터를 저장하는 방법으로, 저장한 데이터는 그대로 다시 읽어 올 수 있다. 피클 파일은 보통 .p, .pkl, .pickle 등의 확장자로 저장한다.

### 시리즈와 데이터프레임 저장하기


```python
# Name열을 names에 저장
names = scientists['Name']
print(names)
```

    0       Rosaline Franklin
    1          William Gosset
    2    Florence Nightingale
    3             Marie Curie
    4           Rachel Carson
    5               John Snow
    6             Alan Turing
    7            Johann Gauss
    Name: Name, dtype: object



```python
# to_pickle() 사용하여 피클로 저장
names.to_pickle('/Users/kwon/Desktop/scientists_names_series.pickle')
```

> - 피클로 파이썬 객체를 저장하면 파이썬과 디스크 저장 공간에 최적화된 상태로 데이터를 저장할 수 있다. 그러나 파이썬을 사용하지 않으면 데이터를 읽을 수 없다.


```python
# 데이터프레임 피클로 저장
scientists.to_pickle('/Users/kwon/Desktop/scientists_df.pickle')
```

### 피클 데이터 읽어 오기
- `pd.read_pickle()`함수를 사용하면 피클 데이터를 읽어 올 수 있다.


```python
# names 시리지를 저장했던 피클 파일 읽기
series_pickle = pd.read_pickle('/Users/kwon/Desktop/scientists_names_series.pickle')
print(series_pickle)
```

    0       Rosaline Franklin
    1          William Gosset
    2    Florence Nightingale
    3             Marie Curie
    4           Rachel Carson
    5               John Snow
    6             Alan Turing
    7            Johann Gauss
    Name: Name, dtype: object



```python
# 데이터프레임을 저장했던 피클 파일 읽기
dataframe_pickle = pd.read_pickle('/Users/kwon/Desktop/scientists_df.pickle')
print(dataframe_pickle)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  age_years age_days_assign  age_year_assign  
    0 1920-07-25 1958-04-16 13779 days       37.0      13779 days             37.0  
    1 1876-06-13 1937-10-16 22404 days       61.0      22404 days             61.0  
    2 1820-05-12 1910-08-13 32964 days       90.0      32964 days             90.0  
    3 1867-11-07 1934-07-04 24345 days       66.0      24345 days             66.0  
    4 1907-05-27 1964-04-14 20777 days       56.0      20777 days             56.0  
    5 1813-03-15 1858-06-16 16529 days       45.0      16529 days             45.0  
    6 1912-06-23 1954-06-07 15324 days       41.0      15324 days             41.0  
    7 1777-04-30 1855-02-23 28422 days       77.0      28422 days             77.0  


## V-2. CSV와 TSV 파일로 저장하고 불러오기
- CSV 파일은 데이터를 쉼표로 구분한 파일로, 가장 유연한 데이터 저장 형식이다. 물론, 다른 문자로 구분할 수도 있다. 탭으로 구분하면 TSV 파일이 되며 세미콜론(;)을 사용하기도 한다.
- 기본적으로 테이터프레임의 행 번호(index)도 CSV 파일에 저장한다. 이렇게 되면 CSV의 첫 번째 열에 열 이름 없이 행 번호만 나열된다. 이열은 다시 판다스로 읽을 때 걸림돌이 된다. 이럴 때는 매개변수 index를 False로 지정하여 CSV 파일을 저장하면 이 문제를 방지할 수 있다.


## V-3. 엑셀로 저장하기
- CSV 다음으로 널리 사용하는 데이터 형식은 엑셀이다. 사실 엑셀은 색상과 기타 불필요한 정보가 데이터셋과 함께 저장되므로 데이터 과학 분야에서 잘 사용하지는 않는다.

### 시리즈와 데이터프레임 저장하기
- 시리즈 자료구조는 `to_excel()`메서드를 제공하지 않는다. 그러므로 시리즈를 엑셀 파일로 추출하려면 이를 열이 하나인 데이터프레임으로 변환해야 한다.


```python
# openpyxl 라이브러리 설치
!pip install openpyxl
```

    Requirement already satisfied: openpyxl in /opt/anaconda3/envs/py3.11/lib/python3.11/site-packages (3.1.5)
    Requirement already satisfied: et-xmlfile in /opt/anaconda3/envs/py3.11/lib/python3.11/site-packages (from openpyxl) (2.0.0)



```python
names = scientists['Name']
print(names)
```

    0       Rosaline Franklin
    1          William Gosset
    2    Florence Nightingale
    3             Marie Curie
    4           Rachel Carson
    5               John Snow
    6             Alan Turing
    7            Johann Gauss
    Name: Name, dtype: object



```python
# to_frame()메서드로 시리즈를 데이터프레임으로 변환
names_df = names.to_frame()

# 데이터프레임의 to_excel() 메서드로 엑셀 파일 저장
names_df.to_excel('/Users/kwon/Desktop/scientists_names_series_df.xls',
                  engine = 'openpyxl')
```


```python
# 매개변수 sheet_name을 사용하여 엑셀 파일의 특정 시트에 결과 저장
scientists.to_excel('/Users/kwon/Desktop/scientists_df.xlsx',
                    sheet_name = "scientists", # 이름이 scientists인 시트에 저장
                    index = False)
```

## V-4. 다양한 형식으로 저장하기

### feather 파일로 저장하기
- feather 파일은 데이터프레임을 R과 같은 다른 언어에서 읽을 수 있는 이진 객체로 저장할 때 사용한다. 이 형식의 파일은 CSV 파일보다 읽고 쓰는 속도가 빠르다.
- 자세한 사용 방법은 `to_feater()`[메서드](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_feather.html)와 feather 파일 형식의 공식 [문서](https://arrow.apache.org/docs/python/feather.html)를 참고


```python
# feather 포매터 설치
!pip install pyarrow
```

    Requirement already satisfied: pyarrow in /opt/anaconda3/envs/py3.11/lib/python3.11/site-packages (18.1.0)



```python
# to_feather() 사용하여 feather객체 저장
scientists.to_feather('/Users/kwon/Desktop/scientists.feather')
```


```python
# pd.read_feather() 사용하여 파일 읽기
sci_feather = pd.read_feather('/Users/kwon/Desktop/scientists.feather')
print(sci_feather)
```

                       Name        Born        Died  Age          Occupation  \
    0     Rosaline Franklin  1920-07-25  1958-04-16   61             Chemist   
    1        William Gosset  1876-06-13  1937-10-16   45        Statistician   
    2  Florence Nightingale  1820-05-12  1910-08-13   37               Nurse   
    3           Marie Curie  1867-11-07  1934-07-04   77             Chemist   
    4         Rachel Carson  1907-05-27  1964-04-14   90           Biologist   
    5             John Snow  1813-03-15  1858-06-16   56           Physician   
    6           Alan Turing  1912-06-23  1954-06-07   66  Computer Scientist   
    7          Johann Gauss  1777-04-30  1855-02-23   41       Mathematician   
    
         born_dt    died_dt   age_days  age_years age_days_assign  age_year_assign  
    0 1920-07-25 1958-04-16 13779 days       37.0      13779 days             37.0  
    1 1876-06-13 1937-10-16 22404 days       61.0      22404 days             61.0  
    2 1820-05-12 1910-08-13 32964 days       90.0      32964 days             90.0  
    3 1867-11-07 1934-07-04 24345 days       66.0      24345 days             66.0  
    4 1907-05-27 1964-04-14 20777 days       56.0      20777 days             56.0  
    5 1813-03-15 1858-06-16 16529 days       45.0      16529 days             45.0  
    6 1912-06-23 1954-06-07 15324 days       41.0      15324 days             41.0  
    7 1777-04-30 1855-02-23 28422 days       77.0      28422 days             77.0  


### 딕셔너리로 변환하기
- 판다스 시리즈와 데이터프레임 객체는 `to_dict()`메서드도 제공한다. 이 메서드는 시리즈 또는 데이터프레임 객체를 파이썬 딕셔너리 객체로 변환한다. 이 형식은 판다스 없이 시리즈나 데이터프레임의 데이터를 사용해야 할 때 유용하다.


```python
# 두 행 추출
sci_sub_dict = scientists.head(2)

# 데이터프레임을 딕셔너리로 변환
sci_dict = sci_sub_dict.to_dict()

# pprint 라이브러리를 사용하여 딕셔너리 결과 출력
import pprint
pprint.pprint(sci_dict)
```

    {'Age': {0: 61, 1: 45},
     'Born': {0: '1920-07-25', 1: '1876-06-13'},
     'Died': {0: '1958-04-16', 1: '1937-10-16'},
     'Name': {0: 'Rosaline Franklin', 1: 'William Gosset'},
     'Occupation': {0: 'Chemist', 1: 'Statistician'},
     'age_days': {0: Timedelta('13779 days 00:00:00'),
                  1: Timedelta('22404 days 00:00:00')},
     'age_days_assign': {0: Timedelta('13779 days 00:00:00'),
                         1: Timedelta('22404 days 00:00:00')},
     'age_year_assign': {0: 37.0, 1: 61.0},
     'age_years': {0: 37.0, 1: 61.0},
     'born_dt': {0: Timestamp('1920-07-25 00:00:00'),
                 1: Timestamp('1876-06-13 00:00:00')},
     'died_dt': {0: Timestamp('1958-04-16 00:00:00'),
                 1: Timestamp('1937-10-16 00:00:00')}}



```python
# 변환한 딕셔너리를 다시 판다스 데이터프레임으로 읽기
sci_dict_df = pd.DataFrame.from_dict(sci_dict)
print(sci_dict_df)
```

                    Name        Born        Died  Age    Occupation    born_dt  \
    0  Rosaline Franklin  1920-07-25  1958-04-16   61       Chemist 1920-07-25   
    1     William Gosset  1876-06-13  1937-10-16   45  Statistician 1876-06-13   
    
         died_dt   age_days  age_years age_days_assign  age_year_assign  
    0 1958-04-16 13779 days       37.0      13779 days             37.0  
    1 1937-10-16 22404 days       61.0      22404 days             61.0  


- [x] 날짜와 시간은 처리할 떄 조심!!
    - scientists 데이터셋은 날짜와 시간 정보를 포함하기 때문에 단순히 딕셔너리 출력 결과를 복사, 붙여 넣기 하여 pd.DataFrame.from_dict() 함수에 문자열로 넘기면 안된다. NameError: name 'Timedelta' is not define 오류가 발생하기 때문이다. 날짜와 시간은 화면에 출력된 것과 다른 형식으로 저장된다. 날짜를 처리해야 한다면 object와 같은 일반적인 형식으로 변환하고 그 값을 다시 날짜로 변환해야 한다.

### JSON으로 저장하기
- JSON 데이터는 또 다른 일반 텍스트 파일 형식이다. `to_json()`을 사용하면 날짜와 시간을 다시 읽을 수 있다는 장점이 있다.


```python
# 매개변수 indent의 인수로 2를 전달하면 2칸 들여쓰기를 적용
sci_json = sci_sub_dict.to_json(orient = 'records', indent = 2, date_format = "iso")
pprint.pprint(sci_json)
```

    ('[\n'
     '  {\n'
     '    "Name":"Rosaline Franklin",\n'
     '    "Born":"1920-07-25",\n'
     '    "Died":"1958-04-16",\n'
     '    "Age":61,\n'
     '    "Occupation":"Chemist",\n'
     '    "born_dt":"1920-07-25T00:00:00.000",\n'
     '    "died_dt":"1958-04-16T00:00:00.000",\n'
     '    "age_days":"P13779DT0H0M0S",\n'
     '    "age_years":37.0,\n'
     '    "age_days_assign":"P13779DT0H0M0S",\n'
     '    "age_year_assign":37.0\n'
     '  },\n'
     '  {\n'
     '    "Name":"William Gosset",\n'
     '    "Born":"1876-06-13",\n'
     '    "Died":"1937-10-16",\n'
     '    "Age":45,\n'
     '    "Occupation":"Statistician",\n'
     '    "born_dt":"1876-06-13T00:00:00.000",\n'
     '    "died_dt":"1937-10-16T00:00:00.000",\n'
     '    "age_days":"P22404DT0H0M0S",\n'
     '    "age_years":61.0,\n'
     '    "age_days_assign":"P22404DT0H0M0S",\n'
     '    "age_year_assign":61.0\n'
     '  }\n'
     ']')



```python
# 출력 결과를 복사하여 데이터프레임으로 변환
sci_json_df = pd.read_json(
    ('[\n'
 '  {\n'
 '    "Name":"Rosaline Franklin",\n'
 '    "Born":"1920-07-25",\n'
 '    "Died":"1958-04-16",\n'
 '    "Age":61,\n'
 '    "Occupation":"Chemist",\n'
 '    "born_dt":"1920-07-25T00:00:00.000",\n'
 '    "died_dt":"1958-04-16T00:00:00.000",\n'
 '    "age_days":"P13779DT0H0M0S",\n'
 '    "age_years":37.0,\n'
 '    "age_days_assign":"P13779DT0H0M0S",\n'
 '    "age_year_assign":37.0\n'
 '  },\n'
 '  {\n'
 '    "Name":"William Gosset",\n'
 '    "Born":"1876-06-13",\n'
 '    "Died":"1937-10-16",\n'
 '    "Age":45,\n'
 '    "Occupation":"Statistician",\n'
 '    "born_dt":"1876-06-13T00:00:00.000",\n'
 '    "died_dt":"1937-10-16T00:00:00.000",\n'
 '    "age_days":"P22404DT0H0M0S",\n'
 '    "age_years":61.0,\n'
 '    "age_days_assign":"P22404DT0H0M0S",\n'
 '    "age_year_assign":61.0\n'
 '  }\n'
 ']'),
    orient = "records"
)
print(sci_json_df)
```

                    Name        Born        Died  Age    Occupation  \
    0  Rosaline Franklin  1920-07-25  1958-04-16   61       Chemist   
    1     William Gosset  1876-06-13  1937-10-16   45  Statistician   
    
                       born_dt                  died_dt        age_days  \
    0  1920-07-25T00:00:00.000  1958-04-16T00:00:00.000  P13779DT0H0M0S   
    1  1876-06-13T00:00:00.000  1937-10-16T00:00:00.000  P22404DT0H0M0S   
    
       age_years age_days_assign  age_year_assign  
    0         37  P13779DT0H0M0S               37  
    1         61  P22404DT0H0M0S               61  


    /var/folders/52/9g__g13d4zjgvrl6vdg9_ln80000gn/T/ipykernel_1128/15127984.py:2: FutureWarning: Passing literal json to 'read_json' is deprecated and will be removed in a future version. To read from a literal string, wrap it in a 'StringIO' object.
      sci_json_df = pd.read_json(



```python
# dtypes로 자료형 확인
print(sci_json_df.dtypes)
```

    Name               object
    Born               object
    Died               object
    Age                 int64
    Occupation         object
    born_dt            object
    died_dt            object
    age_days           object
    age_years           int64
    age_days_assign    object
    age_year_assign     int64
    dtype: object



```python
# died_dt를 datetime으로 변환한 값을 died_dt_json 열에 저장
sci_json_df["died_dt_json"] = pd.to_datetime(sci_json_df["died_dt"])
print(sci_json_df)
```

                    Name        Born        Died  Age    Occupation  \
    0  Rosaline Franklin  1920-07-25  1958-04-16   61       Chemist   
    1     William Gosset  1876-06-13  1937-10-16   45  Statistician   
    
                       born_dt                  died_dt        age_days  \
    0  1920-07-25T00:00:00.000  1958-04-16T00:00:00.000  P13779DT0H0M0S   
    1  1876-06-13T00:00:00.000  1937-10-16T00:00:00.000  P22404DT0H0M0S   
    
       age_years age_days_assign  age_year_assign died_dt_json  
    0         37  P13779DT0H0M0S               37   1958-04-16  
    1         61  P22404DT0H0M0S               61   1937-10-16  



```python
# dtype로 자료형 확인
print(sci_json_df.dtypes)
```

    Name                       object
    Born                       object
    Died                       object
    Age                         int64
    Occupation                 object
    born_dt                    object
    died_dt                    object
    age_days                   object
    age_years                   int64
    age_days_assign            object
    age_year_assign             int64
    died_dt_json       datetime64[ns]
    dtype: object


> - died_dt 열과 died_dt_json 열의 dtype 차이를 확인할 수 있다.

## V-5. 다양한 데이터 저장 유형

|내보내기 메서드|설명|
|:---|:---|
|to_clipboard()|데이터를 붙여 넣을 수 있도록 시스템 클립보드에 저장|
|to_dict()|데이터를 파이썬 딕셔너리로 변환|
|to_gbq()|데이터를 구글 빅쿼리 테이블로 변환|
|to_hdf()|데이터를 계층적 데이터 형식(HDF)으로 저장|
|to_html()|데이터를 HTML 테이블로 변환|
|to_json()|데이터를 JSON 문자열로 변환|
|to_latex()|데이터를 LATEX 정형 데이터 환경으로 변환|
|to_records()|데이터를 레코드 배열로 변환|
|to_string()|데이터프레임을 문자열로 출력|
|to_sql()|데이터를 SQL 데이터베이스에 저장|


```python

```
