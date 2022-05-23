---
layout: single
title:  신용등급 예측 대회 EDA3
categories: 신용등급예측대회
tag: [python, EDA, ML]
toc: true
author_profile: true
sidebar:
  nav: docs
---

다음 포스트에 이어서 이번엔 각 변수에 어떤 값이 많고 값들중에서 credit이 어떻게 분포되어 있는지 countplot과 histplot으로 확인해봤습니다.

앞 포스트에서 말했지만 credit이 낮을수록 신용등급이 높은 거라고 합니다. 매우 헷갈리지만 꼭 참고해야할 사항입니다.

변수별로 알아보기 전에 타겟인 credit에 대한 정규성 검정을 먼저 해보겠습니다.
데이터양이 5000이상이기 때문에 앤더슨 달링  정규성 검정을 시행하였습니다.

```python
stats.anderson(x = train['credit'], dist = 'norm')
```
    AndersonResult(statistic=3950.0460986285034, critical_values=array([0.576, 0.656, 0.787, 0.918, 1.092]), significance_level=array([15. , 10. ,  5. ,  2.5,  1. ]))
    
통계량이 3950이 나와서 정규성을 만족하지 못합니다.

# 범주형 변수

범주형 변수에 대해서 먼저 알아보겠습니다. 먼저 시각화를 보여주고 등분산성 검정인 레벤 검정과 집단간 차이를 분석하는 크루스칼 윌리스 검정을 시행하였습니다.   

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

시각화 결과
- F,M에 따라 credit 분포, 평균의 차이가 크지 않음

왼쪽그림은 항목마다 타겟인 0,1,2가 어떻게 분포되있는지 알수 있고, 오른쪽그림은 항목마다의 타겟 평균을 시각화 한 것입니다 여기에서는 성별에 따라 타겟의 평균이 크게 차이가 나지 않는 것으로 생각할 수 있습니다. 

**변수만 바꿔서 코드를 사용했기 때문에 똑같은 코드는 생략하겠습니다**

## car

    N    16410
    Y    10047
    Name: car, dtype: int64
    
![png](/assets/images/credit_predict/output_30_1.png)
    
- 자동차 소유 여부에 따라 credit 분포, 평균의 차이가 미세하게 남</br>

일반적으로는 자동차를 소유한 것이 신용이 더 좋아야할 것 같은데, 이 데이터에서는 그렇지가 않습니다. 제가 생각하기에는 아마 차량 소유자가 상대적으로 적어서 그렇거나 자동차 유지비, 자동차 리스 비용, 자동차 할부금등에 영향을 받은 것 같습니다.


## reality


    Y    17830
    N     8627
    Name: reality, dtype: int64
    

![png](/assets/images/credit_predict/output_33_1.png)
    

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
    

- 나머지 0,1,2,3,4 일 때는 비슷하나 5일때는 낮은 credit이 나온 것을 확인할 수 있었음.

</br>

이미 신용이 좋지 않기 때문에 아이를 안낳는 경우, 신용이 좋아 아이를 더 낳는 경우가 있다고 생각했습니다. 아이의 수가 7, 14, 19일 때, 신용점수가 낮은 것을 확인할 수 있습니다. 논리적으로는 아이가 많으면 신용점수가 낮아질 확률이 높을 것 같습니다.  하지만 데이터상 해당 데이터가 이상치이고, 범주에 속한 데이터가 매우 적기 때문에 유의미하게 사용할 수는 없을 것 같다는 생각을 하였습니다.

## income_type

    Working                 13645
    Commercial associate     6202
    Pensioner                4449
    State servant            2154
    Student                     7
    Name: income_type, dtype: int64
    
![png](/assets/images/credit_predict/output_39_1.png)
    
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
    

- 교육 타입에 따라 credit 분포, 평균의 차이가 미세하게 있음
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
    
- 주거타입에 따라 다양한 분포와 평균을 보임
- Rented apartment일 경우 credit이 더 낮음 -> 데이터가 적어 나온 것으로 예상
</br>

대부분 데이터가 한쪽으로 몰려있기 때문에 다른 항목의 데이터 수가 작아져서 항목마다 유의미한 차이가 나오지 않습니다.

## FLAG_MOBIL

    1    26457
    
    Name: FLAG_MOBIL, dtype: int64
    

![png](/assets/images/credit_predict/output_51_1.png)
    
- 0값이 없음 -> 의미없는 변수임으로 제거</br>

## work_phone

    0    20511
    1     5946
    Name: work_phone, dtype: int64
    

![png](/assets/images/credit_predict/output_54_1.png)
    
- 직장용 휴대폰을 소지한 사람이 미세하게 낮은 credit을 가짐

## phone

    0    18672
    1     7785
    Name: phone, dtype: int64
  
![png](/assets/images/credit_predict/output_57_1.png)
    
phone은 집전화에 대한 데이터라고 하네요

- phone이 없는 사람이 더 낮은 credit을 가짐

최근 모두 휴대폰을 소지하면서 집전화를 사용하지 않는 집이 많고, 가족구성원이 모두 휴대폰을 가지지 않았을 경우에 집전화가 있다고 가정하면 이해가 됩니다.

## email


    0    24042
    1     2415
    Name: email, dtype: int64
    
![png](/assets/images/credit_predict/output_60_1.png)

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
    

- 가족 수에 따라 credit이 매우 큰 차이를 보임
- 가족수가 7이상이면 데이터가 매우 적어져 판단이 어려워짐

child_num과 비슷하게 생각하였습니다. child_num과 family_size는 상관관계가 크므로 둘 중에 한 변수는 삭제해야 합니다. 아이의 수가 직관적으로 신용점수에 더 영향을 줄 것 같아서 family_size를 없앨 예정입니다. 가족 구성원 중에 성인이 포함될 수 있기 때문에 family_size는 상대적으로 부적절할 것 같습니다.

## credit

    2    16968
    1     6267
    0     3222
    Name: credit, dtype: int64

![png](/assets/images/credit_predict/output_69_1.png)


- 타겟 변수가 매우 불균형함. => 샘플링 고려</br>

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
    


- 수입이 1인 구간에서 0이 더 많이 나옴 
- 수입이 높은데 credit이 높게 나옴</br>

이것도 잘못된 데이터인감...

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

오늘 포스팅은 여기서 마치겠습니다.