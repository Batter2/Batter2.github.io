# GradientBoostingClassifier


```python
import numpy as np
import pandas as pd
from sklearn.tree import DecisionTreeClassifier
```


```python
# train 엑셀파일 불러오기

data_train = pd.read_csv("train.csv", index_col="no")
data_train
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
      <th>age</th>
      <th>workclass</th>
      <th>fnlwgt</th>
      <th>education</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
      <th>income</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>25</td>
      <td>Private</td>
      <td>219199</td>
      <td>11th</td>
      <td>7</td>
      <td>Divorced</td>
      <td>Machine-op-inspct</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>39</td>
      <td>Private</td>
      <td>52978</td>
      <td>Some-college</td>
      <td>10</td>
      <td>Divorced</td>
      <td>Other-service</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1721</td>
      <td>55</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35</td>
      <td>Private</td>
      <td>196899</td>
      <td>Bachelors</td>
      <td>13</td>
      <td>Never-married</td>
      <td>Handlers-cleaners</td>
      <td>Not-in-family</td>
      <td>Asian-Pac-Islander</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>Haiti</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>64</td>
      <td>Private</td>
      <td>135527</td>
      <td>Assoc-voc</td>
      <td>11</td>
      <td>Divorced</td>
      <td>Tech-support</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24</td>
      <td>Private</td>
      <td>60783</td>
      <td>Some-college</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Transport-moving</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>70</td>
      <td>United-States</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>29301</th>
      <td>20</td>
      <td>Private</td>
      <td>100605</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Other</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>Puerto-Rico</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29302</th>
      <td>21</td>
      <td>Private</td>
      <td>372636</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29303</th>
      <td>18</td>
      <td>Self-emp-not-inc</td>
      <td>258474</td>
      <td>10th</td>
      <td>6</td>
      <td>Never-married</td>
      <td>Farming-fishing</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29304</th>
      <td>33</td>
      <td>Private</td>
      <td>157446</td>
      <td>11th</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Craft-repair</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>65</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29305</th>
      <td>65</td>
      <td>?</td>
      <td>94809</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Widowed</td>
      <td>?</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>29305 rows × 15 columns</p>
</div>




```python
# test 엑셀파일 불러오기

data_test = pd.read_csv("test.csv", index_col="no")
data_test
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
      <th>age</th>
      <th>workclass</th>
      <th>fnlwgt</th>
      <th>education</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29306</th>
      <td>18</td>
      <td>?</td>
      <td>245274</td>
      <td>Some-college</td>
      <td>10</td>
      <td>Never-married</td>
      <td>?</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>16</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29307</th>
      <td>29</td>
      <td>Private</td>
      <td>83003</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Other-service</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29308</th>
      <td>45</td>
      <td>Private</td>
      <td>35136</td>
      <td>Bachelors</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29309</th>
      <td>42</td>
      <td>Self-emp-not-inc</td>
      <td>64631</td>
      <td>Bachelors</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29310</th>
      <td>41</td>
      <td>Private</td>
      <td>195821</td>
      <td>Doctorate</td>
      <td>16</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1902</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48838</th>
      <td>45</td>
      <td>Self-emp-not-inc</td>
      <td>116789</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Craft-repair</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>60</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48839</th>
      <td>48</td>
      <td>Private</td>
      <td>185079</td>
      <td>HS-grad</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Exec-managerial</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48840</th>
      <td>63</td>
      <td>Private</td>
      <td>117473</td>
      <td>Some-college</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Prof-specialty</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>4386</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48841</th>
      <td>18</td>
      <td>Private</td>
      <td>150817</td>
      <td>11th</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48842</th>
      <td>31</td>
      <td>Private</td>
      <td>341632</td>
      <td>Assoc-acdm</td>
      <td>12</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>46</td>
      <td>United-States</td>
    </tr>
  </tbody>
</table>
<p>19537 rows × 14 columns</p>
</div>




