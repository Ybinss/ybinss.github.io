---
layout: post
title:  "[Python] 그래프 그리기" 
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

# I. 데이터 시각화란?
- 시각화를 설명할 때 자주 사용하는 데이터셋 예제는 앤스컴 콰르텟이다. 이 데이터셋은 통계 그래프의 중요성을 강조하고자 영국의 통계학자 프랭크 앤스컴이 만들었다.
- 앤스컴 콰르텟은 4개의 데이터셋으로 구성되며 각 데이터셋에는 2개의 연속 변수가 있다. 4개 데이터셋은 평균, 분산, 상관관계, 회귀선이 모두 같다. 따라서 기술 통계만 보면 마치 같은 데이터셋처럼 보일 수 있다. 하지만 이를 시각화하면 4가지 모두 경향이 다르다는 사실을 직관적으로 알 수 있다. 이런 점에서 시각화는 데이터 분석에서 아주 중요한 요소라고 할 수 있다.


```python
# 데이터셋 불러오고 anscombe에 저장
import seaborn as sns

anscombe = sns.load_dataset("anscombe")
print(anscombe)
```

       dataset     x      y
    0        I  10.0   8.04
    1        I   8.0   6.95
    2        I  13.0   7.58
    3        I   9.0   8.81
    4        I  11.0   8.33
    5        I  14.0   9.96
    6        I   6.0   7.24
    7        I   4.0   4.26
    8        I  12.0  10.84
    9        I   7.0   4.82
    10       I   5.0   5.68
    11      II  10.0   9.14
    12      II   8.0   8.14
    13      II  13.0   8.74
    14      II   9.0   8.77
    15      II  11.0   9.26
    16      II  14.0   8.10
    17      II   6.0   6.13
    18      II   4.0   3.10
    19      II  12.0   9.13
    20      II   7.0   7.26
    21      II   5.0   4.74
    22     III  10.0   7.46
    23     III   8.0   6.77
    24     III  13.0  12.74
    25     III   9.0   7.11
    26     III  11.0   7.81
    27     III  14.0   8.84
    28     III   6.0   6.08
    29     III   4.0   5.39
    30     III  12.0   8.15
    31     III   7.0   6.42
    32     III   5.0   5.73
    33      IV   8.0   6.58
    34      IV   8.0   5.76
    35      IV   8.0   7.71
    36      IV   8.0   8.84
    37      IV   8.0   8.47
    38      IV   8.0   7.04
    39      IV   8.0   5.25
    40      IV  19.0  12.50
    41      IV   8.0   5.56
    42      IV   8.0   7.91
    43      IV   8.0   6.89


# II. matplotlib 라이브러리란?
- matplotlib는 널리 사용하는 파이썬 시각화 라이브러리이다. 이 라이브러리는 매우 유연하므로 사용자가 그래프의 모든 요소를 제어할 수 있다.
- 하위 패키지 pyplot을 불러오면 라이브러리의 다양한 시각화 기능을 사용할 수 있다. 여기서는 matplotlib.pyplot을 불러와 plt라는 별칭을 부여한다.


```python
import matplotlib.pyplot as plt
```

- 대부분의 기본 그래프는 `plt.plot()`을 호출하면 그릴 수 있다.


```python
# 앤스컴 콰르텟 데이터셋에서 dataset 열의 값이 I인 데이터를 추출하여 그래프 그리기
dataset_1 = anscombe[anscombe['dataset'] == 'I']
plt.plot(dataset_1['x'], dataset_1['y'])
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_8_0.png)
    



```python
# 점 그래프 그리기
plt.plot(dataset_1['x'], dataset_1['y'], 'o')
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_9_0.png)
    



```python
# 데이터셋 구분
dataset_2 = anscombe[anscombe['dataset'] == 'II']
dataset_3 = anscombe[anscombe['dataset'] == 'III']
dataset_4 = anscombe[anscombe['dataset'] == 'IV']
```

