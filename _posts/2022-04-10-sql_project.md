---
layout: single
title:  프로세스 데이터 SQL 프로젝트
categories: 프로젝트
tag: [article, SQL]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

부트캠프에서 SQL 프로젝트를 진행했었는데, 당시에는 SQL 지식이 부족해서 엑셀로만 흐지부지하게 했었습니다. 이번에는 자격증도 따긴 했으니 데이터베이스를 직접 사용하면서 조금만 더 깊게 진행해보았습니다.

SQL은 MYSQL을 사용하였고, 데이터베이스 프로그램은 MYSQL Workbench를 이용하였습니다. 개인적인 생각으로는 MYSQL Workbench가 저같은 초보자들에게 조금 쉬운 것 같았습니다. 


데이터는 프로세스 데이터입니다. 정확히 어떤 서비스인지는 알 수 없지만 case마다 고객의 문의를 받고 수리하는 과정까지가 표현되어 있습니다.

간단히 이런방식으로 표현되어 있습니다.

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
      <th>caseID</th>
      <th>taskID</th>
      <th>originator</th>
      <th>eventtype</th>
      <th>timestamp</th>
      <th>contact</th>
      <th>RepairType</th>
      <th>objectKey</th>
      <th>RepairInternally</th>
      <th>EstimatedRepairTime</th>
      <th>RepairCode</th>
      <th>RepairOK</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>FirstContact</td>
      <td>Dian</td>
      <td>complete</td>
      <td>1970-01-02 8:08</td>
      <td>Phone</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>MakeTicket</td>
      <td>Dian</td>
      <td>start</td>
      <td>1970-01-02 8:08</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>MakeTicket</td>
      <td>Dian</td>
      <td>complete</td>
      <td>1970-01-02 8:11</td>
      <td>None</td>
      <td>E</td>
      <td>1340.0</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>ArrangeSurvey</td>
      <td>Dian</td>
      <td>start</td>
      <td>1970-01-02 8:11</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>ArrangeSurvey</td>
      <td>Dian</td>
      <td>complete</td>
      <td>1970-01-02 8:16</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>

저는 데이터 분석을 위해 이 raw 데이터를 워크벤치에 넣어주고 , view들을 만들어주고 파이썬으로 view를 가져와서 데이터분석을 진행하겠습니다.

이번 포스팅에서는 repairtype에 따른 프로세스 시간, task 개수가 변화가 있는지 확인해 볼 예정입니다.

먼저 view들을 설명하겠습니다.
데이터는 practice라는 스키마에 저장되어 있습니다.

# duration

```
VIEW `duration` AS
    SELECT 
        `raw_data`.`caseID` AS `caseID`,
        MIN(`raw_data`.`timestamp`) AS `starttime`,
        MAX(`raw_data`.`timestamp`) AS `endtime`
    FROM
        `raw_data`
    GROUP BY `raw_data`.`caseID`
```
![png](/assets/images/sql_project/duration.PNG)

view로 저장하면서 자동으로 DDL로 바뀌었습니다.
저는 프로세스 데이터이기 때문에 프로세스 시작시간과 종료시간이 중요하다고 생각했기 때문에 이를 새로운 view로 만들어줬습니다. group by로 case를 그룹화 해준 후, timestamp의 최솟값과 최대값을 이용하여 데이터를 뽑아주었습니다.

# maketicket

```
VIEW `maketicket` AS
    SELECT 
        `raw_data`.`caseID` AS `caseID`,
        `raw_data`.`RepairType` AS `RepairType`,
        `raw_data`.`objectKey` AS `ObjectKey`
    FROM
        `raw_data`
    WHERE
        ((`raw_data`.`taskID` = 'MakeTicket')
            AND (`raw_data`.`eventtype` = 'complete'))
```
![png](/assets/images/sql_project/process.PNG)

maketicket이라는 task에 있던 특성들을 뽑아주었습니다. repairtype 에 따라 프로세스 지속시간이나 수리방법이 바뀔 수 있기 때문에 이에 대한 분석이 필요할 것 같아 이러한 방법으로 추출을 해주었습니다. eventype이 complete일 때 특성이 표시되기 때문에 조건을 넣어줬습니다.

# survey

