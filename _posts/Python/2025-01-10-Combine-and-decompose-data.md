---
layout: post
title:  "[Python] 데이터 결합하고 분해하기" 
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

# I. 데이터 묶어 분석하기
- 이 장에서는 깔끔한 데이터 논문에서 소개한 세 번째 기준인 "관측 단위별로 데이터 표를 구성해야 한다."에 초점을 맞춘다. 즉, 관측 단위별로 정리한 다양한 표를 결합하여 데이터셋을 만들고 분석을 진행한다.
- 예를 들어 주식 데이터를 분석하는 과정에서 기업 정보 데이터셋과 주식 가격 데이터셋이 있을 때 첨단 산업 기업의 주식 가격 데이터를 보려면 어떻게 해야 할까? 일단 기업 정보 데이터에서 첨단 기술을 가진 기업을 찾아야 한다. 그리고 이 기업의 주식 가격을 찾아야한다. 그런 다음 찾아낸 2개의 데이터를 연결하면 된다.
- 보통 데이터의 중복을 방지하고자 데이터를 여러 개의 표로 나누어 저장하곤 한다. 이렇게 연관이 깊은 데이터끼리 모아서 표를 구성하므로 데이터를 묶어 필요한 데이터를 만드는 과정이 반드시 필요하다.
- 하나의 데이터셋을 여러 부분으로 분할하여 관리할 때도 있다. 예를 들어 시계열 데이터를 날짜별로 분리해서 파일을 관리할 수 있다. 또는 데이터의 크기가 너무 커서 적당한 크기의 데이터 파일로 나누어 관리할 수도 있다. 이럴 때도 여러 데이터를 묶어 단일 데이터프레임으로 결합하고 데이터를 분석한다.

# II. 데이터 연결하기
- 데이터를 결합하는 가장 쉬운 방법은 데이터를 연결하는 것이다. 데이터에 행이나 열을 추가한다. 여러 부분으로 나눈 데이터를 결합하거나 기존 데이터셋에 다른 데이터셋을 추가할 때 데이터를 연결한다. 


```python
# concat_1.csv, concat_2.csv, concat_3.csv 데이터셋 불러오기
import pandas as pd

df1 = pd.read_csv('./data/concat_1.csv')
df2 = pd.read_csv('./data/concat_2.csv')
df3 = pd.read_csv('./data/concat_3.csv')
```


```python
print(df1)
```

        A   B   C   D
    0  a0  b0  c0  d0
    1  a1  b1  c1  d1
    2  a2  b2  c2  d2
    3  a3  b3  c3  d3



```python
print(df2)
```

        A   B   C   D
    0  a4  b4  c4  d4
    1  a5  b5  c5  d5
    2  a6  b6  c6  d6
    3  a7  b7  c7  d7



```python
print(df3)
```

         A    B    C    D
    0   a8   b8   c8   d8
    1   a9   b9   c9   d9
    2  a10  b10  c10  d10
    3  a11  b11  c11  d11


## II-1. 데이터프레임 살펴보기


```python
# df1 index 확인
print(df1.index)
```

    RangeIndex(start=0, stop=4, step=1)


- index는 데이터프레임의 축이다. 판다스는 축을 기준으로 작동하므로 이런 용어가 매우 중요하다. 인덱스와 다른 축은 columns로 얻을 수 있는 열이다.


```python
# df1 columns 확인
print(df1.columns)
```

    Index(['A', 'B', 'C', 'D'], dtype='object')


> - 이는 데이터프레임의 열 이름을 나타낸다.


```python
# df1 values 확인
print(df1.values) # 값으로 구성된 넘파이 배열 반환
```

    [['a0' 'b0' 'c0' 'd0']
     ['a1' 'b1' 'c1' 'd1']
     ['a2' 'b2' 'c2' 'd2']
     ['a3' 'b3' 'c3' 'd3']]


## II-2. 행 연결하기
- 판다스의 `concat()` 함수를 사용하면 데이터프레임을 연결할 수 있다.

### 행 방향 연결하기


