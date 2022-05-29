---
layout: single
title:  부동산 데이터 프로젝트
categories: 프로젝트
tag: [python, EDA]
toc: true
author_profile: true
sidebar:
  nav: docs
---

데이터분석 부트캠프에서 캐글의 부동산 가격 예측 대회 데이터를 통해서 데이터 분석 프로젝트를 진행해봤습니다.
https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques/data


원래는 가격 예측을 하는 프로젝트이지만 데이터에 대한 인사이트를 뽑아내기 위해 새로운 관점으로 데이터를 보려고 노력했습니다.

81개의 컬럼, 1460개의 로우로 구성된 데이터입니다. 캐글의 원 데이터는 컬럼이 더 많은데, 파생된 데이터인 것 같습니다. 피쳐 설명은 위의 링크에 모두 설명이 되어 있습니다.

# 데이터 훑기
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
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 81 columns</p>
</div>


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
      <th>Id</th>
      <th>MSSubClass</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>MasVnrArea</th>
      <th>BsmtFinSF1</th>
      <th>...</th>
      <th>WoodDeckSF</th>
      <th>OpenPorchSF</th>
      <th>EnclosedPorch</th>
      <th>3SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1201.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1452.000000</td>
      <td>1460.000000</td>
      <td>...</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>730.500000</td>
      <td>56.897260</td>
      <td>70.049958</td>
      <td>10516.828082</td>
      <td>6.099315</td>
      <td>5.575342</td>
      <td>1971.267808</td>
      <td>1984.865753</td>
      <td>103.685262</td>
      <td>443.639726</td>
      <td>...</td>
      <td>94.244521</td>
      <td>46.660274</td>
      <td>21.954110</td>
      <td>3.409589</td>
      <td>15.060959</td>
      <td>2.758904</td>
      <td>43.489041</td>
      <td>6.321918</td>
      <td>2007.815753</td>
      <td>180921.195890</td>
    </tr>
    <tr>
      <th>std</th>
      <td>421.610009</td>
      <td>42.300571</td>
      <td>24.284752</td>
      <td>9981.264932</td>
      <td>1.382997</td>
      <td>1.112799</td>
      <td>30.202904</td>
      <td>20.645407</td>
      <td>181.066207</td>
      <td>456.098091</td>
      <td>...</td>
      <td>125.338794</td>
      <td>66.256028</td>
      <td>61.119149</td>
      <td>29.317331</td>
      <td>55.757415</td>
      <td>40.177307</td>
      <td>496.123024</td>
      <td>2.703626</td>
      <td>1.328095</td>
      <td>79442.502883</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>20.000000</td>
      <td>21.000000</td>
      <td>1300.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1872.000000</td>
      <td>1950.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>2006.000000</td>
      <td>34900.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>365.750000</td>
      <td>20.000000</td>
      <td>59.000000</td>
      <td>7553.500000</td>
      <td>5.000000</td>
      <td>5.000000</td>
      <td>1954.000000</td>
      <td>1967.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.000000</td>
      <td>2007.000000</td>
      <td>129975.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>730.500000</td>
      <td>50.000000</td>
      <td>69.000000</td>
      <td>9478.500000</td>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>1973.000000</td>
      <td>1994.000000</td>
      <td>0.000000</td>
      <td>383.500000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>25.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>2008.000000</td>
      <td>163000.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1095.250000</td>
      <td>70.000000</td>
      <td>80.000000</td>
      <td>11601.500000</td>
      <td>7.000000</td>
      <td>6.000000</td>
      <td>2000.000000</td>
      <td>2004.000000</td>
      <td>166.000000</td>
      <td>712.250000</td>
      <td>...</td>
      <td>168.000000</td>
      <td>68.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>8.000000</td>
      <td>2009.000000</td>
      <td>214000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1460.000000</td>
      <td>190.000000</td>
      <td>313.000000</td>
      <td>215245.000000</td>
      <td>10.000000</td>
      <td>9.000000</td>
      <td>2010.000000</td>
      <td>2010.000000</td>
      <td>1600.000000</td>
      <td>5644.000000</td>
      <td>...</td>
      <td>857.000000</td>
      <td>547.000000</td>
      <td>552.000000</td>
      <td>508.000000</td>
      <td>480.000000</td>
      <td>738.000000</td>
      <td>15500.000000</td>
      <td>12.000000</td>
      <td>2010.000000</td>
      <td>755000.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 38 columns</p>
