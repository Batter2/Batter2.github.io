---
layout: single
title:  "Python_Pandas"
categories: Python
toc: true
---

```python
# Pandas 라이브러리 불러오기

import pandas as pd
```

### Series 생성
- index와 value로 구성된 1차원 자료형


```python
population = pd.Series([9904312, 3448737, 2890451, 2466052])
```


```python
population
```




    0    9904312
    1    3448737
    2    2890451
    3    2466052
    dtype: int64




```python
# 인덱스를 지정하여 생성
# 시리즈를 만드는 과정에 넣어주어야함

population = pd.Series([9904312, 3448737, 2890451, 2466052], 
                       index=['서울','부산','인천','대구'])
population
```




    서울    9904312
    부산    3448737
    인천    2890451
    대구    2466052
    dtype: int64



### Series 데이터 확인
- index : 인덱스 확인
- values : 값 확인
- dtype : 데이터 타입 확인


```python
# population의 인덱스 확인

population.index
```




    Index(['서울', '부산', '인천', '대구'], dtype='object')




```python
# population의 값 확인

population.values
```




    array([9904312, 3448737, 2890451, 2466052], dtype=int64)




```python
# population의 데이터 타입 확인

population.dtype
```




    dtype('int64')



### Series에 이름 지정
- name
- index.name


```python
# Series에 이름 달기

population.name = '인구'
```


```python
population
```




    서울    9904312
    부산    3448737
    인천    2890451
    대구    2466052
    Name: 인구, dtype: int64




```python
# Series에 있는 인덱스에 이름 달기

population.index.name = "도시"
```


```python
population
```




    도시
    서울    9904312
    부산    3448737
    인천    2890451
    대구    2466052
    Name: 인구, dtype: int64



### Series 연산


```python
population/1000000
```




    도시
    서울    9.904312
    부산    3.448737
    인천    2.890451
    대구    2.466052
    Name: 인구, dtype: float64



### Series 인덱싱, 슬라이싱


```python
# 인덱싱 : 값을 가르치는 것
# Series에서는 index에 값을 지정하면
# index번호와 지정한 index 둘 다 인덱싱 조건에 활용이 가능하다.

print("인구수 : " , population[1])
print("인구수 : " , population["대구"])
```

    인구수 :  3448737
    인구수 :  2466052
    


```python
# 한번에 여러 값 인덱싱 하기
# 원하는 순서대로 값을 가져올 수 있음.

population[[0,2,1,2]]
```




    도시
    서울    9904312
    인천    2890451
    부산    3448737
    인천    2890451
    Name: 인구, dtype: int64




```python
population[["서울","인천","부산"]]
```




    도시
    서울    9904312
    인천    2890451
    부산    3448737
    Name: 인구, dtype: int64




```python
# Boolean 인덱싱
# 조건을 걸어서, 해당하는 조건의 데이터만 가져오는 것

population > 3000000
```




    도시
    서울     True
    부산     True
    인천    False
    대구    False
    Name: 인구, dtype: bool




```python
population[population > 3000000]
```




    도시
    서울    9904312
    부산    3448737
    Name: 인구, dtype: int64




```python
# 두 가지 이상의 조건으로 Boolean 인덱싱 하는 과정
# 인구수가 2500000이상이고, 5000000만 이하인 데이터 가져오기
# not = ~

population[(population >= 2500000)&(population<=5000000)]
```




    도시
    부산    3448737
    인천    2890451
    Name: 인구, dtype: int64




```python
# 슬라이싱

print(population[1:3])
print(population["부산":"인천"])
```

    도시
    부산    3448737
    인천    2890451
    Name: 인구, dtype: int64
    도시
    부산    3448737
    인천    2890451
    Name: 인구, dtype: int64
    

### 딕셔너리 객체로 Series 생성
- 딕셔너리는 key와 value로 구성
- 딕셔너리의 key값은 index
- 딕셔너리의 value값은 value


```python
data={"서울":9904312, "부산":3448737, "인천":2890451,"대전":1490158}
data
```




    {'서울': 9904312, '부산': 3448737, '인천': 2890451, '대전': 1490158}




```python
population2=pd.Series(data)
population2
```




    서울    9904312
    부산    3448737
    인천    2890451
    대전    1490158
    dtype: int64




```python
# 2010년 도시별 인구

print(population2.index)
print(population2.values)
print(population2.dtype)
```

    Index(['서울', '부산', '인천', '대전'], dtype='object')
    [9904312 3448737 2890451 1490158]
    int64
    


```python
# 2015년 도시별 인구

print(population.index)
print(population.values)
```

    Index(['서울', '부산', '인천', '대구'], dtype='object', name='도시')
    [9904312 3448737 2890451 2466052]
    


```python
# 결측치 생성(NaN)

ds = population-population2
```


```python
ds
```




    대구    NaN
    대전    NaN
    부산    0.0
    서울    0.0
    인천    0.0
    dtype: float64




```python
# 결측치의 여부를 판단하는 함수
# 결측치를 판단하여 True/False 값으로 변환

print(ds.isnull())
print("\n")
print(ds[ds.isnull()])
```

    대구     True
    대전     True
    부산    False
    서울    False
    인천    False
    dtype: bool
    
    
    대구   NaN
    대전   NaN
    dtype: float64
    


```python
# 결측치 아닌 값들만 출력

print(ds.notnull())
print("\n")
print(ds[ds.notnull()])
```

    대구    False
    대전    False
    부산     True
    서울     True
    인천     True
    dtype: bool
    
    
    부산    0.0
    서울    0.0
    인천    0.0
    dtype: float64
    

### Series 데이터 갱신, 추가, 삭제


```python
# 인덱싱을 이용해서 데이터를 갱신, 추가, 삭제
# 시리즈[인덱스]=값

# 데이터 추가 및 수정
ds["부산"] = 1.6
ds["서울"] = 2.5
ds["광주"] = 1.0

# 데이터 삭제
del ds["대전"]
```


```python
print(ds)
```

    대구    NaN
    부산    1.6
    서울    2.5
    인천    0.0
    광주    1.0
    dtype: float64
    

### DataFrame

#### DataFrame 생성
- pandas Series : 1차원
- pandas DataFrame : 2차원


```python
# 딕셔너리 객체로 DataFrame 생성

data = {"2015":[9904312,3448737,2890451,2466052],
        "2010":[9631482,3393191,2632035,2431774]}
data
```




    {'2015': [9904312, 3448737, 2890451, 2466052],
     '2010': [9631482, 3393191, 2632035, 2431774]}




```python
# 딕셔너리의 key 값 : 컬럼 이름

df = pd.DataFrame(data)
```


```python
# DataFrame의 인덱스 수정

df.index=["서울","부산","인천","대구"]
```


```python
df
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2466052</td>
      <td>2431774</td>
    </tr>
  </tbody>
</table>
</div>




```python
# list데이터를 사용해서 DataFrame생성

data =[[9904312,3448737,2890451,2466052],[9631482,3393191,2632035,2431774]]
data
```




    [[9904312, 3448737, 2890451, 2466052], [9631482, 3393191, 2632035, 2431774]]




```python
df2=pd.DataFrame(data,index=["2015","2010"],columns=["서울","부산","인천","대구"])

df2
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
      <th>서울</th>
      <th>부산</th>
      <th>인천</th>
      <th>대구</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015</th>
      <td>9904312</td>
      <td>3448737</td>
      <td>2890451</td>
      <td>2466052</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>9631482</td>
      <td>3393191</td>
      <td>2632035</td>
      <td>2431774</td>
    </tr>
  </tbody>
</table>
</div>




```python
test=[[9904312,9631482],[3448737,3393191],[2890451,2632035],[2466052,2431774]]
```