```python
# 연결할 모든 데이터프레임을 concat() 함수에 리스트로 전달
row_concat = pd.concat([df1, df2, df3])
print(row_concat)
```

         A    B    C    D
    0   a0   b0   c0   d0
    1   a1   b1   c1   d1
    2   a2   b2   c2   d2
    3   a3   b3   c3   d3
    0   a4   b4   c4   d4
    1   a5   b5   c5   d5
    2   a6   b6   c6   d6
    3   a7   b7   c7   d7
    0   a8   b8   c8   d8
    1   a9   b9   c9   d9
    2  a10  b10  c10  d10
    3  a11  b11  c11  d11


> - 결과에서 알 수 있듯이 `concat()`은 데이터프레임을 단순하게 이어 붙인다. 그러므로 행 이름(인덱스)을 보면 0부터 3까지의 숫자가 반복되는 것을 알 수 있다. 이는 세 데이터프레임의 인덱스를 단순히 누적했다는 것을 나타낸다.


```python
# 네 번째 행 추출
print(row_concat.iloc[3, :])
```

    A    a3
    B    b3
    C    c3
    D    d3
    Name: 3, dtype: object



```python
# 시리즈 생성
new_row_series = pd.Series(['n1', 'n2', 'n3', 'n4'])
print(new_row_series)
```

    0    n1
    1    n2
    2    n3
    3    n4
    dtype: object



```python
# 시리즈를 새로운 행으로 df1에 추가
print(pd.concat([df1, new_row_series]))
```

         A    B    C    D    0
    0   a0   b0   c0   d0  NaN
    1   a1   b1   c1   d1  NaN
    2   a2   b2   c2   d2  NaN
    3   a3   b3   c3   d3  NaN
    0  NaN  NaN  NaN  NaN   n1
    1  NaN  NaN  NaN  NaN   n2
    2  NaN  NaN  NaN  NaN   n3
    3  NaN  NaN  NaN  NaN   n4


> - 기존에 있던 값과 어긋난 새로운 열이 생성 되었다.

- 위 문제를 해결하려면 시리즈를 데이터프레임으로 바꿔야 한다. 이 데이터프레임은 하나의 데이터 행을 포함하고 열 이름은 데이터가 덧붙여질 각 위치의 열 이름으로 설정한다.


```python
# 데이터프레임 생성
new_row_df = pd.DataFrame(
    data = [["n1", "n2", "n3", "n4"]],
    columns = ["A", "B", "C", "D"],
)
print(new_row_df)
```

        A   B   C   D
    0  n1  n2  n3  n4



```python
# 생성한 데이터프레임을 df1에 연결
print(pd.concat([df1, new_row_df]))
```

        A   B   C   D
    0  a0  b0  c0  d0
    1  a1  b1  c1  d1
    2  a2  b2  c2  d2
    3  a3  b3  c3  d3
    0  n1  n2  n3  n4


> - `concat()`은 여러 데이터프레임을 한 번에 연결할 때 가장 자주 사용하는 함수이다.

### 새로운 인덱스 설정하기
- 기존 데이터의 행 인덱스를 무시하고 새로운 인덱스를 부여하려면 매개변수 ignore_index를 사용하면 된다. 이 매개변수를 True로 설정하면 인덱스가 0부터 시작하는 정수로 중복값 없이 다시 설정된다.


```python
# ingnore_idex를 사용하여 데이터를 연결한 후에 인덱스를 다시 설정
row_concat_i = pd.concat([df1, df2, df3], ignore_index = True)
print(row_concat_i)
```

          A    B    C    D
    0    a0   b0   c0   d0
    1    a1   b1   c1   d1
    2    a2   b2   c2   d2
    3    a3   b3   c3   d3
    4    a4   b4   c4   d4
    5    a5   b5   c5   d5
    6    a6   b6   c6   d6
    7    a7   b7   c7   d7
    8    a8   b8   c8   d8
    9    a9   b9   c9   d9
    10  a10  b10  c10  d10
    11  a11  b11  c11  d11


