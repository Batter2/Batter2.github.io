---
layout: single
title:  "Python_numpy"
categories: kaggle
toc: true
---

# numpy의 특징
- 파이썬 자료형인 list와 (비슷한) 형태를 띄고있다.
- 빠르고 효율적인 산술연산을 제공하는 n차원 배열을 제공한다.
- 반복문 없이 전체 데이터 배열 연산이 가능하다.


```python
# alias : 라이브러리 이름을 불러올 때 별칭을 부여한다.
# 스크립트 파일 실행 후 import 실행 해주어야함!!!
# numpy를 불러오는 과정

import numpy as np
```

### numpy.ndarray 클래스
- 동일한 자료형을 가지는 값들이 배열 형태로 존재한다.
ex) [1,2,"문자"] : 불가능
- N차원 형태로 구성이 가능하다.
- 각 값들의 양의 정수로 index가 부여되어 있다.
- ndarray를 줄여서 array라고 표현한다.
- numpy에서 차원(dimension)을 rank, axis라고 부르기도 한다.


```python
# 리스트 생성

list1=[1,2,3,4,5]
list1
```




    [1, 2, 3, 4, 5]




```python
# array생성(1차원)
# 리스트를 먼저 생성해서 array화 시킨 것
# array화를 하는 이유 : for문 사용하지 않기 위함

arr=np.array(list1)
arr
```




    array([1, 2, 3, 4, 5])




```python
# array생성(1차원) 
# 직접 바로 생성

arr1=np.array([2,3,4,5,6])
arr1
```




    array([2, 3, 4, 5, 6])




```python
# array생성(2차원)

arr2=np.array([[1,2,3],[4,5,6]])
arr2
```




    array([[1, 2, 3],
           [4, 5, 6]])




```python
# 배열의 크기 확인하기
# 형식 : array이름.shape

arr1.shape
```




    (5,)




```python
# 콤마(,)를 기준으로 앞은행 뒤는 열 
# ex) (2,3) : 2행3열

arr2.shape
```




    (2, 3)




```python
# 배열의 전체 요소 개수 확인하기
# array이름.size

print(arr2)
print(arr2.size)
```

    [[1 2 3]
     [4 5 6]]
    6
    


```python
# 배열의 타입 확인

s="ㅇㅇ"
type(s)
```




    str




```python
# 배열의 타입 확인
# array이름.dtype

print(arr1)
print(arr1.dtype)
```

    [2 3 4 5 6]
    int32
    


```python
print(arr2)
print(arr2.dtype)
```

    [[1 2 3]
     [4 5 6]]
    int32
    


```python
# 배열의 차원 확인
# array이름.ndim

print(arr1.ndim)
print(arr2.ndim)
```

    1
    2
    


```python
# array 만들기(3차원)

arr3=np.array([[[1,2],[3,4]],[[5,6],[7,8]]])
arr3
```




    array([[[1, 2],
            [3, 4]],
    
           [[5, 6],
            [7, 8]]])




```python
# 개수,타입,차원, 크기

print(f"배열의 개수 : {arr3.size}")
print(f"배열의 타입 : {arr3.dtype}")
print(f"배열의 차원 : {arr3.ndim}")
print(f"배열의 크기 : {arr3.shape}")
```

    배열의 개수 : 8
    배열의 타입 : int32
    배열의 차원 : 3
    배열의 크기 : (2, 2, 2)
    


```python
# 특정한 값으로 배열 생성하기
# array이름.zeros() : 모든 값이 0인 채로 배열을 생성해주는 함수

arr_zeros = np.zeros((2,3))
arr_zeros
```




    array([[0., 0., 0.],
           [0., 0., 0.]])




```python
# 모든 값인 1인 채로 배열을 생성해주는 함수

arr_ones=np.ones((3,4))
arr_ones
```




    array([[1., 1., 1., 1.],
           [1., 1., 1., 1.],
           [1., 1., 1., 1.]])




```python
# 특정한 값을 배열 생성하기
# array이름.full((행,열),특정한 값)

arr_full=np.full((5,5),2)
arr_full
```




    array([[2, 2, 2, 2, 2],
           [2, 2, 2, 2, 2],
           [2, 2, 2, 2, 2],
           [2, 2, 2, 2, 2],
           [2, 2, 2, 2, 2]])




