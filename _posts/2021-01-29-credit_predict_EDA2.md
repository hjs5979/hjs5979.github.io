---
layout: single
title:  신용등급 예측 대회 EDA2
categories: 신용등급예측대회
tag: [python, EDA]
toc: true
sidebar:
  nav: docs
---

**변수간에 상관관계가 있는 지 확인해보겠습니다~~**
**먼저 전처리가 필요하겠죵~**

## 상관분석 전처리

```python
# 인덱스 미리 제거
train.drop(['index'], axis=1, inplace=True)
```


```python
# 변수분석에 방해가 되는 DAYS_EMPLOYED의 양수값을 0으로 변경
index = train[train['DAYS_EMPLOYED'] >= 0].index
train.loc[index,'DAYS_EMPLOYED'] = 0
```


```python
# 숫자형 변수와 범주형 변수 미리 분리
cate_feat = []
num_feat = []
for col in train.columns:
    target = train[col]
    if target.nunique() <=20:
        cate_feat.append(col)
    else:
        num_feat.append(col)
print('범주형 :', cate_feat)
print('연속형: ', num_feat)
```

    범주형 : ['gender', 'car', 'reality', 'child_num', 'income_type', 'edu_type', 'family_type', 'house_type', 'FLAG_MOBIL', 'work_phone', 'phone', 'email', 'occyp_type', 'family_size', 'credit']
    연속형:  ['income_total', 'DAYS_BIRTH', 'DAYS_EMPLOYED', 'begin_month']

## 상관관계 분석

### 범주형간 상관관계 분석 : Cramer's V 상관계수 사용

- 크래머 상관계수는 범주형 변수간에 사용할 수 있는 통계값입니다!!
- 딱히 라이브러리가 없어서 직접 계산...

```python
import numpy as np
#Cramer’s V = √(X2/n) / min(c-1, r-1)
from scipy.stats import chi2_contingency, chisquare
# cate_feat.remove('FLAG_MOBIL')

corr_list= []
for i in cate_feat:
    c_list = []
    for j in cate_feat:
        ct = pd.crosstab(train[i],train[j])
        result=chi2_contingency(observed=ct)
        c = np.sqrt(result[0]/(len(train)*(min(len(train[i].unique()),len(train[j].unique()))-1)))
        c_list.append(c)
    corr_list.append(c_list)
corr_df = pd.DataFrame(corr_list,columns=cate_feat, index=cate_feat)
```


```python
sns.set(rc = {'figure.figsize':(20,12)})
sns.heatmap(corr_df,vmin=-1,vmax=1,cmap='RdBu',linewidths=.1,annot=True, fmt='.2f')
```

![png](/assets/images/credit_predict/output_20_1.png)

### 숫자형간 상관관계 분석 : 피어슨

- 범주형에 있는 숫자형 변수도 모두 넣었습니다!


```python
numeric_feat = []
for col in train.columns:
    if train[col].dtype == 'int64' or 'float64':
        numeric_feat.append(col)
        
num_corr = train[numeric_feat].corr(method='pearson') #spearman

sns.set(rc = {'figure.figsize':(20,12)})
sns.heatmap(num_corr,vmin=-1,vmax=1,cmap='RdBu',linewidths=.1,annot=True, fmt='.2f')
```
![png](/assets/images/credit_predict/output_22_1.png)
    
### 상관관계 분석 결과

- family_size와 child_num은 양의 상관관계가 큼
- DAYS_EMPLOYED와 DAYS_BIRTH는 음의 상관관계가 있음
- 모든 변수가 credit과 상관관계가 없음