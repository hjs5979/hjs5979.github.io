---
layout: single
title: 단순회귀분석, 잔차분석, 다중회귀분석
categories: 회귀
tag: [Statistic, regression]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

회귀분석은 독립변수가 종속변수에 영향을 미치는지 알아보고자 할 때 실시하는 분석방법입니다.

# 단순회귀분석

단순회귀분석은 독립변수 하나, 종속변수가 하나인 분석방법입니다.

다음과 같은 가정을 따릅니다.

- 가정
  - 오차의 등분산성
  - 오차의 독립성
  - 오차의 정규성 (오차의 평균은 0)
  - 가정된 모형은 옳다(선형성)

<br>

회귀분석은 모형을 가정해놓고, 적합된 회귀식(예측하는 회귀식)을 만든 후에, 가정된 모형과 적합된 회귀식의 오차를 최소화하는 방법을 사용합니다.

- 가정된 모형 : $Y_{i}= \alpha + \beta X_{i} + \epsilon_{i}$

- 적합된 회귀식 : $\hat{Y_{i}} = a + bX_{i}$

- 잔차 : $\epsilon_{i} = Y_{i}-\hat{Y_{i}}$

=> $e_{i}$의 제곱합(SSE)을 최소화하는 방향으로 추정

- 잔차의 제곱합 : $\sum_{i=1}^n(\epsilon_{i})^2 =\sum_{i=1}^n(Y_{i}-\hat{Y_{i}})$

잔차의 제곱합을 $\alpha$ 와 $\beta$로 편미분하고 그 값을 0으로 하는 $\alpha$를 a, \beta를 b로 설정하여 식으로 표현하면 다음과 같이 나오게 됩니다.

$a = \bar{Y} - b\bar{X}$

$b = \frac{s_{XY}}{s_{X}^{2}}$

a는 절편, b는 회귀계수로 사용합니다.
(이러한 방법을 최소제곱법(OLS)라고 합니다.)

<br>

- 분산분석

모델의 유의성, 적합도를 검정하기 위해 분산분석을 시행합니다.

- SSR(처리의 제곱합) = $\sum_{i=1}^{n}(\hat{Y_{i}}-\bar{Y})^2$
- SSE(오차의 제곱합) = $\sum_{i=1}^{n}(Y_{i}-\hat{Y_{i}})^2$
- SST(제곱합의 합) = $\sum_{i=1}^{n}(Y_{i}-\bar{Y})^2$

| 요인 | 제곱합 | 자유도 | 평균제곱      | F값       |
| ---- | ------ | ------ | ------------- | --------- |
| 처리 | SSR    | 1      | MSR=SSR/1     | F=MSR/MSE |
| 오차 | SSE    | n-2    | MSE=SSE/(n-2) |
| 합   | SST    | n-1    |

- 적합도

적합도는 다음과 같은 식으로 표현합니다.

- $R^{2} = SSR/SST$

전체의 오차에서 처리의 오차가 얼마나 차지 하는 지 표현한 것입니다. 이를 통해서 모델이 얼마나 적합되었는지 추정합니다.
R^2가 높을수록 모델의 적합도가 높다고 생각합니다.

- F검정(모델의 유의성)
  - $H_{0} : \beta = 0$
  - $H_{1} : \beta \ne 0$

모델의 유의성에 대하서 F검정을 통해 검정합니다. $\beta$가 0이면 모델이 유의하지 않은 것으로 생각할 수 있습니다.

모수 $\beta$에 관한 추론(T검정)

- $H_{0} : \beta = 0$
- $H_{1} : \beta \ne 0$

각 변수의 유의성을 검정합니다.

ex) 광고비와 판매액 데이터를 통해 회귀분석을 시행합니다. 광고비가 독립변수, 판매액이 종속변수입니다.

