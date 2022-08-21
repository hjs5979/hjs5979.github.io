---
layout: single
title:  신용등급 예측 대회 EDA
categories: 신용등급예측대회
tag: [python, EDA, ML]
toc: true
author_profile: true
sidebar:
  nav: docs
---

![png](/assets/images/credit_predict/title.PNG)

<https://dacon.io/competitions/official/235713/overview/description>

# 신용카드 사용자 연체 예측

옛날에 참여했던 대회인데 이제 정리하고 올리게 됐습니다.

목표는 여러 고객 데이터 변수들을 이용하여 credit(신용점수) 변수를 예측하는 것입니다.

주어진 변수는 다음과 같습니다. 타겟변수를 포함해서 19개입니다.

- gender: 성별
- car: 차량 소유 여부
- reality: 부동산 소유 여부
- child_num: 자녀 수
- income_total: 연간 소득
- income_type: 소득 분류
- edu_type: 교육 수준
- family_type: 결혼 여부
- house_type: 생활 방식
- DAYS_BIRTH: 출생일, 데이터 수집 당시 (0)부터 역으로 셈  <br/> 즉, -1은 데이터 수집일 하루 전에 태어났음을 의미
- DAYS_EMPLOYED: 업무 시작일, 데이터 수집 당시 (0)부터 역으로 셈 <br/> 즉, -1은 데이터 수집일 하루 전부터 일을 시작함을 의미
<br/> 양수 값은 고용되지 않은 상태를 의미!
- FLAG_MOBIL: 핸드폰 소유 여부
- work_phone: 업무용 전화 소유 여부
- phone: 전화 소유 여부
- email: 이메일 소유 여부
- occyp_type: 직업 유형		
- family_size: 가족 규모
- begin_month: 신용카드 발급 월, 데이터 수집 당시 (0)부터 역으로 셈 <br/> 즉, -1은 데이터 수집일 한 달 전에 신용카드를 발급함을 의미
- credit: 사용자의 신용카드 대금 연체를 기준으로 한 신용도<br/>  **=>낮을 수록 높은 신용의 신용카드 사용자를 의미함**


# 기본 EDA

간단히 변수에 대해서 살펴보겠습니다.

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 26457 entries, 0 to 26456
    Data columns (total 20 columns):
     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   index          26457 non-null  int64  
     1   gender         26457 non-null  object 
     2   car            26457 non-null  object 
     3   reality        26457 non-null  object 
     4   child_num      26457 non-null  int64  
     5   income_total   26457 non-null  float64
     6   income_type    26457 non-null  object 
     7   edu_type       26457 non-null  object 
     8   family_type    26457 non-null  object 
     9   house_type     26457 non-null  object 
     10  DAYS_BIRTH     26457 non-null  int64  
     11  DAYS_EMPLOYED  26457 non-null  int64  
     12  FLAG_MOBIL     26457 non-null  int64  
     13  work_phone     26457 non-null  int64  
     14  phone          26457 non-null  int64  
     15  email          26457 non-null  int64  
     16  occyp_type     18286 non-null  object 
     17  family_size    26457 non-null  int64  
     18  begin_month    26457 non-null  int64  
     19  credit         26457 non-null  int64  
    dtypes: float64(1), int64(11), object(8)
    memory usage: 4.0+ MB
    

26457개의 데이터가 있고, occyp_type에 약 8000개의 결측치가 있는 것을 확인할 수 있습니다. 숫자형 데이터가 12개, 오브젝트형이 8개입니다.