## II-3. 열 연결하기
- 열을 연결하는 것은 행을 연결하는 방법과 비슷하다. 가장 큰 차이점을 `concat()` 함수의 매개변수 axis이다. axis의 기본값은 0(또는 "index")이므로 매개변수를 따로 지정하지 않으면 데이터를 기본값인 행 방향으로 덧붙여 연결한다. 그러나 axis를 1 또는 "columns"로 설정하면 열 방향으로 연결한다.

### 열 방향 연결하기


```python
# df1, df2, df3를 열 방향으로 연결
col_concat = pd.concat([df1, df2, df3], axis = "columns")
print(col_concat)
```

        A   B   C   D   A   B   C   D    A    B    C    D
    0  a0  b0  c0  d0  a4  b4  c4  d4   a8   b8   c8   d8
    1  a1  b1  c1  d1  a5  b5  c5  d5   a9   b9   c9   d9
    2  a2  b2  c2  d2  a6  b6  c6  d6  a10  b10  c10  d10
    3  a3  b3  c3  d3  a7  b7  c7  d7  a11  b11  c11  d11


> - 결과 데이터프레임을 보면 행을 기준으로 데이터를 연결했을 때 행 인덱스가 그대로 덧붙여진 것과 마찬가지로 열 이름이 단순히 덧붙여진 것을 알 수 있다.


```python
# 열 이름을 기준으로 데이터 추출
print(col_concat['A'])
```

        A   A    A
    0  a0  a4   a8
    1  a1  a5   a9
    2  a2  a6  a10
    3  a3  a7  a11



```python
# new_col_list라는 이름의 새로운 열로 추가
col_concat['new_col_list'] = ['n1', 'n2', 'n3', 'n4']
print(col_concat)
```

        A   B   C   D   A   B   C   D    A    B    C    D new_col_list
    0  a0  b0  c0  d0  a4  b4  c4  d4   a8   b8   c8   d8           n1
    1  a1  b1  c1  d1  a5  b5  c5  d5   a9   b9   c9   d9           n2
    2  a2  b2  c2  d2  a6  b6  c6  d6  a10  b10  c10  d10           n3
    3  a3  b3  c3  d3  a7  b7  c7  d7  a11  b11  c11  d11           n4



```python
# 시리즈 추가
col_concat['new_col_series'] = pd.Series(['n1', 'n2', 'n3', 'n4'])
print(col_concat)
```

        A   B   C   D   A   B   C   D    A    B    C    D new_col_list  \
    0  a0  b0  c0  d0  a4  b4  c4  d4   a8   b8   c8   d8           n1   
    1  a1  b1  c1  d1  a5  b5  c5  d5   a9   b9   c9   d9           n2   
    2  a2  b2  c2  d2  a6  b6  c6  d6  a10  b10  c10  d10           n3   
    3  a3  b3  c3  d3  a7  b7  c7  d7  a11  b11  c11  d11           n4   
    
      new_col_series  
    0             n1  
    1             n2  
    2             n3  
    3             n4  



```python
# ignore_index를 True로 설정
print(pd.concat([df1, df2, df3], axis = "columns", ignore_index = True)) # 열 이름을 다시 설정
```

       0   1   2   3   4   5   6   7    8    9    10   11
    0  a0  b0  c0  d0  a4  b4  c4  d4   a8   b8   c8   d8
    1  a1  b1  c1  d1  a5  b5  c5  d5   a9   b9   c9   d9
    2  a2  b2  c2  d2  a6  b6  c6  d6  a10  b10  c10  d10
    3  a3  b3  c3  d3  a7  b7  c7  d7  a11  b11  c11  d11


## II-4. 인덱스나 열 이름이 다른 데이터 연결하기

### 열 이름이 다른 데이터 행 방향 연결하기


```python
# df1, df2, df3의 열 이름 바꾸기
df1.columns = ['A', 'B', 'C', 'D']
df2.columns = ['E', 'F', 'G', 'H']
df3.columns = ['A', 'C', 'F', 'H']
```


```python
print(df1)
```

        A   B   C   D
    0  a0  b0  c0  d0
    1  a1  b1  c1  d1
    2  a2  b2  c2  d2
    3  a3  b3  c3  d3



