---
layout: single
title:  "서울시 구별 CCTV 현황 분석하기"
categories: MachineLearning
toc: true
---

### 1. 실습목표
1. pandas, matplotlib 사용하기
2. 서울시 각 구별 CCTV 현황 살펴보기, 인구대기 CCTV 비율이 높은/낮은 지역 알아보기


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

### csv 파일 읽기 - 서울시 구별 CCTV 현황


```python
CCTV_Seoul = pd.read_csv('CCTV_in_Seoul.csv')
CCTV_Seoul.head(3)
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
      <th>기관명</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼 이름 바꾸기
CCTV_Seoul.rename(columns={'기관명':'구'}, inplace=True)

# inplace 또는 변수를 사용
```


```python
CCTV_Seoul
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
    </tr>
    <tr>
      <th>4</th>
      <td>관악구</td>
      <td>1496</td>
      <td>846</td>
      <td>260</td>
      <td>390</td>
      <td>613</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>707</td>
      <td>573</td>
      <td>78</td>
      <td>53</td>
      <td>174</td>
    </tr>
    <tr>
      <th>6</th>
      <td>구로구</td>
      <td>1561</td>
      <td>1142</td>
      <td>173</td>
      <td>246</td>
      <td>323</td>
    </tr>
    <tr>
      <th>7</th>
      <td>금천구</td>
      <td>1015</td>
      <td>674</td>
      <td>51</td>
      <td>269</td>
      <td>354</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>1265</td>
      <td>542</td>
      <td>57</td>
      <td>451</td>
      <td>516</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
    </tr>
    <tr>
      <th>10</th>
      <td>동대문구</td>
      <td>1294</td>
      <td>1070</td>
      <td>23</td>
      <td>198</td>
      <td>579</td>
    </tr>
    <tr>
      <th>11</th>
      <td>동작구</td>
      <td>1091</td>
      <td>544</td>
      <td>341</td>
      <td>103</td>
      <td>314</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>962</td>
      <td>844</td>
      <td>50</td>
      <td>68</td>
      <td>292</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
    </tr>
    <tr>
      <th>15</th>
      <td>성동구</td>
      <td>1062</td>
      <td>730</td>
      <td>91</td>
      <td>241</td>
      <td>265</td>
    </tr>
    <tr>
      <th>16</th>
      <td>성북구</td>
      <td>1464</td>
      <td>1009</td>
      <td>78</td>
      <td>360</td>
      <td>204</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>904</td>
      <td>495</td>
      <td>214</td>
      <td>195</td>
      <td>373</td>
    </tr>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
    </tr>
    <tr>
      <th>22</th>
      <td>종로구</td>
      <td>1002</td>
      <td>464</td>
      <td>314</td>
      <td>211</td>
      <td>630</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
    </tr>
  </tbody>
</table>
</div>



#####  엑셀파일 읽기 - 서울시 인구현황


```python
pop_Seoul = pd.read_excel("population_in_Seoul.xls")
pop_Seoul.head(2)
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
      <th>기간</th>
      <th>자치구</th>
      <th>세대</th>
      <th>인구</th>
      <th>인구.1</th>
      <th>인구.2</th>
      <th>인구.3</th>
      <th>인구.4</th>
      <th>인구.5</th>
      <th>인구.6</th>
      <th>인구.7</th>
      <th>인구.8</th>
      <th>세대당인구</th>
      <th>65세이상고령자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>기간</td>
      <td>자치구</td>
      <td>세대</td>
      <td>합계</td>
      <td>합계</td>
      <td>합계</td>
      <td>한국인</td>
      <td>한국인</td>
      <td>한국인</td>
      <td>등록외국인</td>
      <td>등록외국인</td>
      <td>등록외국인</td>
      <td>세대당인구</td>
      <td>65세이상고령자</td>
    </tr>
    <tr>
      <th>1</th>
      <td>기간</td>
      <td>자치구</td>
      <td>세대</td>
      <td>계</td>
      <td>남자</td>
      <td>여자</td>
      <td>계</td>
      <td>남자</td>
      <td>여자</td>
      <td>계</td>
      <td>남자</td>
      <td>여자</td>
      <td>세대당인구</td>
      <td>65세이상고령자</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 읽어오고 싶은 행, 열 선택하여 불러오기
# header : 읽고 싶은 row index (0부터 시작)
# usecols: : 읽고 싶은 columns 선택

