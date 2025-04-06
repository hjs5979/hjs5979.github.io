---
layout: single
title:  왜도/첨도, 범위, 중앙값, 백분위수/사분위수
categories: 분포
tag: [Statistic, discriptive]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 왜도(skewness)

데이터가 대칭 상태에서 얼마나 일그러졌는가?

$\frac{1}{n}\sum_{i=1}^{n}(\frac{x_{i} - \bar{x}}{s})^3$

정규분포의 왜도는 0 => 양수면 왼쪽으로 치우치고, 음수면 오른쪽으로 치우침

보통 -2 ~ 2일 때 왜도가 크지 않다고 판단한다고 합니다.

# 첨도(kurtosis)

분포가 얼마나 뾰족한지?

$\frac{1}{n}\sum_{i=1}^{n}(\frac{x_{i} - \bar{x}}{s})^4 - 3$

정규분포의 첨도는 0 (-3이 없으면 3일 것이다. 보통은 정규분포의 첨도를 0으로 만들기 위해 첨도를 계산할 때 -3을 해줌) => 양수면 더 뾰족하고, 음수면 뭉툭해집니다.

# 범위(range)

데이터에서 '최댓값 - 최솟값'을 범위라고 함

# 중앙값

- 데이터 크기 n이 홀수라면 작은 것부터 $\frac{n+1}{2}$번째 값

- 데이터 크기 n이 짝수라면 작은 것부터 $\frac{n}{2}$번째 값과 $\frac{n}{2} + 1$번째 값의 평균

# 백분위수/사분위수

백분위수는 크기가 있는 값들로 이뤄진 자료를 순서대로 나열했을 때 백분율로 나타낸 특정 위치의 값을 이르는 것을 말합니다.

사분위수에는는 1,2,3 사분위수가 있고, 1사분위수는 25 백분위수, 2사분위수는 50 백분위수, 3사분위수는 75 사분위수입니다.

IQR(Interquartile range)는 Q3 - Q1를 의미합니다.
=> 통계분석에서 이상치를 설정할 때, Q1 - (1.5 * IQR), Q3 + (1.5 * IQR) 를 이상치의 제한선으로 설정하는 경우가 있습니다.

```python

first = np.quantile(data, 25)
third = np.quantile(data, 75)

IQR = third - first
```

오늘 포스팅은 여기까지입니다.

