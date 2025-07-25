---
layout: post
title:  "[Andrew Ng 강의 정리] 이진 분류, 로지스틱 회귀" 
#subtitle: programmers
tags: [Deep Learning, Logistic Regression]
categories: Deep_Learning
---

# Binary Classification

> **이진 분류(Binary Classfication)**의 기본 개념과 이를 위한 **로지스틱 회귀(Logistic Regression)** 알고리즘을 소개한다.
> 특히, 신경망 구현 시 효율적인 데이터 처리를 위해 **훈련 세트(Training Set)**를 **명시적인 for 루프 없이 처리**하고, **순전파(Forward Propagation)**와 **역전파(Backward Propagation)**의 중요성을 강조하며, 데이터를 행렬 형태로 구성하는 표기법을 설명한다.

## I. 신경망의 기본 개념 소개

- 이번 강의에서는 **신경망**의 기본 원리와 중요한 구현 기술들을 설명한다.
- 특히, 전체 훈련 데이터를 효율적으로 처리하는 방법과, 학습 과정에서의 순전파와 역전파의 역할을 다룬다.
- 이 내용을 이해하기 위해 **로지스틱 회귀**를 예시로 사용한다.

## II. 이진 분류 문제와 데이터 표현 방법

- **이진 분류**는 두 가지 결과를 구분하는 알고리즘이다.
    + 예) 이미지를 보고 '고양이'인지 '비고양이'인지 판단하는 문제
- 입력 데이터는 이미지이며, 컴퓨터에 저장하는 방법은 RGB 채널별로 **3개의 행렬**로 표현된다.
- 만약 이미지 크기가 64×64이면, 각각의 채널은 64×64 행렬이고, 전체 픽셀 값들을 하나의 긴 벡터로 펼친다.
    + 이 경우, 벡터의 차원은 64×64×3 = 12888
- 목표는 이 벡터를 입력으로 받아서, '1'이면 고양이 '0'이면 비고양이로 분류하는 것이다.

## III. 데이터 구조와 표기법

- 훈련 데이터는 **여러 개의 샘플**로 구성된다.
- 각각의 샘플은 입력 벡터 X와 레이블 Y로 이루어진 쌍이다.
- 전체 훈련 세트는 M개 샘플로 구성되며, 각각은 $(X_1, Y_1), (X_2, Y_2),..., (X_M, Y_M)$이다.
- X는 N차원 벡터를 M개 열로 쌓은 **행렬**로 표현하며, 크기는 N × M이다.
- Y는 1 × M 크기의 행렬로, 각 열이 하나의 샘플 **레이블**이다.
- 이렇게 표기하면, 신경망 구현이 훨씬 간단해지고 효율적이다.

출처 :<https://youtu.be/eqEc66RFY0I>

# Logistic Regression

> **로지스틱 회귀(Logistic Regression)에 대해 다루며, 특히 **이진 분류 문제**에서 0 또는 1의 출력 레이블을 예측하는 데 사용되는 학습 알고리즘임을 설명한다.
> 선형 회귀와 달리 출력값이 0과 1 사이의 확률이 되도록 **시그모이드 함수**를 적용하여 $\hat{Y}$을 생성하는 방법을 상세히 소개한다.
> 궁극적으로 로지스틱 회귀는 주어진 입력 X에 대해 Y가 1일 확률을 예측하는 데 사용되는 **W와 B 매개변수를 학습**하는 것이 목표다.

## I. 로지스틱 회귀 개념과 목적

- 로지스틱 회귀는 **이진 분류 문제**에 사용하는 학습 알고리즘이다.
    + 예) 사진이 고양이인지 아닌지 구별하는 문제에서, 입력 특징 벡터 X에 대해 **Y가 1일 확률 또는 가능성**을 예측하는 것
- 즉, $\hat{Y}$은 Y가 1일 확률 또는 가능성으로, 입력 X가 고양이 사진일 확률을 의미한다.

## II. 출력값 $\hat{Y}$의 의미

- $\hat{Y}$은 **Y가 1일 확률**을 나타내며, 이는 조건부 확률이다.
- X가 사진일 때, $\hat{Y}$은 이 사진이 고양이일 확률을 알려준다.
- X는 **행렬 또는 벡터**형태이고, 파라미터는 W와 B이다.
    + W는 n차원 벡터, B는 실수

## III. 선형 함수와 그 한계

- 초기 시도: $\hat{Y}$을 $W^T X+B$로 계산하는 것이다.
    + 이는 선형 회귀에서 사용하는 방버
- 하지만, 이 방법은 적합하지 않다.
    + 왜냐하면, $W^T X+B$는 0과 1 사이의 값이 아니기 때문에 확률로 적합하지 않기 때문
    + 값이 1보다 크거나 작아질 수 있어, 확률로서 의미가 없음
- 따라서, $\hat{Y}$은 0과 1 사이의 값을 갖도록 만들어야 한다.

## IV. 시그모이드 함수의 역할

- $\hat{Y}$을 **시그모이드 함수**를 적용한 값으로 정의한다.
- 시그모이드 함수는 $Z = W^T X+B$일 때, $\frac{1}{(1 + e^{-Z})}$로 계산된다.
- 시그모이드 함수는 Z 값에 따라 0에서 1로 부드럽게 변화하는 곡선이다.
    + Z가 매우 크면, 시그모이드 값은 거의 1에 가까워짐
    + Z가 매우 작거나 음수이면, 거의 0에 가까워짐 

## V. 시그모이드 함수의 특징과 그래프

- 시그모이드 그래프는 Z가 커질수록 1에 가까워지고, 작아질수록 0에 가까워진다.
- Z가 크면 $e^{-Z}$는 0에 가까워지고, 시그모이드 값은 1에 가까워진다.
- Z가 작거나 음수이면, $e^{-Z}$는 매우 크고, 시그모이드 값은 0에 가까워진다.
- 이 특성 덕분에, 확률값으로 적합하게 만들어준다.

## VI. 파라미터 학습과 표기법

- **로지스틱 회귀의 목표**는 W와 B를 학습하여 $\hat{Y}$이 Y = 1일 확률을 잘 예측하게 하는 것이다.
- 파라미터 W와 B는 별도로 유지하는 것이 일반적이다.
- 일부 수학적 표기법에서는, X0이라는 특성을 1로 두고, W와 B를 하나의 벡터로 묶는 방식도 있다.
- 하지만, 이 강의에서는 W와 B를 별개로 유지하는 표기법을 사용한다.

## VII. 모델 학습을 위한 비용 함수

- 로지스틱 회귀에서 파라미터 W와 B를 학습하려면, **비용 함수**를 정의해야 한다.

출처 :<https://youtu.be/hjrYrynGWGA>