</div>

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1460 entries, 0 to 1459
    Data columns (total 81 columns):
     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   Id             1460 non-null   int64  
     1   MSSubClass     1460 non-null   int64  
     2   MSZoning       1460 non-null   object 
     3   LotFrontage    1201 non-null   float64
     4   LotArea        1460 non-null   int64  
     5   Street         1460 non-null   object 
     6   Alley          91 non-null     object 
     7   LotShape       1460 non-null   object 
     8   LandContour    1460 non-null   object 
     9   Utilities      1460 non-null   object 
     10  LotConfig      1460 non-null   object 
     11  LandSlope      1460 non-null   object 
     12  Neighborhood   1460 non-null   object 
     13  Condition1     1460 non-null   object 
     14  Condition2     1460 non-null   object 
     15  BldgType       1460 non-null   object 
     16  HouseStyle     1460 non-null   object 
     17  OverallQual    1460 non-null   int64  
     18  OverallCond    1460 non-null   int64  
     19  YearBuilt      1460 non-null   int64  
     20  YearRemodAdd   1460 non-null   int64  
     21  RoofStyle      1460 non-null   object 
     22  RoofMatl       1460 non-null   object 
     23  Exterior1st    1460 non-null   object 
     24  Exterior2nd    1460 non-null   object 
     25  MasVnrType     1452 non-null   object 
     26  MasVnrArea     1452 non-null   float64
     27  ExterQual      1460 non-null   object 
     28  ExterCond      1460 non-null   object 
     29  Foundation     1460 non-null   object 
     30  BsmtQual       1423 non-null   object 
     31  BsmtCond       1423 non-null   object 
     32  BsmtExposure   1422 non-null   object 
     33  BsmtFinType1   1423 non-null   object 
     34  BsmtFinSF1     1460 non-null   int64  
     35  BsmtFinType2   1422 non-null   object 
     36  BsmtFinSF2     1460 non-null   int64  
     37  BsmtUnfSF      1460 non-null   int64  
     38  TotalBsmtSF    1460 non-null   int64  
     39  Heating        1460 non-null   object 
     40  HeatingQC      1460 non-null   object 
     41  CentralAir     1460 non-null   object 
     42  Electrical     1459 non-null   object 
     43  1stFlrSF       1460 non-null   int64  
     44  2ndFlrSF       1460 non-null   int64  
     45  LowQualFinSF   1460 non-null   int64  
     46  GrLivArea      1460 non-null   int64  
     47  BsmtFullBath   1460 non-null   int64  
     48  BsmtHalfBath   1460 non-null   int64  
     49  FullBath       1460 non-null   int64  
     50  HalfBath       1460 non-null   int64  
     51  BedroomAbvGr   1460 non-null   int64  
     52  KitchenAbvGr   1460 non-null   int64  
     53  KitchenQual    1460 non-null   object 
     54  TotRmsAbvGrd   1460 non-null   int64  
     55  Functional     1460 non-null   object 
     56  Fireplaces     1460 non-null   int64  
     57  FireplaceQu    770 non-null    object 
     58  GarageType     1379 non-null   object 
     59  GarageYrBlt    1379 non-null   float64
     60  GarageFinish   1379 non-null   object 
     61  GarageCars     1460 non-null   int64  
     62  GarageArea     1460 non-null   int64  
     63  GarageQual     1379 non-null   object 
     64  GarageCond     1379 non-null   object 
     65  PavedDrive     1460 non-null   object 
     66  WoodDeckSF     1460 non-null   int64  
     67  OpenPorchSF    1460 non-null   int64  
     68  EnclosedPorch  1460 non-null   int64  
     69  3SsnPorch      1460 non-null   int64  
     70  ScreenPorch    1460 non-null   int64  
     71  PoolArea       1460 non-null   int64  
     72  PoolQC         7 non-null      object 
     73  Fence          281 non-null    object 
     74  MiscFeature    54 non-null     object 
     75  MiscVal        1460 non-null   int64  
     76  MoSold         1460 non-null   int64  
     77  YrSold         1460 non-null   int64  
     78  SaleType       1460 non-null   object 
     79  SaleCondition  1460 non-null   object 
     80  SalePrice      1460 non-null   int64  
    dtypes: float64(3), int64(35), object(43)
    memory usage: 924.0+ KB