```python
# 상관관계 구하기(문자열의 values를 가지는 컬럼은 제외)

data_train.corr()
```

    C:\Users\hjy47\AppData\Local\Temp\ipykernel_16148\197071326.py:3: FutureWarning: The default value of numeric_only in DataFrame.corr is deprecated. In a future version, it will default to False. Select only valid columns or specify the value of numeric_only to silence this warning.
      data_train.corr()
    




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
      <th>age</th>
      <th>fnlwgt</th>
      <th>education-num</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>age</th>
      <td>1.000000</td>
      <td>-0.075753</td>
      <td>0.035084</td>
      <td>0.078498</td>
      <td>0.054413</td>
      <td>0.073100</td>
      <td>0.238460</td>
    </tr>
    <tr>
      <th>fnlwgt</th>
      <td>-0.075753</td>
      <td>1.000000</td>
      <td>-0.030600</td>
      <td>-0.005051</td>
      <td>-0.001513</td>
      <td>-0.010154</td>
      <td>-0.002994</td>
    </tr>
    <tr>
      <th>education-num</th>
      <td>0.035084</td>
      <td>-0.030600</td>
      <td>1.000000</td>
      <td>0.127651</td>
      <td>0.083925</td>
      <td>0.147569</td>
      <td>0.331798</td>
    </tr>
    <tr>
      <th>capital-gain</th>
      <td>0.078498</td>
      <td>-0.005051</td>
      <td>0.127651</td>
      <td>1.000000</td>
      <td>-0.031401</td>
      <td>0.088609</td>
      <td>0.221387</td>
    </tr>
    <tr>
      <th>capital-loss</th>
      <td>0.054413</td>
      <td>-0.001513</td>
      <td>0.083925</td>
      <td>-0.031401</td>
      <td>1.000000</td>
      <td>0.055271</td>
      <td>0.135645</td>
    </tr>
    <tr>
      <th>hours-per-week</th>
      <td>0.073100</td>
      <td>-0.010154</td>
      <td>0.147569</td>
      <td>0.088609</td>
      <td>0.055271</td>
      <td>1.000000</td>
      <td>0.231045</td>
    </tr>
    <tr>
      <th>income</th>
      <td>0.238460</td>
      <td>-0.002994</td>
      <td>0.331798</td>
      <td>0.221387</td>
      <td>0.135645</td>
      <td>0.231045</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



- income 컬럼이랑 상관관계가 높은 것 : education-num, age, hours-per-week, capital-gain

## 데이터 전처리
- 전처리 과정(삭제) : fnlwgt, education
- 전처리 과정(수정) : workclass(" ?"를 " Private"로 수정), native-country(" ?"를 " United-States"로 수정)

### 데이터 전처리 삭제
- fnlwgt, education


```python
data_train.columns
```




    Index(['age', 'workclass', 'fnlwgt', 'education', 'education-num',
           'marital-status', 'occupation', 'relationship', 'race', 'sex',
           'capital-gain', 'capital-loss', 'hours-per-week', 'native-country',
           'income'],
          dtype='object')




```python
data_train.drop(["fnlwgt","education"],axis=1,inplace=True)
```


```python
data_train.columns
```




    Index(['age', 'workclass', 'education-num', 'marital-status', 'occupation',
           'relationship', 'race', 'sex', 'capital-gain', 'capital-loss',
           'hours-per-week', 'native-country', 'income'],
          dtype='object')




```python
data_test.columns
```




    Index(['age', 'workclass', 'fnlwgt', 'education', 'education-num',
           'marital-status', 'occupation', 'relationship', 'race', 'sex',
           'capital-gain', 'capital-loss', 'hours-per-week', 'native-country'],
          dtype='object')




```python
data_test.drop(["fnlwgt","education"],axis=1,inplace=True)
```


```python
data_test.columns
```




    Index(['age', 'workclass', 'education-num', 'marital-status', 'occupation',
           'relationship', 'race', 'sex', 'capital-gain', 'capital-loss',
           'hours-per-week', 'native-country'],
          dtype='object')



### 데이터 전처리 수정
- workclass(" ?"를 " Private"로 수정), native-country(" ?"를 " United-States"로 수정)


