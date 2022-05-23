---
layout: single
title:  신용등급 예측 대회 Train
categories: 신용등급예측대회
tag: [python, preprocess, train, ML]
toc: true
author_profile: true
sidebar:
  nav: docs
---
저번 포스트에 이어서 전처리 및 학습부분 작성하겠씁니다

# 전처리 방안

전처리는 catboost를 쓴다는 가정하에 진행하였습니다.
catboost는 범주형 변수에 유리하기 때문에 변수들을 모두 범주화하였습니다.

catboost에 대해 더 알고싶으시면 일단 이분들 글을 참고하시면 될 것 같습니다.
저도 논문 내용을 포스팅할 예정입니다.

<https://dailyheumsi.tistory.com/136>

<https://blog.naver.com/PostView.nhn?blogId=winddori2002&logNo=221931868686&categoryNo=17&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView>

범주형 변수가 많을 때, 유리한 catboost를 사용하였습니다. 때문에 최대한 변수들을 범주형으로 바꿔주려 노력하였습니다. 또한 catboost는 변수를 원-핫 인코딩 해주기 때문에 따로 인코딩을 하지는 않았습니다. 

1. 연속형 변수

- 모든 변수를 10개 구간으로 나누어 범주화합니다.

2. 범주형 변수

- index : 제거

- family_size : 제거 -> child_num과 상관관계가 있고, family_size가 상대적으로 이상치가 많고 표준분포가 크기 때문에 child_num 사용

- child_num : 7이상을 이상치로 생각하고 7을 제외한 최대값인 5로 대체

- FLAG_MOBIL : 범주가 하나 밖에 없으므로 제거

- occyp_type : 결측치로 인해 오류가 생기므로, 결측치를 포함하여 라벨인코딩함

3. 파생변수

- 중복데이터를 위해 파생변수 생성

- 'gender','income_total', 'income_type','family_type','DAYS_BIRTH', 'email','family_size', 'begin_month 섞기

쓴 라이브러리들

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import log_loss
from sklearn.preprocessing import train_test_split
import optuna
from catboost import CatBoostClassifier
from sklearn.model_selection import StratifiedKFold 
```

# 전처리

train과 test를 둘다 전처리해야하기 때문에 함수형식으로 만들어줬습니다. 이게 편하더라고요

```python
def preprocessing(df):
    
    #Income_total : 값이 커서 10000으로 나눠줍니다
    df["income_total"] = df["income_total"] / 10000
    
    bins = np.linspace(df['income_total'].min(), df['income_total'].max(), 10)
    df['income_total'] = np.digitize(df['income_total'], bins)
    
    #DAYS_BIRTH
    bins = np.linspace(df['DAYS_BIRTH'].min(), df['DAYS_BIRTH'].max(), 10)
    df['DAYS_BIRTH'] = np.digitize(df['DAYS_BIRTH'], bins)
    
    #DAYS_EMPLOYED
    bins = np.linspace(df['DAYS_EMPLOYED'].min(), df['DAYS_EMPLOYED'].max(), 10)
    df['DAYS_EMPLOYED'] = np.digitize(df['DAYS_EMPLOYED'], bins)
    
    #begin_month
    bins = np.linspace(df['begin_month'].min(), df['begin_month'].max(), 10)
    df['begin_month'] = np.digitize(df['begin_month'], bins)
    
    #gender
    
    #car
    
    #reality
    
    #child_num
    index = df[df['child_num']>=7].index
    df.loc[index,'child_num'] = 5
    
    #income_type
    
    #edu_type
    
    #family_type
    
    #house_type
    
    #work_phone
    
    #phone
    
    #email
    
    #occyp_type
    le = LabelEncoder()
    le.fit(df['occyp_type'])
    df['occyp_type'] = le.transform(df['occyp_type'])
    
    #파생변수 : 언더바로 이어줬어요
    df['derived_var'] = train['gender'] + '_' + train['income_total'].astype(str) + '_' + train['income_type'] +\
    '_' + train['family_type'] + '_' + train['DAYS_BIRTH'].astype(str) + '_' + train['email'].astype(str) +\
    '_' + train['family_size'].astype(str) + '_' + train['begin_month'].astype(str)
    
    #FLAG_MOBIL -> 제거
    df.drop(['FLAG_MOBIL'], axis=1, inplace=True)
    
    #family_size -> 제거
    df.drop(['family_size'], axis=1, inplace=True)
    
     #index
    df.drop(['index'], axis=1, inplace=True)
    

```

```python
preprocessing(test)
preprocessing(train)
```
전처리는 끝났습니다.

## 학습 및 테스트

```python
train_df = train.drop(['credit'], axis=1)
target_df = train['credit']

X_train, X_test, y_train, y_test = train_test_split(train_df, target_df, test_size = 0.3, stratify=target_df, random_state=0)
```

catboost는 범주형데이터를 지정해주면 범주형데이터를 알아서 인코딩해줍니다.

```python
cat_features = []