```python
df.plot(kind='box',subplots=True,layout = (4,10),figsize=(30,20))
plt.tight_layout()
plt.show()
```

```python
df.duplicated(keep=False).value_counts()
```

![png](./asset/realestate_project/output_10_0.png)
    

먼저 PoolQC, MiscFeature, Alley의 결측값이 눈에 띕니다. 결측값이 전체 데이터의 90% 이상을 차지하기 때문에 이 피쳐는 삭제해주었습니다. 이상치가 상당히 많지만 모두 논리적으로 가능한 값입니다. 이후에 모델링 과정에서 문제가 될 경우 제거해주겠습니다. 또한 중복값이 없었습니다.

```python
df.drop(['Id', 'Alley', 'PoolQC', 'MiscFeature'], axis=1,inplace=True)
```  

# 상관관계 분석
상관관계 분석을 위해 변수를 범주형/연속형으로 나누어주었습니다. 

```python
cate_feat = []
num_feat = []
for col in df.columns:
    target = df[col]
    if target.nunique() <=20:
        cate_feat.append(col)
    else:
        num_feat.append(col)

print('범주형 :', cate_feat)
print('연속형: ', num_feat)
```
    

범주형 변수에 대해서 크라머 V 상관검정을 시행하였습니다.
    
![png](./asset/realestate_project/output_13_1.png)

    
모든 변수에 대해서 앤더슨 달링 검정을 해봤을 때, 정규성을 만족시키지 못하였기 때문에 스피어만 검정을 진행하겠습니다.
이를 위해 숫자형 변수 리스트를 만들어주고 검정을 시행하였습니다.

    
![png](./asset/realestate_project/output_14_1.png)
    

상관검정에서 발견한 것은 OverallQual이 다른 변수들과의 상관관계가 상대적으로 높다는 것을 알 수 있었습니다. 범주형에서 2개, 연속형에서 12가 0.4 이상의 상관관계를 보였습니다. 이 변수들에 대해서 추가적으로 회귀분석을 통해 인과관계에 대해서도 분석해보도록 하겠습니다. 


스피어만 검정 결과에서 상관계수의 절대값이 0.4 이상인 것만 모은 후 리스트화 해주도록 하겠습니다.

```python
spearman = {}
for col in numeric_feat:
    coll, pval = stats.spearmanr(df['OverallQual'],df[col])
    if abs(coll) > 0.4:
        spearman[col] = [coll, pval]
spearman
```

    {'OverallQual': [1.0, 0.0],
     'YearBuilt': [0.6473919603986984, 3.3837658709097335e-174],
     'YearRemodAdd': [0.557723031928875, 4.068738436513822e-120],
     'TotalBsmtSF': [0.4599152234609295, 2.604989880981451e-77],
     '1stFlrSF': [0.4087296736587941, 6.831267613363967e-60],
     'GrLivArea': [0.6032623582271563, 1.9783219674937198e-145],
     'FullBath': [0.5763716805460413, 5.295162531610398e-130],
     'TotRmsAbvGrd': [0.42780630758341637, 4.900426243720122e-66],
     'Fireplaces': [0.4206263039993349, 1.1206644258814144e-63],
     'GarageCars': [0.608755616437385, 9.140758898975893e-149],
     'GarageArea': [0.5415523897289365, 4.868845537716025e-112],
     'OpenPorchSF': [0.4350463110707547, 1.7898074103423994e-68],
     'SalePrice': [0.8098285862017292, 0.0]}

```python
r_list = list(spearman.keys())
```
종속변수로 사용될 OverallQual은 제거해줍니다.

```python
r_list.remove('OverallQual')
```

OLS를 통해 회귀 분석을 시행합니다.