```python
df3=pd.DataFrame(test,index=["서울","부산","인천","대구"],columns=["2015","2010"])
df3
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2466052</td>
      <td>2431774</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Transpose 전치(=행과 열을 바꿔주는 함수)
# 명렁어를 주었을 때만 변하기 때문에 다음에는 원래의 것 사용가능

df2.T

# 명령어를 주어서 변화시킨 후 변화시킨 것을 계속 사용하고 싶을 때
# df2=df2.T
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2466052</td>
      <td>2431774</td>
    </tr>
  </tbody>
</table>
</div>



### DataFrame 값 확인
- values : 값 확인
- index : 인덱스 확인
- columns : 컬럼 확인


```python
df.index
```




    Index(['서울', '부산', '인천', '대구'], dtype='object')




```python
df.columns
```




    Index(['2015', '2010'], dtype='object')




```python
df.values
```




    array([[9904312, 9631482],
           [3448737, 3393191],
           [2890451, 2632035],
           [2466052, 2431774]], dtype=int64)



### DataFrame 인덱싱, 슬라이싱
- index와 columns을 사용해서 인덱싱, 슬라이싱
- index 번호 사용가능(원래 있던 순서!!)
- index 값 사용 가능(보통 내가 붙이는 문자!!)


```python
# 컬럼 인덱싱
# 결과를 Series로 출력(하나의 key값(=하나의 컬럼)만 출력)

df[["2015"]]
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
      <th>2015</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2466052</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 슬라이싱 : 행의 데이터가 출력

df[0:2]
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼 인덱싱
# 복수의 key값 리스트 활용!
# 결과 값이 DetaFrame

df[["2010","2015"]]
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
      <th>2010</th>
      <th>2015</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9631482</td>
      <td>9904312</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3393191</td>
      <td>3448737</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2632035</td>
      <td>2890451</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2431774</td>
      <td>2466052</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 슬라이싱(값(=문자)활용)

df["서울":"부산"]
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 행과 열을 한번에 인덱싱/슬라이싱을 실행해주는 함수가 필요!!
# loc : index 값(=문자)을 이용하는 경우
# iloc : index 번호(=숫자)를 이용하는 경우
# 형식 : df.loc[행,열]
# 형식 : df.iloc[행,열]
# 형식 : df.인덱서[행]

df.loc["부산":"인천",:]
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 새로운 컬럼 생성
# df[컬럼이름]=값

df["2005"]=[9762546, 3512547,2517680,2456016]
```


```python
df
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
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
      <td>9762546</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
      <td>3512547</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
      <td>2517680</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>2466052</td>
      <td>2431774</td>
      <td>2456016</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[:,"2015"]
```




    서울    9904312
    부산    3448737
    인천    2890451
    대구    2466052
    Name: 2015, dtype: int64




```python
# df.인덱서[행]

df.loc["부산"]
```




    2015    3448737
    2010    3393191
    2005    3512547
    Name: 부산, dtype: int64




```python
df.iloc[1:3,:2]
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
      <th>2015</th>
      <th>2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>2890451</td>
      <td>2632035</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[:,0]
```




    서울    9904312
    부산    3448737
    인천    2890451
    대구    2466052
    Name: 2015, dtype: int64




```python
# boolean 인덱싱
# 2010년에 인구수가 3000000이 넘는 도시
# 1. 2010년 데이터 가져오기(비교해야 할 값!)

df["2010"]
```




    서울    9631482
    부산    3393191
    인천    2632035
    대구    2431774
    Name: 2010, dtype: int64




```python
# 2. 3000000이 넘는지 비교

df["2010"] > 3000000
```




    서울     True
    부산     True
    인천    False
    대구    False
    Name: 2010, dtype: bool




```python
df[df["2010"] > 3000000]
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
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>9904312</td>
      <td>9631482</td>
      <td>9762546</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>3448737</td>
      <td>3393191</td>
      <td>3512547</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["2010"][df["2010"]>3000000]
```




    서울    9631482
    부산    3393191
    Name: 2010, dtype: int64




```python
# 2005년에 인구수가 2500000이 넘는 도시

df["2005"][df["2005"]>2500000]
```




    서울    9762546
    부산    3512547
    인천    2517680
    Name: 2005, dtype: int64



#### population 실습


```python
# 외부 데이터 불러오기
# "파일명.확장자"

population_number=pd.read_csv('population_number.csv',encoding='euc-kr', 
                              index_col="도시")
population_number
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
      <th>지역</th>
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
      <th>2000</th>
    </tr>
    <tr>
      <th>도시</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>서울</th>
      <td>수도권</td>
      <td>9904312</td>
      <td>9631482.0</td>
      <td>9762546.0</td>
      <td>9853972</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>경상권</td>
      <td>3448737</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3655437</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>수도권</td>
      <td>2890451</td>
      <td>2632035.0</td>
      <td>NaN</td>
      <td>2466338</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>경상권</td>
      <td>2466052</td>
      <td>2431774.0</td>
      <td>2456016.0</td>
      <td>2473990</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 각각의 값이 나온 횟수를 세어주는 기능

population_number["2015"].value_counts()
```




    9904312    1
    3448737    1
    2890451    1
    2466052    1
    Name: 2015, dtype: int64




```python
# 2010년 카운팅
# 결측치는 카운팅 하지 않는다.

population_number["2010"].value_counts()
```




    9631482.0    1
    2632035.0    1
    2431774.0    1
    Name: 2010, dtype: int64




```python
# 정렬
# sort_index : 인덱스를 기준으로 정렬
# sort_values : 데이터 값을 기준으로 정렬
```


```python
# 기본정렬(=오름차순정렬)

population_number.sort_index()
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
      <th>지역</th>
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
      <th>2000</th>
    </tr>
    <tr>
      <th>도시</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>대구</th>
      <td>경상권</td>
      <td>2466052</td>
      <td>2431774.0</td>
      <td>2456016.0</td>
      <td>2473990</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>경상권</td>
      <td>3448737</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3655437</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>수도권</td>
      <td>9904312</td>
      <td>9631482.0</td>
      <td>9762546.0</td>
      <td>9853972</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>수도권</td>
      <td>2890451</td>
      <td>2632035.0</td>
      <td>NaN</td>
      <td>2466338</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 내림차순 정렬 : ascending=False

population_number.sort_index(ascending=False)
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
      <th>지역</th>
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
      <th>2000</th>
    </tr>
    <tr>
      <th>도시</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>인천</th>
      <td>수도권</td>
      <td>2890451</td>
      <td>2632035.0</td>
      <td>NaN</td>
      <td>2466338</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>수도권</td>
      <td>9904312</td>
      <td>9631482.0</td>
      <td>9762546.0</td>
      <td>9853972</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>경상권</td>
      <td>3448737</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3655437</td>
    </tr>
    <tr>
      <th>대구</th>
      <td>경상권</td>
      <td>2466052</td>
      <td>2431774.0</td>
      <td>2456016.0</td>
      <td>2473990</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 데이터 값을 기준으로 오름차순 정렬

population_number["2010"].sort_values()
```




    도시
    대구    2431774.0
    인천    2632035.0
    서울    9631482.0
    부산          NaN
    Name: 2010, dtype: float64




```python
# 데이터 값을 기준으로 내림차순 정렬

population_number["2010"].sort_values(ascending=False)
```




    도시
    서울    9631482.0
    인천    2632035.0
    대구    2431774.0
    부산          NaN
    Name: 2010, dtype: float64




```python
# by = 컬럼값
# 컬럼 값의 데이터를 기준으로 잡고 정렬 

population_number.sort_values(by="2010")
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
      <th>지역</th>
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
      <th>2000</th>
    </tr>
    <tr>
      <th>도시</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>대구</th>
      <td>경상권</td>
      <td>2466052</td>
      <td>2431774.0</td>
      <td>2456016.0</td>
      <td>2473990</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>수도권</td>
      <td>2890451</td>
      <td>2632035.0</td>
      <td>NaN</td>
      <td>2466338</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>수도권</td>
      <td>9904312</td>
      <td>9631482.0</td>
      <td>9762546.0</td>
      <td>9853972</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>경상권</td>
      <td>3448737</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3655437</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 여러가지 기준을 통해 정렬이 가능함

