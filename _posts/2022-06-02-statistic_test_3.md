---
layout: single
title: 정규모집단의 평균 차이 검정
categories: 검정
tag: [Statistic, Test]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

모평균의 차이에 대한 검정에 대해서 설명드리겠습니다.
독립인 두 모집단 A,B가 정규분포를 따른다는 가정을 합니다.

$H_{0}$ : 모평균이 같다

$H_{1}$ : 모평균이 같지 않다

# 두 모분산을 알 때

$T = \frac{\bar{X_{A}}-\bar{X_{B}}}{\sqrt{\frac{\sigma_{A}^2}{n_{A}} + \frac{\sigma_{B}^2}{n_{B}}}}$가 $N(0,1^2)$를 따름

## 모분산은 모르지만 등분산임은 알때

$T = \frac{\bar{X_{A}}-\bar{X_{B}}}{\sqrt{(\frac{1}{n_{A}} + \frac{1}{n_{B}}})\frac{(n_{A}-1)U_{A}^2+(n_{B}-1)U_{B}^2}{(n_{A}-1)(n_{B}-1)}}$가 $t(n_{A}+n_{B}-2)를 따름$

이러한 경우는 사이파이의 라이브러리를 통해 검정이 가능합니다.
데이터는 서울과 경기에 사는 사람들의 월평균 임금입니다. 각각의 10개의 표본을 뽑았습니다.(등분산이라고 가정합니다.)

```python
서울 = [272, 255, 278, 282, 296, 312, 356, 296, 302, 312]
경기 = [276, 280, 369, 285, 303, 317, 290, 250, 313, 307]

from scipy import stats
t = stats.ttest_ind(남자, 여자, equal_var = True)
```

```
Ttest_indResult(statistic=-0.21754827387924294, pvalue=0.8302281047230314)
```

pvalue가 0.05 이상이기 때문에 귀무가설을 채택합니다. 두 모집단의 평균은 같다고 결론내립니다.

이러한 검정을 student t-test라고도 말합니다.

## 모분산을 모르고, 등분산임도 모를 때

$T = \frac{\bar{X_{A}}-\bar{X_{B}}}{\sqrt{\frac{U_{A}^2}{n_{A}}+\frac{U_{B}^2}{n_{B}}}}$가 $t(f)$를 따름

f는 $(\frac{U^2_{A}}{n_{A}}+\frac{U^2_{B}}{n_{B}})/(\frac{1}{n_{A}-1}(\frac{U^2_{A}}{n_{A}})^2 + \frac{1}{n_{B}-1}(\frac{U^2_{B}}{n_{B}})^2)$ 에 가까운 정수로 설정

위의 경우에서 var_equal 변수만 바꿔주면 됩니다.

```python
t = stats.ttest_ind(df1, df2, equal_var = False)
```

이러한 검정을 welch t-test라고도 말합니다.

## 데이터끼리 대응관계가 있을 때

$T = \frac{\bar{D}}{\sqrt{\frac{U^2_{D}}{n}}}$ 가 t(n-1)을 따름

$\bar{D}$는 $d_{i}$의 평균($d_{i} = x_{i} - y_{i}$)

```python
t = stats.ttest_rel(df1, df2)
```

ex) 다이어트 전, 후의 평균 검정을 예로 들 수 있다. 이 데이터 둘은 원인과 결과라는 대응관계가 존재한다.

오늘 포스팅은 여기까지 하겠습니다.

# Reference

이시이 도시아키. 『통계학 대백과사전』. 안동현(역). 동양북스, 2022.