## II-1. 그림 영역과 하위 그래프 이해하기
- matplotlib은 여러 개의 하위 그래프(Axes 객체)를 하나의 그림 영역(Figure 객체)에 그리는 기능을 제공한다.
- 그림 영역게 하위 그래프를 그릴 때는 다음 세 가지 매개변수가 필요하다.
    + 그림 영역의 하위 그래프 행 개수
    + 그림 영역의 하위 그래프 열 개수
    + 하위 그래프 위치

### 한 번에 4개의 그래프 그리기


```python
fig = plt.figure()
axes1 = fig.add_subplot(2, 2, 1)
axes2 = fig.add_subplot(2, 2, 2)
axes3 = fig.add_subplot(2, 2, 3)
axes4 = fig.add_subplot(2, 2, 4)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_13_0.png)
    



```python
fig = plt.figure()
axes1 = fig.add_subplot(2, 2, 1)
axes2 = fig.add_subplot(2, 2, 2)
axes3 = fig.add_subplot(2, 2, 3)
axes4 = fig.add_subplot(2, 2, 4)

axes1.plot(dataset_1['x'], dataset_1['y'], 'o')
axes2.plot(dataset_2['x'], dataset_2['y'], 'o')
axes3.plot(dataset_3['x'], dataset_3['y'], 'o')
axes4.plot(dataset_4['x'], dataset_4['y'], 'o')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_14_0.png)
    



```python
# 그림 영역과 각 하위 그래프에 제목 달기
fig = plt.figure()
axes1 = fig.add_subplot(2, 2, 1)
axes2 = fig.add_subplot(2, 2, 2)
axes3 = fig.add_subplot(2, 2, 3)
axes4 = fig.add_subplot(2, 2, 4)

axes1.plot(dataset_1['x'], dataset_1['y'], 'o')
axes2.plot(dataset_2['x'], dataset_2['y'], 'o')
axes3.plot(dataset_3['x'], dataset_3['y'], 'o')
axes4.plot(dataset_4['x'], dataset_4['y'], 'o')

axes1.set_title("dataset_1")
axes2.set_title("dataset_2")
axes3.set_title("dataset_3")
axes4.set_title("dataset_4")

fig.suptitle("Anscombe Data")
fig.set_tight_layout(True)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_15_0.png)
    


## II-2. 그래프 구성 요소 이해하기

<img src = "./fig/matplotlib.png" width = "80%">

matplotlib 그래프 구성 요소(출처: <https://matplotlib.org/stable/gallery/showcase/anatomy.html>)

- 각 축을 나타내는 Axis와 이 축을 모은 복수형 Axes는 서로 다르다. 즉, anscombe 예제에서 살펴본 하위 그래프가 Axes이며 각 그래프는 x축 또는 y축인 Axis를 포함한다. 그리고 Axes 4개가 모여 하나의 그림 영역인 Figure를 완성한다.

# III. matplotlib으로 그래프 그리기


```python
# tips 데이터셋 불러오기
tips = sns.load_dataset("tips")
print(tips)
```

         total_bill   tip     sex smoker   day    time  size
    0         16.99  1.01  Female     No   Sun  Dinner     2
    1         10.34  1.66    Male     No   Sun  Dinner     3
    2         21.01  3.50    Male     No   Sun  Dinner     3
    3         23.68  3.31    Male     No   Sun  Dinner     2
    4         24.59  3.61  Female     No   Sun  Dinner     4
    ..          ...   ...     ...    ...   ...     ...   ...
    239       29.03  5.92    Male     No   Sat  Dinner     3
    240       27.18  2.00  Female    Yes   Sat  Dinner     2
    241       22.67  2.00    Male    Yes   Sat  Dinner     2
    242       17.82  1.75    Male     No   Sat  Dinner     2
    243       18.78  3.00  Female     No  Thur  Dinner     2
    
    [244 rows x 7 columns]


## III-1. 일변량 그래프 그리기
- 통계 용어로 변수가 1개일 때 이를 '일변량'이라고 한다. 즉, 일변량 그래프는 변수 하나를 나타낸 그래프이다.

### 히스토그램 그리기
- 히스토그램은 하나의 변수를 시각화하는 가장 일반적인 방법이다. 값을 일정 구간별로 묶어서 해당 구간의 데이터 개수를 빈도로 표시하는 그래프로, 변수의 분포를 나타낸다.


```python
# seaborn의 tips 데이터셋 로드
tips = sns.load_dataset("tips")