pop_Seoul = pd.read_excel("population_in_Seoul.xls", header=2, usecols="B,D,G,J,N")
pop_Seoul.head(3)
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
      <th>자치구</th>
      <th>계</th>
      <th>계.1</th>
      <th>계.2</th>
      <th>65세이상고령자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>합계</td>
      <td>10197604.0</td>
      <td>9926968.0</td>
      <td>270636.0</td>
      <td>1321458.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>중구</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼명 수정
# 많은 컬럼을 변경할 때 유용한 방법

pop_Seoul.columns=["구","인구수","한국인수","외국인수","65세이상고령자수"]
```


```python
pop_Seoul.head()
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
      <th>구</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>합계</td>
      <td>10197604.0</td>
      <td>9926968.0</td>
      <td>270636.0</td>
      <td>1321458.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>중구</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>용산구</td>
      <td>244203.0</td>
      <td>229456.0</td>
      <td>14747.0</td>
      <td>36231.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>성동구</td>
      <td>311244.0</td>
      <td>303380.0</td>
      <td>7864.0</td>
      <td>39997.0</td>
    </tr>
  </tbody>
</table>
</div>



##### 결측치 확인


```python
# 간략한 정보 - 컬럼 수, 데이터 수, 컬럼별 데이터 타입 등
# 결측치 여부 확인 가능

CCTV_Seoul.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 25 entries, 0 to 24
    Data columns (total 6 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   구          25 non-null     object
     1   소계         25 non-null     int64 
     2   2013년도 이전  25 non-null     int64 
     3   2014년      25 non-null     int64 
     4   2015년      25 non-null     int64 
     5   2016년      25 non-null     int64 
    dtypes: int64(5), object(1)
    memory usage: 1.3+ KB



```python
# 결측치 확인 -> 직접 살펴보기

pop_Seoul.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 27 entries, 0 to 26
    Data columns (total 5 columns):
     #   Column     Non-Null Count  Dtype  
    ---  ------     --------------  -----  
     0   구          26 non-null     object 
     1   인구수        26 non-null     float64
     2   한국인수       26 non-null     float64
     3   외국인수       26 non-null     float64
     4   65세이상고령자수  26 non-null     float64
    dtypes: float64(4), object(1)
    memory usage: 1.2+ KB



```python
# 결측치 데이터 가져오기 -> 조건 필터링(불리언 인덱싱)
# 1. "구" 칼럼 가져오기
# 2. "구" 칼럼 데이터의 결측치 판단( .isnull() ) -> 값이 불리언으로 출력
# 3. 2를 통해 결측치 데이터만 가져오기 -> 불리언 인덱싱
```


```python
# 1. "구" 칼럼 가져오기

pop_Seoul.loc[:,"구"]
```




    0       합계
    1      종로구
    2       중구
    3      용산구
    4      성동구
    5      광진구
    6     동대문구
    7      중랑구
    8      성북구
    9      강북구
    10     도봉구
    11     노원구
    12     은평구
    13    서대문구
    14     마포구
    15     양천구
    16     강서구
    17     구로구
    18     금천구
    19    영등포구
    20     동작구
    21     관악구
    22     서초구
    23     강남구
    24     송파구
    25     강동구
    26     NaN
    Name: 구, dtype: object




```python
# 2."구" 칼럼 데이터의 결측치 판단(.isnull())
pop_Seoul.loc[:,"구"].isnull()
```




    0     False
    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15    False
    16    False
    17    False
    18    False
    19    False
    20    False
    21    False
    22    False
    23    False
    24    False
    25    False
    26     True
    Name: 구, dtype: bool




```python
pop_Seoul[pop_Seoul.loc[:,"구"].isnull()]
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
      <th>구</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 결측치 삭제

pop_Seoul.drop(26,inplace=True)

# 확실하게 드랍하기 위해서 inplace 사용
```


