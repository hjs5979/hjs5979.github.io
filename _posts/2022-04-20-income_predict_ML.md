---
layout: single
title:  인구데이터 기반 소득 예측 경진대회 Train
categories: 소득예측대회
tag: [python, ML, Train]
toc: true
author_profile: true
sidebar:
  nav: docs
---




저번에 이어서 전처리와 학습을 해보겠습니다!!

# 전처리 방안

- age는 범주형 변수로 변환

- fnlwgt는 로그변환하고 나머지 연속형 변수는 그대로 사용하기로함 -> 나머지도 모두 변환해서 넣어봤지만 성능이 하락...

- education.num, relationship, native.country 제거

- 범주형 변수를 더 낮은 수준의 범주로 변환할지 고민 -> 변환 x

사용한 라이브러리

```python
import pandas as pd
import numpy as np
import optuna
from sklearn.model_selection import train_test_split
from catboost import CatBoostClassifier
from sklearn.metrics import accuracy_score
from sklearn.model_selection import KFold, StratifiedKFold

train=pd.read_csv('./train.csv')
test=pd.read_csv('./test.csv')
```

# 전처리

전처리를 함수화하였습니다!

```python
def preprocessing(df):
    #age
    bins = np.linspace(df['age'].min(), df['age'].max(), 10)
    df['age'] = np.digitize(df['age'], bins)
    
    #fnlwgt
    df['fnlwgt'] = np.log1p(df['fnlwgt'])
    
    #capital.gain
    
    #capital.loss
    
    #hours.per.week
    
    #education.num
    df.drop(['education.num'], axis=1)
    
    #education
    
    
    #marital.status
    
    
    #relationship
    df.drop(['relationship'], axis=1)
    
    #race
    
    #sex
    
    #workclass
    
    
    #occpation
    
    #native.country
    df.drop(['native.country'], axis=1)
    
```

```python
preprocessing(train)
preprocessing(test)
```
학습데이터와 타겟을 분리합니다~

```python
train_df = train.drop(['target'], axis=1)
target_df = train['target']
```

catboost를 위해 리스트에 범주형 변수들을 넣어줍니다!

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
```

    ['age', 'workclass', 'education', 'education.num', 'marital.status', 'occupation', 'relationship', 'race', 'sex']
    [ 1  2  4  5  6  7  8  9 10]
    

# Train


```python


X_train, X_test, y_train, y_test = train_test_split(train_df, target_df, test_size=0.2, stratify=target_df, random_state=0)

cb = CatBoostClassifier()

cb.fit(X_train,y_train, cat_features=cat_features, verbose=100)

pred = cb.predict(X_test)
print(accuracy_score(pred, y_test))
```

    Learning rate set to 0.031626
    0:	learn: 0.6626343	total: 47.7ms	remaining: 47.7s
    100:	learn: 0.2794633	total: 5.73s	remaining: 51s
    200:	learn: 0.2644652	total: 11.6s	remaining: 46.3s
    300:	learn: 0.2544592	total: 17s	remaining: 39.6s
    400:	learn: 0.2453992	total: 22.7s	remaining: 33.9s
    500:	learn: 0.2388675	total: 28.1s	remaining: 27.9s
    600:	learn: 0.2328611	total: 33.4s	remaining: 22.2s
    700:	learn: 0.2278318	total: 38.9s	remaining: 16.6s
    800:	learn: 0.2232068	total: 44.3s	remaining: 11s
    900:	learn: 0.2189459	total: 49.9s	remaining: 5.48s
    999:	learn: 0.2146759	total: 55.6s	remaining: 0us
    0.8794100636205899
    
0.879가 나왔을 때는 매우 기분이 좋았지...

필요없는 변수가 있나해서 변수중요도도 한번 살펴봤습니다

```python
feature_importances = {}
for i in range(0,len(train_df.columns)):
    feature_importances[train_df.columns[i]]=cb.get_feature_importance()[i]


feature_importances
```




    {'age': 10.300664357707452,
     'workclass': 11.15523870764783,
     'fnlwgt': 5.009930338530053,
     'education': 4.584492290267686,
     'marital.status': 7.525777594971174,
     'occupation': 9.261079890737346,
     'relationship': 14.575559008658356,
     'race': 1.6851195084954749,
     'sex': 1.7066850791084716,
     'capital.gain': 12.995901104327807,
     'capital.loss': 6.247774235661497,
     'hours.per.week': 9.291321571751823}


저번에 했던 신용등급대회 포스팅에서 사용했던 파라미터 범위와 비슷하게 설정했습니다. 정확히 어떤 파라미터였는지 까먹어서 다시 공부중... 


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
      'od_type': trial.suggest_categorical('od_type', ['IncToDec', 'Iter'])}


    model = CatBoostClassifier(**param)

    model.fit(X_train, y_train, eval_set=[(X_test, y_test)], verbose=0, cat_features=cat_features)

    preds = model.predict(X_test)
    pred_labels = np.rint(preds)
    accuracy = accuracy_score(y_test, pred_labels)
    return accuracy
```

파라미터 튜닝 시작!! 통크게 100번!

```python
study = optuna.create_study(direction="maximize")
study.optimize(objective, n_trials=100)
```


```python
print("Number of finished trials: {}".format(len(study.trials)))

print("Best trial:")
trial = study.best_trial

print("  Value: {}".format(trial.value))

print("  Params: ")
for key, value in trial.params.items():
    print("    {}: {}".format(key, value))
```

    Number of finished trials: 100
    Best trial:
      Value: 0.8791208791208791
      Params: 
        objective: CrossEntropy
        colsample_bylevel: 0.0509613126112151
        boosting_type: Ordered
        bootstrap_type: MVS
        learning_rate: 0.052873040650087154
        n_estimators: 8448
        max_depth: 12
        random_strength: 57
        l2_leaf_reg: 1.3757339190257113e-05
        min_child_samples: 66
        max_bin: 292
        od_type: IncToDec
    

계층적 k-fold로 검증 시작!

```python
cat_models={}
    
folds=StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
outcomes=[]

for seed in [0]:
    for n_fold, (train_index, val_index) in enumerate(folds.split(train_df, target_df)):
        print(f'===================================={n_fold+1}============================================')

        X_train, X_val = train_df.iloc[train_index], train_df.iloc[val_index]
        y_train, y_val = target_df.iloc[train_index], target_df.iloc[val_index]

        cat = CatBoostClassifier(**trial.params)
        cat.fit(X_train, y_train,
              eval_set=[(X_train, y_train), (X_val, y_val)],
              cat_features=cat_features,
              verbose=100)

        cat_models[n_fold] = cat

        predictions = cat.predict(X_val)
        
        accuracy=accuracy_score(y_val, predictions)
        outcomes.append(accuracy)
        print(f"FOLD {n_fold+1} : accuracy:{accuracy}")

        print(f'================================================================================\n\n')

mean_outcome=np.mean(outcomes)
print("Mean:{}".format(mean_outcome))
```

제출은 생략합니다~

최종 0.870로 38등했네요 ㅠ 넘 아쉽다!! 다음엔 꼭 10등안에 들어보겠습니다 ㅎㅎ