```python
# 1,2,3,4,5 ~ 50 담긴 리스트를 생성해보자

# 1. 비어있는 리스트(필통) 필요
list1=[]

# 2. 각 인덱스마다 접근해야하는 반복문을 사용
# 3. append() 함수 사용해서 차곡차곡 비어있는 필통에 담아줘야함
for i in range(1,51):
    list1.append(i)

# 4. 출력
print(list1)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50]
    


```python
# numpy를 활용해서 위와 같은 문제를 풀아보자

# 위에서 list와 for문을 활용해서 만들었던 문제를 array로 바꾸어줌
arr= np.array(list1)
arr
```




    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17,
           18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34,
           35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50])




```python
# numpy가 제공하는 arange()함수
# range()함수와 유사
# np.arange(시작할 값, 끝날 값[포함x], 증감자)

np.arange(1,51)
```




    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17,
           18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34,
           35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50])




```python
# numpy가 제공하는 랜덤 값 배열 생성해주는 기능
# np.random.rand(2,3)
# 값은 0부터 1사이의 랜덤한 값
# .rand(행,렬) -> 만들고 싶은 행렬

np.random.rand(2,3)
```




    array([[0.88751481, 0.12684337, 0.3144617 ],
           [0.82140428, 0.6534761 , 0.18643026]])




```python
# .randint(시작할값, 끝날값[포함x])

np.random.randint(1,11)
```




    6




```python
# 랜덤 값 배열 생성하기(다차원ver)
# size=(행,열)

np.random.randint(2,11,size=(2,3))
```




    array([[3, 8, 7],
           [2, 4, 9]])




```python
# 타입을 직접적으로 적용해서 배열 생성하는 방법
# dtype=np.타입

arr_type=np.array([1.2,2.3,3.4], dtype=np.int64)
arr_type
```




    array([1, 2, 3], dtype=int64)




```python
# 타입 변경하기
# .astype("변경할 타입")
# 즉시 적용 x
# 적용하려면 대입을 통해 재갱신 해주어야함!!!

arr_type=arr_type.astype("float64")
arr_type
```




    array([1., 2., 3.])




```python
# 리스트 연산

list1=[1,2,3]
list2=[4,5,6]
```


```python
list1 + list2
```




    [1, 2, 3, 4, 5, 6]




```python
# array연산(각 요소끼리 연산)

arr1=np.array([1,2,3])
arr2=np.array([4,5,6])
```


```python
arr1+arr2
```




    array([5, 7, 9])




```python
arr_a=np.array([[1,2,3],[4,5,6]])
arr_b=np.array([[7,8,9],[10,11,12]])

print(arr_a)
print("="*15)
print(arr_b)
```

    [[1 2 3]
     [4 5 6]]
    ===============
    [[ 7  8  9]
     [10 11 12]]
    


```python
arr_a+arr_b
```




    array([[ 8, 10, 12],
           [14, 16, 18]])




```python
arr_a+3
```




    array([[4, 5, 6],
           [7, 8, 9]])




```python
# array 인덱싱 (1차원 배열)

arr_1=np.array([0,1,2,3,4,5])
arr_1
```




    array([0, 1, 2, 3, 4, 5])




```python
arr_1[2]
```




    2




```python
# array 언덱싱 (2차원 배열)

arr_2=np.array([[1,2,3],[4,5,6]])
arr_2
```




    array([[1, 2, 3],
           [4, 5, 6]])




```python
# array 인덱싱 -> 행과 열에 접근 콤마(,)를 기준으로 잎은 행, 뒤는 열

arr_2[0,0]
```




    1




```python
# array 인덱싱 -> 인덱스에 인덱스를 접근해서 인덱싱

arr_2[0][0]
```




    1




```python
# 1차원 형태의 array 슬라이싱

arr1 = np.arange(10)
arr1
```




    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])




```python
# 슬라이싱한 범위에 원하는 데이터 한번에 넣기

arr1[3:8]=12
arr1
```




    array([ 0,  1,  2, 12, 12, 12, 12, 12,  8,  9])




```python
# .reshape(행,열) : 내가 원하는 배열형태로 재정의