```python
print(df2)
```

        E   F   G   H
    0  a4  b4  c4  d4
    1  a5  b5  c5  d5
    2  a6  b6  c6  d6
    3  a7  b7  c7  d7



```python
print(df3)
```

         A    C    F    H
    0   a8   b8   c8   d8
    1   a9   b9   c9   d9
    2  a10  b10  c10  d10
    3  a11  b11  c11  d11



```python
# concat()을 사용하여 열을 자동으로 정렬하고 결측값은 NaN으로 채우기
row_concat = pd.concat([df1, df2, df3])
print(row_concat)
```

         A    B    C    D    E    F    G    H
    0   a0   b0   c0   d0  NaN  NaN  NaN  NaN
    1   a1   b1   c1   d1  NaN  NaN  NaN  NaN
    2   a2   b2   c2   d2  NaN  NaN  NaN  NaN
    3   a3   b3   c3   d3  NaN  NaN  NaN  NaN
    0  NaN  NaN  NaN  NaN   a4   b4   c4   d4
    1  NaN  NaN  NaN  NaN   a5   b5   c5   d5
    2  NaN  NaN  NaN  NaN   a6   b6   c6   d6
    3  NaN  NaN  NaN  NaN   a7   b7   c7   d7
    0   a8  NaN   b8  NaN  NaN   c8  NaN   d8
    1   a9  NaN   b9  NaN  NaN   c9  NaN   d9
    2  a10  NaN  b10  NaN  NaN  c10  NaN  d10
    3  a11  NaN  b11  NaN  NaN  c11  NaN  d11


- NaN값을 포함하지 않는 한 가지 방법은 연결할 객체 사이에 공통인 열만 유지하는 것이다. 이때 매개변수 join을 사용한다. 기본값은 모든 열을 유지하는 'outer'이므로 데이터 셋 사이에 공통인 열만 유지하면 'inner'로 설정한다.


```python
print(pd.concat([df1, df2, df3], join = 'inner'))
```

    Empty DataFrame
    Columns: []
    Index: [0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3]


> - 데이터프레임 3개 모두에 공통인 열은 없으므로 'inner'로 설정하면 빈 데이터프레임만 남는다.


```python
print(pd.concat([df1, df3], ignore_index = False, join = 'inner'))
```

         A    C
    0   a0   c0
    1   a1   c1
    2   a2   c2
    3   a3   c3
    0   a8   b8
    1   a9   b9
    2  a10  b10
    3  a11  b11


> - 공통인 열이 있는 데이터프레임을 사용하면 공통인 열만 반환한다.

### 인덱스가 다른 데이터 열 방향 연결하기


```python
# 데이터프레임 3개가 서로 다른 인덱스가 되도록 수정
df1.index = [0, 1, 2, 3]
df2.index = [4, 5, 6, 7]
df3.index = [0, 2, 5, 7]
```


```python
print(df1)
```

        A   B   C   D
    0  a0  b0  c0  d0
    1  a1  b1  c1  d1
    2  a2  b2  c2  d2
    3  a3  b3  c3  d3



```python
print(df2)
```

        E   F   G   H
    4  a4  b4  c4  d4
    5  a5  b5  c5  d5
    6  a6  b6  c6  d6
    7  a7  b7  c7  d7



```python
print(df3)
```

         A    C    F    H
    0   a8   b8   c8   d8
    2   a9   b9   c9   d9
    5  a10  b10  c10  d10
    7  a11  b11  c11  d11


- axis = 1 또는 axis = "columns"로 데이터프레임을 연결하면 새로운 데이터프레임을 열 방향으로 연결하고 인덱스가 같은 행끼리 연결한다.