```python
X = sm.add_constant(df[r_list])
model = sm.OLS(df['OverallQual'], X)
result = model.fit()
print(result.summary())
result.aic
```
```
 OLS Regression Results                            
==============================================================================
Dep. Variable:            OverallQual   R-squared:                       0.697
Model:                            OLS   Adj. R-squared:                  0.694
Method:                 Least Squares   F-statistic:                     277.3
Date:                Sun, 29 May 2022   Prob (F-statistic):               0.00
Time:                        03:58:53   Log-Likelihood:                -1673.1
No. Observations:                1460   AIC:                             3372.
Df Residuals:                    1447   BIC:                             3441.
Df Model:                          12                                         
Covariance Type:            nonrobust                                         
================================================================================
                   coef    std err          t      P>|t|      [0.025      0.975]
--------------------------------------------------------------------------------
const          -25.4703      2.620     -9.723      0.000     -30.609     -20.332
YearBuilt        0.0061      0.001      6.056      0.000       0.004       0.008
YearRemodAdd     0.0087      0.001      6.750      0.000       0.006       0.011
TotalBsmtSF      0.0005   8.67e-05      5.898      0.000       0.000       0.001
1stFlrSF        -0.0005      0.000     -5.257      0.000      -0.001      -0.000
GrLivArea        0.0004    9.1e-05      4.515      0.000       0.000       0.001
FullBath         0.1380      0.054      2.541      0.011       0.031       0.245
TotRmsAbvGrd    -0.0390      0.023     -1.722      0.085      -0.083       0.005
Fireplaces       0.1357      0.037      3.657      0.000       0.063       0.208
GarageCars       0.1851      0.062      2.992      0.003       0.064       0.306
GarageArea      -0.0002      0.000     -0.721      0.471      -0.001       0.000
OpenPorchSF      0.0005      0.000      1.554      0.120      -0.000       0.001
SalePrice     7.679e-06   4.95e-07     15.501      0.000    6.71e-06    8.65e-06
==============================================================================
Omnibus:                       12.828   Durbin-Watson:                   2.024
Prob(Omnibus):                  0.002   Jarque-Bera (JB):               18.768
Skew:                          -0.044   Prob(JB):                     8.41e-05
Kurtosis:                       3.548   Cond. No.                     2.59e+07
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 2.59e+07. This might indicate that there are
strong multicollinearity or other numerical problems.
```
f값을 통해 모형 자체는 유의함을 알 수 있다. 하지만 TotRmsAbvGrd, GarageArea, OpenPorchSF의 t값이 유의하지 않습니다. cond.no 값으로 다중공선성도 존재함을 알 수 있기 때문에 단계적 선택법을 통해 변수를 줄여 모델의 적합성을 올리도록 하겠습니다. 선택의 기준은 AIC로 설정하였습니다. R-squared값은 나쁘지 않습니다.

변수선택법에 대해서 따로 파이썬에서 라이브러리를 제공해주지 않기 때문에 아래의 블로그를 참조하여 함수를 만들어주었습니다.
https://todayisbetterthanyesterday.tistory.com/10
```
  OLS Regression Results                            
==============================================================================
Dep. Variable:            OverallQual   R-squared:                       0.671
Model:                            OLS   Adj. R-squared:                  0.670
Method:                 Least Squares   F-statistic:                     990.0
Date:                Sun, 29 May 2022   Prob (F-statistic):               0.00
Time:                        04:08:33   Log-Likelihood:                -1733.0
No. Observations:                1460   AIC:                             3474.
Df Residuals:                    1456   BIC:                             3495.
Df Model:                           3                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const        -19.2197      1.643    -11.699      0.000     -22.442     -15.997
YearBuilt      0.0117      0.001     13.836      0.000       0.010       0.013
GrLivArea      0.0004   5.85e-05      6.883      0.000       0.000       0.001
SalePrice   9.565e-06   4.45e-07     21.502      0.000    8.69e-06    1.04e-05
==============================================================================
Omnibus:                       23.629   Durbin-Watson:                   2.011
Prob(Omnibus):                  0.000   Jarque-Bera (JB):               42.807
Skew:                          -0.042   Prob(JB):                     5.07e-10
Kurtosis:                       3.835   Cond. No.                     1.56e+07
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.56e+07. This might indicate that there are
strong multicollinearity or other numerical problems.
```

모델도 유의하고 변수들도 모두 유의하지만 여전히 다중공선성이 존재한다고 나옵니다. R-squared 값이 약간 감소했지만 나쁘지 않은 값입니다. 이전에 상관분석에서 상관관계가 높게 나왔던 범주형 변수를 추가해보겠습니다.