# Figure 및 Axes 생성
fig = plt.figure()
axes1 = fig.add_subplot(1, 1, 1)

# 히스토그램 생성
axes1.hist(tips['total_bill'], bins = 10)

# 제목 및 축 레이블 설정
axes1.set_title('Histogram of Total Bill')
axes1.set_xlabel('Total Bill')
axes1.set_ylabel('Frequency')

plt.show()

```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_23_0.png)
    


## III-2. 이변량 그래프 그리기
- 통계 용어로 변수 2개를 '이변량'이라고 한다.

### 산점도 그래프 그리기
- 산점도는 연속 변수 사이의 관계를 나타내는 그래프이다.


```python
scatter_plot = plt.figure()
axes1 = scatter_plot.add_subplot(1, 1, 1)

axes1.scatter(tips['total_bill'], tips['tip'])

axes1.set_title('Scatterplot of Total Bill vs Tip')
axes1.set_xlabel('Total Bill')
axes1.set_ylabel('Tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_26_0.png)
    


### 박스 그래프 그리기
- 박스 그래프는 이산 변수와 연속 변수 사이의 관계를 나타낸다.


```python
boxplot = plt.figure()
axes1 = boxplot.add_subplot(1, 1, 1)

axes1.boxplot(
    x = [
        tips[tips['sex'] == 'Female']['tip'],
        tips[tips['sex'] == 'Male']['tip']
    ], # 매개변수 x에 리스트로 데이터를 전달
    labels = ['Female', 'Male']
)

axes1.set_xlabel('Sex')
axes1.set_ylabel('Tip')
axes1.set_title('Boxplot of Tips by Sex')

plt.show()
```

    /var/folders/52/9g__g13d4zjgvrl6vdg9_ln80000gn/T/ipykernel_3457/1998778003.py:4: MatplotlibDeprecationWarning: The 'labels' parameter of boxplot() has been renamed 'tick_labels' since Matplotlib 3.9; support for the old name will be dropped in 3.11.
      axes1.boxplot(



    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_28_1.png)
    


## III-3. 다변량 그래프 그리기

### 변수가 여러개인 그래프 그리기


```python
# 색상 매핑 설정
colors = {"Female": "#f1a340", "Male": "#998ec3"}

# Figure 및 Axes 생성
scatter_plot = plt.figure()
axes1 = scatter_plot.add_subplot(1, 1, 1)

# 산점도 생성
axes1.scatter(data = tips,
              x = 'total_bill',
              y = 'tip',
              s = tips['size']**2*10,
              c = tips['sex'].map(colors),
              alpha = 0.5)

# 그래프 제목 및 축 레이블 설정
axes1.set_title('Colored by Sex and Sized by Size')
axes1.set_xlabel('Total Bill')
axes1.set_ylabel('Tip')

scatter_plot.suptitle('Total Bill vs Tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_31_0.png)
    


# IV. seaborn으로 그래프 그리기
- matplotlib는 파이썬의 핵심 시각화 도구이며 seaborn은 matplotlib에 기반을 둔 통계 그래프에 특화된 라이브러리이다.


```python
# tips 데이터셋 불러오기
import seaborn as sns

tips = sns.load_dataset("tips")
```

- seaborn의 공식 [사이트](https://seaborn.pydata.org)를 방문하면 seaborn의 시각화 함수와 API 문서를 확인할 수 있다.


```python
# 결과 그래프의 글자 크기, 선 굵기, 축 눈금 크기 등 그래프의 전반적인 크기 조정
sns.set_context("paper")
```

## IV-1. 다양한 그래프 그려보기

### 일변량 그래프 그리기


```python
# 히스토그램 그리기
hist, ax = plt.subplots()

sns.histplot(data = tips, x = 'total_bill', ax = ax)
ax.set_title('Total Bill Histogram')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_38_0.png)
    