변수의 통계량에 대해서 살펴보겠습니다.
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
      <th>index</th>
      <th>gender</th>
      <th>car</th>
      <th>reality</th>
      <th>child_num</th>
      <th>income_total</th>
      <th>income_type</th>
      <th>edu_type</th>
      <th>family_type</th>
      <th>house_type</th>
      <th>DAYS_BIRTH</th>
      <th>DAYS_EMPLOYED</th>
      <th>FLAG_MOBIL</th>
      <th>work_phone</th>
      <th>phone</th>
      <th>email</th>
      <th>occyp_type</th>
      <th>family_size</th>
      <th>begin_month</th>
      <th>credit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>26457.000000</td>
      <td>26457</td>
      <td>26457</td>
      <td>26457</td>
      <td>26457.000000</td>
      <td>2.645700e+04</td>
      <td>26457</td>
      <td>26457</td>
      <td>26457</td>
      <td>26457</td>
      <td>26457.000000</td>
      <td>26457.000000</td>
      <td>26457.0</td>
      <td>26457.000000</td>
      <td>26457.000000</td>
      <td>26457.000000</td>
      <td>18286</td>
      <td>26457.000000</td>
      <td>26457.000000</td>
      <td>26457.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>F</td>
      <td>N</td>
      <td>Y</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Working</td>
      <td>Secondary / secondary special</td>
      <td>Married</td>
      <td>House / apartment</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Laborers</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>17697</td>
      <td>16410</td>
      <td>17830</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13645</td>
      <td>17995</td>
      <td>18196</td>
      <td>23653</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4512</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>13228.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.428658</td>
      <td>1.873065e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-15958.053899</td>
      <td>59068.750728</td>
      <td>1.0</td>
      <td>0.224742</td>
      <td>0.294251</td>
      <td>0.091280</td>
      <td>NaN</td>
      <td>2.196848</td>
      <td>-26.123294</td>
      <td>1.519560</td>
    </tr>
    <tr>
      <th>std</th>
      <td>7637.622372</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.747326</td>
      <td>1.018784e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4201.589022</td>
      <td>137475.427503</td>
      <td>0.0</td>
      <td>0.417420</td>
      <td>0.455714</td>
      <td>0.288013</td>
      <td>NaN</td>
      <td>0.916717</td>
      <td>16.559550</td>
      <td>0.702283</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>2.700000e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-25152.000000</td>
      <td>-15713.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>-60.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>6614.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>1.215000e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-19431.000000</td>
      <td>-3153.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>2.000000</td>
      <td>-39.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>13228.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>1.575000e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-15547.000000</td>
      <td>-1539.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>2.000000</td>
      <td>-24.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>19842.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>2.250000e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-12446.000000</td>
      <td>-407.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>3.000000</td>
      <td>-12.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>26456.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.000000</td>
      <td>1.575000e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-7705.000000</td>
      <td>365243.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>20.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
    </tr>
  </tbody>
</table>
</div>

FLAG_MOBIL의 통계량이 표준편차를 제외하고 1로 나오는 것을 보아, 값이 하나밖에 없다는 것을 알 수 있습니다.이러한 변수는 분석에 의미가 없으므로 나중에 지워주는게 좋을 것 같습니다.

다음은 boxplot으로 데이터 분포를 확인하겠습니다.

![png](/assets/images/credit_predict/output_8_0.png)
    
- 역시 FLAG_MOBIL는 값이 1밖에 없습니다. 

- 그 외에는 child_num, family_size, DAYS_EMPLOYED, income_total의 이상치가 눈에 띕니다.  

- DAYS_EMPLOYED는 논리적으로 불가능한 값(365243)이기 때문에 제거해줘야할 것 같습니다.

- Child_num과 family_size의 이상치는 논리적으로 가능한 값이지만 변수를 자세히 살펴보고 처리방법을 생각해야할 것 같습니다.

- income_total는 소득이라는 특성상 이상치가 발생할 수 밖에 없을 것 같습니다. 변수를 자세히 살펴보고 처리방법을 생각해보는 것이 좋을 것 같습니다.

```python
train.duplicated(subset=['income_total', 'DAYS_BIRTH'], keep=False).value_counts()
```

    True     23367
    False     3090
    dtype: int64

