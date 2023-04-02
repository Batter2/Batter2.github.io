---
layout: single
title:  "Linear Model_Regression"
categories: MachineLearning
toc: true
---

### 성적 데이터 생성


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```


```python
data = pd.DataFrame({"시간":[2,4,8,9],"성적":[20,40,80,90]},
                   index=["아라","태윤","지은","강민"])
data
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
      <th>시간</th>
      <th>성적</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>아라</th>
      <td>2</td>
      <td>20</td>
    </tr>
    <tr>
      <th>태윤</th>
      <td>4</td>
      <td>40</td>
    </tr>
    <tr>
      <th>지은</th>
      <td>8</td>
      <td>80</td>
    </tr>
    <tr>
      <th>강민</th>
      <td>9</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>



##### 수학 공식을 이용한 해석적 모델
- LinearRegression


```python
from sklearn.linear_model import LinearRegression
```


```python
linear_model = LinearRegression()
```


```python
data["성적"]
```




    아라    20
    태윤    40
    지은    80
    강민    90
    Name: 성적, dtype: int64




```python
data[["시간"]]
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
      <th>시간</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>아라</th>
      <td>2</td>
    </tr>
    <tr>
      <th>태윤</th>
      <td>4</td>
    </tr>
    <tr>
      <th>지은</th>
      <td>8</td>
    </tr>
    <tr>
      <th>강민</th>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 문제 : 2차원 // 정답 : 1차원
linear_model.fit(data[["시간"]],data["성적"])
```




<style>#sk-container-id-3 {color: black;background-color: white;}#sk-container-id-3 pre{padding: 0;}#sk-container-id-3 div.sk-toggleable {background-color: white;}#sk-container-id-3 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-3 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-3 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-3 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-3 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-3 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-3 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-3 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-3 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-3 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-3 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-3 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-3 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-3 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-3 div.sk-item {position: relative;z-index: 1;}#sk-container-id-3 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-3 div.sk-item::before, #sk-container-id-3 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-3 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-3 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-3 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-3 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-3 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-3 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-3 div.sk-label-container {text-align: center;}#sk-container-id-3 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-3 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-3" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>LinearRegression()</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-3" type="checkbox" checked><label for="sk-estimator-id-3" class="sk-toggleable__label sk-toggleable__label-arrow">LinearRegression</label><div class="sk-toggleable__content"><pre>LinearRegression()</pre></div></div></div></div></div>




```python
# 가중치
print(linear_model.coef_)
# 절편
print(linear_model.intercept_)
```

    [10.]
    7.105427357601002e-15
    