```python
pop_Seoul.tail(3)
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
      <th>구</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>강남구</td>
      <td>570500.0</td>
      <td>565550.0</td>
      <td>4950.0</td>
      <td>63167.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>송파구</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>강동구</td>
      <td>453233.0</td>
      <td>449019.0</td>
      <td>4214.0</td>
      <td>54622.0</td>
    </tr>
  </tbody>
</table>
</div>



##### CCTV 수가 많은/적은 지역을 파악해보자(5개씩)


```python
CCTV_Seoul.sort_values("소계")
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>707</td>
      <td>573</td>
      <td>78</td>
      <td>53</td>
      <td>174</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>904</td>
      <td>495</td>
      <td>214</td>
      <td>195</td>
      <td>373</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>962</td>
      <td>844</td>
      <td>50</td>
      <td>68</td>
      <td>292</td>
    </tr>
    <tr>
      <th>22</th>
      <td>종로구</td>
      <td>1002</td>
      <td>464</td>
      <td>314</td>
      <td>211</td>
      <td>630</td>
    </tr>
    <tr>
      <th>7</th>
      <td>금천구</td>
      <td>1015</td>
      <td>674</td>
      <td>51</td>
      <td>269</td>
      <td>354</td>
    </tr>
    <tr>
      <th>15</th>
      <td>성동구</td>
      <td>1062</td>
      <td>730</td>
      <td>91</td>
      <td>241</td>
      <td>265</td>
    </tr>
    <tr>
      <th>11</th>
      <td>동작구</td>
      <td>1091</td>
      <td>544</td>
      <td>341</td>
      <td>103</td>
      <td>314</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>1265</td>
      <td>542</td>
      <td>57</td>
      <td>451</td>
      <td>516</td>
    </tr>
    <tr>
      <th>10</th>
      <td>동대문구</td>
      <td>1294</td>
      <td>1070</td>
      <td>23</td>
      <td>198</td>
      <td>579</td>
    </tr>
    <tr>
      <th>16</th>
      <td>성북구</td>
      <td>1464</td>
      <td>1009</td>
      <td>78</td>
      <td>360</td>
      <td>204</td>
    </tr>
    <tr>
      <th>4</th>
      <td>관악구</td>
      <td>1496</td>
      <td>846</td>
      <td>260</td>
      <td>390</td>
      <td>613</td>
    </tr>
    <tr>
      <th>6</th>
      <td>구로구</td>
      <td>1561</td>
      <td>1142</td>
      <td>173</td>
      <td>246</td>
      <td>323</td>
    </tr>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
    </tr>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
    </tr>
  </tbody>
</table>
</div>




```python
CCTV_Seoul.sort_values("소계").tail(5)
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
    </tr>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
    </tr>
  </tbody>
</table>
</div>




```python
CCTV_Seoul.sort_values("소계",ascending=False).head(5)
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
    </tr>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
    </tr>
  </tbody>
</table>
</div>




```python
CCTV_Seoul.sort_values("소계").head(5)
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
    </tr>
  </tbody>
</table>
</div>




```python
CCTV_Seoul.sort_values("소계",ascending=False).tail(5)
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
    </tr>
  </tbody>
</table>
</div>



##### 데이터 병합


```python
pop_Seoul
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
      <th>구</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>합계</td>
      <td>10197604.0</td>
      <td>9926968.0</td>
      <td>270636.0</td>
      <td>1321458.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>중구</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>용산구</td>
      <td>244203.0</td>
      <td>229456.0</td>
      <td>14747.0</td>
      <td>36231.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>성동구</td>
      <td>311244.0</td>
      <td>303380.0</td>
      <td>7864.0</td>
      <td>39997.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>372164.0</td>
      <td>357211.0</td>
      <td>14953.0</td>
      <td>42214.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>동대문구</td>
      <td>369496.0</td>
      <td>354079.0</td>
      <td>15417.0</td>
      <td>54173.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>중랑구</td>
      <td>414503.0</td>
      <td>409882.0</td>
      <td>4621.0</td>
      <td>56774.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>성북구</td>
      <td>461260.0</td>
      <td>449773.0</td>
      <td>11487.0</td>
      <td>64692.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>강북구</td>
      <td>330192.0</td>
      <td>326686.0</td>
      <td>3506.0</td>
      <td>54813.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>도봉구</td>
      <td>348646.0</td>
      <td>346629.0</td>
      <td>2017.0</td>
      <td>51312.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>노원구</td>
      <td>569384.0</td>
      <td>565565.0</td>
      <td>3819.0</td>
      <td>71941.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>은평구</td>
      <td>494388.0</td>
      <td>489943.0</td>
      <td>4445.0</td>
      <td>72334.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>327163.0</td>
      <td>314982.0</td>
      <td>12181.0</td>
      <td>48161.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>마포구</td>
      <td>389649.0</td>
      <td>378566.0</td>
      <td>11083.0</td>
      <td>48765.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>양천구</td>
      <td>479978.0</td>
      <td>475949.0</td>
      <td>4029.0</td>
      <td>52975.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>강서구</td>
      <td>603772.0</td>
      <td>597248.0</td>
      <td>6524.0</td>
      <td>72548.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>구로구</td>
      <td>447874.0</td>
      <td>416487.0</td>
      <td>31387.0</td>
      <td>56833.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>금천구</td>
      <td>255082.0</td>
      <td>236353.0</td>
      <td>18729.0</td>
      <td>32970.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>402985.0</td>
      <td>368072.0</td>
      <td>34913.0</td>
      <td>52413.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>동작구</td>
      <td>412520.0</td>
      <td>400456.0</td>
      <td>12064.0</td>
      <td>56013.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>관악구</td>
      <td>525515.0</td>
      <td>507203.0</td>
      <td>18312.0</td>
      <td>68082.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>서초구</td>
      <td>450310.0</td>
      <td>445994.0</td>
      <td>4316.0</td>
      <td>51733.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>강남구</td>
      <td>570500.0</td>
      <td>565550.0</td>
      <td>4950.0</td>
      <td>63167.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>송파구</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>강동구</td>
      <td>453233.0</td>
      <td>449019.0</td>
      <td>4214.0</td>
      <td>54622.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
CCTV_구=set(CCTV_Seoul["구"].unique())
```


