---
layout: single
title:  신용등급 예측 대회 EDA3
categories: 신용등급예측대회
tag: [python, EDA]
toc: true
---

**각 변수별로 credit이 어떻게 분포되어 있는지 countplot과 histplot으로 확인해봤습니당~**

### 범주형 변수

#### gender


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
    
- F,M에 따라 credit 분포, 평균의 차이가 크지 않음

**변수만 바꿔서 코드를 사용했기 때문에 코드는 생략하겠습니당^^**

#### car

    N    16410
    Y    10047
    Name: car, dtype: int64
    
![png](/assets/images/credit_predict/output_30_1.png)
    
- 자동차 소유 여부에 따라 credit 분포, 평균의 차이가 미세하게 남
- 일반적으로는 자동차를 소유한 것이 신용이 더 좋아야한다고 생각되는데, 차량 소유자가 상대적으로 적어서 그렇거나 자동차 유지비등이 신용에 영향을 미쳤다고 생각.


#### reality


    Y    17830
    N     8627
    Name: reality, dtype: int64
    

![png](/assets/images/credit_predict/output_33_1.png)
    


- 부동산 소유 여부에 따라 credit 분포, 평균의 차이가 미세하게 남
- 당연하게도 부동산을 가진 사람이 미세하게 더 낮은 credit값을 가짐

#### child_num


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
    
- 아이의 수가 7, 14, 19일 때, 큰 등급이 나오는 것을 확인할 수 있으나, 이상치이며, 해당 범주에 속한 데이터가 매우 적기 때문에 유의미하게 사용할 수는 없을 것 같음.
- 나머지 0,1,2,3,4 일 때는 비슷하나 5일때는 낮은 credit이 나온 것을 확인할 수 있었음.
- 이미 신용이 좋지 않기 때문에 아이를 안낳는 경우, 신용이 좋아 아이를 더 낳는 경우가 있다고 생각

#### income_type

    Working                 13645
    Commercial associate     6202
    Pensioner                4449
    State servant            2154
    Student                     7
    Name: income_type, dtype: int64
    
![png](/assets/images/credit_predict/output_39_1.png)
    
- 소득 타입에 따라 credit 분포, 평균의 차이가 크지 않음
- Student에 속한 데이터가 적기 때문에 퍼지는 양상을 보임

#### edu_type

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

#### family_type



    Married                 18196
    Single / not married     3496
    Civil marriage           2123
    Separated                1539
    Widow                    1103
    Name: family_type, dtype: int64
    
 
![png](/assets/images/credit_predict/output_45_1.png)
    

- 결혼 상황에 따라 credit 분포가, 평균이 미세하게 다름
- Civil marrige(법률혼)관계에 있을 경우 낮은 credit 수치가 눈에 띔.

#### house_type


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

#### FLAG_MOBIL


    0    20511
    1     5946
    Name: work_phone, dtype: int64
    

![png](/assets/images/credit_predict/output_51_1.png)
    
- 0값이 없음 -> 의미없는 변수임으로 제거

#### work_phone


    0    18672
    1     7785
    Name: phone, dtype: int64
    

![png](/assets/images/credit_predict/output_54_1.png)
    
- 직장용 휴대폰을 소지한 사람이 미세하게 낮은 credit을 가짐

#### phone


    0    24042
    1     2415
    Name: email, dtype: int64
    
  
![png](/assets/images/credit_predict/output_57_1.png)
    

- phone이 없는 사람이 더 낮은 credit을 가짐
- 최근 모두 휴대폰을 소지하면서 phone을 사용하지 않는 집이 많고, 가족구성원이 모두 휴대폰을 가지지 않았을 경우에 phone이 있다고 가정하면 이해가 됨

#### email

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
    
    
![png](/assets/images/credit_predict/output_60_1.png)

- email이 없는 사람이 낮은 credit을 가짐
- 데이터가 많아서 그런 것으로 생각할 수 있음

#### occyp_type

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
    

![png](/assets/images/credit_predict/output_63_1.png)
    
- 직업에 따라 다양한 credit 분포를 보임

#### family_size


    2    16968
    1     6267
    0     3222
    Name: credit, dtype: int64
    

![png](/assets/images/credit_predict/output_66_1.png)
    

- 가족 수에 따라 credit이 매우 큰 차이를 보임

#### credit


    0    24042
    1     2415
    Name: email, dtype: int64
    

![png](/assets/images/credit_predict/output_69_1.png)


- 타겟 변수가 매우 불균형함. => 샘플링 고려 

### 연속형 변수

**범주형으로 처리했을 때, 어떤 양상을 보이는지 확인하기 위해 전처리하였습니당**

```python
# 10개 구간으로 나눔
df = train.copy()
for i in num_feat:
    bins = np.linspace(df[i].min(), df[i].max(), 10)
    df[i] = np.digitize(df[i], bins)
```

#### income_total


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
- 수입이 높은데 credit이 높게 나옴 
=> 잘못된 데이터?

#### DATS_BIRTH

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
    


- 데이터가 적은 구간을 제외하면 credit이 비슷한 분포를 보임

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

## 변수분석 결과

- 타겟 변수에서 2가 가장 많기 때문에 모든 범주형, 연속형에서 2가 매우 많은 비율로 차지하고 있고, 이러한 분포를 뒤엎는 경우는 거의 없었음.
- 결국 유의미한 분류를 위해서는 최대한 많은 변수를 사용하고, 여러 변수를 섞은 파생변수를 만드는 것이 유리해보임