population_number.sort_values(by=["지역","2010"])
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
      <th>지역</th>
      <th>2015</th>
      <th>2010</th>
      <th>2005</th>
      <th>2000</th>
    </tr>
    <tr>
      <th>도시</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>대구</th>
      <td>경상권</td>
      <td>2466052</td>
      <td>2431774.0</td>
      <td>2456016.0</td>
      <td>2473990</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>경상권</td>
      <td>3448737</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3655437</td>
    </tr>
    <tr>
      <th>인천</th>
      <td>수도권</td>
      <td>2890451</td>
      <td>2632035.0</td>
      <td>NaN</td>
      <td>2466338</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>수도권</td>
      <td>9904312</td>
      <td>9631482.0</td>
      <td>9762546.0</td>
      <td>9853972</td>
    </tr>
  </tbody>
</table>
</div>



#### score 실습


```python
# 데이터 로드

score=pd.read_csv("score.csv",encoding="euc-kr",index_col="과목")
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
    </tr>
    <tr>
      <th>과목</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 어떤 반이 점수의 총합이 가장 클지?

score.sum().sort_values(ascending=False)
```




    2반    388
    3반    295
    1반    271
    4반    243
    dtype: int64




```python
# axis(축) 연산되는 방향 설정
# 디폴트값(=기본값) axis=0 : 열의 합
# axis=1 : 행의 합

score.sum(axis=1)
```




    과목
    파이썬    201
    DB     282
    자바     253
    크롤링    298
    Web    163
    dtype: int64




```python
# 실행을 하면 할수록 합계가 증가함.

score["합계"]=score.sum(axis=1)
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
    </tr>
    <tr>
      <th>과목</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 실행을 계속하더라도 합계는 변함없음(=증가X)

score["합계"]=score.iloc[:,:4].sum(axis=1)
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
    </tr>
    <tr>
      <th>과목</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
    </tr>
  </tbody>
</table>
</div>




```python
# score["평균"]=(score.iloc[:,4])/4
# score["평균"]=(score.loc[:,"합계"])/4
score["평균"]=score["합계"]/4
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>과목</th>
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
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
      <td>50.25</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
      <td>70.50</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
      <td>63.25</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
      <td>74.50</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
      <td>40.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
score.mean()
```




    1반     54.20
    2반     77.60
    3반     59.00
    4반     48.60
    합계    239.40
    평균     59.85
    dtype: float64




```python
# 과목별 최대점수

score["최대"]=score.iloc[:5,:4].max(axis=1)
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
      <th>평균</th>
      <th>최대</th>
    </tr>
    <tr>
      <th>과목</th>
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
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
      <td>50.25</td>
      <td>73</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
      <td>70.50</td>
      <td>92</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
      <td>63.25</td>
      <td>92</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
      <td>74.50</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
      <td>40.75</td>
      <td>79</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 과목별 최저점수

score["최저"]=score.iloc[:5,:4].min(axis=1)
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
      <th>평균</th>
      <th>최대</th>
      <th>최저</th>
    </tr>
    <tr>
      <th>과목</th>
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
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
      <td>50.25</td>
      <td>73</td>
      <td>39</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
      <td>70.50</td>
      <td>92</td>
      <td>45</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
      <td>63.25</td>
      <td>92</td>
      <td>45</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
      <td>74.50</td>
      <td>92</td>
      <td>40</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
      <td>40.75</td>
      <td>79</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
maxArr=score[["1반","2반","3반","4반"]].max(axis=1)
minArr=score.loc[:,"1반":"4반"].min(axis=1)
```


```python
maxArr-minArr
```




    과목
    파이썬    34
    DB     47
    자바     47
    크롤링    52
    Web    68
    dtype: int64




```python
# 과목별 최대 - 최소  
# 함수 만들기
# 함수 정의

def max_min(x):
    return x.max()-x.min()
```


```python
# 내가 만든 함수를 DataFrame에 적용할 때 사용하는 함수
# => apply 함수
# 행 또는 열 단위로 더 복잡한 처리를 할 때 사용된다.
```


```python
score["최대-최저"]=score.loc[:"Web",:"4반"].apply(max_min,axis=1)
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
      <th>평균</th>
      <th>최대</th>
      <th>최저</th>
      <th>최대-최저</th>
    </tr>
    <tr>
      <th>과목</th>
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
      <th>파이썬</th>
      <td>45</td>
      <td>44</td>
      <td>73</td>
      <td>39</td>
      <td>201</td>
      <td>50.25</td>
      <td>73</td>
      <td>39</td>
      <td>34</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>282</td>
      <td>70.50</td>
      <td>92</td>
      <td>45</td>
      <td>47</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47</td>
      <td>92</td>
      <td>45</td>
      <td>69</td>
      <td>253</td>
      <td>63.25</td>
      <td>92</td>
      <td>45</td>
      <td>47</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92</td>
      <td>81</td>
      <td>85</td>
      <td>40</td>
      <td>298</td>
      <td>74.50</td>
      <td>92</td>
      <td>40</td>
      <td>52</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11</td>
      <td>79</td>
      <td>47</td>
      <td>26</td>
      <td>163</td>
      <td>40.75</td>
      <td>79</td>
      <td>11</td>
      <td>68</td>
    </tr>
  </tbody>
</table>
</div>




```python
score.loc["평균"]=score.mean()
score
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
      <th>1반</th>
      <th>2반</th>
      <th>3반</th>
      <th>4반</th>
      <th>합계</th>
      <th>평균</th>
      <th>최대</th>
      <th>최저</th>
      <th>최대-최저</th>
    </tr>
    <tr>
      <th>과목</th>
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
      <th>파이썬</th>
      <td>45.0</td>
      <td>44.0</td>
      <td>73.0</td>
      <td>39.0</td>
      <td>201.0</td>
      <td>50.25</td>
      <td>73.0</td>
      <td>39.0</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>DB</th>
      <td>76.0</td>
      <td>92.0</td>
      <td>45.0</td>
      <td>69.0</td>
      <td>282.0</td>
      <td>70.50</td>
      <td>92.0</td>
      <td>45.0</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>자바</th>
      <td>47.0</td>
      <td>92.0</td>
      <td>45.0</td>
      <td>69.0</td>
      <td>253.0</td>
      <td>63.25</td>
      <td>92.0</td>
      <td>45.0</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>크롤링</th>
      <td>92.0</td>
      <td>81.0</td>
      <td>85.0</td>
      <td>40.0</td>
      <td>298.0</td>
      <td>74.50</td>
      <td>92.0</td>
      <td>40.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>Web</th>
      <td>11.0</td>
      <td>79.0</td>
      <td>47.0</td>
      <td>26.0</td>
      <td>163.0</td>
      <td>40.75</td>
      <td>79.0</td>
      <td>11.0</td>
      <td>68.0</td>
    </tr>
    <tr>
      <th>평균</th>
      <td>54.2</td>
      <td>77.6</td>
      <td>59.0</td>
      <td>48.6</td>
      <td>239.4</td>
      <td>59.85</td>
      <td>85.6</td>
      <td>36.0</td>
      <td>49.6</td>
    </tr>
  </tbody>
</table>
</div>



### DataFrame 병합
- concat
- merge


```python
df1 = pd.DataFrame(  {'A':['A0', 'A1', 'A2', 'A3'],
               'B':['B0', 'B1', 'B2', 'B3'],
               'C' : ['C0', 'C1', 'C2', 'C3'],
               'D' : ['D0', 'D1', 'D2', 'D3']} ,
                  index=[0, 1, 2, 3])
df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                   index=[4, 5, 6, 7])
df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']},
                   index=[8, 9, 10, 11])