대회에서 화두가 되었던 중복데이터입니다. income_total과 DAYS_BIRTH를 집합으로 삼아서 중복을 확인해본 결과, 23367개가 중복상태에 있다는 것을 확인할 수 있습니다. 한 고객(income과 생일 동일)이 신용카드를 여러 개 사용하는 경우가 있아서 이런 중복데이터가 생긴 것 같습니다.

## 기본 EDA 결과정리

<br/>

1. child_num, DAYS_EMPLOYED, family_size, income_total에서 이상치 발견
<br/>
=> DAYS_EMPLOYED의 이상치 제거, 나머지는 변수체크

2. 23367개의 행이 중복관계에 있음.
3. FLAG_MOBIL은 값이 1 하나 밖에 없음 -> 제거

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

변수별로 알아보기 전에 타겟인 credit에 대한 정규성 검정을 먼저 해보겠습니다.
데이터양이 5000이상이기 때문에 앤더슨 달링  정규성 검정을 시행하였습니다.

```python
stats.anderson(x = train['credit'], dist = 'norm')
```
```
AndersonResult(statistic=3950.0460986285034, critical_values=array([0.576, 0.656, 0.787, 0.918, 1.092]), significance_level=array([15. , 10. ,  5. ,  2.5,  1. ]))
```
통계량이 3950이 나와서 정규성을 만족하지 못합니다.

# 범주형 변수

범주형 변수에 대해서 먼저 알아보겠습니다. 먼저 시각화를 보여주고 등분산성 검정인 레벤 검정과 집단간 차이를 분석하는 크루스칼 윌리스 검정을 시행하였습니다. 크루스칼 윌리스는 중앙값을 통해 집단간의 차이를 검정합니다.
모든 검정은 유의수준 0.05로 설정되어 있습니다.

## gender

```python
var = cate_feat[0]

print(train[var].value_counts())

f, ax = plt.subplots(1,2,figsize=(12,6))

sns.countplot(x=var,hue='credit',data=train, ax=ax[0])
plt.sca(ax[0])
plt.xticks(rotation=90)

sns.pointplot(x=var,y='credit',data=train, ax=ax[1],join=False)
plt.sca(ax[1])
plt.xticks(rotation=90)

plt.show()
```

    F    17697
    M     8760
    Name: gender, dtype: int64
    
    
![png](/assets/images/credit_predict/output_27_1.png)

```python
l = levene(train['credit'][train['gender']=='M'],
            train['credit'][train['gender']=='F'], center = 'median')

print(l)
```
    LeveneResult(statistic=0.06450894839536328, pvalue=0.7995082562475118)

```python
k = kruskal(train['credit'][train['gender']=='M'], train['credit'][train['gender']=='F'])

print(k)
```
    KruskalResult(statistic=0.1755447765377594, pvalue=0.6752302814510168)

레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.
검정코드 또한 다음 변수부터는 생략하였습니다.

시각화 결과
- F,M에 따라 credit 분포, 평균의 차이가 크지 않음

왼쪽그림은 항목마다 타겟인 0,1,2가 어떻게 분포되있는지 알수 있고, 오른쪽그림은 항목마다의 타겟 평균을 시각화 한 것입니다 여기에서는 성별에 따라 타겟의 평균이 크게 차이가 나지 않는 것으로 생각할 수 있습니다. 

**변수만 바꿔서 코드를 사용했기 때문에 똑같은 코드는 생략하겠습니다**

## car

    N    16410
    Y    10047
    Name: car, dtype: int64
    
![png](/assets/images/credit_predict/output_30_1.png)
    
레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.

- 자동차 소유 여부에 따라 credit 분포, 평균의 차이가 크지 않음.

일반적으로는 자동차를 소유한 것이 신용이 더 좋아야할 것 같은데, 이 데이터에서는 그렇지가 않습니다. 제가 생각하기에는 아마 차량 소유자가 상대적으로 적어서 그렇거나 자동차 유지비, 자동차 리스 비용, 자동차 할부금등에 영향을 받은 것 같습니다.


## reality


    Y    17830
    N     8627
    Name: reality, dtype: int64
    

