---
layout: single
title: 상관관계의 종류
categories: 상관관계
tag: [Statistic, Correlation]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 피어슨 상관분석

피어슨 상관분석은 수치형 데이터(비율척도, 등간척도) 간에 사용합니다.
두 변수 모두 정규성을 만족해야 사용할 수 있습니다.

$$
\rho = \frac{S_{XY}}{S_{X}S_{Y}} = \frac{\sum_{i=1}^{n}(X_{i}-\bar{X})(Y_{i}-\bar{Y})}
{\sqrt{\sum_{i=1}^{n}(X_{i}-\bar{X})^2\sum_{i=1}^{n}(Y_{i}-\bar{Y})^2}}
$$

-> 표본 공분산을 각 변수의 표준편차의 곱으로 나눠주는 것으로 계수를 구합니다.

</br>

검정은 다음과 같이 진행합니다.

- t분포를 사용하여 검정

  - H0 : $\rho = 0$ -> 상관관계 없음
  - H1 : $\rho \ne 0$ -> 상관관계 있음

두가지 예시를 보여드리겠습니다.

1. 광고비와 판매액의 상관관계를 검정하여라.

```python
광고비 = [4,6,6,8,8,9,9,10,12,12]
판매액 = [39,42,45,47,50,50,52,55,57,60]
```

```python
p = stats.pearsonr(광고비,판매액)
print(p)
```

(0.9775318143145577, 1.085156151192339e-06)

p-value가 0.05 이하이므로 귀무가설을 기각하고 상관계수가 0.97이므로 강한 양의 상관관계 있다고 결론내립니다.

2. 수학점수와 공부시간의 상관관게를 검정하여라

```python
수학점수 = [88,87,91,92,94,95,97,100]
공부시간 = [2,4,5,6,7,8,9,10]
```

```python
p = stats.pearsonr(수학점수,공부시간)
print(p)
```

(0.9592154885795401, 0.00016445450858209996)

p-value가 0.05 이하이므로 귀무가설을 기각하고 상관계수가 0.95이므로 강한 양의 상관관계 있다고 결론내립니다.

# 스피어만 순위상관계수

숫자형 변수(순서척도) 간에 사용하는 상관계수입니다. 두 변수는 정규성을 만족할 필요는 없습니다.
데이터를 순서값으로 변경 후 계산합니다. 동일한 순서가 없을 때는 순서값의 피어슨 상관계수와 값과 동일한 값이 나옵니다.

- 동일한 순서가 없을 때는 다음과 같이 계산합니다.
  (순서데이터를 피어슨상관계수에 넣었을 때와 같습니다.)

$$\rho = 1 - \frac{6\{(a_{1}-b_{1})^2+(a_{2}-b_{2})^2+ \cdot\cdot\cdot +(a_{n}-b_{n})^2\}}{n(n^2-1)}$$

</br>

- 동일한 순서가 있을 때는 다음과 같이 계산합니다.

$$T_{x} = 1/12{n(n^2-1)-\sum_{k}c_{k}(c_{k}^2-1)}$$

$$\rho = \frac{T_{x}+T+{y}-\sum_{i=1}^{n}(a_{i}-b_{i})^2}{2\sqrt{T_{x}T_{y}}}$$

https://m.blog.naver.com/pmw9440/221955081796

```python
광고비 = [4,6,6,8,8,9,9,10,12,12]
판매액 = [39,42,45,47,50,50,52,55,57,60]

a = [10, 8.5, 8.5, 6.5, 6.5, 4.5, 4.5, 3, 1.5, 1.5]
a = [10, 9, 8, 7 , 5, 5, 4, 3, 2, 1]
```

```python
p = stats.spearmanr(광고비,판매액)
print(p)
```

SpearmanrResult(correlation=0.9785032270610473, pvalue=9.103819964493901e-07)

p-value가 0.05 이하이므로 귀무가설을 기각하고 상관계수가 0.97이므로 강한 양의 상관관계 있다고 결론내립니다.

```python
p = stats.spearmanr(수학점수,공부시간)
print(p)
```

SpearmanrResult(correlation=0.9761904761904763, pvalue=3.3143960262001043e-05)

p-value가 0.05 이하이므로 귀무가설을 기각하며 상관계수가 0.97이므로 강한 양의 상관관계 있다고 결론내립니다.

많은 분석 보고서에서 히트맵으로 변수간 상관관계를 시각화합니다. 저도 한번 히트맵으로 시각화를 해봤습니다.

```python
df = pd.DataFrame({'광고비':광고비,'판매액':판매액})
df_corr = df.corr(method='spearman')

sns.set(rc = {'figure.figsize':(20,12)})
sns.heatmap(df_corr,vmin=-1,vmax=1,cmap='RdBu',linewidths=.1,annot=True, fmt='.2f')
```

![png](/assets/images/credit_predict/heatmap.png)

# 켄달의 순위 상관계수

- 크기가 n인 2변량 데이터 $(x_{n}, y_{n})$ 가 있을 때 n개 중 2개를 골라 이를 $(x_{i},y_{i}), (x_{j},y_{j})$라고 가정하고 다음 조건을 부여합니다.

