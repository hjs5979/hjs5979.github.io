---
layout: single
title:  정규성 검정과 등분산성 검정
categories: 검정
tag: [Statistic, Test]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

캐글의 부동산 데이터를 사용해서 실습을 진행하였습니다.

# 정규성 검정

어떤 데이터가 정규분포를 따르는지 검정합니다.

- 귀무가설 : 정규분포를 따른다
 - 대립가설 : 정규분포를 따르지 않는다.

## Shapiro - Wilk 검정
 - 표본수가 2000미만일때 사용

```python
from scipy.stats import shapiro

print(shapiro(df['price']))
```
(0.9912054538726807, 1.1467612637261482e-07)

p-value가 0.05 이하이므로 귀무가설을 기각하고 정규분포를 따르지 않는다고 판단합니다.

## Kolmogorov-Smirnov 검정
 - 표본수가 2000이상일때 사용

```python
from scipy.stats import kstest
print(kstest(df['price'], 'norm'))
```
```
KstestResult(statistic=1.0, pvalue=0.0)
```
p-value가 0.05 이하이므로 귀무가설을 기각하고 정규분포를 따르지 않는다고 판단합니다.

## Anderson-darling 검정

앤더슨 달링 검정은 KStest를 수정한 것으로 더 강력하게 검정하는 방법입니다.

```python
from scipy.stats import anderson
anderson(df['OverallQual'])
```
```
AndersonResult(statistic=35.230011585458215, critical_values=array([0.574, 0.654, 0.785, 0.916, 1.089]), significance_level=array([15. , 10. ,  5. ,  2.5,  1. ]))
```
결과를 유의수준 0.05로 해석하려면 significance_level이 5에 해당하는 critical_values인 0.785를 기준으로 삼아야합니다. 통계량이 0.785 이하이면 귀무가설을 기각하는 것입니다. 결과에서 통계량이 35으로 귀무가설을 기각하는 것이 되기 때문에 정규분포를 따르지 않는다고 결론을 내릴 수 있습니다. 

# 등분산성 검정

어떤 데이터간 분산이 같은 지 검정합니다.

(범주형 변수의 항목마다 분산이 같은지 검정할 때 많이 사용됩니다.)

 - 귀무가설 : 분산이 같다
 - 대립가설 : 분산이 다르다

## F검정

$T = \frac{U_{A}^2}{U_{B}^2}$가 F(m-1,n-1)를 따름

## Bartlett 검정

 - 정규성을 만족할 때만 사용가능

```python
from scipy.stats import bartlett, levene

b = bartlett(df['price'][df['GarageCars']==0], 
            df['price'][df['GarageCars']==1], 
            df['price'][df['GarageCars']==2], 
            df['price'][df['GarageCars']==3])
print(b)
```
BartlettResult(statistic=25.450389257703492, pvalue=1.242973178847051e-05)

p-value가 0.05 이하이므로 귀무가설을 기각하고 분산이 같다고 판단합니다.

## Levene 검정 

 - 정규성을 만족하지 않을 때도 사용 가능하지만 바틀렛에 비해 정확도 떨어짐

```python
l = levene(df['price'][df['GarageCars']==0],
            df['price'][df['GarageCars']==1],
            df['price'][df['GarageCars']==2],
            df['price'][df['GarageCars']==3], center = 'median')
print(l)
```
LeveneResult(statistic=8.694999805831733, pvalue=1.027278951826962e-05)

p-value가 0.05 이하이므로 귀무가설을 기각하고 분산이 같다고 판단합니다.