```


```python
df1
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
# concat 

pd.concat([df1,df2,df3])
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
# concat

pd.concat([df1,df2,df3],axis=1)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A4</td>
      <td>B4</td>
      <td>C4</td>
      <td>D4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A5</td>
      <td>B5</td>
      <td>C5</td>
      <td>D5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A6</td>
      <td>B6</td>
      <td>C6</td>
      <td>D6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A7</td>
      <td>B7</td>
      <td>C7</td>
      <td>D7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A8</td>
      <td>B8</td>
      <td>C8</td>
      <td>D8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A9</td>
      <td>B9</td>
      <td>C9</td>
      <td>D9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A10</td>
      <td>B10</td>
      <td>C10</td>
      <td>D10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A11</td>
      <td>B11</td>
      <td>C11</td>
      <td>D11</td>
    </tr>
  </tbody>
</table>
</div>




```python
df4=pd.DataFrame({"B":["B2","B3","B6","B7"],
                 "D":["D2","D3","D6","D7"],
                 "F":["F2","F3","F6","F7"]},
                 index=[2,3,6,7])
df4
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
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B6</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>B7</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>




```python
df1
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# df1과 df4 합치기(axis=0인 경우)
pd.concat([df1,df4])
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>B2</td>
      <td>NaN</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>B3</td>
      <td>NaN</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>B6</td>
      <td>NaN</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>B7</td>
      <td>NaN</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# df1과 df4 합치기(axis=1인 경우)
pd.concat([df1,df4],axis=1)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>D1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B6</td>
      <td>D6</td>
      <td>F6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B7</td>
      <td>D7</td>
      <td>F7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# join 속성
# outer : 합집합, 기본값
# inner : 교집합, 합치는 데이터들이 동일하게 가지고 있는 기준만 출력

pd.concat([df1,df4],axis=1,join="inner")
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>B</th>
      <th>D</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>D2</td>
      <td>B2</td>
      <td>D2</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>D3</td>
      <td>B3</td>
      <td>D3</td>
      <td>F3</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df1,df4],axis=0,join="inner")
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
      <th>B</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>B0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>B2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>B3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>B2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>B3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>B7</td>
      <td>D7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# reset.index() 함수 : "index"라는 컬럼이 만들어지고, 그 칼럼에 기존에
# 가지고 있던 인덱스가 값으로 들어간다.
# 새로운 인덱스가 오름차순으로 만들어진다.

reset_df=pd.concat([df1,df4],axis=0,join="inner").reset_index()
```


```python
# df 데이터 삭제하기
# drop()
# 컬럼이 삭제인 경우 : axis=1
# 데이터(행) 삭제 : axis=0
# 삭제 시킨 데이터를 변수에 반영하고 싶을 때 -> inplace=True

reset_df.drop("index",axis=1,inplace=True)
```


```python
reset_df
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
      <th>B</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>B0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>B2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>B3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>B3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B6</td>
      <td>D6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>B7</td>
      <td>D7</td>
    </tr>
  </tbody>
</table>
</div>




```python
# merge
# 공통된 데이터를 기준으로 데이터를 병합
```


```python
# 데이터 생성

df5 = pd.DataFrame({'key' : ['K0','K2','K3','K4'],
                   'A':['A0','A1','A2','A3'],
                   'B':['B0','B1','B2','B3']})
df6 = pd.DataFrame({'key' : ['K0','K1','K2','K3'],
                   'C':['C0','C1','C2','C3'],
                   'D':['D0','D1','D2','D3']})
```


```python
df5
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
      <th>key</th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>A0</td>
      <td>B0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K2</td>
      <td>A1</td>
      <td>B1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K3</td>
      <td>A2</td>
      <td>B2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K4</td>
      <td>A3</td>
      <td>B3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df6
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
      <th>key</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K1</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K2</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K3</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# concet는 2개 이상 가능, 리스트 사용 -> 병합함수 : join
# merge는 2개만 가능, 리스트 사용X -> 병합함수 : how

pd.merge(df5,df6,on="key")
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
      <th>key</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K2</td>
      <td>A1</td>
      <td>B1</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K3</td>
      <td>A2</td>
      <td>B2</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# merge의 병합함수 : how
# outer : 전체 값 출력
# inner : 공동퇸 데이터의 값만 출력
# left : 왼쪽 데이터(먼저 적은 데이터를 기준으로 출력)
# right : 오른쪽 데이터(나중에 적은 데이터를 기준으로 출력)
# how의 기본값은 inner

pd.merge(df5,df6,on="key", how="right")
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
      <th>key</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>C1</td>
      <td>D1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K2</td>
      <td>A1</td>
      <td>B1</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K3</td>
      <td>A2</td>
      <td>B2</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df7=pd.merge(df5,df6,on="key", how="left")
```


```python
# 결측치 채우는 함수 - fillna

df7.fillna(0)
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
      <th>key</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K0</td>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>D0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K2</td>
      <td>A1</td>
      <td>B1</td>
      <td>C2</td>
      <td>D2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K3</td>
      <td>A2</td>
      <td>B2</td>
      <td>C3</td>
      <td>D3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>K4</td>
      <td>A3</td>
      <td>B3</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 카테고리화

# 데이터
ages=[0,2,10,21,25,34,39,41,58,67,95,43,100,60]

# 구간(초과~이하)
bins=[0,15,25,35,90,99]

# 구간의 이름
labels=["미성년자","청년","중년","장년","노년"]

# 1~15 : 미성년자, 16~25 : 청년, 26~35 : 중년, 36~90 : 장년, 91~99 : 노년
```


```python
# 구간별로 잘라주는 함수 : cut()
# 구간(범위)안에 존재하지 않는 데이터는 결측치로 출력

cats = pd.cut(ages,bins,labels=labels)
cats
```




    [NaN, '미성년자', '미성년자', '청년', '청년', ..., '장년', '노년', '장년', NaN, '장년']
    Length: 14
    Categories (5, object): ['미성년자' < '청년' < '중년' < '장년' < '노년']




```python
# 결측치를 제외 한 항목별 갯수 파악

cats.value_counts()
```




    미성년자    2
    청년      2
    중년      1
    장년      6
    노년      1
    dtype: int64




```python
age = pd.DataFrame(ages, columns=["나이"])
```


```python
# 새 컬럼 생성 - "연령대"

age["연령대"] = cats
age
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
      <th>나이</th>
      <th>연령대</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>미성년자</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>미성년자</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>청년</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25</td>
      <td>청년</td>
    </tr>
    <tr>
      <th>5</th>
      <td>34</td>
      <td>중년</td>
    </tr>
    <tr>
      <th>6</th>
      <td>39</td>
      <td>장년</td>
    </tr>
    <tr>
      <th>7</th>
      <td>41</td>
      <td>장년</td>
    </tr>
    <tr>
      <th>8</th>
      <td>58</td>
      <td>장년</td>
    </tr>
    <tr>
      <th>9</th>
      <td>67</td>
      <td>장년</td>
    </tr>
    <tr>
      <th>10</th>
      <td>95</td>
      <td>노년</td>
    </tr>
    <tr>
      <th>11</th>
      <td>43</td>
      <td>장년</td>
    </tr>
    <tr>
      <th>12</th>
      <td>100</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>60</td>
      <td>장년</td>
    </tr>
  </tbody>
</table>
</div>



### 범죄 현황 실습
- 2015~2017년 광주광역시 범죄현황 데이터를 이용해 전년 대비 지역별 범죄 증감율 구하기


```python
# 데이터 불러오기