```python
# axis = "columns"로 데이터프레임 연결
col_concat = pd.concat([df1, df2, df3], axis = "columns")
print(col_concat)
```

         A    B    C    D    E    F    G    H    A    C    F    H
    0   a0   b0   c0   d0  NaN  NaN  NaN  NaN   a8   b8   c8   d8
    1   a1   b1   c1   d1  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
    2   a2   b2   c2   d2  NaN  NaN  NaN  NaN   a9   b9   c9   d9
    3   a3   b3   c3   d3  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
    4  NaN  NaN  NaN  NaN   a4   b4   c4   d4  NaN  NaN  NaN  NaN
    5  NaN  NaN  NaN  NaN   a5   b5   c5   d5  a10  b10  c10  d10
    6  NaN  NaN  NaN  NaN   a6   b6   c6   d6  NaN  NaN  NaN  NaN
    7  NaN  NaN  NaN  NaN   a7   b7   c7   d7  a11  b11  c11  d11


> - 이때 인덱스가 다른 데이터는 결측값으로 표시한다.


```python
# df1과 df3 중 인덱스가 공통인 0행과 2행만 열 방향으로 연결
print(pd.concat([df1, df3], axis = "columns", join = 'inner'))
```

        A   B   C   D   A   C   F   H
    0  a0  b0  c0  d0  a8  b8  c8  d8
    2  a2  b2  c2  d2  a9  b9  c9  d9


# III. 분할된 데이터 연결하기
- 데이터를 여러 개로 분할하는 이유 중 하나는 파일 크기 때문이다. 즉, 데이터를 분할할수록 데이터 파일 하나의 크기가 작아진다. 많은 서비스에서 조회하거나 공유할 수 있는 파일의 크기를 제한하곤 하므로 인터넷이나 이메일을 통해 데이터를 공유할 때는 데이터를 분할할 때가 흔하다.
- 데이터를 여러 개로 분할하는 또 다른 이유로 데이터 수집 과정을 들 수 있다. 예를 들어 주식 정보를 매일 저장한다면 날짜별로 주식 데이터셋이 나뉠 수 있다.

### 여러 개의 파일로 분할된 데이터 연결하기