```python
import statsmodels.api as sm

광고비 = [4,6,6,8,8,9,9,10,12,12]
판매액 = [39,42,45,47,50,50,52,55,57,60]

x = 광고비
y = 판매액
x = sm.add_constant(x)

model = sm.OLS(y,x)
result = model.fit()
print(result.summary())
```

                                OLS Regression Results
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.956
    Model:                            OLS   Adj. R-squared:                  0.950
    Method:                 Least Squares   F-statistic:                     172.1
    Date:                Sun, 29 May 2022   Prob (F-statistic):           1.09e-06
    Time:                        20:37:52   Log-Likelihood:                -17.016
    No. Observations:                  10   AIC:                             38.03
    Df Residuals:                       8   BIC:                             38.64
    Df Model:                           1
    Covariance Type:            nonrobust
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const         28.6722      1.670     17.166      0.000      24.820      32.524
    x1             2.5033      0.191     13.117      0.000       2.063       2.943
    ==============================================================================
    Omnibus:                        6.181   Durbin-Watson:                   3.388
    Prob(Omnibus):                  0.045   Jarque-Bera (JB):                1.423
    Skew:                          -0.309   Prob(JB):                        0.491
    Kurtosis:                       1.258   Cond. No.                         31.5
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.

f값의 p-value가 0.05이하이기 때문에 모델은 유의하고, 변수의 p-value도 0.05이기 때문에 변수도 유의합니다. $R^2$도 0.956으로 상당히 높게 나왔기 때문에 적합도도 높다고 생각할 수 있습니다.

### 결과값들에 대한 설명

- covariance type : robust / nonrobust => 강건성(이상치, 에러값으로부터 영향을 많이 받는지)
- R-squared : 결정계수 => 회귀직선의 적합도(goodness-of-fit)를 평가하거나 종속변수에 대한 설명변수들의 설명력 평가(독립변수 개수가 많아질수록 그 값이 커지게 됨)
- Adj.R-squared : 수정 결정계수 => 결정계수의 문제점 보완, R-squared 와 차이가 크지 않을 수록 좋음.
- F-statistic : F-통계량 => 모형의 전체적인 유의성 검정
- Prob(F-statistic) : 0.05보다 작으면 모형의 유의하다고 할 수 있음.
- Log-Likelihood : 로그 가능도 => 0에 가까울 수록 모형이 적합
- AIC : -2(로그 가능도 - 추정된 파라미터 개수(변수개수)) => 모형 적합도 판단
- BIC : -2 x 로그 가능도 + p x log n
- Omnibus : 왜도와 첨도가 정규분포와 일치하는 지 검정
- Prob(Omnibus) : 0.05 이하면 정규성x
- Durbin-Watson : 오차항의 자기상관 분석(0에 가까우면 양의 자기상관, 2에 가까우면 자기상관 없음, 4에 가까우면 양의 자기상관)
- Jarque-Bera : 왜도와 첨도가 정규분포와 일치하는 지 검정
- Prob(JB) : 0.05 이하면 정규성x
- Cond. No. : 조건수, 다중공선성 판단

# 잔차분석

잔차분석은 시각화를 통해 진행됩니다.

- 잔차 히스토그램
- 선형성, 등분산성을 판단할 때 사용합니다.

- 잔차들의 Q-Q 산점도
- 정규성을 판단할 때 사용합니다.

```python
pred = result.predict(x)
```

잔차데이터를 통해 regplot을 이용하여 추세선을 그려주고 0에 점선을 만들어줍니다.

```python
sns.set(rc={'figure.figsize':(10,8)})
sns.regplot(pred, result.resid, lowess=True, line_kws={'color': 'red'})
plt.plot([pred.min(), pred.max()], [0, 0], '--', color='grey')
```

![png](/assets/images/statistic/linear.png)

자료들이 랜덤으로 흩어져있으면 선형성, 등분산성, 독립성을 만족한다고 생각합니다. 하지만 U모양, n 모양을 가질 떄는 선형성을 만족시키지 못한다고 보고, 오른쪽으로 점점 흩어지는 나팔모양이 나왔을 때는 등분산성을 만족하지 않는다고 봅니다.

```python
#정규성 검정
from scipy.stats import probplot

fig = plt.figure(figsize=(8,8))
fig.set_facecolor('white')

ax = fig.add_subplot()
probplot(result.resid, dist='norm',plot=ax) ## qq plot 출력
plt.show()
```

![png](/assets/images/statistic/normal.png)

실선에 데이터가 몰려있으면 정규성을 만족한다고 생각할 수 있습니다.
현재 데이터는 약간 벗어나있어서 정규성을 만족한다고 보기는 어려울 수 있습니다.

# 다중회귀분석

다중회귀분석은 종속변수 하나, 독립변수 여러개인 분석입니다. 가정은 다음과 같습니다. 단순회귀분석 가정에 다중공선성이 추가되었습니다.