![png](/assets/images/credit_predict/output_33_1.png)
    
레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.

- 부동산 소유 여부에 따라 credit 분포, 평균의 차이가 미세하게 남
- 당연하게도 부동산을 가진 사람이 미세하게 더 낮은 credit값을 가짐.

부동산을 가진 사람이 신용점수가 높다고 생각할 수 있습니다. 하지만 그 차이가 생각보다 크지 않습니다. 개인신용평가에 대해서 정확히는 모르지만 부동산 대출이 영향을 줬을수도 있을 것 같습니다.

## child_num


    0     18340
    1      5386
    2      2362
    3       306
    4        47
    5        10
    14        3
    7         2
    19        1
    Name: child_num, dtype: int64
    
    
![png](/assets/images/credit_predict/output_36_1.png)
    
레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.
사후 검정이 필요할 것 같은데, 이 프로젝트에서는 진행하지 않았습니다.

- 나머지 0,1,2,3,4 일 때는 비슷하나 5일때는 낮은 credit이 나온 것을 확인할 수 있었음.

이미 신용이 좋지 않기 때문에 아이를 안낳는 경우, 신용이 좋아 아이를 더 낳는 경우가 있다고 생각했습니다. 아이의 수가 7, 14, 19일 때, 신용점수가 낮은 것을 확인할 수 있습니다. 논리적으로는 아이가 많으면 신용점수가 낮아질 확률이 높을 것 같습니다.  하지만 데이터상 해당 데이터가 이상치이고, 범주에 속한 데이터가 매우 적기 때문에 유의미하게 사용할 수는 없을 것 같다는 생각을 하였습니다.

## income_type

    Working                 13645
    Commercial associate     6202
    Pensioner                4449
    State servant            2154
    Student                     7
    Name: income_type, dtype: int64
    
![png](/assets/images/credit_predict/output_39_1.png)

레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.
    
- 소득 타입에 따라 credit 분포, 평균의 차이가 크지 않음

- Student에 속한 데이터가 적기 때문에 퍼지는 양상을 보임

## edu_type

    Secondary / secondary special    17995
    Higher education                  7162
    Incomplete higher                 1020
    Lower secondary                    257
    Academic degree                     23
    Name: edu_type, dtype: int64
    
![png](/assets/images/credit_predict/output_42_1.png)
    
레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.

- 교육 타입에 따라 credit 분포, 평균의 차이가 매우 미세함
- Academic degree에 속한 데이터가 적기 때문에 퍼지는 양상을 보임
- Higher, Academic degree가 교육수준이 높기 때문에 미세하기 더 낮은 credit을 보였다고 생각

## family_type



    Married                 18196
    Single / not married     3496
    Civil marriage           2123
    Separated                1539
    Widow                    1103
    Name: family_type, dtype: int64
    
 
![png](/assets/images/credit_predict/output_45_1.png)

레벤 검정에 의해 등분산성을 만족하지 않고, welch-anova 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.

- 결혼 상황에 따라 credit 분포가, 평균이 미세하게 다름
- Civil marrige 관계에 있을 경우 낮은 credit 수치가 눈에 띔.

Civil marrige은 법률적으로 혼인한 관계이고, Married는 동거같은 비법률적인 혼인관계라고 합니다.

## house_type


    House / apartment      23653
    With parents            1257
    Municipal apartment      818
    Rented apartment         429
    Office apartment         190
    Co-op apartment          110
    Name: house_type, dtype: int64
    

![png](/assets/images/credit_predict/output_48_1.png)

레벤 검정에 의해 등분산성을 만족하지 않고, welch-anova 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.
    
- 주거타입에 따라 다양한 분포와 평균을 보임
- Rented apartment일 경우 credit이 더 낮음 -> 데이터가 적어 나온 것으로 예상


대부분 데이터가 한쪽으로 몰려있기 때문에 다른 항목의 데이터 수가 작아져서 항목마다 유의미한 차이가 나오지 않습니다.