```python
# train의 workclass 컬럼의 values 값 수정

data_train = data_train.replace({"workclass" : " ?"}," Private")
data_train
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
      <th>age</th>
      <th>workclass</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
      <th>income</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>25</td>
      <td>Private</td>
      <td>7</td>
      <td>Divorced</td>
      <td>Machine-op-inspct</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>39</td>
      <td>Private</td>
      <td>10</td>
      <td>Divorced</td>
      <td>Other-service</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1721</td>
      <td>55</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35</td>
      <td>Private</td>
      <td>13</td>
      <td>Never-married</td>
      <td>Handlers-cleaners</td>
      <td>Not-in-family</td>
      <td>Asian-Pac-Islander</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>Haiti</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>64</td>
      <td>Private</td>
      <td>11</td>
      <td>Divorced</td>
      <td>Tech-support</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24</td>
      <td>Private</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Transport-moving</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>70</td>
      <td>United-States</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>29301</th>
      <td>20</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Other</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>Puerto-Rico</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29302</th>
      <td>21</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29303</th>
      <td>18</td>
      <td>Self-emp-not-inc</td>
      <td>6</td>
      <td>Never-married</td>
      <td>Farming-fishing</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29304</th>
      <td>33</td>
      <td>Private</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Craft-repair</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>65</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29305</th>
      <td>65</td>
      <td>Private</td>
      <td>9</td>
      <td>Widowed</td>
      <td>?</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>29305 rows × 13 columns</p>
</div>




```python
# train의 native-country 컬럼의 values 값 수정

data_train = data_train.replace({"native-country" : " ?"}," United-States")
data_train
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
      <th>age</th>
      <th>workclass</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
      <th>income</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>25</td>
      <td>Private</td>
      <td>7</td>
      <td>Divorced</td>
      <td>Machine-op-inspct</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>39</td>
      <td>Private</td>
      <td>10</td>
      <td>Divorced</td>
      <td>Other-service</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1721</td>
      <td>55</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35</td>
      <td>Private</td>
      <td>13</td>
      <td>Never-married</td>
      <td>Handlers-cleaners</td>
      <td>Not-in-family</td>
      <td>Asian-Pac-Islander</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>Haiti</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>64</td>
      <td>Private</td>
      <td>11</td>
      <td>Divorced</td>
      <td>Tech-support</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24</td>
      <td>Private</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Transport-moving</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>70</td>
      <td>United-States</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>29301</th>
      <td>20</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Other</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>Puerto-Rico</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29302</th>
      <td>21</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29303</th>
      <td>18</td>
      <td>Self-emp-not-inc</td>
      <td>6</td>
      <td>Never-married</td>
      <td>Farming-fishing</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29304</th>
      <td>33</td>
      <td>Private</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Craft-repair</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>65</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29305</th>
      <td>65</td>
      <td>Private</td>
      <td>9</td>
      <td>Widowed</td>
      <td>?</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>29305 rows × 13 columns</p>
</div>




```python
# test의 workclass 컬럼의 values 값 수정

data_test = data_test.replace({"workclass" : " ?"}," Private")
data_test
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
      <th>age</th>
      <th>workclass</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29306</th>
      <td>18</td>
      <td>Private</td>
      <td>10</td>
      <td>Never-married</td>
      <td>?</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>16</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29307</th>
      <td>29</td>
      <td>Private</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Other-service</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29308</th>
      <td>45</td>
      <td>Private</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29309</th>
      <td>42</td>
      <td>Self-emp-not-inc</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29310</th>
      <td>41</td>
      <td>Private</td>
      <td>16</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1902</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48838</th>
      <td>45</td>
      <td>Self-emp-not-inc</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Craft-repair</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>60</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48839</th>
      <td>48</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Exec-managerial</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48840</th>
      <td>63</td>
      <td>Private</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Prof-specialty</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>4386</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48841</th>
      <td>18</td>
      <td>Private</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48842</th>
      <td>31</td>
      <td>Private</td>
      <td>12</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>46</td>
      <td>United-States</td>
    </tr>
  </tbody>
</table>
<p>19537 rows × 12 columns</p>
</div>




```python
# test의 native-country 컬럼의 values 값 수정