```python
pop_구=set(pop_Seoul["구"].unique())
```


```python
CCTV_구-pop_구
```




    set()




```python
pop_구-CCTV_구
```




    {'합계'}




```python
data_result=pd.merge(CCTV_Seoul, pop_Seoul,on="구")
```


```python
data_result
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
      <td>570500.0</td>
      <td>565550.0</td>
      <td>4950.0</td>
      <td>63167.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
      <td>453233.0</td>
      <td>449019.0</td>
      <td>4214.0</td>
      <td>54622.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
      <td>330192.0</td>
      <td>326686.0</td>
      <td>3506.0</td>
      <td>54813.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
      <td>603772.0</td>
      <td>597248.0</td>
      <td>6524.0</td>
      <td>72548.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>관악구</td>
      <td>1496</td>
      <td>846</td>
      <td>260</td>
      <td>390</td>
      <td>613</td>
      <td>525515.0</td>
      <td>507203.0</td>
      <td>18312.0</td>
      <td>68082.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>707</td>
      <td>573</td>
      <td>78</td>
      <td>53</td>
      <td>174</td>
      <td>372164.0</td>
      <td>357211.0</td>
      <td>14953.0</td>
      <td>42214.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>구로구</td>
      <td>1561</td>
      <td>1142</td>
      <td>173</td>
      <td>246</td>
      <td>323</td>
      <td>447874.0</td>
      <td>416487.0</td>
      <td>31387.0</td>
      <td>56833.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>금천구</td>
      <td>1015</td>
      <td>674</td>
      <td>51</td>
      <td>269</td>
      <td>354</td>
      <td>255082.0</td>
      <td>236353.0</td>
      <td>18729.0</td>
      <td>32970.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>1265</td>
      <td>542</td>
      <td>57</td>
      <td>451</td>
      <td>516</td>
      <td>569384.0</td>
      <td>565565.0</td>
      <td>3819.0</td>
      <td>71941.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
      <td>348646.0</td>
      <td>346629.0</td>
      <td>2017.0</td>
      <td>51312.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>동대문구</td>
      <td>1294</td>
      <td>1070</td>
      <td>23</td>
      <td>198</td>
      <td>579</td>
      <td>369496.0</td>
      <td>354079.0</td>
      <td>15417.0</td>
      <td>54173.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>동작구</td>
      <td>1091</td>
      <td>544</td>
      <td>341</td>
      <td>103</td>
      <td>314</td>
      <td>412520.0</td>
      <td>400456.0</td>
      <td>12064.0</td>
      <td>56013.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
      <td>389649.0</td>
      <td>378566.0</td>
      <td>11083.0</td>
      <td>48765.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>962</td>
      <td>844</td>
      <td>50</td>
      <td>68</td>
      <td>292</td>
      <td>327163.0</td>
      <td>314982.0</td>
      <td>12181.0</td>
      <td>48161.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
      <td>450310.0</td>
      <td>445994.0</td>
      <td>4316.0</td>
      <td>51733.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>성동구</td>
      <td>1062</td>
      <td>730</td>
      <td>91</td>
      <td>241</td>
      <td>265</td>
      <td>311244.0</td>
      <td>303380.0</td>
      <td>7864.0</td>
      <td>39997.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>성북구</td>
      <td>1464</td>
      <td>1009</td>
      <td>78</td>
      <td>360</td>
      <td>204</td>
      <td>461260.0</td>
      <td>449773.0</td>
      <td>11487.0</td>
      <td>64692.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
      <td>479978.0</td>
      <td>475949.0</td>
      <td>4029.0</td>
      <td>52975.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>904</td>
      <td>495</td>
      <td>214</td>
      <td>195</td>
      <td>373</td>
      <td>402985.0</td>
      <td>368072.0</td>
      <td>34913.0</td>
      <td>52413.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
      <td>244203.0</td>
      <td>229456.0</td>
      <td>14747.0</td>
      <td>36231.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
      <td>494388.0</td>
      <td>489943.0</td>
      <td>4445.0</td>
      <td>72334.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>종로구</td>
      <td>1002</td>
      <td>464</td>
      <td>314</td>
      <td>211</td>
      <td>630</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
      <td>414503.0</td>
      <td>409882.0</td>
      <td>4621.0</td>
      <td>56774.0</td>
    </tr>
  </tbody>
</table>
</div>



##### 인구수대비 CCTV 비율이 높은/낮은 지역 파악해보기!!
- 특성공학 : 컬럼끼리 연산을 통해 의미있는 컬럼을 만드는 작업


```python
data_result["소계"]/data_result["인구수"]*100
```




    0     0.487292
    1     0.170552
    2     0.226535
    3     0.146413
    4     0.284673
    5     0.189970
    6     0.348536
    7     0.397911
    8     0.222170
    9     0.139110
    10    0.350207
    11    0.264472
    12    0.147312
    13    0.294043
    14    0.428594
    15    0.341211
    16    0.317391
    17    0.092587
    18    0.423769
    19    0.224326
    20    0.665020
    21    0.378852
    22    0.615404
    23    0.503603
    24    0.159227
    dtype: float64




```python
# 새 컬럼 생성 - 인구대비CCTV비율