$$\begin{cases} (x_{i}-x_{j})(y_{i}-y_{j})>0, & a_{ij} = 1 \\ (x_{i}-x_{j})(y_{i}-y_{j})<0, & a_{ij}=-1   \end{cases}$$

</br>

- 상관계수 $\tau$를 다음과 같이 계산합니다.

$$\tau = \frac{\sum_{i<j}a_{ij}}{_{n}\mathrm{C}_{2}}$$

수학점수와 공부시간을 켄달의 순위상관계수를 구하여라.

```python
노동시간 = [10,9,9,8,8,8]
수면시간 = [6,7,8,8,7,6.5]
```

```python
stats.kendalltau(노동시간,수면시간)
```

KendalltauResult(correlation=-0.25087260300212727, pvalue=0.5243108008991835)

p-value가 0.05 이상이므로 귀무가설을 기각하지 못하고 상관관계가 없다고 결론내립니다.

# 크라메르의 연관계수

- 범주형 변수(2개 이상의 범주를 가짐)간 상관관계를 구할 때 사용합니다.

이런 형식의 데이터를 생각하시면 될 것 같습니다.

| -                                                      | $B_{1}\cdots B_{l}$                                                                         | 합계                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| $$\begin{matrix}A_{1} \\ \vdots \\ A_{k}\end{matrix}$$ | $$\begin{matrix} 0 & \cdots & 0 \\ \vdots & \ddots &\vdots \\ 0 & \cdots & 0 \end{matrix}$$ | $$\begin{matrix}a {1} \\ \vdots \\ a_{k}\end{matrix}$$ |
| 합계                                                   | $$\begin{matrix} b_{1} \cdots b_{l} \end{matrix}$$                                          | n                                                      |

</br>

상관계수는 다음과 같이 계산합니다.

$$V = \sqrt{\frac{\sum_{i,j}\frac{x_{ij}^2}{a_{i}b_{j}}-1}{min(k,l)-1}} = \sqrt{\frac{\chi^2}{n(min(k,l)-1)}}$$

- 카이제곱 통계량을 사용하기 때문에 양수값만 존재합니다.
  $$ 0 \leqq V \leqq 1$$

예시를 살펴보겠습니다.

1. 연령에 따라서 지역별 여행 선호지에 상관관계가 있는 지 확인하시오.

|            | 20대 | 30대 | 40대 | 50대 | 합계 |
| ---------- | ---- | ---- | ---- | ---- | ---- |
| 북아메리카 | 9    | 14   | 10   | 55   | 38   |
| 유럽       | 5    | 11   | 13   | 6    | 35   |
| 아시아     | 6    | 5    | 7    | 9    | 27   |
| 합계       | 20   | 30   | 30   | 20   | 100  |

```python
from scipy.stats import chi2_contingency

a = pd.DataFrame(data={'20대':[9,5,6,20],'30대':[14,11,5,30],'40대':[10,13,7,30],'50대':[55,6,9,20],'합계':[38,35,27,100]}, index=['북아메리카','유럽','아시아','합계'])

result=chi2_contingency(observed=a)

row = 3
col = 4
n = 100

c = np.sqrt(result[0]/(n*(min(row,col)-1)))
print(c, result[1])
```

0.5738687975708358 1.885694064910255e-09

p-value가 0.05 이하이므로 귀무가설을 기각하고 상관계수가 0.57이므로 약간의 양의 상관관계가 있다고 결론내린다.

# 자기상관계수

어떤 시계열 데이터에서 시간에 따라서 스스로 상관이 있는지 알아보겠습니다.

- 시계열의 평균

$$\bar{y} = \frac{1}{T}(y_{1} + y_{2} + \cdots + y_{T})$$

- 시간차가 k인 자기공분산입니다. 시간차가 k만큼 있는 데이터와의 공분산을 구해준 다음 모두 더해줍니다.  
  $\gamma_{k} = Cov[y_{i}, y_{i-k}] = \frac{1}{T}\sum_{i=k+1}^{T}(y_{i}-\bar{y})
(y_{i-k}-\bar{y})$

- 시간차가 k인 자기상관계수는 이렇게 계산됩니다. 시간차가 k인 자기 공분산을 시간을 옮기지 않고 그대로 분산을 구해준것으로 나눠줍니다.
  $$\rho_{k} = \frac{\gamma_{k}}{\gamma_{0}}$$

divvy 데이터로 예시를 보여드리겠습니다.

```python
import matplotlib.pyplot as plt
from statsmodels.graphics.tsaplots import plot_acf
from statsmodels.tsa.stattools import acf

df = pd.read_csv('divvy_daily.csv')
series = df['rides']


print(series.head)
print(acf(series))
plot_acf(series)

```

```
1        111
2          6
3        181
4         32
        ...
1453    1117
1454    1267
1455    1049
1456     519
1457     593
Name: rides, Length: 1458, dtype: int64>
```

![png](/assets/images/statistic/acf.png)

y축의 도수가 상관계수이고 x축 도수는 시간차 k를 나타냅니다.
시간차의 상관없이 상관계수가 0.7이상이므로 이 데이터는 자기상관이 있다고 결론내릴 수 있습니다.

오늘은 상관계수에 대해서 알아봤습니다. 오늘 포스팅은 여기서 마치겠습니다.
