---
layout: single
title:  인구데이터 기반 소득 예측 경진대회 EDA
categories: 소득예측대회
tag: [python, EDA, ML]
toc: true
author_profile: true
sidebar:
  nav: docs
---

![png](/assets/images/income_predict/title.PNG)

<https://dacon.io/competitions/official/235892/overview/description>

오랜만에 데이콘 대회에 참여해서 결과를 정리해봅니다!<br>
인구데이터를 통해 0,1로 구분된 타겟을 구분하면 되는 대회입니다.<br> 
타켓은 소득을 기준으로 구분되어 있네요
이전에 진행했던 신용카드대회와 코드와 내용은 비슷합니다.

</br>

변수부터 알아보겠습니다.

## 변수

- id : 샘플 아이디
- age : 나이
- workclass : 일 유형
- fnlwgt : CPS(Current Population Survey) 가중치
- education : 교육수준
- education.num : 교육수준 번호
- marital.status : 결혼 상태
- occupation : 직업
- relationship : 가족관계
- race : 인종
- sex : 성별
- capital.gain : 자본 이익
- capital.loss : 자본 손실
- hours.per.week : 주당 근무시간
- native.country : 본 국적
- target : 소득
 - 0 = <=50K (5만 달러 이하)
 - 1 = >50K (5만 달러 초과) 

fnlwgt은 미국 인구 조사에서 부여한 final weight라고 한다. 어떤 방식으로 계산되는 지는 명시되어 있지 않았습니다.

# EDA
---

## 기본 EDA

```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 17480 entries, 0 to 17479
    Data columns (total 16 columns):
     #   Column          Non-Null Count  Dtype 
    ---  ------          --------------  ----- 
     0   id              17480 non-null  int64 
     1   age             17480 non-null  int64 
     2   workclass       15644 non-null  object
     3   fnlwgt          17480 non-null  int64 
     4   education       17480 non-null  object
     5   education.num   17480 non-null  int64 
     6   marital.status  17480 non-null  object
     7   occupation      15637 non-null  object
     8   relationship    17480 non-null  object
     9   race            17480 non-null  object
     10  sex             17480 non-null  object
     11  capital.gain    17480 non-null  int64 
     12  capital.loss    17480 non-null  int64 
     13  hours.per.week  17480 non-null  int64 
     14  native.country  16897 non-null  object
     15  target          17480 non-null  int64 
    dtypes: int64(8), object(8)
    memory usage: 2.1+ MB
    
workclass와 occupation에 결측치가 존재합니다.
둘다 직업관련이라 직업이 없는 사람이 결측치로 나타났는지 확인을 해봐야할 것 같습니다.

```
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>age</th>
      <th>workclass</th>
      <th>fnlwgt</th>
      <th>education</th>
      <th>education.num</th>
      <th>marital.status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital.gain</th>
      <th>capital.loss</th>
      <th>hours.per.week</th>
      <th>native.country</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>17480.000000</td>
      <td>17480.000000</td>
      <td>15644</td>
      <td>1.748000e+04</td>
      <td>17480</td>
      <td>17480.000000</td>
      <td>17480</td>
      <td>15637</td>
      <td>17480</td>
      <td>17480</td>
      <td>17480</td>
      <td>17480.000000</td>
      <td>17480.00000</td>
      <td>17480.000000</td>
      <td>16897</td>
      <td>17480.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>NaN</td>
      <td>16</td>
      <td>NaN</td>
      <td>7</td>
      <td>14</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Private</td>
      <td>NaN</td>
      <td>HS-grad</td>
      <td>NaN</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United-States</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>11568</td>
      <td>NaN</td>
      <td>5566</td>
      <td>NaN</td>
      <td>8003</td>
      <td>2113</td>
      <td>6972</td>
      <td>14864</td>
      <td>11590</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>15393</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8739.500000</td>
      <td>38.720995</td>
      <td>NaN</td>
      <td>1.897610e+05</td>
      <td>NaN</td>
      <td>10.036556</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1076.644508</td>
      <td>83.87460</td>
      <td>40.002460</td>
      <td>NaN</td>
      <td>0.234897</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5046.185688</td>
      <td>14.079617</td>
      <td>NaN</td>
      <td>1.049929e+05</td>
      <td>NaN</td>
      <td>2.604415</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7439.498620</td>
      <td>396.03288</td>
      <td>12.671265</td>
      <td>NaN</td>
      <td>0.423947</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>17.000000</td>
      <td>NaN</td>
      <td>1.228500e+04</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>4369.750000</td>
      <td>27.000000</td>
      <td>NaN</td>
      <td>1.181558e+05</td>
      <td>NaN</td>
      <td>9.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>38.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>8739.500000</td>
      <td>37.000000</td>
      <td>NaN</td>
      <td>1.781340e+05</td>
      <td>NaN</td>
      <td>10.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>40.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>13109.250000</td>
      <td>48.000000</td>
      <td>NaN</td>
      <td>2.373180e+05</td>
      <td>NaN</td>
      <td>12.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>45.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>17479.000000</td>
      <td>90.000000</td>
      <td>NaN</td>
      <td>1.455435e+06</td>
      <td>NaN</td>
      <td>16.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>99999.000000</td>
      <td>4356.00000</td>
      <td>99.000000</td>
      <td>NaN</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>

hours.per.week에 일주일에 99시간 일하는 사람이 있는데 이상치로 구분하고 제거해야할 것 같습니다.

</br>

# 상관분석

다음은 상관관계 분석을 위해 변수를 구분했습니다