```python
# 밀도 분포 그래프 그리기
den, ax = plt.subplots()

sns.kdeplot(data = tips, x = 'total_bill', ax = ax)

ax.set_title('Total Bill Density')
ax.set_xlabel('Total Bill')
ax.set_ylabel('Unit Probability')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_39_0.png)
    


> - 밀도 분포 그래프는 히스토그램과 같이 일변량 분포를 시각화하며, 커널 밀도 추정 그래프라고도 한다.


```python
# 러그 그래프 그리기
rug, ax = plt.subplots()

sns.rugplot(data = tips, x = 'total_bill', ax = ax)
sns.histplot(data = tips, x = 'total_bill', ax = ax)

ax.set_title('Rug Plot and Histogram of Total Bill')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_41_0.png)
    


> - 러그 그래프는 변수 분포를 1차원으로 나타내며, 일반적으로 다른 유형의 그래프에 추가 정보를 제공할 때 사용한다.


```python
# 분포 그래프 그리기
fig = sns.displot(data = tips, x = 'total_bill', kde = True, rug = True)

fig.set_axis_labels(x_var = 'Total Bill', y_var = 'Count')
fig.figure.suptitle('Distribution of Total Bill')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_43_0.png)
    


> - `sns.displot()` 함수를 사용하면 여러 개의 일변량 그래프를 하나의 그래프로 표현할 수 있다.


```python
# 막대 그래프 그리기
count, ax = plt.subplots()

sns.countplot(data = tips, x = 'day', palette = 'viridis', ax = ax)

ax.set_title('Count of days')
ax.set_xlabel('Day of the Week')
ax.set_ylabel('Frequency')

plt.show()
```

    /var/folders/52/9g__g13d4zjgvrl6vdg9_ln80000gn/T/ipykernel_3457/2271171005.py:4: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `x` variable to `hue` and set `legend=False` for the same effect.
    
      sns.countplot(data = tips, x = 'day', palette = 'viridis', ax = ax)



    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_45_1.png)
    


> - 막대그래프는 히스토그램과 비슷하지만 구간별로 나누어 분포를 표현하지 않고 막대로 이산변수의 개수를 표현한다는 데 차이가 있다.

### 이변량 그래프 그리기


```python
# 산점도 그래프 그리기①
scatter, ax = plt.subplots()

sns.scatterplot(data = tips, x = 'total_bill', y = 'tip', ax = ax)

ax.set_title('Scatter Plot of Total Bill and Tip')
ax.set_xlabel('Total Bill')
ax.set_ylabel('Tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_48_0.png)
    



```python
# 산점도 그래프 그리기②
reg, ax = plt.subplots()

sns.regplot(data = tips, x = 'total_bill', y = 'tip', ax = ax)

ax.set_title('Scatter Plot of Total Bill and Tip')
ax.set_xlabel('Total Bill')
ax.set_ylabel('Tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_49_0.png)
    



```python
# 산점도 그래프 그리기③
fig = sns.lmplot(data = tips, x = 'total_bill', y = 'tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_50_0.png)
    



```python
# 조인트 그래프 그리기
joint = sns.jointplot(data = tips, x = 'total_bill', y = 'tip')
joint.set_axis_labels(xlabel = 'Total Bill', ylabel = 'Tip')

joint.figure.suptitle('Joint Plot of Total Bill and Tip', y = 1.03)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_51_0.png)
    



```python
# 육각 그래프 그리기
hexbin = sns.jointplot(data = tips, x = "total_bill", y = "tip", kind = "hex")

hexbin.set_axis_labels(xlabel = 'Total Bill', ylabel = 'Tip')
hexbin.figure.suptitle('Hexbin Joint Plot of Total Bill and Tip', y = 1.03)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_52_0.png)
    



```python
# 2차원 밀도 분포 그래프 그리기
kde, ax = plt.subplots()

sns.kdeplot(data = tips, x = "total_bill", y = "tip", fill = True, ax = ax)

ax.set_title('Kernel Density Plot of Total Bill and Tip')
ax.set_xlabel('Total Bill')
ax.set_ylabel('Tip')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_53_0.png)
    



```python
# sns.jointplot()으로 밀도 분포 그래프 그리기
kde2d = sns.jointplot(data = tips, x = "total_bill", y = "tip", kind = "kde")

kde2d.set_axis_labels(xlabel = 'Total Bill', ylabel = 'Tip')
kde2d.figure.suptitle('Hexbin Joint Plot of Total Bill and Tip', y = 1.03)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_54_0.png)
    