```
VIEW `survey` AS
    SELECT 
        `raw_data`.`caseID` AS `caseID`,
        `raw_data`.`RepairInternally` AS `RepairInternally`,
        `raw_data`.`EstimatedRepairTime` AS `EstimatedRepairTime`,
        `raw_data`.`RepairCode` AS `RepairCode`
    FROM
        `raw_data`
    WHERE
        ((`raw_data`.`taskID` = 'Survey')
            AND (`raw_data`.`eventtype` = 'complete'))
```
![png](/assets/images/sql_project/survey.PNG)

survey라는 task에 있는 특성을 뽑아주었습니다. 이 task에서 프로세스에 관한 중요 정보들이 표현되고 있기 때문에 따로 추출하였습니다. maketicket처럼 eventtype이 complete일 때 정보들이 나타나기 때문에 조건을 넣어주었습니다.

# repair

```
VIEW `repair` AS
    SELECT 
        `raw_data`.`caseID` AS `caseID`,
        COUNT((CASE
            WHEN (`raw_data`.`taskID` = 'InternRepair') THEN 1
        END)) AS `InternRepair`,
        COUNT((CASE
            WHEN (`raw_data`.`taskID` = 'ImmediateRepair') THEN 1
        END)) AS `ImmediateRepair`,
        COUNT((CASE
            WHEN (`raw_data`.`taskID` = 'ExternRepair') THEN 1
        END)) AS `ExternRepair`
    FROM
        `raw_data`
    WHERE
        (`raw_data`.`eventtype` = 'start')
    GROUP BY `raw_data`.`caseID`
```
![png](/assets/images/sql_project/repair.PNG)

프로세스에서 수리는 InternRepair, ImmediateRepair, ExternRepair 세가지 방법으로 이루어집니다. 여러 방법이 중복되어 나타나기도 하고 한 방법이 두번 이루어지기도 합니다. 이러한 수리 방법이 프로세스 시간에 영향을 미치는지 분석하기 위해 case마다 각가지 수리방법이 나타난 횟수를 숫자로 표현하였습니다.

추가적으로 프로세스를 도식화한 그림입니다.

![png](/assets/images/sql_project/duration.PNG)

# task_number

```
VIEW `task_number` AS
    SELECT 
        `raw_data`.`caseID` AS `caseID`,
        COUNT(`raw_data`.`caseID`) AS `n_task`
    FROM
        `raw_data`
    GROUP BY `raw_data`.`caseID`
```
![png](/assets/images/sql_project/task_number.PNG)

위에서 말했던 프로세스 특성들이 task 횟수에 영향을 주고받지 않을까하는 가설을 세울 수 있습니다. 분석을 위해 case마다의 task횟수를 추출하였습니다.


# 파이썬 데이터 분선

이제 파이썬을 통해서 view를 가져와서 데이터분석을 진행해보겠습니다. 파이썬의 pymysql을 통해 mysql과 연결하였습니다.

repairtype에 따른 task 횟수에 차이가 있는 지 확인해보겠습니다.

```python
sql = "SELECT b.caseID, a.RepairType, b.n_task FROM practice.task_number b inner join practice.maketicket a on a.caseID = b.caseID;"
cursor.execute(sql)
sql1 = cursor.fetchall()
```

```python
result = pd.DataFrame(sql1)
result
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
      <th>caseID</th>
      <th>RepairType</th>
      <th>n_task</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>E</td>
      <td>14</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>P</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>E</td>
      <td>14</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>P</td>
      <td>14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>P</td>
      <td>14</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>922</th>
      <td>996</td>
      <td>E</td>
      <td>14</td>
    </tr>
    <tr>
      <th>923</th>
      <td>997</td>
      <td>E</td>
      <td>14</td>
    </tr>
    <tr>
      <th>924</th>
      <td>998</td>
      <td>E</td>
      <td>14</td>
    </tr>
    <tr>
      <th>925</th>
      <td>999</td>
      <td>B</td>
      <td>13</td>
    </tr>
    <tr>
      <th>926</th>
      <td>1000</td>
      <td>E</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
<p>927 rows × 3 columns</p>
</div>

먼저 KS검정을 통해 정규성을 확인해본 결과 정규성을 만족하지 못했기 때문에 만휘트니 검정을 통해 repairtype이 E일 때와 repairtype 이 P일 때를 비교해보겠습니다.


```python
import pandas as pd
from scipy.stats import mannwhitneyu