## FLAG_MOBIL

    1    26457
    
    Name: FLAG_MOBIL, dtype: int64
    

![png](/assets/images/credit_predict/output_51_1.png)
    
- 0값이 없음 -> 의미없는 변수임으로 제거

## work_phone

    0    20511
    1     5946
    Name: work_phone, dtype: int64

레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.

![png](/assets/images/credit_predict/output_54_1.png)
    
- 직장용 휴대폰을 소지한 사람이 미세하게 낮은 credit을 가짐

## phone

    0    18672
    1     7785
    Name: phone, dtype: int64
  
![png](/assets/images/credit_predict/output_57_1.png)
    
phone은 집전화에 대한 데이터라고 하네요

레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.

- phone이 없는 사람이 더 낮은 credit을 가짐

최근 모두 휴대폰을 소지하면서 집전화를 사용하지 않는 집이 많고, 가족구성원이 모두 휴대폰을 가지지 않았을 경우에 집전화가 있다고 가정하면 이해가 됩니다.

## email


    0    24042
    1     2415
    Name: email, dtype: int64
    
![png](/assets/images/credit_predict/output_60_1.png)

레벤 검정에 의해 등분산성을 만족하지 않고, welch-anova 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.

- email이 없는 사람이 낮은 credit을 가짐
- 데이터가 많아서 그런 것으로 생각할 수 있음

데이터가 많아서 생기는 오류이거나 조사할 때의 오류라고 생각됩니다. 최근 이메일이 없는 사람을 찾기 힘들기 때문에 이러한 데이터 결과가 유의미해 보이지 않습니다.

## occyp_type

    Laborers                 4512
    Core staff               2646
    Sales staff              2539
    Managers                 2167
    Drivers                  1575
    High skill tech staff    1040
    Accountants               902
    Medicine staff            864
    Cooking staff             457
    Security staff            424
    Cleaning staff            403
    Private service staff     243
    Low-skill Laborers        127
    Waiters/barmen staff      124
    Secretaries                97
    Realty agents              63
    HR staff                   62
    IT staff                   41
    Name: occyp_type, dtype: int64

![png](/assets/images/credit_predict/output_63_1.png)

레벤 검정에 의해 등분산성을 만족하지 않고, welch-anova 검정에 의해 집단간의 차이가 있다는 결론이 나왔습니다.
    
- 직업에 따라 다양한 credit 분포를 보임

## family_size

    2     14106
    1      5109
    3      4632
    4      2260
    5       291
    6        44
    7         9
    15        3
    9         2
    20        1
    Name: family_size, dtype: int64

![png](/assets/images/credit_predict/output_66_1.png)
    
레벤 검정에 의해 등분산성을 만족하고, 크루스칼 검정에 의해 집단간의 차이가 없다고 볼 수 있습니다.

- 가족 수에 따라 credit이 매우 큰 차이를 보임
- 가족수가 7이상이면 데이터가 매우 적어져 판단이 어려워짐

child_num과 비슷하게 생각하였습니다. child_num과 family_size는 상관관계가 크므로 둘 중에 한 변수는 삭제해야 합니다. 아이의 수가 직관적으로 신용점수에 더 영향을 줄 것 같아서 family_size를 없앨 예정입니다. 가족 구성원 중에 성인이 포함될 수 있기 때문에 family_size는 상대적으로 부적절할 것 같습니다.

## credit

    2    16968
    1     6267
    0     3222
    Name: credit, dtype: int64

![png](/assets/images/credit_predict/output_69_1.png)


- 타겟 변수가 매우 불균형함. => 샘플링 고려

smote 오버 샘플링을 해서 학습을 해봤지만 성능이 좋아지지는 않았습니다.

# 연속형 변수

---

**범주형으로 처리했을 때(10개 구간), 어떤 양상을 보이는지 확인하기 위해 전처리하고 시각화하였습니다.**