```python
# 막대그래프 그리기
import numpy as np

bar, ax = plt.subplots()

sns.barplot(data = tips, x = "time", y = "total_bill", estimator = np.mean, ax = ax)

ax.set_title('Bar Plot of Average Total Bill for Time of Day')
ax.set_xlabel('Time of Day')
ax.set_ylabel('Average Total Bill')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_55_0.png)
    



```python
# 박스 그래프 그리기
box, ax = plt.subplots()

sns.boxplot(data = tips, x = 'time', y = 'total_bill', ax = ax)

ax.set_title('Box Plot of Average Total Bill for Time of Day')
ax.set_xlabel('Time of Day')
ax.set_ylabel('Average Total Bill')

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_56_0.png)
    



```python
# 바이올린 그래프 그리기
viloin, ax = plt.subplots()

sns.violinplot(data = tips, x = 'time', y = 'total_bill', ax = ax)

ax.set_title('Violin Plot of Average Total Bill for Time of Day')
ax.set_xlabel('Time of Day')
ax.set_ylabel('Total Bill')

plt.show()

```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_57_0.png)
    



```python
# 박스 그래프와 바이올린 그래프 함께 그리기
box_violin, (ax1, ax2) = plt.subplots(nrows = 1, ncols = 2)

sns.boxplot(data = tips, x = 'time', y = 'total_bill', ax = ax1)
sns.violinplot(data = tips, x = 'time', y = 'total_bill', ax = ax2)

ax1.set_title('Box Plot')
ax1.set_xlabel('Time of Day')
ax1.set_ylabel('Total Bill')

ax2.set_title('Violin Plot')
ax2.set_xlabel('Time of Day')
ax2.set_ylabel('Total Bill')

box_violin.suptitle("Comparison of Box Plot with Violin Plot")

box_violin.set_tight_layout(True)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_58_0.png)
    



```python
# 관계 그래프 그리기①
fig = sns.pairplot(data = tips)

fig.figure.suptitle('Pairwise Relationships of the Tips Data', y = 1.03)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_59_0.png)
    



```python
# 관계 그래프 그리기②
pair_grid = sns.PairGrid(tips, diag_sharey = False)

pair_grid = pair_grid.map_upper(sns.regplot)
pair_grid = pair_grid.map_lower(sns.kdeplot)
pair_grid = pair_grid.map_diag(sns.histplot)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_60_0.png)
    


### 다변량 그래프 그리기


```python
# 그래프에 색상 입히기
violin, ax = plt.subplots()

sns.violinplot(data = tips,
               x = "time",
               y = "total_bill",
               hue = "smoker", # 색으로 구분할 변수는 hue에 지정
               split = True,
               palette = "viridis",
               ax = ax)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_62_0.png)
    



```python
scatter = sns.lmplot(data = tips,
               x = "total_bill",
               y = "tip",
               hue = "smoker", # 색으로 구분할 변수는 hue에 지정
               fit_reg = False,
               palette = "viridis",)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_63_0.png)
    



```python
fig = sns.pairplot(tips, hue = "time", palette = "viridis")
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_64_0.png)
    



```python
# 그래프 크기와 모양 조절하기
fig, ax = plt.subplots()

sns.scatterplot(data = tips,
               x = "total_bill",
               y = "tip",
               hue = "smoker", # 색으로 구분할 변수는 hue에 지정
               size = "size", 
               palette = "viridis",
               ax = ax)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_65_0.png)
    


### 그래프 나눠 그리기
- 데이터를 추출하고 그림 영역을 만들어 하위 그래프를 그리는 대신 seaborn의 패싯 기능을 사용하면 그래프를 손쉽게 나눠서 그릴 수 있다.
- 패싯을 사용하려면 데이터셋이 이른바 깔끔한 데이터여야 한다.


```python
# 1개의 범주형 변수로 그래프 나누기
anscombe = sns.load_dataset("anscombe")