- 가정
  - 오차의 등분산성
  - 오차의 독립성
  - 오차의 정규성 (오차의 평균은 0)
  - 가정된 모형은 옳다(선형성)
  - 다중공선성(변수간의 상관관계 높음)

<br>

분석 개념은 단순회귀분석과 같습니다. 하지만 독립변수가 많기 때문에 이를 행렬을 통해서 표현합니다.

- 가정된 모형 : $Y_{i}= \beta_{0} + \beta_{1}X_{i} + \cdot + \beta_{k}X_{k} + \epsilon_{i}$

-> $Y = X\beta + \epsilon$

- 적합된 회귀식 : $\hat{Y_{i}} = X\hat{\beta}$

- 잔차 : $\epsilon_{i} = Y-X\hat{\beta}$

=> $e_{i}$의 제곱합(SSE)을 최소화하는 방향으로 추정

- 잔차의 제곱합 : $\sum_{i=1}^{n}\epsilon_{i}^2 = (Y-X\hat{\beta})^{T}(Y-X\hat{\beta})$

단순회귀분석과 마찬가지로 미분를 통해 최적값을 구합니다. 다중회귀분석은 행렬 자체에 절편이 포함되기 때문에 $\beta$ 행렬만 나오게 됩니다.

$\hat{\beta} = (X^{T}X)^{-1}X^{T}Y$

이를 회귀계수와 절편으로 사용합니다.
<br>

- 분산분석

| 요인 | 제곱합 | 자유도 | 평균제곱        | F값       |
| ---- | ------ | ------ | --------------- | --------- |
| 처리 | SSR    | k      | MSR=SSR/k       | F=MSR/MSE |
| 오차 | SSE    | n-k-1  | MSE=SSE/(n-k-1) |
| 합   | SST    | n-1    |

https://blog.naver.com/jhkang8420/221565825889

- F검정(모델의 유의성)

  - $H_{0} : \beta = 0$
  - $H_{1} : \beta \ne 0$

- 적합도
  - $R^{2} = SSR/SST$

모수 $\beta$에 관한 추론

- $H_{0} : \beta = 0$
- $H_{1} : \beta \ne 0$

```python
지름 = [21.0,21.8,22.3,26.6,27.1,27.4,27.9,27.9,29.7,32.7,32.7,33.7,34.7,35.0,40.6]
높이 = [21.33,19.81,19.20,21.94,24.68,25.29,20.11,22.86,21.03,22.55,25.90,26.21,21.64,19.50,21.94]
부피 = [0.291,0.291,0.288,0.464,0.532,0.557,0.441,0.515,0.603,0.628,0.956,0.775,0.727,0.704,1.084]
```

```python
import statsmodels.api as sm

df = pd.DataFrame([지름,높이,부피], index=['지름','높이','부피'])
df = df.transpose()

model = sm.OLS.from_formula('부피 ~ 지름 + 높이',data=df)
result = model.fit()
print(result.summary())
```

                                OLS Regression Results
    ==============================================================================
    Dep. Variable:                     부피   R-squared:                       0.924
    Model:                            OLS   Adj. R-squared:                  0.912
    Method:                 Least Squares   F-statistic:                     73.12
    Date:                Sun, 29 May 2022   Prob (F-statistic):           1.90e-07
    Time:                        20:37:53   Log-Likelihood:                 20.392
    No. Observations:                  15   AIC:                            -34.78
    Df Residuals:                      12   BIC:                            -32.66
    Df Model:                           2
    Covariance Type:            nonrobust
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept     -1.0236      0.188     -5.458      0.000      -1.432      -0.615
    지름             0.0370      0.003     10.590      0.000       0.029       0.045
    높이             0.0237      0.008      2.844      0.015       0.006       0.042
    ==============================================================================
    Omnibus:                        4.763   Durbin-Watson:                   2.566
    Prob(Omnibus):                  0.092   Jarque-Bera (JB):                2.434
    Skew:                           0.953   Prob(JB):                        0.296
    Kurtosis:                       3.511   Cond. No.                         389.
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.

f검정의 p-value가 0.05 이하이므로 모델이 유의하고, 변수들의 t검정 p-value 또한 모두 0.05 이하이므로 변수 모두가 유의합니다. $R^2$ 또한 0.924로 매우 높기 때문에 모델 적합도가 높다고 생각할 수 있습니다.
