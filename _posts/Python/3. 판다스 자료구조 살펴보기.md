---
layout: post
title:  "[Python] 판다스 자료구조 살펴보기" 
subtitle: 판다스 입문
tags: [pandas]
categories: Python
---
<span style="color:gray">
**By Yong Bin Kwon, with Comments by Jung In Seo**
</span>

<details>
    <summary> Reference </summary>
        
<img width = "30%" alt = "fig1" src = "https://github.com/user-attachments/assets/59dee25b-108e-4f51-9c93-7425b38f9351" />
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