data_result["인구대비CCTV비율"] = (data_result["소계"]/data_result["인구수"])*100
```


```python
data_result
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
      <th>인구대비CCTV비율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
      <td>570500.0</td>
      <td>565550.0</td>
      <td>4950.0</td>
      <td>63167.0</td>
      <td>0.487292</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
      <td>453233.0</td>
      <td>449019.0</td>
      <td>4214.0</td>
      <td>54622.0</td>
      <td>0.170552</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
      <td>330192.0</td>
      <td>326686.0</td>
      <td>3506.0</td>
      <td>54813.0</td>
      <td>0.226535</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
      <td>603772.0</td>
      <td>597248.0</td>
      <td>6524.0</td>
      <td>72548.0</td>
      <td>0.146413</td>
    </tr>
    <tr>
      <th>4</th>
      <td>관악구</td>
      <td>1496</td>
      <td>846</td>
      <td>260</td>
      <td>390</td>
      <td>613</td>
      <td>525515.0</td>
      <td>507203.0</td>
      <td>18312.0</td>
      <td>68082.0</td>
      <td>0.284673</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>707</td>
      <td>573</td>
      <td>78</td>
      <td>53</td>
      <td>174</td>
      <td>372164.0</td>
      <td>357211.0</td>
      <td>14953.0</td>
      <td>42214.0</td>
      <td>0.189970</td>
    </tr>
    <tr>
      <th>6</th>
      <td>구로구</td>
      <td>1561</td>
      <td>1142</td>
      <td>173</td>
      <td>246</td>
      <td>323</td>
      <td>447874.0</td>
      <td>416487.0</td>
      <td>31387.0</td>
      <td>56833.0</td>
      <td>0.348536</td>
    </tr>
    <tr>
      <th>7</th>
      <td>금천구</td>
      <td>1015</td>
      <td>674</td>
      <td>51</td>
      <td>269</td>
      <td>354</td>
      <td>255082.0</td>
      <td>236353.0</td>
      <td>18729.0</td>
      <td>32970.0</td>
      <td>0.397911</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>1265</td>
      <td>542</td>
      <td>57</td>
      <td>451</td>
      <td>516</td>
      <td>569384.0</td>
      <td>565565.0</td>
      <td>3819.0</td>
      <td>71941.0</td>
      <td>0.222170</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
      <td>348646.0</td>
      <td>346629.0</td>
      <td>2017.0</td>
      <td>51312.0</td>
      <td>0.139110</td>
    </tr>
    <tr>
      <th>10</th>
      <td>동대문구</td>
      <td>1294</td>
      <td>1070</td>
      <td>23</td>
      <td>198</td>
      <td>579</td>
      <td>369496.0</td>
      <td>354079.0</td>
      <td>15417.0</td>
      <td>54173.0</td>
      <td>0.350207</td>
    </tr>
    <tr>
      <th>11</th>
      <td>동작구</td>
      <td>1091</td>
      <td>544</td>
      <td>341</td>
      <td>103</td>
      <td>314</td>
      <td>412520.0</td>
      <td>400456.0</td>
      <td>12064.0</td>
      <td>56013.0</td>
      <td>0.264472</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
      <td>389649.0</td>
      <td>378566.0</td>
      <td>11083.0</td>
      <td>48765.0</td>
      <td>0.147312</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>962</td>
      <td>844</td>
      <td>50</td>
      <td>68</td>
      <td>292</td>
      <td>327163.0</td>
      <td>314982.0</td>
      <td>12181.0</td>
      <td>48161.0</td>
      <td>0.294043</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
      <td>450310.0</td>
      <td>445994.0</td>
      <td>4316.0</td>
      <td>51733.0</td>
      <td>0.428594</td>
    </tr>
    <tr>
      <th>15</th>
      <td>성동구</td>
      <td>1062</td>
      <td>730</td>
      <td>91</td>
      <td>241</td>
      <td>265</td>
      <td>311244.0</td>
      <td>303380.0</td>
      <td>7864.0</td>
      <td>39997.0</td>
      <td>0.341211</td>
    </tr>
    <tr>
      <th>16</th>
      <td>성북구</td>
      <td>1464</td>
      <td>1009</td>
      <td>78</td>
      <td>360</td>
      <td>204</td>
      <td>461260.0</td>
      <td>449773.0</td>
      <td>11487.0</td>
      <td>64692.0</td>
      <td>0.317391</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
      <td>0.092587</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
      <td>479978.0</td>
      <td>475949.0</td>
      <td>4029.0</td>
      <td>52975.0</td>
      <td>0.423769</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>904</td>
      <td>495</td>
      <td>214</td>
      <td>195</td>
      <td>373</td>
      <td>402985.0</td>
      <td>368072.0</td>
      <td>34913.0</td>
      <td>52413.0</td>
      <td>0.224326</td>
    </tr>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
      <td>244203.0</td>
      <td>229456.0</td>
      <td>14747.0</td>
      <td>36231.0</td>
      <td>0.665020</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
      <td>494388.0</td>
      <td>489943.0</td>
      <td>4445.0</td>
      <td>72334.0</td>
      <td>0.378852</td>
    </tr>
    <tr>
      <th>22</th>
      <td>종로구</td>
      <td>1002</td>
      <td>464</td>
      <td>314</td>
      <td>211</td>
      <td>630</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
      <td>0.615404</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
      <td>0.503603</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
      <td>414503.0</td>
      <td>409882.0</td>
      <td>4621.0</td>
      <td>56774.0</td>
      <td>0.159227</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_result.sort_values("인구대비CCTV비율").head(5)
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
      <th>인구대비CCTV비율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
      <td>0.092587</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
      <td>348646.0</td>
      <td>346629.0</td>
      <td>2017.0</td>
      <td>51312.0</td>
      <td>0.139110</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
      <td>603772.0</td>
      <td>597248.0</td>
      <td>6524.0</td>
      <td>72548.0</td>
      <td>0.146413</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
      <td>389649.0</td>
      <td>378566.0</td>
      <td>11083.0</td>
      <td>48765.0</td>
      <td>0.147312</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
      <td>414503.0</td>
      <td>409882.0</td>
      <td>4621.0</td>
      <td>56774.0</td>
      <td>0.159227</td>
    </tr>
  </tbody>
