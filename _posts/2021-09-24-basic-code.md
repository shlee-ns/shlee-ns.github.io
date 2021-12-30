---
title:  "Pandas와 Numpy 기본적인 코드 정리"
excerpt: "외상성 뇌손상 환자의 조기사망 예측 모델을 만들 때 사용했던 Pandas와 Numpy 코드들을 정리했다."
toc: true
toc_sticky: true

categories:
  - ML
tags:
  - Blog
---


외상성 뇌손상 환자의 조기사망을 예측하는 머신 러닝 모델을 만들었다.  

시작하기 전에는 사실 scikit-learn에서 classification 모델들을 불러서 자료들만 넣으면, 예측 모델이 뚝딱 만들어지는 줄 알았다. 하지만 생각보다 신경 쓸 것들이 꽤 많았다. 

특히 **데이터 전처리의 중요성**을 체감하면서 동시에 Pandas와 Numpy의 소중함을 알게 되었다.

이번 포스팅에서는 외상성 뇌손상 환자의 조기사망 예측 모델을 만들 때 사용했던 **Pandas**와 **Numpy** 코드들을 정리하고자 한다.

---

## 1. Pandas

### 1. CSV 파일 불러오기  
```python
df = pd.read_csv('data.csv') 
```
* `' '`에 파일 경로를 입력한다  

* df는 임의로 정한 변수이므로 변경 가능하다.


### 2. 데이터 확인
```python
df.head()
df.describe()
df.info()
df.shape
```

### 3. 칼럼 이름 확인, 바꾸기

```python
df.columns
df.columns = ['col', 'col', 'col']
df.rename(columns={'Before':'After'})
```

### 4. 칼럼 1개 선택
```python
y= df['outcome']
```

### 5. 칼럼 여러개 선택
```python
num = df.loc[:, ['age', 'INR', 'glucose', 'MLS', 'SDH']]
```

### 6. 칼럼 결합
``` python
X = pd.concat([num, cat], axis=1)
```

* `axis=0` 으로 설정하면 행 결합

### 7. 칼럼 삭제 
```python
df_1 = df.drop(['INR'], axis = 1, inplace=True)
```
* `inplace=True`로 지정하면 df 에서 완전 삭제
* `inplace=False`로 지정하면 df_1 에서만 `'age'` 칼럼이 없고 df 에는 남아 있다.

### 8. 칼럼 만들기
```python
df['instant'] = 1
```

* `'instant'`라는 새로운 칼럼을 만들고 모든 행에 1이라는 값을 부여

### 9. 칼럼 연산
```python
df.loc[df['age'] < 0, 'age'] = 0
df.loc[df['basal_cistern'] == 1, 'basal_cistern'] = 0
```
* `'age'` 칼럼에서 0 보다 작은 값이 있을 때 `'age'` 값을 0으로 바꾸라는 의미
* `'basal_cistern'` 의 값이 1일 때 `'basal_cistern'` 값을 0으로 바꾼다
* .loc 을 붙히지 않으면 해당 행 전체를 0으로 만들어버린다.

### 10. 칼럼 순서 바꾸기
```python
df = df.loc[:,['MLS', 'SDH', 'age', 'instant']]
```

### 11. Numpy 배열로 변환
```python
nd_df = df.values
```

---

## 2. Numpy

### 1. list 를 numpy로 변환
```python
beta = np.array[[1, 2, 3, 4]]
```

### 2. numpy 모양 변경
```python
beta = beta.reshape(4, 1)
```

### 3. 내적
```python
z = nd_df.dot(beta)
```
* nd_df가 m x n 행령, beta 가 n x m' 행렬이면 내적한 z 는 m x m' 행렬이 된다.
* nd_df의 열의 갯수와 beta의 행의 갯수가 같아야 한다.

### 4. Arrane

```python
np.arange(2, 10, 3)
```

numpy 모듈의 arange 함수는 반열린구간 [start, stop) 에서 step 의 크기만큼 일정하게 떨어져 있는 숫자들을 array 형태로 반환해 주는 함수다.

출처: https://codepractice.tistory.com/88 [코딩 연습]