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

# I. 자료형 살펴보기


```python
# 라이브러리와 데이터셋 불러오기
import pandas as pd
import seaborn as sns

tips = sns.load_dataset("tips")
```


```python
# 자료형 확인
print(tips.dtypes)
```

    total_bill     float64
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    dtype: object


> - tips데이터셋에는 int64형, float64형, category형 데이터가 있다.
>   + int64 : 소수점이 없는 숫자
>   + float64 : 소수점이 있는 수자
>   + category : 범주형 변수

# II. 자료형 변환하기
- 저장한 자료형에 따라 해당 열의 데이터에 적용할 수 있는 함수나 계산이 달라진다. 따라서 특정 열에 적용하고 싶은 연산과 자료형이 맞지 않다면 자료형을 변환하는 방법을 알아야 한다.

### 문자열로 변환하기
- tips 데이터셋을 보면 sex, smoker, day, time 변수는 category 형이다. 일반적으로는 변수가 숫자형이 아니라면 문자열로 취급하는 것이 좋다. 하지만 category형을 사용하면 문자열보다 성능이 좋다.


```python
# astype() 메서드에 'str'을 전달하여 자료형 변환
tips['sex_str'] = tips['sex'].astype('str')
print(tips.dtypes)
```

    total_bill     float64
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object


### 숫자로 변환하기
- `astype()` 메서드는 데이터프레임의 열을 대상으로 호출할 수 있으며 원하는 dtype으로 변환할 때 사용하는 메서드이다.


```python
# float64형의 total_bill 열을 object형으로 변환
tips['total_bill'] = tips['total_bill'].astype('str')
print(tips.dtypes)
```

    total_bill      object
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object



```python
# 원래 자료형인 float64형으로 되돌리기
tips['total_bill'] = tips['total_bill'].astype('float')
print(tips.dtypes)
```

    total_bill     float64
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object


### 숫자형으로 변환하는 to_numeric() 함수 사용하기
- 변수를 int나 float와 같은 숫자형으로 변환할 때는 판다스의 `to_numeric()` 함수를 사용할 수도 있다.


```python
# tips 데이터셋의 첫 10개 행을 추출하여 tips_sub_miss에 저장
tips_sub_miss = tips.head(10).copy()
# tota_bill 열의 몇 개 값을 'missing'으로 바꾸기
tips_sub_miss.loc[[1, 3, 5, 7], 'total_bill'] = 'missing'

print(tips_sub_miss)
```

      total_bill   tip     sex smoker  day    time  size sex_str
    0      16.99  1.01  Female     No  Sun  Dinner     2  Female
    1    missing  1.66    Male     No  Sun  Dinner     3    Male
    2      21.01  3.50    Male     No  Sun  Dinner     3    Male
    3    missing  3.31    Male     No  Sun  Dinner     2    Male
    4      24.59  3.61  Female     No  Sun  Dinner     4  Female
    5    missing  4.71    Male     No  Sun  Dinner     4    Male
    6       8.77  2.00    Male     No  Sun  Dinner     2    Male
    7    missing  3.12    Male     No  Sun  Dinner     4    Male
    8      15.04  1.96    Male     No  Sun  Dinner     2    Male
    9      14.78  3.23    Male     No  Sun  Dinner     2    Male


    /var/folders/52/9g__g13d4zjgvrl6vdg9_ln80000gn/T/ipykernel_2760/1034073615.py:4: FutureWarning: Setting an item of incompatible dtype is deprecated and will raise an error in a future version of pandas. Value 'missing' has dtype incompatible with float64, please explicitly cast to a compatible dtype first.
      tips_sub_miss.loc[[1, 3, 5, 7], 'total_bill'] = 'missing'



```python
# 자료형 확인
print(tips_sub_miss.dtypes)
```

    total_bill      object
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object


- `to_numeric()` 함수에는 숫자로 변환할 수 없는 값이 있을 때 대처하는 방법을 설정할 수 있는 매개변수 errors가 있다. 기본값은 raise로, 이것은 숫자로 변환할 수 없는 값을 발견하면 오류가 발생한다는 뜻이다.   
- 매개변수 errors에 설정할 수 있는 값은 다음과 같다.
    + 'raise': 변환할 수 없는 값이 있을 때 오류를 발생시킨다.
    + 'coerce': 변환할 수 없는 값이 있을 때 NaN으로 값을 설정한다.
    + 'ignore': 변환할 수 없는 값이 있을 때 해당 값을 그대로 사용한다.


```python
# errors에 'ignore'을 인수로 전달
tips_sub_miss['total_bill'] = pd.to_numeric(tips_sub_miss['total_bill'],
                                            errors = 'ignore')
print(tips_sub_miss)
```

      total_bill   tip     sex smoker  day    time  size sex_str
    0      16.99  1.01  Female     No  Sun  Dinner     2  Female
    1    missing  1.66    Male     No  Sun  Dinner     3    Male
    2      21.01  3.50    Male     No  Sun  Dinner     3    Male
    3    missing  3.31    Male     No  Sun  Dinner     2    Male
    4      24.59  3.61  Female     No  Sun  Dinner     4  Female
    5    missing  4.71    Male     No  Sun  Dinner     4    Male
    6       8.77  2.00    Male     No  Sun  Dinner     2    Male
    7    missing  3.12    Male     No  Sun  Dinner     4    Male
    8      15.04  1.96    Male     No  Sun  Dinner     2    Male
    9      14.78  3.23    Male     No  Sun  Dinner     2    Male


    /var/folders/52/9g__g13d4zjgvrl6vdg9_ln80000gn/T/ipykernel_2760/2243345418.py:2: FutureWarning: errors='ignore' is deprecated and will raise in a future version. Use to_numeric without passing `errors` and catch exceptions explicitly instead
      tips_sub_miss['total_bill'] = pd.to_numeric(tips_sub_miss['total_bill'],


> - 오류가 발생하지 않지만 열의 자료형도 변하지 않는다.


```python
print(tips_sub_miss.dtypes)
```

    total_bill      object
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object



```python
# 'coerce'를 인수로 전달
tips_sub_miss['total_bill'] = pd.to_numeric(tips_sub_miss['total_bill'],
                                            errors = 'coerce')
print(tips_sub_miss)
```

       total_bill   tip     sex smoker  day    time  size sex_str
    0       16.99  1.01  Female     No  Sun  Dinner     2  Female
    1         NaN  1.66    Male     No  Sun  Dinner     3    Male
    2       21.01  3.50    Male     No  Sun  Dinner     3    Male
    3         NaN  3.31    Male     No  Sun  Dinner     2    Male
    4       24.59  3.61  Female     No  Sun  Dinner     4  Female
    5         NaN  4.71    Male     No  Sun  Dinner     4    Male
    6        8.77  2.00    Male     No  Sun  Dinner     2    Male
    7         NaN  3.12    Male     No  Sun  Dinner     4    Male
    8       15.04  1.96    Male     No  Sun  Dinner     2    Male
    9       14.78  3.23    Male     No  Sun  Dinner     2    Male


> - 'missing' 문자열이 모두 NaN으로 대체되고 total_bill의 자료형이 float64형으로 바뀐다.


```python
print(tips_sub_miss.dtypes)
```

    total_bill     float64
    tip            float64
    sex           category
    smoker        category
    day           category
    time          category
    size             int64
    sex_str         object
    dtype: object