```python
# pathlib 모듈을 사용하여 특정 파일 이름 규칙의 모든 파일 불러오기
from pathlib import Path

billboard_data_files = (
    Path(".")
    .glob("./data/billboard_by_week/billboard-*.csv") # billboard-로 시작하는 모든 csv 파일 불러오기
)

billboard_data_files = sorted(list(billboard_data_files))
print(billboard_data_files)
```

    [PosixPath('data/billboard_by_week/billboard-01.csv'), PosixPath('data/billboard_by_week/billboard-02.csv'), PosixPath('data/billboard_by_week/billboard-03.csv'), PosixPath('data/billboard_by_week/billboard-04.csv'), PosixPath('data/billboard_by_week/billboard-05.csv'), PosixPath('data/billboard_by_week/billboard-06.csv'), PosixPath('data/billboard_by_week/billboard-07.csv'), PosixPath('data/billboard_by_week/billboard-08.csv'), PosixPath('data/billboard_by_week/billboard-09.csv'), PosixPath('data/billboard_by_week/billboard-10.csv'), PosixPath('data/billboard_by_week/billboard-11.csv'), PosixPath('data/billboard_by_week/billboard-12.csv'), PosixPath('data/billboard_by_week/billboard-13.csv'), PosixPath('data/billboard_by_week/billboard-14.csv'), PosixPath('data/billboard_by_week/billboard-15.csv'), PosixPath('data/billboard_by_week/billboard-16.csv'), PosixPath('data/billboard_by_week/billboard-17.csv'), PosixPath('data/billboard_by_week/billboard-18.csv'), PosixPath('data/billboard_by_week/billboard-19.csv'), PosixPath('data/billboard_by_week/billboard-20.csv'), PosixPath('data/billboard_by_week/billboard-21.csv'), PosixPath('data/billboard_by_week/billboard-22.csv'), PosixPath('data/billboard_by_week/billboard-23.csv'), PosixPath('data/billboard_by_week/billboard-24.csv'), PosixPath('data/billboard_by_week/billboard-25.csv'), PosixPath('data/billboard_by_week/billboard-26.csv'), PosixPath('data/billboard_by_week/billboard-27.csv'), PosixPath('data/billboard_by_week/billboard-28.csv'), PosixPath('data/billboard_by_week/billboard-29.csv'), PosixPath('data/billboard_by_week/billboard-30.csv'), PosixPath('data/billboard_by_week/billboard-31.csv'), PosixPath('data/billboard_by_week/billboard-32.csv'), PosixPath('data/billboard_by_week/billboard-33.csv'), PosixPath('data/billboard_by_week/billboard-34.csv'), PosixPath('data/billboard_by_week/billboard-35.csv'), PosixPath('data/billboard_by_week/billboard-36.csv'), PosixPath('data/billboard_by_week/billboard-37.csv'), PosixPath('data/billboard_by_week/billboard-38.csv'), PosixPath('data/billboard_by_week/billboard-39.csv'), PosixPath('data/billboard_by_week/billboard-40.csv'), PosixPath('data/billboard_by_week/billboard-41.csv'), PosixPath('data/billboard_by_week/billboard-42.csv'), PosixPath('data/billboard_by_week/billboard-43.csv'), PosixPath('data/billboard_by_week/billboard-44.csv'), PosixPath('data/billboard_by_week/billboard-45.csv'), PosixPath('data/billboard_by_week/billboard-46.csv'), PosixPath('data/billboard_by_week/billboard-47.csv'), PosixPath('data/billboard_by_week/billboard-48.csv'), PosixPath('data/billboard_by_week/billboard-49.csv'), PosixPath('data/billboard_by_week/billboard-50.csv'), PosixPath('data/billboard_by_week/billboard-51.csv'), PosixPath('data/billboard_by_week/billboard-52.csv'), PosixPath('data/billboard_by_week/billboard-53.csv'), PosixPath('data/billboard_by_week/billboard-54.csv'), PosixPath('data/billboard_by_week/billboard-55.csv'), PosixPath('data/billboard_by_week/billboard-56.csv'), PosixPath('data/billboard_by_week/billboard-57.csv'), PosixPath('data/billboard_by_week/billboard-58.csv'), PosixPath('data/billboard_by_week/billboard-59.csv'), PosixPath('data/billboard_by_week/billboard-60.csv'), PosixPath('data/billboard_by_week/billboard-61.csv'), PosixPath('data/billboard_by_week/billboard-62.csv'), PosixPath('data/billboard_by_week/billboard-63.csv'), PosixPath('data/billboard_by_week/billboard-64.csv'), PosixPath('data/billboard_by_week/billboard-65.csv'), PosixPath('data/billboard_by_week/billboard-66.csv'), PosixPath('data/billboard_by_week/billboard-67.csv'), PosixPath('data/billboard_by_week/billboard-68.csv'), PosixPath('data/billboard_by_week/billboard-69.csv'), PosixPath('data/billboard_by_week/billboard-70.csv'), PosixPath('data/billboard_by_week/billboard-71.csv'), PosixPath('data/billboard_by_week/billboard-72.csv'), PosixPath('data/billboard_by_week/billboard-73.csv'), PosixPath('data/billboard_by_week/billboard-74.csv'), PosixPath('data/billboard_by_week/billboard-75.csv'), PosixPath('data/billboard_by_week/billboard-76.csv')]



```python
# 리스트로 변환
billboard_data_files = list(billboard_data_files)
```


```python
# 각 파일을 데이터프레임으로 불러오기
billboard01 = pd.read_csv(billboard_data_files[0])
billboard02 = pd.read_csv(billboard_data_files[1])
billboard03 = pd.read_csv(billboard_data_files[2])
```


