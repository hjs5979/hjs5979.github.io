---
layout: single
title:  다중비교
categories: 다중비교
tag: [Statistic, Multiplecomparison]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 본페르니 교정

다중비교는 평균 등과 같은 통계값을 여러 번 비교하여 차이를 분석하는 것을 가리킵니다.

먼저 부분적인 귀무가설 집합을 다음과 같이 정의합니다.

$\digamma =  \{H_{1}, H_{2},\cdots, H_{k} \}$

본페르니 검정은 $\digamma$에 대해 유의수준 $\alpha$로 검정하고자 귀무가설 $H_{i}$에 대한 유의수준 $\alpha/k$ 의 검정을 k번 수행하는 것입니다.$

$H_{i}$를 기각하는 사건을 $A_{i}$라 할 때, 귀무가설 H_{i}를 유의수준 $\alpha/k$로 검정한다는 것은 $P(A_{i})\le \alpha/k$ 가 성립하도록 검정통계량과 기각역을 조정하는 것으로 생각할 수 있습니다.

$\digamma$를 기각하는 사건 B는 $A_{1} \cup A_{2} \cup \cdots \cup A_{k}$ 로 나타낼 수 있으며 다음 식이 성립합니다.

$P(B) = A_{1} \cup A_{2} \cup \cdots \cup A_{k} \le P(A_{1}) + P(A_{2}) + \cdots + P(A_{k}) \le (\alpha/k) \times k = \alpha $

이런 본페르니 교정 방식은 검정력이 낮다는 단점이 존재합니다.

ex)

%https://hjs5979.github.io/%EB%B6%84%EC%82%B0%EB%B6%84%EC%84%9D/%EB%8B%A4%EC%A4%91%EB%B9%84%EA%B5%90/anova1/%
의 일원분산분석의 예시에서 이어져서 진행됩니다. 

```python
from statsmodels.sandbox.stats.multicomp import MultiComparison
import scipy.stats

comp = MultiComparison(df2['기록'],df2['자동차'])

# Bonferroni 방법을 통해 모든 그룹간 ttest를 진행
result = comp.allpairtest(scipy.stats.ttest_ind, method='bonf')
result[0]
```

# 투키-크레이머

투키-크레이머 방법은 비교할 k개의 그룹($A_{1}, \cdot, A_{k}$)이 모두 정규분포를 따르고 분산이 같다고 가정합니다. 그룹 $A_{i}$의 표본 크기를 $n_{i}$, 평균을 $\bar{x_{i}}$, 비편향분산을 $V_{i}$로 정하면 오차의 자유도 $\phi_{e}$ 는 N-K이고 오차 분산 $V_{e}$는 $\frac{\sum^{k}_{i=1}{(n_{i}-1)V_{i}}}{N-K}$로 계산됩니다.

$귀무가설 H_{i,j} : \mu_{i} \ne \mu{j}$
$대립가설 H'_{i,j} : \mu_{i} \neq \mu{j}$

로 두고, k개의 그룹 중에 두 그룹을 고르는 경우의 수인 $_{k}C_{2}$개의 원소를 가진 귀무가설 집합 $\digamma$를 생성합니다. 생성된 각각의 귀무가설에 다음식과 같은 검정통계량을 게산할 수 있습니다.

$t_{ij} = \frac{\bar{x_{i}}-\bar{x-{j}}}{\sqrt{V_{e}(\frac{1}{n_{1}}+\frac{1}{n_{2}})}}$

이를 이용하여 

$|t_{ij}| \geq \frac{q(k,\phi_{e},\alpha)}{\sqrt{2}}$

$|t_{ij}| < \frac{q(k,\phi_{e},\alpha)}{\sqrt{2}}$

여기서 

q(k,\phi_{e},\alpha)는 스튜던트화 범위 분포 상위 $\alpha$ 지점입니다.

ex)

%https://hjs5979.github.io/%EB%B6%84%EC%82%B0%EB%B6%84%EC%84%9D/%EB%8B%A4%EC%A4%91%EB%B9%84%EA%B5%90/anova1/%
의 일원분산분석의 예시에서 이어져서 진행됩니다.

```python
from statsmodels.stats.multicomp import pairwise_tukeyhsd

hsd = pairwise_tukeyhsd(df2['기록'], df2['자동차'], alpha=0.05)
hsd.summary()
```

# Reference

이시이 도시아키. 『통계학 대백과사전』. 안동현(역). 동양북스, 2022.