df2015=pd.read_csv("2015.csv",encoding="euc-kr",index_col="관서명")
df2016=pd.read_csv("2016.csv",encoding="euc-kr",index_col="관서명")
df2017=pd.read_csv("2017.csv",encoding="euc-kr",index_col="관서명")
```


```python
df2015
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>18</td>
      <td>44</td>
      <td>750</td>
      <td>8425</td>
      <td>9593</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>18</td>
      <td>47</td>
      <td>758</td>
      <td>5409</td>
      <td>8301</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>17</td>
      <td>66</td>
      <td>776</td>
      <td>3433</td>
      <td>11774</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>9</td>
      <td>33</td>
      <td>42</td>
      <td>104</td>
      <td>58</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>1</td>
      <td>26</td>
      <td>511</td>
      <td>2781</td>
      <td>5618</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>7</td>
      <td>7</td>
      <td>223</td>
      <td>548</td>
      <td>6098</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>92</td>
      <td>1100</td>
      <td>1155</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>6</td>
      <td>86</td>
      <td>583</td>
      <td>970</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>7</td>
      <td>98</td>
      <td>447</td>
      <td>1483</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>2</td>
      <td>8</td>
      <td>13</td>
      <td>10</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>63</td>
      <td>379</td>
      <td>703</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>1</td>
      <td>27</td>
      <td>55</td>
      <td>770</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>10</td>
      <td>172</td>
      <td>2050</td>
      <td>2483</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>8</td>
      <td>153</td>
      <td>1471</td>
      <td>2124</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>15</td>
      <td>167</td>
      <td>876</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>10</td>
      <td>7</td>
      <td>27</td>
      <td>19</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>5</td>
      <td>91</td>
      <td>665</td>
      <td>1366</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>69</td>
      <td>184</td>
      <td>1695</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>3</td>
      <td>70</td>
      <td>962</td>
      <td>1081</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>1</td>
      <td>3</td>
      <td>53</td>
      <td>506</td>
      <td>941</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>1</td>
      <td>3</td>
      <td>52</td>
      <td>418</td>
      <td>1260</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>19</td>
      <td>3</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>325</td>
      <td>675</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>74</td>
      <td>582</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>14</td>
      <td>256</td>
      <td>2570</td>
      <td>2621</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>18</td>
      <td>212</td>
      <td>1852</td>
      <td>2319</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>5</td>
      <td>28</td>
      <td>216</td>
      <td>948</td>
      <td>3168</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>11</td>
      <td>11</td>
      <td>30</td>
      <td>10</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>12</td>
      <td>153</td>
      <td>770</td>
      <td>1544</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>5</td>
      <td>52</td>
      <td>148</td>
      <td>1614</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>4</td>
      <td>12</td>
      <td>160</td>
      <td>1743</td>
      <td>2253</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>10</td>
      <td>135</td>
      <td>996</td>
      <td>1922</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>8</td>
      <td>129</td>
      <td>736</td>
      <td>2585</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>81</td>
      <td>639</td>
      <td>1181</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>3</td>
      <td>0</td>
      <td>43</td>
      <td>85</td>
      <td>1398</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2016
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>17</td>
      <td>47</td>
      <td>701</td>
      <td>6052</td>
      <td>8599</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>18</td>
      <td>47</td>
      <td>713</td>
      <td>4242</td>
      <td>7631</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>21</td>
      <td>54</td>
      <td>758</td>
      <td>3455</td>
      <td>10747</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>14</td>
      <td>25</td>
      <td>37</td>
      <td>132</td>
      <td>57</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>3</td>
      <td>25</td>
      <td>491</td>
      <td>2862</td>
      <td>5267</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>4</td>
      <td>4</td>
      <td>230</td>
      <td>461</td>
      <td>5423</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>8</td>
      <td>83</td>
      <td>832</td>
      <td>1142</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>7</td>
      <td>70</td>
      <td>679</td>
      <td>1002</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>10</td>
      <td>71</td>
      <td>543</td>
      <td>1497</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>17</td>
      <td>7</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>7</td>
      <td>50</td>
      <td>460</td>
      <td>773</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>1</td>
      <td>18</td>
      <td>66</td>
      <td>717</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>2</td>
      <td>11</td>
      <td>174</td>
      <td>1417</td>
      <td>2288</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>11</td>
      <td>158</td>
      <td>963</td>
      <td>1994</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>13</td>
      <td>169</td>
      <td>894</td>
      <td>2874</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>6</td>
      <td>7</td>
      <td>22</td>
      <td>18</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>92</td>
      <td>749</td>
      <td>1301</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>3</td>
      <td>70</td>
      <td>123</td>
      <td>1555</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>4</td>
      <td>64</td>
      <td>768</td>
      <td>1028</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>1</td>
      <td>4</td>
      <td>54</td>
      <td>544</td>
      <td>883</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>1</td>
      <td>5</td>
      <td>55</td>
      <td>348</td>
      <td>1198</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>12</td>
      <td>2</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>38</td>
      <td>264</td>
      <td>599</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>72</td>
      <td>597</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>6</td>
      <td>7</td>
      <td>205</td>
      <td>1788</td>
      <td>2142</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>8</td>
      <td>158</td>
      <td>1156</td>
      <td>1906</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>6</td>
      <td>10</td>
      <td>178</td>
      <td>933</td>
      <td>2700</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>6</td>
      <td>1</td>
      <td>50</td>
      <td>13</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>2</td>
      <td>4</td>
      <td>135</td>
      <td>759</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>42</td>
      <td>124</td>
      <td>1285</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>17</td>
      <td>175</td>
      <td>1247</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>17</td>
      <td>147</td>
      <td>898</td>
      <td>1798</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>6</td>
      <td>16</td>
      <td>147</td>
      <td>723</td>
      <td>2382</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>31</td>
      <td>7</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>1</td>
      <td>9</td>
      <td>85</td>
      <td>620</td>
      <td>1125</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>48</td>
      <td>72</td>
      <td>1250</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2017
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>9</td>
      <td>33</td>
      <td>725</td>
      <td>4816</td>
      <td>8366</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>9</td>
      <td>32</td>
      <td>732</td>
      <td>3487</td>
      <td>7553</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>10</td>
      <td>61</td>
      <td>824</td>
      <td>3046</td>
      <td>11018</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>8</td>
      <td>28</td>
      <td>71</td>
      <td>115</td>
      <td>88</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>0</td>
      <td>26</td>
      <td>523</td>
      <td>2493</td>
      <td>5235</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>2</td>
      <td>7</td>
      <td>230</td>
      <td>438</td>
      <td>5695</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>발생건수</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>검거건수</td>
      <td>0</td>
      <td>1</td>
      <td>91</td>
      <td>0</td>
      <td>37</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>검거인원</td>
      <td>0</td>
      <td>1</td>
      <td>105</td>
      <td>0</td>
      <td>149</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>구속</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>53</td>
      <td>0</td>
      <td>69</td>
    </tr>
    <tr>
      <th>광주지방경찰청</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
      <td>0</td>
      <td>73</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>77</td>
      <td>624</td>
      <td>1090</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>70</td>
      <td>470</td>
      <td>953</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>4</td>
      <td>76</td>
      <td>483</td>
      <td>1538</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>19</td>
      <td>9</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>57</td>
      <td>395</td>
      <td>801</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>0</td>
      <td>17</td>
      <td>69</td>
      <td>728</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>7</td>
      <td>196</td>
      <td>1142</td>
      <td>2293</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>7</td>
      <td>172</td>
      <td>708</td>
      <td>2065</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>23</td>
      <td>188</td>
      <td>708</td>
      <td>2971</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>20</td>
      <td>31</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>15</td>
      <td>123</td>
      <td>582</td>
      <td>1356</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>59</td>
      <td>106</td>
      <td>1584</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>4</td>
      <td>68</td>
      <td>577</td>
      <td>898</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>4</td>
      <td>51</td>
      <td>522</td>
      <td>799</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>5</td>
      <td>47</td>
      <td>363</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>18</td>
      <td>1</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>2</td>
      <td>29</td>
      <td>271</td>
      <td>591</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>1</td>
      <td>17</td>
      <td>74</td>
      <td>515</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>215</td>
      <td>1546</td>
      <td>2176</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>204</td>
      <td>1127</td>
      <td>1997</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>6</td>
      <td>260</td>
      <td>898</td>
      <td>2863</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>4</td>
      <td>39</td>
      <td>33</td>
      <td>22</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>0</td>
      <td>159</td>
      <td>730</td>
      <td>1314</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>2</td>
      <td>62</td>
      <td>135</td>
      <td>1527</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>12</td>
      <td>169</td>
      <td>927</td>
      <td>1909</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>10</td>
      <td>144</td>
      <td>660</td>
      <td>1702</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>22</td>
      <td>148</td>
      <td>594</td>
      <td>2390</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>25</td>
      <td>18</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>7</td>
      <td>102</td>
      <td>515</td>
      <td>1104</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>4</td>
      <td>40</td>
      <td>54</td>
      <td>1268</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 2017년에만 있는 데이터 삭제하기 : "광주지방경찰청"