```python
# 데이터 확인
print(billboard01)
```

         year            artist                    track  time date.entered week  \
    0    2000             2 Pac  Baby Don't Cry (Keep...  4:22   2000-02-26  wk1   
    1    2000           2Ge+her  The Hardest Part Of ...  3:15   2000-09-02  wk1   
    2    2000      3 Doors Down               Kryptonite  3:53   2000-04-08  wk1   
    3    2000      3 Doors Down                    Loser  4:24   2000-10-21  wk1   
    4    2000          504 Boyz            Wobble Wobble  3:35   2000-04-15  wk1   
    ..    ...               ...                      ...   ...          ...  ...   
    312  2000       Yankee Grey     Another Nine Minutes  3:10   2000-04-29  wk1   
    313  2000  Yearwood, Trisha          Real Live Woman  3:55   2000-04-01  wk1   
    314  2000   Ying Yang Twins  Whistle While You Tw...  4:19   2000-03-18  wk1   
    315  2000     Zombie Nation            Kernkraft 400  3:30   2000-09-02  wk1   
    316  2000   matchbox twenty                     Bent  4:12   2000-04-29  wk1   
    
         rating  
    0      87.0  
    1      91.0  
    2      81.0  
    3      76.0  
    4      57.0  
    ..      ...  
    312    86.0  
    313    85.0  
    314    95.0  
    315    99.0  
    316    60.0  
    
    [317 rows x 7 columns]



```python
# 각 데이터프레임의 shape 확인
print(billboard01.shape)
print(billboard02.shape)
print(billboard03.shape)
```

    (317, 7)
    (317, 7)
    (317, 7)



```python
# concat()으로 불러온 데이터 연결
billboard = pd.concat([billboard01, billboard02, billboard03])

# 연결한 데이터프레임의 shape 확인
print(billboard.shape)
```

    (951, 7)



```python
# 데이터프레임이 올바르게 연결되었는지 assert문으로 행의 개수를 비교하여 확인
assert (
    billboard01.shape[0]
    + billboard02.shape[0]
    + billboard03.shape[0]
    == billboard.shape[0]
)
```

> - 분할한 데이터가 많을수록 이렇게 직접 각 데이터프레임을 저장하는 것은 번거로운 작업이다. 이럴 때는 루프 구문이나 리스트 컴프리헨션을 사용하여 데이터 불러오기 작업을 자동화하는 것이 좋다.

### 루프 구문으로 여러 개의 파일 불러오기
- 루프 구문으로 여러 개의 파일을 불러오는 방법은 다음과 같다.
    + 먼저 빈 리스트를 만들고 루프를 사용하여 각 CSV 파일을 순회하면서 판다스 데이터프레임으로 불러온 다음, 마지막으로 데이터프레임을 리스트에 추가한다. 


```python
from pathlib import Path

billboard_data_files = (
    Path(".")
    .glob("./data/billboard_by_week/billboard-*.csv")
)

# 빈 리스트를 생성
list_billboard_df = []

# CSV 파일명 리스트를 순회
for csv_filename in billboard_data_files:

    # 아래 코드를 주석 해제하면 CSV 파일 이름을 확인할 수 있다.
    # print(csv_filename)

    # CSV 파일을 데이터프레임으로 불러오기
    df = pd.read_csv(csv_filename)

    # 데이터프레임을 리스트에 추가
    list_billboard_df.append(df)

# 데이터프레임 개수를 출력
print(len(list_billboard_df))
    
```

    76


- [x] 필요하다면 제너레이터는 리스트로 변환하자!!
    + `Path.glob()` 메서드는 제너레이터를 반환한다. 즉, 각 요소를 순회할 때 해당 항목은 사라진다는 의미이다. 이러한 동작은 파이썬이 메모리에 모든 요소를 저장하지 않도록 하므로 컴퓨팅 리소스를 절약하기 좋다. 하지만 필요할 때마다 제너레이터를 다시 생성해야 한다는 단점이 있다.
    + 제너레이터의 요소에 자주 접근해야 한다면 `list()` 함수를 사용하여 list(billboard_data_files)처럼 제너레이터를 일반 파이썬 리스트로 변환하는 것이 좋다. 그러면 모든 요소를 리스트로 저장하므로 여러 번 접근할 수 있다.


```python
# list_billboard_df의 첫 번째 요소의 유형 확인
print(type(list_billboard_df[0]))
```

    <class 'pandas.core.frame.DataFrame'>