arr2 = np.arange(50).reshape(5,10)
arr2
```




    array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9],
           [10, 11, 12, 13, 14, 15, 16, 17, 18, 19],
           [20, 21, 22, 23, 24, 25, 26, 27, 28, 29],
           [30, 31, 32, 33, 34, 35, 36, 37, 38, 39],
           [40, 41, 42, 43, 44, 45, 46, 47, 48, 49]])




```python
arr2[:2,:]
```




    array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9],
           [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]])




```python
arr2[2:4,5:8]
```




    array([[25, 26, 27],
           [35, 36, 37]])



### Boolean인덱싱(색인,필터링)
- 배열 안에서 "조건"을 만족하는 True값들만 추출해주는 인덱싱 방법


```python
# array 생성하기(1차원 배열)

score=np.array([80,75,55,96,30])
score
```




    array([80, 75, 55, 96, 30])




```python
# 80점 이상인 데이터만 추출

score>=80
```




    array([ True, False, False,  True, False])




```python
# 위에서 추출한 데이터(=80점 이상인 데이터)를 인덱싱

score[score>=80]
```




    array([80, 96])




```python
# 문자열 array 생성

name = np.array(["수환","세연","정민","강우"])
name
```




    array(['수환', '세연', '정민', '강우'], dtype='<U2')




```python
# boolean 타입의 array 생성

bol=np.array([False,True,False,True])
bol
```




    array([False,  True, False,  True])




```python
# boolean으로 걸러준 친구를 이용해서 인덱싱

name[bol]
```




    array(['세연', '강우'], dtype='<U2')




```python
name[name=="수환"]
```




    array(['수환'], dtype='<U2')




```python
# 2차원 array 생성

score=np.array([[60,60],[70,70],[80,80],[90,90]])
score
```




    array([[60, 60],
           [70, 70],
           [80, 80],
           [90, 90]])




```python
# name이라는 변수에서 "수환" 이라는 데이터가 있나?
# boolean 값만 추출된 상태

name=="수환"
```




    array([ True, False, False, False])




```python
# name 자체에서 boolean indexing 한 것

name[name=="수환"]
```




    array(['수환'], dtype='<U2')




```python
# score에서 name이 "세연"의 데이터가 궁금하다

score[name=="세연"]
```




    array([[70, 70]])




```python
# 1부터 9까지 랜덤한 array 배열 생성(2,5)

arr = np.random.randint(1,10,(2,5))
arr
```




    array([[5, 5, 8, 2, 8],
           [2, 5, 2, 4, 3]])




```python
# sum() 함수 사용
# array이름.sum() : 파이썬 내부에서 자체적으로 제공하는 sum() 함수
# numpy.sum(array이름) : numpy에서 제공하는 sum() 함수

print(arr.sum())
print(np.sum(arr))
```

    44
    44
    


```python
# mean() 함수 사용 : 평균 구하는 함수
# array이름.mean() : 파이썬 내부에서 자체적으로 제공하는 mean() 함수
# numpy.mean(array이름) : numpy에서 제공하는 mean() 함수

print(arr.mean())
print(np.mean(arr))
```

    4.4
    4.4
    


```python
arr1 = np.arange(1,6)
arr1
```




    array([1, 2, 3, 4, 5])




```python
# sqrt() 함수 사용 : 제곱근 구하는 함수
# numpy.sqrt(array이름) : numpy에서 제공하는 sqrt() 함수

print(np.sqrt(arr1))
```

    [1.         1.41421356 1.73205081 2.         2.23606798]
    


```python
arr2=np.array([-1,-2,-3,-4,-5])
arr2
```




    array([-1, -2, -3, -4, -5])




```python
# abs() 함수 : 절댓값 구하는 함수
# numpy.abs(array이름) : numpy에서 제공하는 abs() 함수

print(np.abs(arr2))
```

    [1 2 3 4 5]
    

#  Universally 함수

### 단일 배열에 사용하는 함수()

![image.png](attachment:image.png)

### 서로 다른 배열 간에 사용하는 함수

![image.png](attachment:image.png)