data_test = data_test.replace({"native-country" : " ?"}," United-States")
data_test
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
      <th>age</th>
      <th>workclass</th>
      <th>education-num</th>
      <th>marital-status</th>
      <th>occupation</th>
      <th>relationship</th>
      <th>race</th>
      <th>sex</th>
      <th>capital-gain</th>
      <th>capital-loss</th>
      <th>hours-per-week</th>
      <th>native-country</th>
    </tr>
    <tr>
      <th>no</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29306</th>
      <td>18</td>
      <td>Private</td>
      <td>10</td>
      <td>Never-married</td>
      <td>?</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>16</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29307</th>
      <td>29</td>
      <td>Private</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Other-service</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29308</th>
      <td>45</td>
      <td>Private</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29309</th>
      <td>42</td>
      <td>Self-emp-not-inc</td>
      <td>13</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>29310</th>
      <td>41</td>
      <td>Private</td>
      <td>16</td>
      <td>Married-civ-spouse</td>
      <td>Exec-managerial</td>
      <td>Wife</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>1902</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>48838</th>
      <td>45</td>
      <td>Self-emp-not-inc</td>
      <td>9</td>
      <td>Married-civ-spouse</td>
      <td>Craft-repair</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>60</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48839</th>
      <td>48</td>
      <td>Private</td>
      <td>9</td>
      <td>Never-married</td>
      <td>Exec-managerial</td>
      <td>Not-in-family</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>50</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48840</th>
      <td>63</td>
      <td>Private</td>
      <td>10</td>
      <td>Married-civ-spouse</td>
      <td>Prof-specialty</td>
      <td>Husband</td>
      <td>White</td>
      <td>Male</td>
      <td>4386</td>
      <td>0</td>
      <td>40</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48841</th>
      <td>18</td>
      <td>Private</td>
      <td>7</td>
      <td>Never-married</td>
      <td>Sales</td>
      <td>Own-child</td>
      <td>White</td>
      <td>Female</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>United-States</td>
    </tr>
    <tr>
      <th>48842</th>
      <td>31</td>
      <td>Private</td>
      <td>12</td>
      <td>Married-civ-spouse</td>
      <td>Tech-support</td>
      <td>Husband</td>
      <td>Black</td>
      <td>Male</td>
      <td>0</td>
      <td>0</td>
      <td>46</td>
      <td>United-States</td>
    </tr>
  </tbody>
</table>
<p>19537 rows × 12 columns</p>
</div>



## 원 핫 인코딩
- values값이 문자열인 컬럼을 values값을 0 또는 1로 변경시켜주는 과정


```python
categorical_features = ['workclass','marital-status', 'occupation',
                        'relationship',"race",'sex',"native-country"]
categorical_features
```




    ['workclass',
     'marital-status',
     'occupation',
     'relationship',
     'race',
     'sex',
     'native-country']




```python
for feature_name in categorical_features:
    # 원핫인코딩
    one_hot = pd.get_dummies(data_train[feature_name],prefix=feature_name)
    
    # 기존 문자 형태 컬럼을 삭제
    data_train.drop(feature_name,axis=1,inplace=True)
    
    # 기존 data.train 데이터에 원핫데이터 병합하기
    data_train=pd.concat([data_train,one_hot],axis=1)
```


```python
categorical_features2 = ['workclass','marital-status', 'occupation',
                        'relationship',"race",'sex',"native-country"]
categorical_features2
```




    ['workclass',
     'marital-status',
     'occupation',
     'relationship',
     'race',
     'sex',
     'native-country']




```python
for feature_name in categorical_features2:
    # 원핫인코딩
    one_hot = pd.get_dummies(data_test[feature_name],prefix=feature_name)
    
    # 기존 문자 형태 컬럼을 삭제
    data_test.drop(feature_name,axis=1,inplace=True)
    
    # 기존 data.train 데이터에 원핫데이터 병합하기
    data_test=pd.concat([data_test,one_hot],axis=1)
```

## train과 test 비교
- train과 test를 비교하여 한쪽에만 있는 컬럼이 있는지 확인하기(학습과 평가를 위해선 컬럼의 이름과 순서가 일치해야함)