</table>
</div>




```python
per_sort=data_result.sort_values("인구대비CCTV비율",ascending=False)
per_sort
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
      <th>구</th>
      <th>소계</th>
      <th>2013년도 이전</th>
      <th>2014년</th>
      <th>2015년</th>
      <th>2016년</th>
      <th>인구수</th>
      <th>한국인수</th>
      <th>외국인수</th>
      <th>65세이상고령자수</th>
      <th>인구대비CCTV비율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>용산구</td>
      <td>1624</td>
      <td>1368</td>
      <td>218</td>
      <td>112</td>
      <td>398</td>
      <td>244203.0</td>
      <td>229456.0</td>
      <td>14747.0</td>
      <td>36231.0</td>
      <td>0.665020</td>
    </tr>
    <tr>
      <th>22</th>
      <td>종로구</td>
      <td>1002</td>
      <td>464</td>
      <td>314</td>
      <td>211</td>
      <td>630</td>
      <td>162820.0</td>
      <td>153589.0</td>
      <td>9231.0</td>
      <td>25425.0</td>
      <td>0.615404</td>
    </tr>
    <tr>
      <th>23</th>
      <td>중구</td>
      <td>671</td>
      <td>413</td>
      <td>190</td>
      <td>72</td>
      <td>348</td>
      <td>133240.0</td>
      <td>124312.0</td>
      <td>8928.0</td>
      <td>20764.0</td>
      <td>0.503603</td>
    </tr>
    <tr>
      <th>0</th>
      <td>강남구</td>
      <td>2780</td>
      <td>1292</td>
      <td>430</td>
      <td>584</td>
      <td>932</td>
      <td>570500.0</td>
      <td>565550.0</td>
      <td>4950.0</td>
      <td>63167.0</td>
      <td>0.487292</td>
    </tr>
    <tr>
      <th>14</th>
      <td>서초구</td>
      <td>1930</td>
      <td>1406</td>
      <td>157</td>
      <td>336</td>
      <td>398</td>
      <td>450310.0</td>
      <td>445994.0</td>
      <td>4316.0</td>
      <td>51733.0</td>
      <td>0.428594</td>
    </tr>
    <tr>
      <th>18</th>
      <td>양천구</td>
      <td>2034</td>
      <td>1843</td>
      <td>142</td>
      <td>30</td>
      <td>467</td>
      <td>479978.0</td>
      <td>475949.0</td>
      <td>4029.0</td>
      <td>52975.0</td>
      <td>0.423769</td>
    </tr>
    <tr>
      <th>7</th>
      <td>금천구</td>
      <td>1015</td>
      <td>674</td>
      <td>51</td>
      <td>269</td>
      <td>354</td>
      <td>255082.0</td>
      <td>236353.0</td>
      <td>18729.0</td>
      <td>32970.0</td>
      <td>0.397911</td>
    </tr>
    <tr>
      <th>21</th>
      <td>은평구</td>
      <td>1873</td>
      <td>1138</td>
      <td>224</td>
      <td>278</td>
      <td>468</td>
      <td>494388.0</td>
      <td>489943.0</td>
      <td>4445.0</td>
      <td>72334.0</td>
      <td>0.378852</td>
    </tr>
    <tr>
      <th>10</th>
      <td>동대문구</td>
      <td>1294</td>
      <td>1070</td>
      <td>23</td>
      <td>198</td>
      <td>579</td>
      <td>369496.0</td>
      <td>354079.0</td>
      <td>15417.0</td>
      <td>54173.0</td>
      <td>0.350207</td>
    </tr>
    <tr>
      <th>6</th>
      <td>구로구</td>
      <td>1561</td>
      <td>1142</td>
      <td>173</td>
      <td>246</td>
      <td>323</td>
      <td>447874.0</td>
      <td>416487.0</td>
      <td>31387.0</td>
      <td>56833.0</td>
      <td>0.348536</td>
    </tr>
    <tr>
      <th>15</th>
      <td>성동구</td>
      <td>1062</td>
      <td>730</td>
      <td>91</td>
      <td>241</td>
      <td>265</td>
      <td>311244.0</td>
      <td>303380.0</td>
      <td>7864.0</td>
      <td>39997.0</td>
      <td>0.341211</td>
    </tr>
    <tr>
      <th>16</th>
      <td>성북구</td>
      <td>1464</td>
      <td>1009</td>
      <td>78</td>
      <td>360</td>
      <td>204</td>
      <td>461260.0</td>
      <td>449773.0</td>
      <td>11487.0</td>
      <td>64692.0</td>
      <td>0.317391</td>
    </tr>
    <tr>
      <th>13</th>
      <td>서대문구</td>
      <td>962</td>
      <td>844</td>
      <td>50</td>
      <td>68</td>
      <td>292</td>
      <td>327163.0</td>
      <td>314982.0</td>
      <td>12181.0</td>
      <td>48161.0</td>
      <td>0.294043</td>
    </tr>
    <tr>
      <th>4</th>
      <td>관악구</td>
      <td>1496</td>
      <td>846</td>
      <td>260</td>
      <td>390</td>
      <td>613</td>
      <td>525515.0</td>
      <td>507203.0</td>
      <td>18312.0</td>
      <td>68082.0</td>
      <td>0.284673</td>
    </tr>
    <tr>
      <th>11</th>
      <td>동작구</td>
      <td>1091</td>
      <td>544</td>
      <td>341</td>
      <td>103</td>
      <td>314</td>
      <td>412520.0</td>
      <td>400456.0</td>
      <td>12064.0</td>
      <td>56013.0</td>
      <td>0.264472</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강북구</td>
      <td>748</td>
      <td>369</td>
      <td>120</td>
      <td>138</td>
      <td>204</td>
      <td>330192.0</td>
      <td>326686.0</td>
      <td>3506.0</td>
      <td>54813.0</td>
      <td>0.226535</td>
    </tr>
    <tr>
      <th>19</th>
      <td>영등포구</td>
      <td>904</td>
      <td>495</td>
      <td>214</td>
      <td>195</td>
      <td>373</td>
      <td>402985.0</td>
      <td>368072.0</td>
      <td>34913.0</td>
      <td>52413.0</td>
      <td>0.224326</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>1265</td>
      <td>542</td>
      <td>57</td>
      <td>451</td>
      <td>516</td>
      <td>569384.0</td>
      <td>565565.0</td>
      <td>3819.0</td>
      <td>71941.0</td>
      <td>0.222170</td>
    </tr>
    <tr>
      <th>5</th>
      <td>광진구</td>
      <td>707</td>
      <td>573</td>
      <td>78</td>
      <td>53</td>
      <td>174</td>
      <td>372164.0</td>
      <td>357211.0</td>
      <td>14953.0</td>
      <td>42214.0</td>
      <td>0.189970</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강동구</td>
      <td>773</td>
      <td>379</td>
      <td>99</td>
      <td>155</td>
      <td>377</td>
      <td>453233.0</td>
      <td>449019.0</td>
      <td>4214.0</td>
      <td>54622.0</td>
      <td>0.170552</td>
    </tr>
    <tr>
      <th>24</th>
      <td>중랑구</td>
      <td>660</td>
      <td>509</td>
      <td>121</td>
      <td>177</td>
      <td>109</td>
      <td>414503.0</td>
      <td>409882.0</td>
      <td>4621.0</td>
      <td>56774.0</td>
      <td>0.159227</td>
    </tr>
    <tr>
      <th>12</th>
      <td>마포구</td>
      <td>574</td>
      <td>314</td>
      <td>118</td>
      <td>169</td>
      <td>379</td>
      <td>389649.0</td>
      <td>378566.0</td>
      <td>11083.0</td>
      <td>48765.0</td>
      <td>0.147312</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강서구</td>
      <td>884</td>
      <td>388</td>
      <td>258</td>
      <td>184</td>
      <td>81</td>
      <td>603772.0</td>
      <td>597248.0</td>
      <td>6524.0</td>
      <td>72548.0</td>
      <td>0.146413</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>485</td>
      <td>238</td>
      <td>159</td>
      <td>42</td>
      <td>386</td>
      <td>348646.0</td>
      <td>346629.0</td>
      <td>2017.0</td>
      <td>51312.0</td>
      <td>0.139110</td>
    </tr>
    <tr>
      <th>17</th>
      <td>송파구</td>
      <td>618</td>
      <td>529</td>
      <td>21</td>
      <td>68</td>
      <td>463</td>
      <td>667483.0</td>
      <td>660584.0</td>
      <td>6899.0</td>
      <td>72506.0</td>
      <td>0.092587</td>
    </tr>
  </tbody>
</table>
</div>



#### #그래프 그리기!!


```python
from matplotlib import rc
rc("font",family="Malgun Gothic")
```


```python
plt.figure(figsize=(10,5)) #figsize(가로,세로)
plt.barh(per_sort["구"],per_sort["인구대비CCTV비율"])
plt.grid(True)
plt.xlabel("인구대기 CCTV 비율")
plt.ylabel("구별")
```




    Text(0, 0.5, '구별')




​    
![png](output_42_1.png)
​    