df2017.drop("광주지방경찰청",axis=0,inplace=True)
```


```python
df2017
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>9</td>
      <td>33</td>
      <td>725</td>
      <td>4816</td>
      <td>8366</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>9</td>
      <td>32</td>
      <td>732</td>
      <td>3487</td>
      <td>7553</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>10</td>
      <td>61</td>
      <td>824</td>
      <td>3046</td>
      <td>11018</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>8</td>
      <td>28</td>
      <td>71</td>
      <td>115</td>
      <td>88</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>0</td>
      <td>26</td>
      <td>523</td>
      <td>2493</td>
      <td>5235</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>2</td>
      <td>7</td>
      <td>230</td>
      <td>438</td>
      <td>5695</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>77</td>
      <td>624</td>
      <td>1090</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>70</td>
      <td>470</td>
      <td>953</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>4</td>
      <td>76</td>
      <td>483</td>
      <td>1538</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>19</td>
      <td>9</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>57</td>
      <td>395</td>
      <td>801</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>0</td>
      <td>17</td>
      <td>69</td>
      <td>728</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>7</td>
      <td>196</td>
      <td>1142</td>
      <td>2293</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>7</td>
      <td>172</td>
      <td>708</td>
      <td>2065</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>23</td>
      <td>188</td>
      <td>708</td>
      <td>2971</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>20</td>
      <td>31</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>15</td>
      <td>123</td>
      <td>582</td>
      <td>1356</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>59</td>
      <td>106</td>
      <td>1584</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>4</td>
      <td>68</td>
      <td>577</td>
      <td>898</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>4</td>
      <td>51</td>
      <td>522</td>
      <td>799</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>5</td>
      <td>47</td>
      <td>363</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>18</td>
      <td>1</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>2</td>
      <td>29</td>
      <td>271</td>
      <td>591</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>1</td>
      <td>17</td>
      <td>74</td>
      <td>515</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>215</td>
      <td>1546</td>
      <td>2176</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>204</td>
      <td>1127</td>
      <td>1997</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>6</td>
      <td>260</td>
      <td>898</td>
      <td>2863</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>4</td>
      <td>39</td>
      <td>33</td>
      <td>22</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>0</td>
      <td>159</td>
      <td>730</td>
      <td>1314</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>2</td>
      <td>62</td>
      <td>135</td>
      <td>1527</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>12</td>
      <td>169</td>
      <td>927</td>
      <td>1909</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>10</td>
      <td>144</td>
      <td>660</td>
      <td>1702</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>22</td>
      <td>148</td>
      <td>594</td>
      <td>2390</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>25</td>
      <td>18</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>7</td>
      <td>102</td>
      <td>515</td>
      <td>1104</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>4</td>
      <td>40</td>
      <td>54</td>
      <td>1268</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 데이터 총합 구하기(=범죄 총 발생 건수의 총합 구하기)
# 새로운 칼럼 생성 : "20xx총계"

df2017["2017총계"]=df2017.iloc[:,1:7].sum(axis=1)
df2017
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2017총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>9</td>
      <td>33</td>
      <td>725</td>
      <td>4816</td>
      <td>8366</td>
      <td>13949</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>9</td>
      <td>32</td>
      <td>732</td>
      <td>3487</td>
      <td>7553</td>
      <td>11813</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>10</td>
      <td>61</td>
      <td>824</td>
      <td>3046</td>
      <td>11018</td>
      <td>14959</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>8</td>
      <td>28</td>
      <td>71</td>
      <td>115</td>
      <td>88</td>
      <td>310</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>0</td>
      <td>26</td>
      <td>523</td>
      <td>2493</td>
      <td>5235</td>
      <td>8277</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>2</td>
      <td>7</td>
      <td>230</td>
      <td>438</td>
      <td>5695</td>
      <td>6372</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>77</td>
      <td>624</td>
      <td>1090</td>
      <td>1799</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>70</td>
      <td>470</td>
      <td>953</td>
      <td>1501</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>4</td>
      <td>76</td>
      <td>483</td>
      <td>1538</td>
      <td>2105</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>19</td>
      <td>9</td>
      <td>35</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>57</td>
      <td>395</td>
      <td>801</td>
      <td>1254</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>0</td>
      <td>17</td>
      <td>69</td>
      <td>728</td>
      <td>816</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>7</td>
      <td>196</td>
      <td>1142</td>
      <td>2293</td>
      <td>3638</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>7</td>
      <td>172</td>
      <td>708</td>
      <td>2065</td>
      <td>2952</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>23</td>
      <td>188</td>
      <td>708</td>
      <td>2971</td>
      <td>3890</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>8</td>
      <td>6</td>
      <td>20</td>
      <td>31</td>
      <td>65</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>15</td>
      <td>123</td>
      <td>582</td>
      <td>1356</td>
      <td>2076</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>59</td>
      <td>106</td>
      <td>1584</td>
      <td>1749</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>4</td>
      <td>68</td>
      <td>577</td>
      <td>898</td>
      <td>1547</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>0</td>
      <td>4</td>
      <td>51</td>
      <td>522</td>
      <td>799</td>
      <td>1376</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>0</td>
      <td>5</td>
      <td>47</td>
      <td>363</td>
      <td>1107</td>
      <td>1522</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>18</td>
      <td>1</td>
      <td>22</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>2</td>
      <td>29</td>
      <td>271</td>
      <td>591</td>
      <td>893</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>1</td>
      <td>17</td>
      <td>74</td>
      <td>515</td>
      <td>607</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>215</td>
      <td>1546</td>
      <td>2176</td>
      <td>3945</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>5</td>
      <td>204</td>
      <td>1127</td>
      <td>1997</td>
      <td>3336</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>6</td>
      <td>260</td>
      <td>898</td>
      <td>2863</td>
      <td>4030</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>4</td>
      <td>39</td>
      <td>33</td>
      <td>22</td>
      <td>101</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>0</td>
      <td>159</td>
      <td>730</td>
      <td>1314</td>
      <td>2203</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>2</td>
      <td>62</td>
      <td>135</td>
      <td>1527</td>
      <td>1726</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>12</td>
      <td>169</td>
      <td>927</td>
      <td>1909</td>
      <td>3020</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>10</td>
      <td>144</td>
      <td>660</td>
      <td>1702</td>
      <td>2519</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>22</td>
      <td>148</td>
      <td>594</td>
      <td>2390</td>
      <td>3157</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>25</td>
      <td>18</td>
      <td>63</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>7</td>
      <td>102</td>
      <td>515</td>
      <td>1104</td>
      <td>1728</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>4</td>
      <td>40</td>
      <td>54</td>
      <td>1268</td>
      <td>1366</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2015["2015총계"]=df2015.iloc[:,1:7].sum(axis=1)
