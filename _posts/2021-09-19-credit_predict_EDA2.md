---
layout: single
title:  신용등급 예측 대회 EDA2
categories: 신용등급예측대회
tag: [python, EDA, ML]
toc: true
author_profile: true
sidebar:
  nav: docs
---
저번 포스트에 이어서 EDA를 진행하도록 하겠습니다.
변수간에 상관관계가 있는 지 확인하기 위해 상관계수를 시각화 해봤습니다.

먼저 전처리를 진행하였습니다.

## 상관분석 전처리

```python
# 인덱스 미리 제거
train.drop(['index'], axis=1, inplace=True)
```
인덱스는 순서대로 부여되는 것이기 때문에 제거해줍니다.

```python
# 변수분석에 방해가 되는 DAYS_EMPLOYED의 양수값을 0으로 변경
index = train[train['DAYS_EMPLOYED'] >= 0].index
train.loc[index,'DAYS_EMPLOYED'] = 0
```
이전 포스트에서 말했듯이 DAYS_EMPLOYED에 비논리적인 이상치가 있었습니다. 이를 제거해줍니다.

상관계수는 변수가 숫자형/범주형이냐에 따라 다른 방법이 사용될 수 있기 때문에 변수를 분리해서 사용합니다. 저는 항목이 20개 이한인 변수는 모두  범주형 변수로 구분하고 Cramers's V 상관계수를 통해 분석하였습니다. 숫자형은 등간/비율이냐 순서이냐에 따라 다른데, 피어슨과 스피어만으로 나뉘는데 애매한 부분이 있기 때문에 둘다 시행하였습니다.

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

- 크래머 상관계수는 항목이 2개 이상인 범주형 변수간에 사용할 수 있는 상관계수입니다. 카이제곱 통계량을 통한 독립성 검정으로 상관계수를 계산하는 방식입니다. 파이썬에 따로 라이브러리가 있는 것 같지 않아서 직접 계산하여 히트맵을 만들었습니다.

```python
#Cramer’s V = √(X2/n) / min(c-1, r-1)

#카이제곱 통계량 계산하는 라이브러리
from scipy.stats import chi2_contingency, chisquare

#FlAG_MOBIL 제거(값이 하나라 0 division 오류발생)
cate_feat2 = cate_feat.copy()
cate_feat2.remove('FLAG_MOBIL')

corr_list= []

for i in cate_feat2:
    c_list = []
    for j in cate_feat2:
        ct = pd.crosstab(train[i],train[j])
        result=chi2_contingency(observed=ct)
        c = np.sqrt(result[0]/(len(train)*(min(len(train[i].unique()),len(train[j].unique()))-1)))
        c_list.append(c)
    corr_list.append(c_list)
corr_df = pd.DataFrame(corr_list,columns=cate_feat2, index=cate_feat2)
```


```python
sns.set(rc = {'figure.figsize':(20,12)})
sns.heatmap(corr_df,vmin=-1,vmax=1,cmap='RdBu',linewidths=.1,annot=True, fmt='.2f')
```

![png](/assets/images/credit_predict/output_20_1.png)

- family_size와 child_num은 양의 상관관계가 엄청 큰게 눈에 띕니다. 아이 수가 많으면 가족 사이즈가 커질수 밖에 없기 때문에 당연한 결과인 것 같습니다.
- 타겟인 credit이랑은 모두 상관관계가 크게 없다고 생각할 수 있습니다. occyp_type은 결측치 때문에 상관관계가 조금 낮게 나왔네요. 정확성을 위해 카이제곱 통계량을 통해 pvalue를 살펴본 결과, 0.05이하이기 때문에 family_size와 child_num의 상관관계는 유의하다는 결론을 내릴 수 있었습니다.
- family_type과 family_size / gender와 occyp_type이 약한 상관관계를 보이고 있는데, 가족타입과 가족크기 / 성별과 직업이 논리적으로는 관계가 있어보입니다. 하지만 상관계수가 크게 높지 않기 때문에 참고만 하겠습니다. 나머지 값들은 상관관계가 크지 않기 때문에 따로 살펴볼 이유는 없을 것 같습니다.
- 보통 0.5 이상을 강한 상관관계, 0.2~0.5을 약한 상관관계, 0.2이하를 상관관계가 거의 없다고 봅니다.
(사회과학 분야의 경우 0.2도 강한 상관관계로 보는 경향이 있다고 합니다.)

### 숫자형 변수간 상관관계 분석

모든 변수에 대해서 앤더슨 달링 정규성 검정을 한 결과 모든 변수가 정규성을 만족하지 못하였습니다. 때문에 숫자형 변수간의 상관관계를 분석하기 위해 스피어만 상관계수를 사용하였습니다. 범주형에 있던 숫자형 변수도 모두 넣어서 진행하였습니다.

#### 스피어만 상관관계 분석

![png](/assets/images/credit_predict/spearman.png)
    
FLAG_MOBIL은 항목이 하나밖에 없어서 상관관계가 안나왔습니다. 두 분석에서 동일하게 family_size와 child_num를 제외하면 0.6을 넘는 상관관계는 없습니다.
p-value도 살펴본 결과, 거의 0에 가까운 값이 나왔기 때문에 두 변수는 확실히 상관관계가 있다고 말할 수 있습니다.

### 상관관계 분석 결과

- family_size와 child_num은 분명히 양의 상관관계가 있음
- 모든 변수가 credit과 상관관계가 없음

데이터 분석에 있어서 변수간에 상관관계는 중요합니다. 상관관계가 있느냐 없느냐에 따라 진행할 수 있는 검정도 다르고 가정도 달라집니다.

오늘은 이만 포스팅을 마치겠습니다.