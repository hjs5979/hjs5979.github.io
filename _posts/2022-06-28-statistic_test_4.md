---
layout: single
title:  카이제곱검정(적합도검정, 독립성검정)
categories: 검정
tag: [Statistic, Test]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

적합도검정과 독립성검정은 카이제곱 분포를 통해 검정합니다. (동질성 검정은 독립성 검정과 유사한 부분이 있어서 후에 설명드리겠습니다.)


# 적합도 검정

- 적합도검정은 관측 데이터의 분포와 예측(기대) 데이터의 분포가 같은지 검정합니다.
- 범주형 독립변수 1개 존재할 때 사용가능합니다.
 
  - 귀무가설 : 변수의 관측분포와 기대분포가 동일하다
  - 대립가설 : 변수의 관측분포와 기대분포가 다르다

|-|A1|A2|$\cdots$|합계|
|-|-|-|-|-|
|관측도수|$X_{1}$|$X_{2}$|$\cdots$|n|
|기대도수|$np_{1}$|$np_{2}$|$\cdots$|n|

$T = \frac{(X_{1})}{  }$


ex) 일반적으로 동전을 10번 던질 때, 앞면이 나올 확률을 1/2이기 때문에 앞면이 5번나오고 뒷면이 5번 나올 것으로 **예측**할 수 있습니다. 하지만 실제로 10번을 던졌을 때는 이와는 다른 결과가 관측될 수 있습니다. 이러한 경우에 관측의 결과가 예측의 결과와 다른지 같은지 평가해야하기 때문에 적합도 검정을 사용합니다. 정상적인 동전을 던졌다면 귀무가설을 수용할 것입니다.

|-|앞면|뒷면|합계|
|-|-|-|-|
|관측도수|4|6|10|
|기대도수|5|5|10|

# 독립성 검정

- 두 범주형 변수의 속성들이 서로 독립인지 검정합니다.

  - 귀무가설 : 두 변수는 독립이다.(두 변수의 범주에 따라 도수에 차이가 없다.)  
  - 대립가설 : 두 변수는 독립이 아니다.(두 변수의 범주에 따라 도수에 차이가 있다.)


- 2x2 교차표인 경우(두 변수의 범주가 두개)

|-|B1|B2|합계|
|-|-|-|-|
|A1|a|b|a+b|
|A2|c|d|c+d|
|합계|a+c|b+d|n|

$T = \frac{n(ad-bc)}{(a+c)(b+d)(a+b)(c+d)}$는 카이제곱 $\chi$^2(1)을 따름

- kxl 교차표인 경우(두 변수의 범주가 각각 k,l개)

|-|B1|$\cdots$|$B_{l}$|합계|
|-|-|-|-|-|
|A1|$x_{11}$|$\cdots$|$x_{1l}$|$a_{1}$|
|$\vdots$|$\vdots$|$\ddots$|$\vdots$|$\vdots$|
|$A_{k}$|$x_{k1}$|$\cdots$|$x_{kl}$|$a_{k}$|
|합계|$b_{1}$|$\cdots$|$b_{l}$|n|

$T = \sum_{l\le i \le k, l\le j \le k}{\frac{(x_{ij}-y_{ij})^2}{y_{ij}}}$
가 $\chi^2$((k-1)(l-1))을 따름



```python
from scipy.stats import chi2_contingency

#크로스탭 생성
ct = pd.crosstab(df['KitchenAbvGr'],df['GarageCars']) 

result=chi2_contingency(observed=ct)
print("1. 카이제곱 통계량:", result[0])
print("2. p-value:", result[1])
print("3. df:", result[2]) #(행의개수-1)*(열의개수-1)
```

# Reference

이시이 도시아키. 『통계학 대백과사전』. 안동현(역). 동양북스, 2022.