```python
# train 컬럼 확인

a = set(data_train.columns)
a
```




    {'age',
     'capital-gain',
     'capital-loss',
     'education-num',
     'hours-per-week',
     'income',
     'marital-status_ Divorced',
     'marital-status_ Married-AF-spouse',
     'marital-status_ Married-civ-spouse',
     'marital-status_ Married-spouse-absent',
     'marital-status_ Never-married',
     'marital-status_ Separated',
     'marital-status_ Widowed',
     'native-country_ Cambodia',
     'native-country_ Canada',
     'native-country_ China',
     'native-country_ Columbia',
     'native-country_ Cuba',
     'native-country_ Dominican-Republic',
     'native-country_ Ecuador',
     'native-country_ El-Salvador',
     'native-country_ England',
     'native-country_ France',
     'native-country_ Germany',
     'native-country_ Greece',
     'native-country_ Guatemala',
     'native-country_ Haiti',
     'native-country_ Holand-Netherlands',
     'native-country_ Honduras',
     'native-country_ Hong',
     'native-country_ Hungary',
     'native-country_ India',
     'native-country_ Iran',
     'native-country_ Ireland',
     'native-country_ Italy',
     'native-country_ Jamaica',
     'native-country_ Japan',
     'native-country_ Laos',
     'native-country_ Mexico',
     'native-country_ Nicaragua',
     'native-country_ Outlying-US(Guam-USVI-etc)',
     'native-country_ Peru',
     'native-country_ Philippines',
     'native-country_ Poland',
     'native-country_ Portugal',
     'native-country_ Puerto-Rico',
     'native-country_ Scotland',
     'native-country_ South',
     'native-country_ Taiwan',
     'native-country_ Thailand',
     'native-country_ Trinadad&Tobago',
     'native-country_ United-States',
     'native-country_ Vietnam',
     'native-country_ Yugoslavia',
     'occupation_ ?',
     'occupation_ Adm-clerical',
     'occupation_ Armed-Forces',
     'occupation_ Craft-repair',
     'occupation_ Exec-managerial',
     'occupation_ Farming-fishing',
     'occupation_ Handlers-cleaners',
     'occupation_ Machine-op-inspct',
     'occupation_ Other-service',
     'occupation_ Priv-house-serv',
     'occupation_ Prof-specialty',
     'occupation_ Protective-serv',
     'occupation_ Sales',
     'occupation_ Tech-support',
     'occupation_ Transport-moving',
     'race_ Amer-Indian-Eskimo',
     'race_ Asian-Pac-Islander',
     'race_ Black',
     'race_ Other',
     'race_ White',
     'relationship_ Husband',
     'relationship_ Not-in-family',
     'relationship_ Other-relative',
     'relationship_ Own-child',
     'relationship_ Unmarried',
     'relationship_ Wife',
     'sex_ Female',
     'sex_ Male',
     'workclass_ Federal-gov',
     'workclass_ Local-gov',
     'workclass_ Never-worked',
     'workclass_ Private',
     'workclass_ Self-emp-inc',
     'workclass_ Self-emp-not-inc',
     'workclass_ State-gov',
     'workclass_ Without-pay'}




```python
# test 컬럼 학인

b = set(data_test.columns)
b
```




    {'age',
     'capital-gain',
     'capital-loss',
     'education-num',
     'hours-per-week',
     'marital-status_ Divorced',
     'marital-status_ Married-AF-spouse',
     'marital-status_ Married-civ-spouse',
     'marital-status_ Married-spouse-absent',
     'marital-status_ Never-married',
     'marital-status_ Separated',
     'marital-status_ Widowed',
     'native-country_ Cambodia',
     'native-country_ Canada',
     'native-country_ China',
     'native-country_ Columbia',
     'native-country_ Cuba',
     'native-country_ Dominican-Republic',
     'native-country_ Ecuador',
     'native-country_ El-Salvador',
     'native-country_ England',
     'native-country_ France',
     'native-country_ Germany',
     'native-country_ Greece',
     'native-country_ Guatemala',
     'native-country_ Haiti',
     'native-country_ Honduras',
     'native-country_ Hong',
     'native-country_ Hungary',
     'native-country_ India',
     'native-country_ Iran',
     'native-country_ Ireland',
     'native-country_ Italy',
     'native-country_ Jamaica',
     'native-country_ Japan',
     'native-country_ Laos',
     'native-country_ Mexico',
     'native-country_ Nicaragua',
     'native-country_ Outlying-US(Guam-USVI-etc)',
     'native-country_ Peru',
     'native-country_ Philippines',
     'native-country_ Poland',
     'native-country_ Portugal',
     'native-country_ Puerto-Rico',
     'native-country_ Scotland',
     'native-country_ South',
     'native-country_ Taiwan',
     'native-country_ Thailand',
     'native-country_ Trinadad&Tobago',
     'native-country_ United-States',
     'native-country_ Vietnam',
     'native-country_ Yugoslavia',
     'occupation_ ?',
     'occupation_ Adm-clerical',
     'occupation_ Armed-Forces',
     'occupation_ Craft-repair',
     'occupation_ Exec-managerial',
     'occupation_ Farming-fishing',
     'occupation_ Handlers-cleaners',
     'occupation_ Machine-op-inspct',
     'occupation_ Other-service',
     'occupation_ Priv-house-serv',
     'occupation_ Prof-specialty',
     'occupation_ Protective-serv',
     'occupation_ Sales',
     'occupation_ Tech-support',
     'occupation_ Transport-moving',
     'race_ Amer-Indian-Eskimo',
     'race_ Asian-Pac-Islander',
     'race_ Black',
     'race_ Other',
     'race_ White',
     'relationship_ Husband',
     'relationship_ Not-in-family',
     'relationship_ Other-relative',
     'relationship_ Own-child',
     'relationship_ Unmarried',
     'relationship_ Wife',
     'sex_ Female',
     'sex_ Male',
     'workclass_ Federal-gov',
     'workclass_ Local-gov',
     'workclass_ Never-worked',
     'workclass_ Private',
     'workclass_ Self-emp-inc',
     'workclass_ Self-emp-not-inc',
     'workclass_ State-gov',
     'workclass_ Without-pay'}