```python
# 10개 구간으로 나눔
df = train.copy()
for i in num_feat:
    bins = np.linspace(df[i].min(), df[i].max(), 10)
    df[i] = np.digitize(df[i], bins)
```



## income_total

```python
var = num_feat[0]

f, axes = plt.subplots(1, figsize=(8, 6))

sns.histplot(x=train[var],bins=10,hue=train['credit'])
```

![png](/assets/images/credit_predict/output_74_1.png)


```python
print(df[var].value_counts())

f, ax = plt.subplots(1,2,figsize=(15,8))

sns.countplot(x=var,hue='credit',data=df, ax=ax[0])
plt.sca(ax[0])
plt.xticks(rotation=90)

sns.pointplot(x=var,y='credit',data=df, ax=ax[1],join=False)
plt.sca(ax[1])
plt.xticks(rotation=90)

plt.show()
```

    1     16583
    2      8724
    3       876
    4       186
    5        43
    6        34
    10        5
    8         4
    7         2
    Name: income_total, dtype: int64
    


    
![png](/assets/images/credit_predict/output_75_1.png)

```python
from statsmodels.miscmodels.ordinal_model import OrderedModel

mod_log = OrderedModel(df['credit'],
                        df['income_total'],
                        distr='logit')

res_log = mod_log.fit(method='bfgs', disp=False)
res_log.summary()
```

순서형 로지스틱 회귀모형을 통해 credit과 income_total의 관계를 검정해보려 하였으나, AIC와 BIC의 통계값이 너무 커 모델의 적합성의 의심되기 때문에 이는 시각적 방법으로 대체한다.

- 수입이 1인 구간에서 0이 더 많이 나옴 
- 수입이 높은데 credit이 높게 나옴


#### DAYS_BIRTH

![png](/assets/images/credit_predict/output_78_1.png)
    

    6     4247
    7     3970
    8     3775
    5     3517
    4     3158
    3     3015
    2     2614
    1     1120
    9     1040
    10       1
    Name: DAYS_BIRTH, dtype: int64
    


![png](/assets/images/credit_predict/output_79_1.png)
    

- 데이터가 적은 구간을 제외하면 credit이 비슷한 분포를 보임 

#### DAYS_EMPLOYED
 
![png](/assets/images/credit_predict/output_82_0.png)
    
    9     9966
    8     6477
    10    4438
    7     2804
    6     1344
    5      746
    4      383
    3      183
    2       65
    1       51
    Name: DAYS_EMPLOYED, dtype: int64
    
![png](/assets/images/credit_predict/output_83_1.png)
    
- 일을 오래했을수록 신용등급이 좋아보임

#### begin_month


![png](/assets/images/credit_predict/output_86_0.png)

    8     4161
    7     3760
    9     3215
    5     3062
    4     2954
    6     2930
    2     2247
    3     2175
    1     1722
    10     231
    Name: begin_month, dtype: int64

  
![png](/assets/images/credit_predict/output_87_1.png)


- 데이터가 적은 구간을 제외하면 credit이 비슷한 분포를 보임
- 9구간에서 1이 더 많이 나옴

```python
train.duplicated(subset=['gender','income_total', 'income_type',
                         'family_type','DAYS_BIRTH',
                         'email','family_size', 'begin_month'], keep=False).value_counts()

```

    False    21956
    True      4501
    dtype: int64

'gender','income_total', 'income_type','family_type','DAYS_BIRTH', 'email','family_size', 'begin_month'를 함께 봤을 때
중복값이 가장 적게 나타남. 

=> 이 변수들을 섞어 파생변수 생성

## 변수분석 결과

- 타겟 변수에서 2가 가장 많기 때문에 모든 범주형, 연속형에서 2가 매우 많은 비율로 차지하고 있고, 이러한 분포를 뒤엎는 경우는 거의 없었음.
- 결국 유의미한 분류를 위해서는 최대한 많은 변수를 사용하고, 여러 변수를 섞은 파생변수를 만드는 것이 유리해보임