print(mannwhitneyu(result[result['RepairType'] == 'E']['n_task'], result[result['RepairType'] == 'P']['n_task']))
```

    MannwhitneyuResult(statistic=76256.5, pvalue=0.013737995121110713)

p값이 0.05 이하이므로 차이가 있다는 대립가설을 취합니다.
    

```python
print(result[result['RepairType'] == 'E']['n_task'].mean())
print(result[result['RepairType'] == 'P']['n_task'].mean())
```

    14.218918918918918
    14.134396355353076

평균을 비교해보니 E일 때 task 갯수가 더 많습니다.


프로세스 지속기간에 대해서도 검정해보겠습니다.

```python
sql = "SELECT a.caseID, b.RepairType, a.starttime, a.endtime FROM practice.duration a inner join practice.maketicket b on a.caseID = b.caseID;"
cursor.execute(sql)
sql2 = cursor.fetchall()
```

```python
result = pd.DataFrame(sql2)
result
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
      <th>caseID</th>
      <th>RepairType</th>
      <th>starttime</th>
      <th>endtime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>E</td>
      <td>1970-01-02 8:08</td>
      <td>1970-01-17 8:12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>P</td>
      <td>1970-01-08 5:17</td>
      <td>1970-01-12 8:57</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>E</td>
      <td>1970-01-03 1:03</td>
      <td>1970-01-07 7:04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>P</td>
      <td>1970-01-03 8:23</td>
      <td>1970-01-04 23:35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>P</td>
      <td>1970-01-07 20:41</td>
      <td>1970-01-10 6:05</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>922</th>
      <td>996</td>
      <td>E</td>
      <td>1970-01-01 18:08</td>
      <td>1970-01-02 8:49</td>
    </tr>
    <tr>
      <th>923</th>
      <td>997</td>
      <td>E</td>
      <td>1970-01-06 20:58</td>
      <td>1970-01-15 4:14</td>
    </tr>
    <tr>
      <th>924</th>
      <td>998</td>
      <td>E</td>
      <td>1970-01-07 1:37</td>
      <td>1970-01-19 6:54</td>
    </tr>
    <tr>
      <th>925</th>
      <td>999</td>
      <td>B</td>
      <td>1970-01-07 12:23</td>
      <td>1970-01-07 5:11</td>
    </tr>
    <tr>
      <th>926</th>
      <td>1000</td>
      <td>E</td>
      <td>1970-01-07 20:17</td>
      <td>1970-01-20 2:25</td>
    </tr>
  </tbody>
</table>
<p>927 rows × 4 columns</p>
</div>

지속기간을 구하기 위해 starttime과 endtime을 datetime으로 변환 후 차이를 구한뒤에 다시 int값으로 바꿔주었습니다. workbench에서 미리 뷰에 값을 넣는게 효율적이었을 것 같습니다.

```python
result['starttime'] = pd.to_datetime(result['starttime'])
result['endtime'] = pd.to_datetime(result['endtime'])
```
```python
result['interval'] = result['starttime'] - result['endtime']
```
```python
i = result['interval'].dt.days
```
```python
result['interval'] = i
```

마찬가지로 ks검정 결과, 정규성을 만족하지 못하였기 때문에 만휘트니 검정을 시행합니다.

```python
import pandas as pd
from scipy.stats import mannwhitneyu

print(mannwhitneyu(result[result['RepairType'] == 'E']['interval'], result[result['RepairType'] == 'P']['interval']))
```

    MannwhitneyuResult(statistic=50508.0, pvalue=5.424343663436233e-21)

p값이 0.05이하이기 때문에 차이가 있다고 결론내립니다.

```python
print(result[result['RepairType'] == 'E']['interval'].mean())
print(result[result['RepairType'] == 'P']['interval'].mean())
```

    -6.948648648648649
    -3.906605922551253

task 횟수와 마찬가지로 E일 때 더 지속기간이 길다고 결론 내릴 수 있습니다.

결과적으로 repairtype이 E일 때 task 갯수가 많고 시간이 오래걸린다고 결론내릴 수 있고, 이에 따라서 이러한 경우에 인원을 더 배치한다던가 수리방법을 개선하는 방안을 새로 짜는 전략을 세울 수 있습니다.

오늘 포스팅은 여기서 마치겠습니다.