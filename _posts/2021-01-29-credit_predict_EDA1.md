---
layout: single
title:  신용등급 예측 대회 EDA1
categories: 신용등급예측대회
tag: [python, EDA]
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 데이콘 신용카드 사용자 연체 예측

데이콘에서 진행하는 대회에 참여해봤습니다!

대회 목표는 여러 고객 데이터 변수들을 이용하여 credit(신용점수) 변수를 예측하는 것!!!

변수는 다음과 같습니다! 타겟변수를 포함해서 19개네용

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


**이제 EDA 시작합니다!**


# 기본 EDA

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

```python
train=pd.read_csv('./train.csv')
test=pd.read_csv('./test.csv')
```

```python
train.info()
```

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
    


```python
train.describe(include='all')
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

```python
train.plot(kind='box',subplots=True,layout = (7,4),figsize=(20,30))
plt.tight_layout()
plt.show()
```

![png](/assets/images/credit_predict/output_8_0.png)
    

```python
# sns.pairplot(train) 생략
```

```python
train.duplicated(subset=['income_total', 'DAYS_BIRTH'], keep=False).value_counts()
```

    True     23367
    False     3090
    dtype: int64

<br/>

## 기본 EDA 결과정리

<br/>

1. child_num, days_employed, family_size, income_total에서 이상치 발견
<br/>
=> income_total은 변수 특성에 따라 이상치 처리를 할 필요성이 적고 나머지는 이상치 처리가 필요해 보임.
2. child_num과 family_size는 상관관계가 있음.
3. 23367개의 행이 중복관계에 있음.
4. FLAG_MOBIL은 범주가 1 하나 밖에 없음
5. DAYS_EMPLOYED에 이상치 존재(양수)


## 변수분석 전처리

<br/>

```python
# 인덱스 미리 제거
train.drop(['index'], axis=1, inplace=True)
```


```python
# 변수분석에 방해가 되는 DAYS_EMPLOYED의 양수값을 0으로 변경
train[train['DAYS_EMPLOYED'] >= 0] = 0
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
    