df2015
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2015총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>18</td>
      <td>44</td>
      <td>750</td>
      <td>8425</td>
      <td>9593</td>
      <td>18830</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>18</td>
      <td>47</td>
      <td>758</td>
      <td>5409</td>
      <td>8301</td>
      <td>14533</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>17</td>
      <td>66</td>
      <td>776</td>
      <td>3433</td>
      <td>11774</td>
      <td>16066</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>9</td>
      <td>33</td>
      <td>42</td>
      <td>104</td>
      <td>58</td>
      <td>246</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>1</td>
      <td>26</td>
      <td>511</td>
      <td>2781</td>
      <td>5618</td>
      <td>8937</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>7</td>
      <td>7</td>
      <td>223</td>
      <td>548</td>
      <td>6098</td>
      <td>6883</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>92</td>
      <td>1100</td>
      <td>1155</td>
      <td>2355</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>6</td>
      <td>86</td>
      <td>583</td>
      <td>970</td>
      <td>1649</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>7</td>
      <td>98</td>
      <td>447</td>
      <td>1483</td>
      <td>2039</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>2</td>
      <td>8</td>
      <td>13</td>
      <td>10</td>
      <td>36</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>63</td>
      <td>379</td>
      <td>703</td>
      <td>1149</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>1</td>
      <td>27</td>
      <td>55</td>
      <td>770</td>
      <td>854</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>10</td>
      <td>172</td>
      <td>2050</td>
      <td>2483</td>
      <td>4720</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>8</td>
      <td>153</td>
      <td>1471</td>
      <td>2124</td>
      <td>3760</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>15</td>
      <td>167</td>
      <td>876</td>
      <td>3080</td>
      <td>4142</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>10</td>
      <td>7</td>
      <td>27</td>
      <td>19</td>
      <td>66</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>5</td>
      <td>91</td>
      <td>665</td>
      <td>1366</td>
      <td>2127</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>69</td>
      <td>184</td>
      <td>1695</td>
      <td>1949</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>3</td>
      <td>70</td>
      <td>962</td>
      <td>1081</td>
      <td>2117</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>1</td>
      <td>3</td>
      <td>53</td>
      <td>506</td>
      <td>941</td>
      <td>1504</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>1</td>
      <td>3</td>
      <td>52</td>
      <td>418</td>
      <td>1260</td>
      <td>1734</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>19</td>
      <td>3</td>
      <td>28</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>325</td>
      <td>675</td>
      <td>1040</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>74</td>
      <td>582</td>
      <td>666</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>14</td>
      <td>256</td>
      <td>2570</td>
      <td>2621</td>
      <td>5466</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>18</td>
      <td>212</td>
      <td>1852</td>
      <td>2319</td>
      <td>4406</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>5</td>
      <td>28</td>
      <td>216</td>
      <td>948</td>
      <td>3168</td>
      <td>4365</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>11</td>
      <td>11</td>
      <td>30</td>
      <td>10</td>
      <td>65</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>12</td>
      <td>153</td>
      <td>770</td>
      <td>1544</td>
      <td>2479</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>5</td>
      <td>52</td>
      <td>148</td>
      <td>1614</td>
      <td>1821</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>4</td>
      <td>12</td>
      <td>160</td>
      <td>1743</td>
      <td>2253</td>
      <td>4172</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>4</td>
      <td>10</td>
      <td>135</td>
      <td>996</td>
      <td>1922</td>
      <td>3067</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>8</td>
      <td>129</td>
      <td>736</td>
      <td>2585</td>
      <td>3461</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>0</td>
      <td>4</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
      <td>27</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>81</td>
      <td>639</td>
      <td>1181</td>
      <td>1905</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>3</td>
      <td>0</td>
      <td>43</td>
      <td>85</td>
      <td>1398</td>
      <td>1529</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2016["2016총계"]=df2016.iloc[:,1:7].sum(axis=1)
df2016
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2016총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>17</td>
      <td>47</td>
      <td>701</td>
      <td>6052</td>
      <td>8599</td>
      <td>15416</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거건수</td>
      <td>18</td>
      <td>47</td>
      <td>713</td>
      <td>4242</td>
      <td>7631</td>
      <td>12651</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>검거인원</td>
      <td>21</td>
      <td>54</td>
      <td>758</td>
      <td>3455</td>
      <td>10747</td>
      <td>15035</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>구속</td>
      <td>14</td>
      <td>25</td>
      <td>37</td>
      <td>132</td>
      <td>57</td>
      <td>265</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>불구속</td>
      <td>3</td>
      <td>25</td>
      <td>491</td>
      <td>2862</td>
      <td>5267</td>
      <td>8648</td>
    </tr>
    <tr>
      <th>광주지방경찰청계</th>
      <td>기타</td>
      <td>4</td>
      <td>4</td>
      <td>230</td>
      <td>461</td>
      <td>5423</td>
      <td>6122</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>8</td>
      <td>83</td>
      <td>832</td>
      <td>1142</td>
      <td>2068</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>7</td>
      <td>70</td>
      <td>679</td>
      <td>1002</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>검거인원</td>
      <td>4</td>
      <td>10</td>
      <td>71</td>
      <td>543</td>
      <td>1497</td>
      <td>2125</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>구속</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>17</td>
      <td>7</td>
      <td>31</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>7</td>
      <td>50</td>
      <td>460</td>
      <td>773</td>
      <td>1290</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>기타</td>
      <td>2</td>
      <td>1</td>
      <td>18</td>
      <td>66</td>
      <td>717</td>
      <td>804</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>2</td>
      <td>11</td>
      <td>174</td>
      <td>1417</td>
      <td>2288</td>
      <td>3892</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거건수</td>
      <td>3</td>
      <td>11</td>
      <td>158</td>
      <td>963</td>
      <td>1994</td>
      <td>3129</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>검거인원</td>
      <td>3</td>
      <td>13</td>
      <td>169</td>
      <td>894</td>
      <td>2874</td>
      <td>3953</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>6</td>
      <td>7</td>
      <td>22</td>
      <td>18</td>
      <td>56</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>4</td>
      <td>92</td>
      <td>749</td>
      <td>1301</td>
      <td>2146</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>3</td>
      <td>70</td>
      <td>123</td>
      <td>1555</td>
      <td>1751</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>4</td>
      <td>64</td>
      <td>768</td>
      <td>1028</td>
      <td>1865</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거건수</td>
      <td>1</td>
      <td>4</td>
      <td>54</td>
      <td>544</td>
      <td>883</td>
      <td>1486</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>검거인원</td>
      <td>1</td>
      <td>5</td>
      <td>55</td>
      <td>348</td>
      <td>1198</td>
      <td>1607</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>구속</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>12</td>
      <td>2</td>
      <td>21</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>불구속</td>
      <td>0</td>
      <td>1</td>
      <td>38</td>
      <td>264</td>
      <td>599</td>
      <td>902</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>기타</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>72</td>
      <td>597</td>
      <td>684</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>6</td>
      <td>7</td>
      <td>205</td>
      <td>1788</td>
      <td>2142</td>
      <td>4148</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>8</td>
      <td>158</td>
      <td>1156</td>
      <td>1906</td>
      <td>3233</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>검거인원</td>
      <td>6</td>
      <td>10</td>
      <td>178</td>
      <td>933</td>
      <td>2700</td>
      <td>3827</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>구속</td>
      <td>3</td>
      <td>6</td>
      <td>1</td>
      <td>50</td>
      <td>13</td>
      <td>73</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>불구속</td>
      <td>2</td>
      <td>4</td>
      <td>135</td>
      <td>759</td>
      <td>1402</td>
      <td>2302</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>42</td>
      <td>124</td>
      <td>1285</td>
      <td>1452</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>17</td>
      <td>175</td>
      <td>1247</td>
      <td>1999</td>
      <td>3443</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거건수</td>
      <td>5</td>
      <td>17</td>
      <td>147</td>
      <td>898</td>
      <td>1798</td>
      <td>2865</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>검거인원</td>
      <td>6</td>
      <td>16</td>
      <td>147</td>
      <td>723</td>
      <td>2382</td>
      <td>3274</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>구속</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>31</td>
      <td>7</td>
      <td>63</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>불구속</td>
      <td>1</td>
      <td>9</td>
      <td>85</td>
      <td>620</td>
      <td>1125</td>
      <td>1840</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>기타</td>
      <td>1</td>
      <td>0</td>
      <td>48</td>
      <td>72</td>
      <td>1250</td>
      <td>1371</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 구분 컬럼에서 "발생건수"인 데이터 판별