anscombe_plot = sns.relplot(data = anscombe,
                            x = "x",
                            y = "y",
                            kind = "scatter",
                            col = "dataset",
                            col_wrap = 2,
                            height = 2,
                            aspect = 1.6)

anscombe_plot.figure.set_tight_layout(True)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_67_0.png)
    



```python
# 여러 개의 범주형 변수로 그래프 나누기
colors = {
    "Yes": "#f1a340",
    "No": "#998ec3",
}

facet2 = sns.relplot(data = tips,
                     x = "total_bill",
                     y = "tip",
                     hue = "smoker",
                     style = "sex",
                     kind = "scatter",
                     col = "day",
                     row = "time",
                     palette = colors,
                     height = 1.7)

# 나눠진 각 그래프에 제목을 붙이기
facet2.set_titles(row_template = "{row_name}", col_template = "{col_name}")

# 그래프에 범주를 추가하는 코드
sns.move_legend(facet2,
                loc = "lower center",
                bbox_to_anchor = (0.5, 1),
                ncol = 2, # 범주의 열 개수
                title = None, # 범주 제목
                frameon = False) # 범주 테두리를 숨김

facet2.figure.set_tight_layout(True)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_68_0.png)
    



```python
# FacetGrid 객체로 그래프 직접 나누기
facet = sns.FacetGrid(tips, col = 'time')

facet.map(sns.histplot, 'total_bill')
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_69_0.png)
    



```python
facet = sns.FacetGrid(tips,
                      col = 'day',
                      col_wrap = 2,
                      hue = 'sex',
                      palette = "viridis")

facet.map(plt.scatter, 'total_bill', 'tip')
facet.add_legend()
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_70_0.png)
    



```python
facet = sns.FacetGrid(tips,
                      col = 'time',
                      row = 'smoker',
                      hue = 'sex',
                      palette = "viridis")

facet.map(plt.scatter, 'total_bill', 'tip')
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_71_0.png)
    



```python
facet = sns.catplot(x = "day",
                    y = "total_bill",
                    hue = "sex",
                    data = tips,
                    row = "smoker",
                    col = "time",
                    kind = "violin",
                    height = 3)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_72_0.png)
    


## IV-2. seaborn 스타일 알아보기

### 스타일 알아보기


```python
fig, ax = plt.subplots()
sns.violinplot(data = tips,
               x = "time",
               y = "total_bill",
               hue = "sex",
               split = True,
               ax = ax)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_75_0.png)
    



```python
# 특정 그래프만 다른 스타일로 그리기
with sns.axes_style("darkgrid"):
    fig, ax = plt.subplots()
    sns.violinplot(data = tips,
                   x = "time",
                   y = "total_bill",
                   hue = "sex",
                   split = True,
                   ax = ax)

plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_76_0.png)
    



```python
# 그 밖의 스타일
seaborn_styles = ["darkgrid", "whitegrid", "dark", "white", "ticks"]

fig = plt.figure()
for idx, style in enumerate(seaborn_styles):
    plot_position = idx + 1
    with sns.axes_style(style):
        ax = fig.add_subplot(2, 3, plot_position)
        violin = sns.violinplot(data = tips, x = "time", y = "total_bill", ax = ax)
        violin.set_title(style)

fig.set_tight_layout(True)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_77_0.png)
    


### 그래프 컨텍스트 설정하기