```python
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

    범주형 : ['workclass', 'education', 'education.num', 'marital.status', 'occupation', 'relationship', 'race', 'sex', 'target']
    연속형:  ['id', 'age', 'fnlwgt', 'capital.gain', 'capital.loss', 'hours.per.week', 'native.country']

범주형 분석 후에 숫자형 분석을 진행했습니다.

![png](/assets/images/income_predict/output_9_1.png)

education과 education.num의 상관관계 1이 나왔습니다.
education과 education.num은 확인해본 결과 같은 변수였습니다. 항목 이름만 바꾼 같은 변수이기 때문에, 둘중에 한 변수는 제거해 줄 예정입니다.

<br>

sex, relationship도 상관관계가 있는데, husband, wife 같은 항목 때문에 그런 것 같다는 생각을 했습니다.

다음은 숫자형 변수 상관관계를 구해봤습니다.
    
![png](/assets/images/income_predict/output_10_1.png)

숫자형에서 눈에 띄는 상관관계는 없었습니다.

```python
import matplotlib.pyplot as plt 
train.plot(kind='box',subplots=True,layout = (7,4),figsize=(20,30))
plt.tight_layout()
plt.show()
```
![png](/assets/images/income_predict/output_11_0.png)
    
이상치가 상당히 많이 나왔습니다.
특히 capital.gain에 튀어나온 저 값은 없애야 겠네요.

## 기본 EDA 결과


1. workclass, occupation 에 결측치 존재

- 직업이 없으면 결측치 인가? -> 생각해 볼 점

- test 데이터에는 workclass의 Never-worked가 없음


3. education.num과 education은 같은 변수 -> 하나를 제거

4. relationship, sex가 상관관계가 있음(0.6 이상)
 
6. 이상치

- 이상치가 매우 많은데 자본이익이나 자본손실의 경우 0이 많기 떄문에 많을 수 밖에 없고, 주당 근무시간과 fnlwgt의 변수 특성상 이상치가 많을 수 밖에 없음. 또한 대부분 극단적인 이상치가 아니기 때문에 어떻게 처리해야할지 고민


# 변수분석
---

중복되는 코드는 생략하겠습니다!

## 연속형변수

연속형 변수는 히스토그램으로 설정하고, hue를 target으로 설정해서 각 구간마다 target이 어떻게 분포되어 있는 지 확인했습니다. 그리고 정규분포를 따르지 않는 것이 많아서 로그변환한 것도 시각화 하여 살펴봤습니다 

### age


```python
f, axes = plt.subplots(2, figsize=(8, 6))

sns.histplot(x=train['age'],bins=10,hue=train['target'], ax=axes[0])
sns.histplot(x=np.log1p(train['age']),bins=10,hue=(train['target']), ax=axes[1])
```

![png](/assets/images/income_predict/output_18_1.png)
    

- 나이대에 따라 타겟의 비율이 다른 것을 확인 -> 나이대로 범주형 변수 생성

### fnlwgt

![png](/assets/images/income_predict/output_21_1.png)
    

- 점수별로 큰 차이가 있는 것 같지는 않음

### capital.gain

![png](/assets/images/income_predict/output_24_1.png)


- 0 값이 너무 많음 -> 로그변환 의미x

- 자본이익이 많다고 해서 무조건 타겟이 1인 것도 아님

### capital.loss
    
![png](/assets/images/income_predict/output_27_1.png)

- 자본이익과 마찬가지

### hours.per.week

![png](/assets/images/income_predict/output_30_1.png)

- 업무시간이 늘수록 타겟이 1인 비율이 확실하게 느는 것을 확인할 수 있음



## 서열형 변수

서열형도 분류했는데 하나밖에 없네요 ㅋㅋ

### education.num

```python
sns.histplot(x=train['education.num'],bins=10,hue=train['target'])
```

![png](/assets/images/income_predict/output_34_1.png)
    


- 교육수준이 높을 수록 타겟이 1일 확율이 훨씬 높음


## 명목형 변수

명목형은 countplot이랑 pointplot을 이용해서 항목마다 타겟의 비율을 시각화하고 타겟의 평균값을 시각화했습니다. 

### edcation


![png](/assets/images/income_predict/output_38_0.png)
    


- 교육수준이 높을 수록 타겟 1의 비율이 높음

### marital.status


```python
i='marital.status'

f, ax = plt.subplots(1,2,figsize=(12,6))

sns.countplot(x=i,hue='target',data=train, ax=ax[0])
plt.sca(ax[0])
plt.xticks(rotation=90)

sns.pointplot(x=i,y='target',data=train, ax=ax[1],join=False)
plt.sca(ax[1])
plt.xticks(rotation=90)

plt.show()
```
    
![png](/assets/images/income_predict/output_41_0.png)
    


- Married-civ-spouse가 다른 항목에 비해 1의 비율의 압도적으로 많음

### relationship

![png](/assets/images/income_predict/output_44_0.png)
    


- Husband, Wife가 다른 항목에 비해 1의 비율이 압도적으로 높음

### race

![png](/assets/images/income_predict/output_47_0.png)
    


- white가 상대적으로 타켓 1의 비율이 높음

### sex
    
![png](/assets/images/income_predict/output_50_0.png)
    


- 남성이 타겟 1의 비율이 높음

### workclass
 
![png](/assets/images/income_predict/output_53_0.png)
    


- workclass에 따라 확실히 타겟의 비율이 크게 다름

### occupation
 
![png](/assets/images/income_predict/output_56_0.png)
    


- 직업 종류에 따라 타겟의 분포가 다양함

### native.country

![png](/assets/images/income_predict/output_59_0.png)
    

- 범주가 너무 많음

## 변수 분석 결과

- age는 범주형 변수로 변환

- fnlwgt는 로그변환하고 나머지 연속형 변수는 그대로 사용하기로함

- education.num, relationship, native.country 제거

- 범주형 변수를 더 낮은 수준의 범주로 변환할지 고민