def column_index(df, cat_features):
    cols = df.columns.values
    sidx = np.argsort(cols)
    return sidx[np.searchsorted(cols, cat_features, sorter=sidx)]

for col in train_df.columns:
    target = train_df[col]
    if target.nunique() <= 20:
        cat_features.append(col)
    cat_features_idx = column_index(train_df, cat_features)
print(cat_features)
print(cat_features_idx)


    ['gender', 'car', 'reality', 'child_num', 'income_total', 'income_type', 'edu_type', 'family_type', 'house_type', 'DAYS_BIRTH', 'DAYS_EMPLOYED', 'work_phone', 'phone', 'email', 'occyp_type', 'begin_month']
    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15]

# 저런식으로 하니 파생변수가 빠져버려서 직접 리스트에 넣었습니다.
cat_features.append("derived_var")

```

obtuna를 통해서 하이퍼 파라미터를 튜닝하였습니다. 각 파라미터의 범위는 이분의 공유코드를 참고했습니다.
<https://dacon.io/competitions/official/235713/codeshare/2757?page=2&dtype=recent>

obtuna를 사용하기 위해서 파라미터 범위를 지정해주고 학습하는 함수를 만들어줍니다. 이 대회는 평가기준이 log loss이기 때문에 log loss를 사용해줍니다.

하이퍼 파라미터에 대한 내용은 후에 포스팅할 예정입니다.
```python
def objective(trial):
    param = {
      "random_state":42,
      'learning_rate' : trial.suggest_loguniform('learning_rate', 0.01, 0.3),
      'bagging_temperature' :trial.suggest_loguniform('bagging_temperature', 0.01, 100.00),
      "n_estimators":trial.suggest_int("n_estimators", 1000, 10000),
      "max_depth":trial.suggest_int("max_depth", 4, 16),
      'random_strength' :trial.suggest_int('random_strength', 0, 100),
      "colsample_bylevel":trial.suggest_float("colsample_bylevel", 0.4, 1.0),
      "l2_leaf_reg":trial.suggest_float("l2_leaf_reg",1e-8,3e-5),
      "min_child_samples": trial.suggest_int("min_child_samples", 5, 100),
      "max_bin": trial.suggest_int("max_bin", 200, 500),
      'od_type': trial.suggest_categorical('od_type', ['IncToDec', 'Iter']),
    }

    X_train, X_valid, y_train, y_valid = train_test_split(train_df,target_df,test_size=0.3,stratify=target_df, random_state=0)

    cat = CatBoostClassifier(**param)
    cat.fit(X_train, y_train,
          eval_set=[(X_train, y_train), (X_valid,y_valid)],
          early_stopping_rounds=100,cat_features=cat_features,
          verbose=0)
    cat_pred = cat.predict_proba(X_valid)
    log_score = log_loss(y_valid, cat_pred)

    return log_score
```
optuna.create_study를 통해 튜닝을 시작합니다. 코랩의 GPU를 통해 10번 돌렸습니다. 

```python
study = optuna.create_study(
    study_name = 'cat_parameter_opt',
    direction = 'minimize')

study.optimize(objective, n_trials=10)
print("Best Score:",study.best_value)
print("Best trial",study.best_trial.params)
```

더 정확한 평가를 위해 계층적 k-fold 교차검증을 실시했습니다.
계층적 k-fold 교차검증은 이 블로그에 잘 설명되어 있습니다.
<https://huidea.tistory.com/30>

```python

# Catboost 계층적 k -fold
def stratified_kfold_cat(params, n_fold, train_df, target_df, test_df):
    folds = StratifiedKFold(n_splits=n_fold, shuffle=True, random_state=0)
    splits = folds.split(train_df, target_df) 
    oof = np.zeros((train_df.shape[0], 3))# 모든 값이 0인 (26457,3)의 array 생성
    preds = np.zeros((test_df.shape[0], 3))

    for fold, (train_idx, valid_idx) in enumerate(splits):# n_fold (0~n), (train 인덱스 번호, valid 인덱스 번호) train과 valid를 1:n-1로 나눔
        print(f"============ Fold {fold} ============\n")
        X_train, X_valid = train_df.iloc[train_idx], train_df.iloc[valid_idx]
        y_train, y_valid = target_df.iloc[train_idx], target_df.iloc[valid_idx]

        model = CatBoostClassifier(**params)

        model.fit(X_train, y_train,
            eval_set=(X_valid, y_valid),
            early_stopping_rounds=100, #100번 이상
            verbose=0, cat_features=cat_features)

        oof[valid_idx] = model.predict_proba(X_valid) # valid_idx 마다 preds 계산
        preds += model.predict_proba(t) / n_fold # fold 마다 계산하고 n으로 나눔

    log_score = log_loss(target_df, oof)
    print(f"Log Loss Score: {log_score:.5f}\n")
    return oof, preds

oof, preds = stratified_kfold_cat(study.best_trial.params, 5, train_df, target_df, test)
```

이 후 제출 과정은 생략하였습니다.