```python
import pandas as pd

contexts = pd.DataFrame({
    "paper": sns.plotting_context("paper"),
    "notebook": sns.plotting_context("notebook"),
    "talk": sns.plotting_context("talk"),
    "poster": sns.plotting_context("poster"),
})
print(contexts)
```

                           paper  notebook    talk  poster
    axes.linewidth           1.0      1.25   1.875     2.5
    grid.linewidth           0.8      1.00   1.500     2.0
    lines.linewidth          1.2      1.50   2.250     3.0
    lines.markersize         4.8      6.00   9.000    12.0
    patch.linewidth          0.8      1.00   1.500     2.0
    xtick.major.width        1.0      1.25   1.875     2.5
    ytick.major.width        1.0      1.25   1.875     2.5
    xtick.minor.width        0.8      1.00   1.500     2.0
    ytick.minor.width        0.8      1.00   1.500     2.0
    xtick.major.size         4.8      6.00   9.000    12.0
    ytick.major.size         4.8      6.00   9.000    12.0
    xtick.minor.size         3.2      4.00   6.000     8.0
    ytick.minor.size         3.2      4.00   6.000     8.0
    font.size                9.6     12.00  18.000    24.0
    axes.labelsize           9.6     12.00  18.000    24.0
    axes.titlesize           9.6     12.00  18.000    24.0
    xtick.labelsize          8.8     11.00  16.500    22.0
    ytick.labelsize          8.8     11.00  16.500    22.0
    legend.fontsize          8.8     11.00  16.500    22.0
    legend.title_fontsize    9.6     12.00  18.000    24.0



```python
# 각 컨테스트를 적용한 그래프
context_styles = contexts.columns

fig = plt.figure()

for idx, context in enumerate(context_styles):
    plot_position = idx + 1
    with sns.plotting_context(context):
        ax = fig.add_subplot(2, 2, plot_position)
        violin = sns.violinplot(data = tips, x = "time", y = "total_bill", ax = ax)
        violin.set_title(context)

fig.set_tight_layout(True)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_80_0.png)
    


## IV-3. seaborn 공식 문서를 읽는 방법


```python
# 박스 그래프와 바이올린 그래프를 비교하는 예제
box_violin, (ax1, ax2) = plt.subplots(nrows = 1, ncols = 2)

sns.boxplot(data = tips, x = 'time', y = 'total_bill', ax = ax1)
sns.violinplot(data = tips, x = 'time', y = 'total_bill', ax = ax2)

ax1.set_title('Box Plot')
ax1.set_xlabel('Time of Day')
ax1.set_ylabel('Total Bill')

ax2.set_title('Violin plot')
ax2.set_xlabel('Time of day')
ax2.set_ylabel('Total Bill')

box_violin.suptitle("Comparison of Box Plot with Violin Plot")

box_violin.set_tight_layout(True)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_82_0.png)
    



```python
# 관계 그래프 예제
fig = sns.pairplot(data = tips)
fig.figure.suptitle('Pairwise Relationships of the Tips Data', y = 1.03)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_83_0.png)
    


# V. 판다스로 그래프 그리기

### 판다스로 다양한 그래프 그리기


```python
# 히스토그램 그리기
fig, ax = plt.subplots()
tips['total_bill'].plot.hist(ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_86_0.png)
    



```python
# total_bill, tip 열 추출
fig, ax = plt.subplots()
tips[['total_bill', 'tip']].plot.hist(alpha = 0.5, bins = 20, ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_87_0.png)
    



```python
# 밀도 분포 그래프 그리기
fig, ax = plt.subplots()
tips['tip'].plot.kde(ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_88_0.png)
    



```python
# 산점도 그리기
fig, ax = plt.subplots()
tips.plot.scatter(x = 'total_bill', y = 'tip', ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_89_0.png)
    



```python
# 육각 그래프 그리기
fig, ax = plt.subplots()
tips.plot.hexbin(x = 'total_bill', y = 'tip', ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_90_0.png)
    



```python
fig, ax = plt.subplots()
tips.plot.hexbin(x = 'total_bill', y = 'tip', gridsize = 10, ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_91_0.png)
    



```python
# 박스 그래프 그리기
fig, ax = plt.subplots()
tips.plot.box(ax = ax)
plt.show()
```


    
![png](4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_files/4.%20%EA%B7%B8%EB%9E%98%ED%94%84%20%EA%B7%B8%EB%A6%AC%EA%B8%B0_92_0.png)
    



```python

```