```python
from statsmodels.stats.anova import anova_lm
model = sm.OLS.from_formula('OverallQual ~ YearBuilt + FullBath + GarageCars + SalePrice + KitchenQual + ExterQual', df)
result = model.fit()
print(result.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:            OverallQual   R-squared:                       0.714
    Model:                            OLS   Adj. R-squared:                  0.712
    Method:                 Least Squares   F-statistic:                     361.6
    Date:                Sun, 29 May 2022   Prob (F-statistic):               0.00
    Time:                        03:28:22   Log-Likelihood:                -1631.0
    No. Observations:                1460   AIC:                             3284.
    Df Residuals:                    1449   BIC:                             3342.
    Df Model:                          10                                         
    Covariance Type:            nonrobust                                         
    =====================================================================================
                            coef    std err          t      P>|t|      [0.025      0.975]
    -------------------------------------------------------------------------------------
    Intercept            -0.9259      1.748     -0.530      0.596      -4.356       2.504
    KitchenQual[T.Fa]    -0.6901      0.171     -4.032      0.000      -1.026      -0.354
    KitchenQual[T.Gd]    -0.2395      0.100     -2.392      0.017      -0.436      -0.043
    KitchenQual[T.TA]    -0.4795      0.112     -4.292      0.000      -0.699      -0.260
    ExterQual[T.Fa]      -1.7662      0.259     -6.814      0.000      -2.275      -1.258
    ExterQual[T.Gd]      -0.3650      0.131     -2.780      0.006      -0.623      -0.107
    ExterQual[T.TA]      -0.9506      0.145     -6.577      0.000      -1.234      -0.667
    YearBuilt             0.0031      0.001      3.538      0.000       0.001       0.005
    FullBath              0.1960      0.045      4.347      0.000       0.108       0.284
    GarageCars            0.1193      0.036      3.302      0.001       0.048       0.190
    SalePrice           7.66e-06   4.23e-07     18.123      0.000    6.83e-06    8.49e-06
    ==============================================================================
    Omnibus:                       21.200   Durbin-Watson:                   2.020
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               37.166
    Skew:                          -0.027   Prob(JB):                     8.50e-09
    Kurtosis:                       3.780   Cond. No.                     1.78e+07
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.78e+07. This might indicate that there are
    strong multicollinearity or other numerical problems.
    

모델과 변수 모두 유의하고 AIC값이 감소하였습니다. R_squared 값은 증가하였습니다. 여전히 다중공선성이 있다고 나옵니다. 다중공선성을 판단하는 지표인 VIF를 통해 다중공선성을 평가해보겠습니다.

```python
from statsmodels.stats.outliers_influence import variance_inflation_factor

model.exog_names
variance_inflation_factor(model.exog, 1)

pd.DataFrame({'컬럼': column, 'VIF': variance_inflation_factor(model.exog, i)} 
             for i, column in enumerate(model.exog_names)
             if column != 'Intercept')
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
      <th>컬럼</th>
      <th>VIF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KitchenQual[T.Fa]</td>
      <td>2.018734</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KitchenQual[T.Gd]</td>
      <td>6.379096</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KitchenQual[T.TA]</td>
      <td>8.268799</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ExterQual[T.Fa]</td>
      <td>1.690686</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ExterQual[T.Gd]</td>
      <td>10.162757</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ExterQual[T.TA]</td>
      <td>13.034309</td>
    </tr>
    <tr>
      <th>6</th>
      <td>YearBuilt</td>
      <td>1.912422</td>
    </tr>
    <tr>
      <th>7</th>
      <td>FullBath</td>
      <td>1.634292</td>
    </tr>
    <tr>
      <th>8</th>
      <td>GarageCars</td>
      <td>1.931341</td>
    </tr>
    <tr>
      <th>9</th>
      <td>SalePrice</td>
      <td>2.985857</td>
    </tr>
  </tbody>
</table>
</div>

ExterQual의 GD, TA 범주가 10 이상으로 VIF가 높게 나왔습니다. 보통 VIF는 10이상일 때 다중공선성이 있다고 판단합니다. 하지만 이는 사실 주관적인 값이기 때문에 분석가의 재량에 맡기는 경우가 많습니다. 저는 R-squared 값과 AIC 값이 적절하게 나왔기 때문에 더이상 변수처리를 하지 않고 사용하도록 하겠습니다. 이 식 