df2015_crime=df2015[df2015["구분"] == "발생건수"]
df2015_crime
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2015총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>18</td>
      <td>44</td>
      <td>750</td>
      <td>8425</td>
      <td>9593</td>
      <td>18830</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>92</td>
      <td>1100</td>
      <td>1155</td>
      <td>2355</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>10</td>
      <td>172</td>
      <td>2050</td>
      <td>2483</td>
      <td>4720</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>3</td>
      <td>70</td>
      <td>962</td>
      <td>1081</td>
      <td>2117</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>14</td>
      <td>256</td>
      <td>2570</td>
      <td>2621</td>
      <td>5466</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>4</td>
      <td>12</td>
      <td>160</td>
      <td>1743</td>
      <td>2253</td>
      <td>4172</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2016_crime=df2016[df2016["구분"] == "발생건수"]
df2016_crime
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2016총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>17</td>
      <td>47</td>
      <td>701</td>
      <td>6052</td>
      <td>8599</td>
      <td>15416</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>8</td>
      <td>83</td>
      <td>832</td>
      <td>1142</td>
      <td>2068</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>2</td>
      <td>11</td>
      <td>174</td>
      <td>1417</td>
      <td>2288</td>
      <td>3892</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>1</td>
      <td>4</td>
      <td>64</td>
      <td>768</td>
      <td>1028</td>
      <td>1865</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>6</td>
      <td>7</td>
      <td>205</td>
      <td>1788</td>
      <td>2142</td>
      <td>4148</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>5</td>
      <td>17</td>
      <td>175</td>
      <td>1247</td>
      <td>1999</td>
      <td>3443</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2017_crime=df2017[df2017["구분"] == "발생건수"]
df2017_crime
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
      <th>구분</th>
      <th>살인</th>
      <th>강도</th>
      <th>강간·강제추행</th>
      <th>절도</th>
      <th>폭력</th>
      <th>2017총계</th>
    </tr>
    <tr>
      <th>관서명</th>
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
      <th>광주지방경찰청계</th>
      <td>발생건수</td>
      <td>9</td>
      <td>33</td>
      <td>725</td>
      <td>4816</td>
      <td>8366</td>
      <td>13949</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>77</td>
      <td>624</td>
      <td>1090</td>
      <td>1799</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>7</td>
      <td>196</td>
      <td>1142</td>
      <td>2293</td>
      <td>3638</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>발생건수</td>
      <td>0</td>
      <td>4</td>
      <td>68</td>
      <td>577</td>
      <td>898</td>
      <td>1547</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>5</td>
      <td>215</td>
      <td>1546</td>
      <td>2176</td>
      <td>3945</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>발생건수</td>
      <td>3</td>
      <td>12</td>
      <td>169</td>
      <td>927</td>
      <td>1909</td>
      <td>3020</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 증감율 계산을 위해 총계 컬럼만 가지고 오기
s5=df2015_crime["2015총계"]
s5
```




    관서명
    광주지방경찰청계    18830
    광주동부경찰서      2355
    광주서부경찰서      4720
    광주남부경찰서      2117
    광주북부경찰서      5466
    광주광산경찰서      4172
    Name: 2015총계, dtype: int64




```python
s6=df2016_crime["2016총계"]
s6
```




    관서명
    광주지방경찰청계    15416
    광주동부경찰서      2068
    광주서부경찰서      3892
    광주남부경찰서      1865
    광주북부경찰서      4148
    광주광산경찰서      3443
    Name: 2016총계, dtype: int64




```python
s7=df2017_crime["2017총계"]
s7
```




    관서명
    광주지방경찰청계    13949
    광주동부경찰서      1799
    광주서부경찰서      3638
    광주남부경찰서      1547
    광주북부경찰서      3945
    광주광산경찰서      3020
    Name: 2017총계, dtype: int64




```python
# 증감율 계산하기
s1=(s6-s5)/s5*100
s1
```




    관서명
    광주지방경찰청계   -18.130643
    광주동부경찰서    -12.186837
    광주서부경찰서    -17.542373
    광주남부경찰서    -11.903637
    광주북부경찰서    -24.112697
    광주광산경찰서    -17.473634
    dtype: float64




```python
s2=(s7-s6)/s6*100
s2
```




    관서명
    광주지방경찰청계    -9.516087
    광주동부경찰서    -13.007737
    광주서부경찰서     -6.526208
    광주남부경찰서    -17.050938
    광주북부경찰서     -4.893925
    광주광산경찰서    -12.285797
    dtype: float64




```python
# df.columns=[바꿀 컬럼 이름]
df_crime=pd.concat([s5,s1,s6,s2,s7],axis=1)
df_crime.columns=["2015총계","2015-2016 증감율","2016총계","2016-2017 증감율","2017총계"]
df_crime
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
      <th>2015총계</th>
      <th>2015-2016 증감율</th>
      <th>2016총계</th>
      <th>2016-2017 증감율</th>
      <th>2017총계</th>
    </tr>
    <tr>
      <th>관서명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>광주지방경찰청계</th>
      <td>18830</td>
      <td>-18.130643</td>
      <td>15416</td>
      <td>-9.516087</td>
      <td>13949</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>2355</td>
      <td>-12.186837</td>
      <td>2068</td>
      <td>-13.007737</td>
      <td>1799</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>4720</td>
      <td>-17.542373</td>
      <td>3892</td>
      <td>-6.526208</td>
      <td>3638</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>2117</td>
      <td>-11.903637</td>
      <td>1865</td>
      <td>-17.050938</td>
      <td>1547</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>5466</td>
      <td>-24.112697</td>
      <td>4148</td>
      <td>-4.893925</td>
      <td>3945</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>4172</td>
      <td>-17.473634</td>
      <td>3443</td>
      <td>-12.285797</td>
      <td>3020</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 시리즈 이름 바꾸기
s1.name="2015-2016 증감율"
s2.name="2016-2017 증감율"
df_crime
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
      <th>2015총계</th>
      <th>2015-2016 증감율</th>
      <th>2016총계</th>
      <th>2016-2017 증감율</th>
      <th>2017총계</th>
    </tr>
    <tr>
      <th>관서명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>광주지방경찰청계</th>
      <td>18830</td>
      <td>-18.130643</td>
      <td>15416</td>
      <td>-9.516087</td>
      <td>13949</td>
    </tr>
    <tr>
      <th>광주동부경찰서</th>
      <td>2355</td>
      <td>-12.186837</td>
      <td>2068</td>
      <td>-13.007737</td>
      <td>1799</td>
    </tr>
    <tr>
      <th>광주서부경찰서</th>
      <td>4720</td>
      <td>-17.542373</td>
      <td>3892</td>
      <td>-6.526208</td>
      <td>3638</td>
    </tr>
    <tr>
      <th>광주남부경찰서</th>
      <td>2117</td>
      <td>-11.903637</td>
      <td>1865</td>
      <td>-17.050938</td>
      <td>1547</td>
    </tr>
    <tr>
      <th>광주북부경찰서</th>
      <td>5466</td>
      <td>-24.112697</td>
      <td>4148</td>
      <td>-4.893925</td>
      <td>3945</td>
    </tr>
    <tr>
      <th>광주광산경찰서</th>
      <td>4172</td>
      <td>-17.473634</td>
      <td>3443</td>
      <td>-12.285797</td>
      <td>3020</td>
    </tr>
  </tbody>
</table>
</div>


