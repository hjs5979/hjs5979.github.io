---
layout: single
title:  카이제곱 분포, t분포, F분포
categories: 분포
tag: [Statistic, distribution]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

검정에서 자주 쓰이는 세가지 분포에 대해서 간단히 정리해보겠습니다.

셋 모두 모집단의 어떤 통계량을 사용하여 임의적으로 만든 분포라고 설명할 수 있습니다. 

일단 여기서 모집단은 표준정규분포 $N(0,1^2)$로 가정합니다.

# 카이제곱분포

카이제곱분포는 표본의 편차제곱합이 따르는 확률 분포를 나타냅니다.

독립인 확률변수 $Y_{1}^{2},Y_{2}^{2},Y_{3}^{2},\cdots ,Y_{n}^{2}$가 표준정규분포를 따를 때, 확률 변수 X를 다음과 같은 식으로 가정합니다.

$X = Y_{1}^{2} + Y_{2}^{2} + Y_{3}^{2} + \cdots + Y_{n}^{2}$

X가 따르는 확률변수를 '자유도 n인 카이제곱분포'라고 하며 $\chi^2(n)$로 나타냅니다.

편차제곱합의 분포를 나타내는 것이기 때문에 분산과 크게 관련이 있습니다. 분산의 분포를 나타내는 것으로 해석할 수 있기 때문에 표본을 통해 모집단의 분산을 추정할 수 있고, 분포끼리의 차이를 검정할 때도 사용됩니다.

![webp](/assets/images/statistic/1581101414393.webp)
(출처 : https://www.jmp.com/ko_kr/statistics-knowledge-portal/chi-square-test/chi-square-distribution.html )

# t분포

t분포는 표본평균을 표본분산으로 표준화한 값이 따르는 확률분포입니다.

독립인 확률변수 Y,Z가 있고 확률변수 Y가 표준정규분포를 따르고 Z가 자유도 n인 카이제곱분포를 따른다고 할 때 확률변수 X를 다음과 같은 식이라 가정합니다.

$X = \frac{Y}{\sqrt{\frac{Z}{n}}}$

이 때, X가 따르는 확률 분포를 지유도 n인 t분포라고 하며, t(n)으로 나타냅니다.

$\sqrt{\frac{Z}{n}}$는 사실상 표준편차로 생각할 수 있을 것입니다. 결국 t분포는 표준화된 값을 분포로 나타낸 것으로 생각할 수 있습니다. 

t분포에는 스튜던트화라는 개념이 들어가는데, 위 식에서 Y값을 모르는 경우가 많기 때문에 이를 비편항분산(불편분산)으로 대체하여 사용하고 그 분포를 자유도 n-1인 t분포로 나타냅니다. 이를 스튜던트화라고 합니다.

t분포는 표본평균을 이용하여 정규분포의 평균을 해석하는데에 많이 사용됩니다. 특히 평균차이검정에서 많이 사용되는 편입니다. 

![png](/assets/images/statistic/405px-Student_t_pdf.png)

(출처 : https://www.jmp.com/ko_kr/statistics-knowledge-portal/t-test/chi-square-distribution.html )


# F분포

F분포는 모분산이 같은 표본분산 2개의 비율이 따르는 확률분포입니다.

독립인 확률변수 Y,Z가 있고 Y가 자유도 m인 카이제곱 분포를 따르고 Z가 자유도 n인 카이제곱 분포를 따른다고 할 때, 확률변수 X를 다음과 같이 가정할 수 있습니다.

$X = \frac{\frac{Y}{m}}{\frac{Z}{n}}$

이 때, X는 자유도 (m,n)인 F분포를 따른다고 하며, F(m,n)으로 나타냅니다.

식에서 유추할 수 있듯이 모집단 2개의 분산이 같은지 또는 분산분석에서 그룹간 변동/오차 변동의 크기를 비교할 때 사용됩니다.

F분포는 특이한 특성을 가지고 있습니다.
$F_{m,n}(a) = \frac{1}{F_{n,m}(1-a)}$가 성립된다는 것입니다. 이를 통해 F분포표에 없는 값을 찾아낼 수 있습니다.

![png](/assets/images/statistic/405px-Student_f_pdf.png)

(출처 : https://www.jmp.com/ko_kr/statistics-knowledge-portal/f-test/chi-square-distribution.html )
