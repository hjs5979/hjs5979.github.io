---
layout: single
title:  주성분분석
categories: 주성분분석
tag: [Statistic, PCA]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 주성분분석

주성분분석은 차원 축소의 한 방법으로, 높은 차원의 데이터를 낮은 차원의 데이터로 축약하는 방법입니다. 원 데이터의 분포를 반영하면서 차원을 축소 시킬 수 있다는 점에서 일부 변수를 뽑아서 사용하는 것보다 우수하다는 장점이 있습니다. 
예를 들어, 변수가 10개 있는 회귀분석모델이 있다고 가정할 떄, 주성분분석을 통해 변수를 2~3개로 줄이는 것을 주성분분석이라고 말할 수 있습니다.

ex) 웹툰전문가(자칭) 5명이 한 웹툰을 개연성/그림체/캐릭터성으로 0~5점ㅇ 사이로 평가했습니다. 3개의 변수를 2개로 축소해보세요. 

|웹툰전문가|개연성|그림체|캐릭터성|
|---|---|---|---|
|A|3|1|2|
|B|2|3|3|
|C|5|2|1|
|D|1|5|5|
|E|4|4|4|

파이썬을 통해 실습해봤습니다.

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
import pandas as pd

data = [[3,1,2],[2,3,3],[5,2,1],[1,5,5],[4,4,4]]
df = pd.DataFrame(data, columns = ['개연성','그림체','캐릭터성'])

# PCA 모델 선언
pca = PCA(n_components=2)

#fit( )과 transform( ) 을 호출하여 PCA 변환 데이터 반환
pca.fit(df)
wt_pca = pca.transform(df)
print(wt_pca)

# PCA 환된 데이터의 컬럼명을 각각 pca_component_1, pca_component_2로 명명
pca_columns=['pca_component_1','pca_component_2']
irisDF_pca = pd.DataFrame(wt_pca,columns = pca_columns)

print(pca.explained_variance_ratio_)
```

```python
# 2차원 데이터로 변환된 것 확인
[[-1.79229654 -1.26422098]
 [ 0.51694721 -0.81881408]
 [-2.87089461  0.75969277]
 [ 3.45342557 -0.20952391]
 [ 0.69281837  1.53286621]]

 #기여율 확인
[0.80425809 0.17464745]
```

변수 2개로 변환했을 시 총 기여율은 약 97% 인 것으로 확인됩니다.(정보량 손실 거의 없음)

보통 PCA 작업 후에 기여율이 급격히 감소(엘보우기법)하는 변수까지 사용하는 경향이 강합니다.