```python
# 첫 번째 요소 데이터 확인
print(list_billboard_df[0])
```

         year            artist                    track  time date.entered  week  \
    0    2000             2 Pac  Baby Don't Cry (Keep...  4:22   2000-02-26  wk15   
    1    2000           2Ge+her  The Hardest Part Of ...  3:15   2000-09-02  wk15   
    2    2000      3 Doors Down               Kryptonite  3:53   2000-04-08  wk15   
    3    2000      3 Doors Down                    Loser  4:24   2000-10-21  wk15   
    4    2000          504 Boyz            Wobble Wobble  3:35   2000-04-15  wk15   
    ..    ...               ...                      ...   ...          ...   ...   
    312  2000       Yankee Grey     Another Nine Minutes  3:10   2000-04-29  wk15   
    313  2000  Yearwood, Trisha          Real Live Woman  3:55   2000-04-01  wk15   
    314  2000   Ying Yang Twins  Whistle While You Tw...  4:19   2000-03-18  wk15   
    315  2000     Zombie Nation            Kernkraft 400  3:30   2000-09-02  wk15   
    316  2000   matchbox twenty                     Bent  4:12   2000-04-29  wk15   
    
         rating  
    0       NaN  
    1       NaN  
    2      38.0  
    3      72.0  
    4      78.0  
    ..      ...  
    312     NaN  
    313     NaN  
    314     NaN  
    315     NaN  
    316     3.0  
    
    [317 rows x 7 columns]



```python
# concat()으로 연결
billboard_loop_concat = pd.concat(list_billboard_df)
print(billboard_loop_concat.shape)
```

    (24092, 7)


### 리스트 컴프리헨션으로 여러 개 파일 불러오기
- 파이썬에는 리스트 컴프리헨션이라 부르는 구문이 있는데, 이 구문은 루프를 돌면서 항목을 리스트에 추가한다.


```python
# CSV 파일을 리스트 컴프리헨션으로 대체하여 데이터프레임으로 저장
billboard_data_files = (
    Path(".")
    .glob("./data/billboard_by_week/billboard-*.csv")
)

list_billboard_df = []
for csv_filename in billboard_data_files:
    df = pd.read_csv(csv_filename)
    list_billboard_df.append(df)

billboard_data_files = (
    Path(".")
    .glob("./data/billboard_by_week/billboard-*.csv")
)

billboard_dfs = [pd.read_csv(data) for data in billboard_data_files] # 루프를 돌며 리스트를 만드는 리스트 컴프리헨션
```


```python
# 데이터 유형 확인
print(type(billboard_dfs))
```

    <class 'list'>



```python
print(len(billboard_dfs))
```

    76



```python
# concat()으로 데이터 연결
billboard_concat_comp = pd.concat(billboard_dfs)
print(billboard_concat_comp)
```

         year            artist                    track  time date.entered  week  \
    0    2000             2 Pac  Baby Don't Cry (Keep...  4:22   2000-02-26  wk15   
    1    2000           2Ge+her  The Hardest Part Of ...  3:15   2000-09-02  wk15   
    2    2000      3 Doors Down               Kryptonite  3:53   2000-04-08  wk15   
    3    2000      3 Doors Down                    Loser  4:24   2000-10-21  wk15   
    4    2000          504 Boyz            Wobble Wobble  3:35   2000-04-15  wk15   
    ..    ...               ...                      ...   ...          ...   ...   
    312  2000       Yankee Grey     Another Nine Minutes  3:10   2000-04-29  wk18   
    313  2000  Yearwood, Trisha          Real Live Woman  3:55   2000-04-01  wk18   
    314  2000   Ying Yang Twins  Whistle While You Tw...  4:19   2000-03-18  wk18   
    315  2000     Zombie Nation            Kernkraft 400  3:30   2000-09-02  wk18   
    316  2000   matchbox twenty                     Bent  4:12   2000-04-29  wk18   
    
         rating  
    0       NaN  
    1       NaN  
    2      38.0  
    3      72.0  
    4      78.0  
    ..      ...  
    312     NaN  
    313     NaN  
    314     NaN  
    315     NaN  
    316     3.0  
    
    [24092 rows x 7 columns]