```python
# 예측
linear_model.predict([[7]])
```

    D:\anaconda3\lib\site-packages\sklearn\base.py:420: UserWarning: X does not have valid feature names, but LinearRegression was fitted with feature names
      warnings.warn(
    




    array([70.])



### 경사하강법
- 가중치(w)의 변화에 따른 비용함수(cost)값의 변화를 확인해보자!

###### H(x)


```python
def h(w,x):
    return w*x+0
```

###### Cost function


```python
def cost(data, target, weight):      # MSE
    y_pre = h(weight,data)           # 예측값
    return ((y_pre-target) ** 2).mean() # cost값

# ** : 제곱
```


```python
cost(data["시간"],data["성적"],10)
```




    0.0




```python
weight_range=range(-10,31)
```


```python
cost_list = []
for w in weight_range:
    c = cost(data["시간"],data["성적"],w)
    cost_list.append(c)
cost_list
```




    [16500.0,
     14891.25,
     13365.0,
     11921.25,
     10560.0,
     9281.25,
     8085.0,
     6971.25,
     5940.0,
     4991.25,
     4125.0,
     3341.25,
     2640.0,
     2021.25,
     1485.0,
     1031.25,
     660.0,
     371.25,
     165.0,
     41.25,
     0.0,
     41.25,
     165.0,
     371.25,
     660.0,
     1031.25,
     1485.0,
     2021.25,
     2640.0,
     3341.25,
     4125.0,
     4991.25,
     5940.0,
     6971.25,
     8085.0,
     9281.25,
     10560.0,
     11921.25,
     13365.0,
     14891.25,
     16500.0]




```python
plt.plot(weight_range,cost_list)
plt.show()
```


    
![output_19_0](https://user-images.githubusercontent.com/112329594/229342796-420006ed-f944-4119-adb4-9b26f2d6e610.png)
    


- SGDRegressor 사용하기!


```python
from sklearn.linear_model import SGDRegressor
```


```python
sgd_model = SGDRegressor(max_iter=5000, # 가중치 업데이트 횟수
    eta0=0.01, # 학습률(learning rate)
                        verbose=1) # 학습 과정 확인 가능!
```


```python
# 학습
sgd_model.fit(data[["시간"]],data["성적"])
```

    -- Epoch 1
    Norm: 8.49, NNZs: 1, Bias: 1.207604, T: 4, Avg. loss: 959.643225
    Total training time: 0.00 seconds.
    -- Epoch 2
    Norm: 9.50, NNZs: 1, Bias: 1.323629, T: 8, Avg. loss: 22.057521
    Total training time: 0.00 seconds.
    -- Epoch 3
    Norm: 9.72, NNZs: 1, Bias: 1.346364, T: 12, Avg. loss: 1.523383
    Total training time: 0.00 seconds.
    -- Epoch 4
    Norm: 9.79, NNZs: 1, Bias: 1.351327, T: 16, Avg. loss: 0.322710
    Total training time: 0.00 seconds.
    -- Epoch 5
    Norm: 9.81, NNZs: 1, Bias: 1.348677, T: 20, Avg. loss: 0.210055
    Total training time: 0.00 seconds.
    -- Epoch 6
    Norm: 9.81, NNZs: 1, Bias: 1.343348, T: 24, Avg. loss: 0.191302
    Total training time: 0.00 seconds.
    -- Epoch 7
    Norm: 9.81, NNZs: 1, Bias: 1.339149, T: 28, Avg. loss: 0.194562
    Total training time: 0.00 seconds.
    -- Epoch 8
    Norm: 9.82, NNZs: 1, Bias: 1.335912, T: 32, Avg. loss: 0.188646
    Total training time: 0.00 seconds.
    -- Epoch 9
    Norm: 9.82, NNZs: 1, Bias: 1.331458, T: 36, Avg. loss: 0.188640
    Total training time: 0.00 seconds.
    -- Epoch 10
    Norm: 9.81, NNZs: 1, Bias: 1.326197, T: 40, Avg. loss: 0.182491
    Total training time: 0.00 seconds.
    -- Epoch 11
    Norm: 9.81, NNZs: 1, Bias: 1.322140, T: 44, Avg. loss: 0.185861
    Total training time: 0.00 seconds.
    -- Epoch 12
    Norm: 9.82, NNZs: 1, Bias: 1.319053, T: 48, Avg. loss: 0.182591
    Total training time: 0.00 seconds.
    -- Epoch 13
    Norm: 9.82, NNZs: 1, Bias: 1.315052, T: 52, Avg. loss: 0.182776
    Total training time: 0.00 seconds.
    -- Epoch 14
    Norm: 9.82, NNZs: 1, Bias: 1.310339, T: 56, Avg. loss: 0.177754
    Total training time: 0.00 seconds.
    -- Epoch 15
    Norm: 9.82, NNZs: 1, Bias: 1.306604, T: 60, Avg. loss: 0.180372
    Total training time: 0.00 seconds.
    -- Epoch 16
    Norm: 9.82, NNZs: 1, Bias: 1.303717, T: 64, Avg. loss: 0.177737
    Total training time: 0.00 seconds.
    -- Epoch 17
    Norm: 9.82, NNZs: 1, Bias: 1.300053, T: 68, Avg. loss: 0.177852
    Total training time: 0.00 seconds.
    -- Epoch 18
    Norm: 9.82, NNZs: 1, Bias: 1.295733, T: 72, Avg. loss: 0.173541
    Total training time: 0.00 seconds.
    -- Epoch 19
    Norm: 9.82, NNZs: 1, Bias: 1.292242, T: 76, Avg. loss: 0.175694
    Total training time: 0.00 seconds.
    -- Epoch 20
    Norm: 9.82, NNZs: 1, Bias: 1.289511, T: 80, Avg. loss: 0.173438
    Total training time: 0.00 seconds.
    -- Epoch 21
    Norm: 9.83, NNZs: 1, Bias: 1.286095, T: 84, Avg. loss: 0.173501
    Total training time: 0.00 seconds.
    -- Epoch 22
    Norm: 9.82, NNZs: 1, Bias: 1.282070, T: 88, Avg. loss: 0.169686
    Total training time: 0.00 seconds.
    -- Epoch 23
    Norm: 9.82, NNZs: 1, Bias: 1.278771, T: 92, Avg. loss: 0.171517
    Total training time: 0.00 seconds.
    -- Epoch 24
    Norm: 9.83, NNZs: 1, Bias: 1.276165, T: 96, Avg. loss: 0.169525
    Total training time: 0.00 seconds.
    -- Epoch 25
    Norm: 9.83, NNZs: 1, Bias: 1.272944, T: 100, Avg. loss: 0.169551
    Total training time: 0.00 seconds.
    -- Epoch 26
    Norm: 9.82, NNZs: 1, Bias: 1.269153, T: 104, Avg. loss: 0.166108
    Total training time: 0.00 seconds.
    -- Epoch 27
    Norm: 9.82, NNZs: 1, Bias: 1.266012, T: 108, Avg. loss: 0.167701
    Total training time: 0.00 seconds.
    -- Epoch 28
    Norm: 9.83, NNZs: 1, Bias: 1.263511, T: 112, Avg. loss: 0.165907
    Total training time: 0.00 seconds.
    -- Epoch 29
    Norm: 9.83, NNZs: 1, Bias: 1.260449, T: 116, Avg. loss: 0.165905
    Total training time: 0.00 seconds.
    -- Epoch 30
    Norm: 9.82, NNZs: 1, Bias: 1.256850, T: 120, Avg. loss: 0.162756
    Total training time: 0.00 seconds.
    -- Epoch 31
    Norm: 9.82, NNZs: 1, Bias: 1.253843, T: 124, Avg. loss: 0.164165
    Total training time: 0.00 seconds.
    -- Epoch 32
    Norm: 9.83, NNZs: 1, Bias: 1.251432, T: 128, Avg. loss: 0.162525
    Total training time: 0.00 seconds.
    -- Epoch 33
    Norm: 9.83, NNZs: 1, Bias: 1.248503, T: 132, Avg. loss: 0.162502
    Total training time: 0.00 seconds.
    -- Epoch 34
    Norm: 9.83, NNZs: 1, Bias: 1.245068, T: 136, Avg. loss: 0.159593
    Total training time: 0.00 seconds.
    -- Epoch 35
    Norm: 9.83, NNZs: 1, Bias: 1.242176, T: 140, Avg. loss: 0.160854
    Total training time: 0.00 seconds.
    -- Epoch 36
    Norm: 9.83, NNZs: 1, Bias: 1.239843, T: 144, Avg. loss: 0.159340
    Total training time: 0.00 seconds.
    -- Epoch 37
    Norm: 9.83, NNZs: 1, Bias: 1.237030, T: 148, Avg. loss: 0.159301
    Total training time: 0.00 seconds.
    -- Epoch 38
    Norm: 9.83, NNZs: 1, Bias: 1.233736, T: 152, Avg. loss: 0.156591
    Total training time: 0.00 seconds.
    -- Epoch 39
    Norm: 9.83, NNZs: 1, Bias: 1.230945, T: 156, Avg. loss: 0.157731
    Total training time: 0.00 seconds.
    -- Epoch 40
    Norm: 9.83, NNZs: 1, Bias: 1.228683, T: 160, Avg. loss: 0.156322
    Total training time: 0.00 seconds.
    -- Epoch 41
    Norm: 9.83, NNZs: 1, Bias: 1.225971, T: 164, Avg. loss: 0.156270
    Total training time: 0.00 seconds.
    -- Epoch 42
    Norm: 9.83, NNZs: 1, Bias: 1.222800, T: 168, Avg. loss: 0.153730
    Total training time: 0.00 seconds.
    -- Epoch 43
    Norm: 9.83, NNZs: 1, Bias: 1.220100, T: 172, Avg. loss: 0.154770
    Total training time: 0.00 seconds.
    -- Epoch 44
    Norm: 9.83, NNZs: 1, Bias: 1.217900, T: 176, Avg. loss: 0.153449
    Total training time: 0.00 seconds.
    -- Epoch 45
    Norm: 9.83, NNZs: 1, Bias: 1.215278, T: 180, Avg. loss: 0.153387
    Total training time: 0.00 seconds.
    -- Epoch 46
    Norm: 9.83, NNZs: 1, Bias: 1.212217, T: 184, Avg. loss: 0.150995
    Total training time: 0.00 seconds.
    -- Epoch 47
    Norm: 9.83, NNZs: 1, Bias: 1.209599, T: 188, Avg. loss: 0.151948
    Total training time: 0.00 seconds.
    -- Epoch 48
    Norm: 9.83, NNZs: 1, Bias: 1.207457, T: 192, Avg. loss: 0.150705
    Total training time: 0.00 seconds.
    -- Epoch 49
    Norm: 9.83, NNZs: 1, Bias: 1.204915, T: 196, Avg. loss: 0.150634
    Total training time: 0.00 seconds.
    -- Epoch 50
    Norm: 9.83, NNZs: 1, Bias: 1.201953, T: 200, Avg. loss: 0.148372
    Total training time: 0.00 seconds.
    -- Epoch 51
    Norm: 9.83, NNZs: 1, Bias: 1.199409, T: 204, Avg. loss: 0.149251
    Total training time: 0.00 seconds.
    -- Epoch 52
    Norm: 9.84, NNZs: 1, Bias: 1.197319, T: 208, Avg. loss: 0.148075
    Total training time: 0.00 seconds.
    -- Epoch 53
    Norm: 9.84, NNZs: 1, Bias: 1.194851, T: 212, Avg. loss: 0.147997
    Total training time: 0.00 seconds.
    -- Epoch 54
    Norm: 9.83, NNZs: 1, Bias: 1.191979, T: 216, Avg. loss: 0.145850
    Total training time: 0.00 seconds.
    -- Epoch 55
    Norm: 9.83, NNZs: 1, Bias: 1.189502, T: 220, Avg. loss: 0.146665
    Total training time: 0.00 seconds.
    -- Epoch 56
    Norm: 9.84, NNZs: 1, Bias: 1.187462, T: 224, Avg. loss: 0.145548
    Total training time: 0.00 seconds.
    -- Epoch 57
    Norm: 9.84, NNZs: 1, Bias: 1.185060, T: 228, Avg. loss: 0.145464
    Total training time: 0.00 seconds.
    -- Epoch 58
    Norm: 9.83, NNZs: 1, Bias: 1.182270, T: 232, Avg. loss: 0.143421
    Total training time: 0.00 seconds.
    -- Epoch 59
    Norm: 9.84, NNZs: 1, Bias: 1.179855, T: 236, Avg. loss: 0.144178
    Total training time: 0.00 seconds.
    -- Epoch 60
    Norm: 9.84, NNZs: 1, Bias: 1.177860, T: 240, Avg. loss: 0.143115
    Total training time: 0.00 seconds.
    -- Epoch 61
    Norm: 9.84, NNZs: 1, Bias: 1.175520, T: 244, Avg. loss: 0.143026
    Total training time: 0.00 seconds.
    -- Epoch 62
    Norm: 9.84, NNZs: 1, Bias: 1.172805, T: 248, Avg. loss: 0.141076
    Total training time: 0.00 seconds.
    -- Epoch 63
    Norm: 9.84, NNZs: 1, Bias: 1.170449, T: 252, Avg. loss: 0.141783
    Total training time: 0.00 seconds.
    -- Epoch 64
    Norm: 9.84, NNZs: 1, Bias: 1.168496, T: 256, Avg. loss: 0.140767
    Total training time: 0.00 seconds.
    -- Epoch 65
    Norm: 9.84, NNZs: 1, Bias: 1.166213, T: 260, Avg. loss: 0.140675
    Total training time: 0.00 seconds.
    -- Epoch 66
    Norm: 9.84, NNZs: 1, Bias: 1.163568, T: 264, Avg. loss: 0.138809
    Total training time: 0.00 seconds.
    -- Epoch 67
    Norm: 9.84, NNZs: 1, Bias: 1.161266, T: 268, Avg. loss: 0.139472
    Total training time: 0.00 seconds.
    -- Epoch 68
    Norm: 9.84, NNZs: 1, Bias: 1.159353, T: 272, Avg. loss: 0.138499
    Total training time: 0.00 seconds.
    -- Epoch 69
    Norm: 9.84, NNZs: 1, Bias: 1.157123, T: 276, Avg. loss: 0.138404
    Total training time: 0.00 seconds.
    -- Epoch 70
    Norm: 9.84, NNZs: 1, Bias: 1.154542, T: 280, Avg. loss: 0.136615
    Total training time: 0.00 seconds.
    -- Epoch 71
    Norm: 9.84, NNZs: 1, Bias: 1.152291, T: 284, Avg. loss: 0.137238
    Total training time: 0.00 seconds.
    -- Epoch 72
    Norm: 9.84, NNZs: 1, Bias: 1.150415, T: 288, Avg. loss: 0.136305
    Total training time: 0.00 seconds.
    -- Epoch 73
    Norm: 9.84, NNZs: 1, Bias: 1.148235, T: 292, Avg. loss: 0.136207
    Total training time: 0.00 seconds.
    -- Epoch 74
    Norm: 9.84, NNZs: 1, Bias: 1.145714, T: 296, Avg. loss: 0.134489
    Total training time: 0.00 seconds.
    -- Epoch 75
    Norm: 9.84, NNZs: 1, Bias: 1.143510, T: 300, Avg. loss: 0.135075
    Total training time: 0.00 seconds.
    -- Epoch 76
    Norm: 9.84, NNZs: 1, Bias: 1.141670, T: 304, Avg. loss: 0.134178
    Total training time: 0.00 seconds.
    -- Epoch 77
    Norm: 9.84, NNZs: 1, Bias: 1.139536, T: 308, Avg. loss: 0.134078
    Total training time: 0.00 seconds.
    -- Epoch 78
    Norm: 9.84, NNZs: 1, Bias: 1.137073, T: 312, Avg. loss: 0.132426
    Total training time: 0.00 seconds.
    -- Epoch 79
    Norm: 9.84, NNZs: 1, Bias: 1.134913, T: 316, Avg. loss: 0.132978
    Total training time: 0.00 seconds.
    -- Epoch 80
    Norm: 9.84, NNZs: 1, Bias: 1.133106, T: 320, Avg. loss: 0.132115
    Total training time: 0.00 seconds.
    -- Epoch 81
    Norm: 9.84, NNZs: 1, Bias: 1.131016, T: 324, Avg. loss: 0.132014
    Total training time: 0.00 seconds.
    -- Epoch 82
    Norm: 9.84, NNZs: 1, Bias: 1.128606, T: 328, Avg. loss: 0.130422
    Total training time: 0.00 seconds.
    -- Epoch 83
    Norm: 9.84, NNZs: 1, Bias: 1.126489, T: 332, Avg. loss: 0.130945
    Total training time: 0.00 seconds.
    -- Epoch 84
    Norm: 9.85, NNZs: 1, Bias: 1.124714, T: 336, Avg. loss: 0.130112
    Total training time: 0.00 seconds.
    -- Epoch 85
    Norm: 9.85, NNZs: 1, Bias: 1.122665, T: 340, Avg. loss: 0.130009
    Total training time: 0.00 seconds.
    -- Epoch 86
    Norm: 9.84, NNZs: 1, Bias: 1.120305, T: 344, Avg. loss: 0.128474
    Total training time: 0.00 seconds.
    -- Epoch 87
    Norm: 9.84, NNZs: 1, Bias: 1.118228, T: 348, Avg. loss: 0.128969
    Total training time: 0.00 seconds.
    -- Epoch 88
    Norm: 9.85, NNZs: 1, Bias: 1.116483, T: 352, Avg. loss: 0.128166
    Total training time: 0.00 seconds.
    -- Epoch 89
    Norm: 9.85, NNZs: 1, Bias: 1.114474, T: 356, Avg. loss: 0.128062
    Total training time: 0.00 seconds.
    -- Epoch 90
    Norm: 9.84, NNZs: 1, Bias: 1.112161, T: 360, Avg. loss: 0.126579
    Total training time: 0.00 seconds.
    -- Epoch 91
    Norm: 9.84, NNZs: 1, Bias: 1.110122, T: 364, Avg. loss: 0.127048
    Total training time: 0.00 seconds.
    -- Epoch 92
    Norm: 9.85, NNZs: 1, Bias: 1.108406, T: 368, Avg. loss: 0.126272
    Total training time: 0.00 seconds.
    -- Epoch 93
    Norm: 9.85, NNZs: 1, Bias: 1.106434, T: 372, Avg. loss: 0.126167
    Total training time: 0.00 seconds.
    -- Epoch 94
    Norm: 9.85, NNZs: 1, Bias: 1.104166, T: 376, Avg. loss: 0.124734
    Total training time: 0.00 seconds.
    -- Epoch 95
    Norm: 9.85, NNZs: 1, Bias: 1.102164, T: 380, Avg. loss: 0.125180
    Total training time: 0.00 seconds.
    -- Epoch 96
    Norm: 9.85, NNZs: 1, Bias: 1.100475, T: 384, Avg. loss: 0.124428
    Total training time: 0.00 seconds.
    -- Epoch 97
    Norm: 9.85, NNZs: 1, Bias: 1.098538, T: 388, Avg. loss: 0.124323
    Total training time: 0.00 seconds.
    -- Epoch 98
    Norm: 9.85, NNZs: 1, Bias: 1.096314, T: 392, Avg. loss: 0.122936
    Total training time: 0.00 seconds.
    -- Epoch 99
    Norm: 9.85, NNZs: 1, Bias: 1.094346, T: 396, Avg. loss: 0.123360
    Total training time: 0.00 seconds.
    -- Epoch 100
    Norm: 9.85, NNZs: 1, Bias: 1.092684, T: 400, Avg. loss: 0.122632
    Total training time: 0.00 seconds.
    -- Epoch 101
    Norm: 9.85, NNZs: 1, Bias: 1.090781, T: 404, Avg. loss: 0.122526
    Total training time: 0.00 seconds.
    -- Epoch 102
    Norm: 9.85, NNZs: 1, Bias: 1.088597, T: 408, Avg. loss: 0.121183
    Total training time: 0.00 seconds.
    -- Epoch 103
    Norm: 9.85, NNZs: 1, Bias: 1.086662, T: 412, Avg. loss: 0.121587
    Total training time: 0.00 seconds.
    -- Epoch 104
    Norm: 9.85, NNZs: 1, Bias: 1.085025, T: 416, Avg. loss: 0.120881
    Total training time: 0.00 seconds.
    -- Epoch 105
    Norm: 9.85, NNZs: 1, Bias: 1.083154, T: 420, Avg. loss: 0.120775
    Total training time: 0.00 seconds.
    -- Epoch 106
    Norm: 9.85, NNZs: 1, Bias: 1.081009, T: 424, Avg. loss: 0.119472
    Total training time: 0.00 seconds.
    -- Epoch 107
    Norm: 9.85, NNZs: 1, Bias: 1.079106, T: 428, Avg. loss: 0.119858
    Total training time: 0.00 seconds.
    -- Epoch 108
    Norm: 9.85, NNZs: 1, Bias: 1.077494, T: 432, Avg. loss: 0.119173
    Total training time: 0.00 seconds.
    -- Epoch 109
    Norm: 9.85, NNZs: 1, Bias: 1.075654, T: 436, Avg. loss: 0.119067
    Total training time: 0.00 seconds.
    -- Epoch 110
    Norm: 9.85, NNZs: 1, Bias: 1.073546, T: 440, Avg. loss: 0.117803
    Total training time: 0.00 seconds.
    -- Epoch 111
    Norm: 9.85, NNZs: 1, Bias: 1.071673, T: 444, Avg. loss: 0.118171
    Total training time: 0.00 seconds.
    -- Epoch 112
    Norm: 9.85, NNZs: 1, Bias: 1.070085, T: 448, Avg. loss: 0.117507
    Total training time: 0.00 seconds.
    -- Epoch 113
    Norm: 9.85, NNZs: 1, Bias: 1.068274, T: 452, Avg. loss: 0.117400
    Total training time: 0.00 seconds.
    -- Epoch 114
    Norm: 9.85, NNZs: 1, Bias: 1.066202, T: 456, Avg. loss: 0.116173
    Total training time: 0.00 seconds.
    -- Epoch 115
    Norm: 9.85, NNZs: 1, Bias: 1.064359, T: 460, Avg. loss: 0.116525
    Total training time: 0.00 seconds.
    -- Epoch 116
    Norm: 9.85, NNZs: 1, Bias: 1.062792, T: 464, Avg. loss: 0.115879
    Total training time: 0.00 seconds.
    -- Epoch 117
    Norm: 9.85, NNZs: 1, Bias: 1.061010, T: 468, Avg. loss: 0.115772
    Total training time: 0.00 seconds.
    -- Epoch 118
    Norm: 9.85, NNZs: 1, Bias: 1.058972, T: 472, Avg. loss: 0.114580
    Total training time: 0.00 seconds.
    -- Epoch 119
    Norm: 9.85, NNZs: 1, Bias: 1.057157, T: 476, Avg. loss: 0.114917
    Total training time: 0.00 seconds.
    -- Epoch 120
    Norm: 9.85, NNZs: 1, Bias: 1.055613, T: 480, Avg. loss: 0.114288
    Total training time: 0.00 seconds.
    -- Epoch 121
    Norm: 9.85, NNZs: 1, Bias: 1.053858, T: 484, Avg. loss: 0.114182
    Total training time: 0.00 seconds.
    -- Epoch 122
    Norm: 9.85, NNZs: 1, Bias: 1.051853, T: 488, Avg. loss: 0.113023
    Total training time: 0.00 seconds.
    -- Epoch 123
    Norm: 9.85, NNZs: 1, Bias: 1.050065, T: 492, Avg. loss: 0.113345
    Total training time: 0.00 seconds.
    -- Epoch 124
    Norm: 9.86, NNZs: 1, Bias: 1.048541, T: 496, Avg. loss: 0.112734
    Total training time: 0.00 seconds.
    -- Epoch 125
    Norm: 9.86, NNZs: 1, Bias: 1.046813, T: 500, Avg. loss: 0.112628
    Total training time: 0.00 seconds.
    -- Epoch 126
    Norm: 9.85, NNZs: 1, Bias: 1.044839, T: 504, Avg. loss: 0.111500
    Total training time: 0.00 seconds.
    -- Epoch 127
    Norm: 9.85, NNZs: 1, Bias: 1.043077, T: 508, Avg. loss: 0.111809
    Total training time: 0.00 seconds.
    -- Epoch 128
    Norm: 9.86, NNZs: 1, Bias: 1.041575, T: 512, Avg. loss: 0.111214
    Total training time: 0.00 seconds.
    -- Epoch 129
    Norm: 9.86, NNZs: 1, Bias: 1.039871, T: 516, Avg. loss: 0.111108
    Total training time: 0.00 seconds.
    -- Epoch 130
    Norm: 9.86, NNZs: 1, Bias: 1.037928, T: 520, Avg. loss: 0.110010
    Total training time: 0.00 seconds.
    -- Epoch 131
    Norm: 9.86, NNZs: 1, Bias: 1.036191, T: 524, Avg. loss: 0.110307
    Total training time: 0.00 seconds.
    -- Epoch 132
    Norm: 9.86, NNZs: 1, Bias: 1.034708, T: 528, Avg. loss: 0.109727
    Total training time: 0.00 seconds.
    -- Epoch 133
    Norm: 9.86, NNZs: 1, Bias: 1.033030, T: 532, Avg. loss: 0.109622
    Total training time: 0.00 seconds.
    -- Epoch 134
    Norm: 9.86, NNZs: 1, Bias: 1.031116, T: 536, Avg. loss: 0.108552
    Total training time: 0.00 seconds.
    -- Epoch 135
    Norm: 9.86, NNZs: 1, Bias: 1.029403, T: 540, Avg. loss: 0.108837
    Total training time: 0.00 seconds.
    -- Epoch 136
    Norm: 9.86, NNZs: 1, Bias: 1.027939, T: 544, Avg. loss: 0.108272
    Total training time: 0.00 seconds.
    -- Epoch 137
    Norm: 9.86, NNZs: 1, Bias: 1.026284, T: 548, Avg. loss: 0.108167
    Total training time: 0.00 seconds.
    -- Epoch 138
    Norm: 9.86, NNZs: 1, Bias: 1.024399, T: 552, Avg. loss: 0.107125
    Total training time: 0.00 seconds.
    -- Epoch 139
    Norm: 9.86, NNZs: 1, Bias: 1.022710, T: 556, Avg. loss: 0.107398
    Total training time: 0.00 seconds.
    -- Epoch 140
    Norm: 9.86, NNZs: 1, Bias: 1.021265, T: 560, Avg. loss: 0.106847
    Total training time: 0.00 seconds.
    -- Epoch 141
    Norm: 9.86, NNZs: 1, Bias: 1.019632, T: 564, Avg. loss: 0.106743
    Total training time: 0.00 seconds.
    -- Epoch 142
    Norm: 9.86, NNZs: 1, Bias: 1.017774, T: 568, Avg. loss: 0.105727
    Total training time: 0.00 seconds.
    -- Epoch 143
    Norm: 9.86, NNZs: 1, Bias: 1.016108, T: 572, Avg. loss: 0.105990
    Total training time: 0.00 seconds.
    -- Epoch 144
    Norm: 9.86, NNZs: 1, Bias: 1.014681, T: 576, Avg. loss: 0.105452
    Total training time: 0.00 seconds.
    -- Epoch 145
    Norm: 9.86, NNZs: 1, Bias: 1.013071, T: 580, Avg. loss: 0.105348
    Total training time: 0.00 seconds.
    -- Epoch 146
    Norm: 9.86, NNZs: 1, Bias: 1.011239, T: 584, Avg. loss: 0.104357
    Total training time: 0.00 seconds.
    -- Epoch 147
    Norm: 9.86, NNZs: 1, Bias: 1.009595, T: 588, Avg. loss: 0.104610
    Total training time: 0.00 seconds.
    Convergence after 147 epochs took 0.00 seconds
    




<style>#sk-container-id-4 {color: black;background-color: white;}#sk-container-id-4 pre{padding: 0;}#sk-container-id-4 div.sk-toggleable {background-color: white;}#sk-container-id-4 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-4 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-4 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-4 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-4 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-4 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-4 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-4 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-4 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-4 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-4 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-4 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-4 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-4 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-4 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-4 div.sk-item {position: relative;z-index: 1;}#sk-container-id-4 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-4 div.sk-item::before, #sk-container-id-4 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-4 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-4 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-4 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-4 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-4 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-4 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-4 div.sk-label-container {text-align: center;}#sk-container-id-4 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-4 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-4" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>SGDRegressor(max_iter=5000, verbose=1)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-4" type="checkbox" checked><label for="sk-estimator-id-4" class="sk-toggleable__label sk-toggleable__label-arrow">SGDRegressor</label><div class="sk-toggleable__content"><pre>SGDRegressor(max_iter=5000, verbose=1)</pre></div></div></div></div></div>




```python
# 예측
sgd_model.predict([[7]])
```

    D:\anaconda3\lib\site-packages\sklearn\base.py:420: UserWarning: X does not have valid feature names, but SGDRegressor was fitted with feature names
      warnings.warn(
    




    array([70.02298853])




```python
print(sgd_model.coef_)
print(sgd_model.intercept_)
```

    [9.85905624]
    [1.00959482]
    
