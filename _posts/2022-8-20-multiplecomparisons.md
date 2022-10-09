---
layout: single
title:  다중비교
categories: 다중비교
tag: [Statistic, Multicomparison]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 본페르니 교정

다중비교는 평균 등과 같은 통계값을 여러 번 비교하여 차이를 분석하는 것을 가리킨다.

먼저 부분적인 귀무가설 집합을 다음과 같이 정의합니다.

$\digamma =  \{H_{1}, H_{2},\cdots, H_{k} \}$

본페르니 검정은 $\digamma$에 대해 유의수준 $\alpha$로 검정하고자 귀무가설 $H_{i}$에 대한 유의수준 $\alpha/k$ 의 검정을 k번 수행하는 것입니다.$

$H_{i}$를 기각하는 사건을 $A_{i}$라 할 때, 귀무가설 H_{i}를 유의수준 $\alpha/k$로 검정한다는 것은 $P(A_{i})\le \alpha/k$ 가 성립하도록 검정통계량과 기각역을 조정하는 것으로 생각할 수 있습니다.

$\digamma$를 기각하는 사건 B는 $A_{1} \cup A_{2} \cup \cdots \cup A_{k}$ 로 나타낼 수 있으며 다음 식이 성립합니다.

$P(B) = A_{1} \cup A_{2} \cup \cdots \cup A_{k} \le P(A_{1}) + P(A_{2}) + \cdots + P(A_{k}) \le (\alpha/k) \times k = \alpha $

이런 본페르니 교정 방식은 검정력이 낮다는 단점이 존재합니다.

ex)


```python
from statsmodels.sandbox.stats.multicomp import MultiComparison
import scipy.stats

comp = MultiComparison(df2['기록'],df2['자동차'])

# Bonferroni 방법을 통해 모든 그룹간 ttest를 진행
result = comp.allpairtest(scipy.stats.ttest_ind, method='bonf')
result[0]
```

# 투키-크레이머