```python
# train과 test의 차집합 구하기

a-b
```




    {'income', 'native-country_ Holand-Netherlands'}




```python
# train과 test의 차집합 구하기

b-a
```




    set()




```python
data_test["native-country_ Holand-Netherlands"]=0;
```

## 학습 준비


```python
# 문제 및 정답 분리
X_data_train = data_train.drop("income", axis=1)
y_data_train = data_train["income"]
```


```python
data_test=data_test[X_data_train.columns]
```


```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import cross_val_score
```


```python
# # 오차를 얼마나 반영할지(학습율)
# gb_model = GradientBoostingClassifier(n_estimators=1000,
#                                      max_depth=5,
#                                      learning_rate=0.01)
```


```python
gb_model = GradientBoostingClassifier(n_estimators=350,
                                     max_depth=6,
                                      max_features=0.5,
                                      min_samples_leaf=10
                                     )
```


```python
gb_model.fit(X_data_train,y_data_train)
```




<style>#sk-container-id-1 {color: black;background-color: white;}#sk-container-id-1 pre{padding: 0;}#sk-container-id-1 div.sk-toggleable {background-color: white;}#sk-container-id-1 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-1 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-1 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-1 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-1 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-1 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-1 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-1 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-1 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-1 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-1 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-1 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-1 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-1 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-1 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-1 div.sk-item {position: relative;z-index: 1;}#sk-container-id-1 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-1 div.sk-item::before, #sk-container-id-1 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-1 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-1 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-1 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-1 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-1 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-1 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-1 div.sk-label-container {text-align: center;}#sk-container-id-1 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-1 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-1" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>GradientBoostingClassifier(max_depth=6, max_features=0.5, min_samples_leaf=10,
                           n_estimators=350)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-1" type="checkbox" checked><label for="sk-estimator-id-1" class="sk-toggleable__label sk-toggleable__label-arrow">GradientBoostingClassifier</label><div class="sk-toggleable__content"><pre>GradientBoostingClassifier(max_depth=6, max_features=0.5, min_samples_leaf=10,
                           n_estimators=350)</pre></div></div></div></div></div>




```python
# 교차검증 및 데이터 수정 후
# 0.8707729056475003

gb_result = cross_val_score(gb_model,X_data_train,y_data_train,cv=5)
gb_result.mean()
```




    0.8696809418188023




```python
pre = gb_model.predict(data_test)
```

## 예측 결과를 엑셀 파일로 저장하기


```python
submission = pd.read_csv("sample_submission.csv")
```


```python
submission
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
      <th>no</th>
      <th>income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>29306</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29307</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>29308</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>29309</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>29310</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>19532</th>
      <td>48838</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19533</th>
      <td>48839</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19534</th>
      <td>48840</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19535</th>
      <td>48841</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19536</th>
      <td>48842</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>19537 rows × 2 columns</p>
</div>




```python
submission["income"] = pre
```


```python
# 원하는 파일명으로 저장
submission.to_csv("submission_6.csv",index=False)
```
