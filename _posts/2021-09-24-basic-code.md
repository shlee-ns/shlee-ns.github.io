---
title:  "Pandas와 Numpy 기본적인 코드 정리"
excerpt: "외상성 뇌손상 환자의 조기사망 예측 모델을 만들 때 사용했던 Pandas와 Numpy 코드들을 정리했다."

categories:
  - Blog
tags:
  - Blog
---


외상성 뇌손상 환자의 조기사망을 예측하는 머신 러닝 모델을 만들었다.  

시작하기 전에는 사실 cikit-learn에서 classification 모델들을 불러서 자료들만 넣으면 예측 모델이 뚝딱 만들어질 줄 알았는데 생각보다 신경 쓸 것들이 꽤 많았다. 

특히 **데이터 전처리의 중요성**을 체감하면서 동시에 Pandas와 Numpy의 소중함을 알게 되었다.

이번 포스팅에서는 외상성 뇌손상 환자의 조기사망 예측 모델을 만들 때 사용했던 **Pandas**와 **Numpy** 코드들을 정리하고자 한다.

---

# 1. Pandas

### 1. CSV 파일 불러오기  
<br>

```python
df = pd.read_csv('data.csv') 
```
* `' '`에 파일 경로를 입력한다  

* df는 임의로 정한 변수이므로 변경 가능하다.
  
<br>

### 2. 데이터 확인
<br>

```python
df.head()
df.describe()
df.info()
df.shape
```

### 3. 데이터 프레임 칼럼 확인, 이름 바꾸기
<br>

```python
df.columns
df.columns = ['col', 'col', 'col']
df.rename(columns={'Before':'After'})
```

### 3. 데이터 프레임 특정 칼럼(열) 1개 선택
<br>

```python
y= df['outcome']
```

### 4. 데이터 프레임에서 칼럼(열) 여러개 선택
<br>

```python
num = df.loc[:, ['age', 'INR', 'glucose', 'MLS', 'SDH']]
```

### 5. 칼럼들 결합하기
<br>

``` python
X = pd.concat([num, cat], axis=1)
```

* `axis=0` 으로 설정하면 행 결합

<br>

### 6. 칼럼 삭제 
<br>

```python
df_1 = df.drop(['INR'], axis = 1, inplace=True)
```
* `inplace=True`로 지정하면 df 에서 완전 삭제
* `inplace=False`로 지정하면 df_1 에서만 `'age'` 칼럼이 없고 df 에는 남아 있다.

<br>

### 7. 칼럼 만들기
<br>

```python
df['instant'] = 1
```

* `'instant'`라는 새로운 칼럼을 만들고 모든 행에 1이라는 값을 부여

<br>

### 8. 칼럼 값 연산
<br>

```python
df.loc[df['age'] < 0, 'age'] = 0
df.loc[df['basal_cistern'] == 1, 'basal_cistern'] = 0
```
* `'age'` 칼럼에서 0 보다 작은 값이 있을 때 `'age'` 값을 0으로 바꾸라는 의미
* `'basal_cistern'` 의 값이 1일 때 `'basal_cistern'` 값을 0으로 바꾼다
* .loc 을 붙히지 않으면 해당 행 전체를 0으로 만들어버린다.

<br>

### 9. 칼럼 순서 바꾸기
<br>

```python
df = df.loc[:,['MLS', 'SDH', 'age', 'instant']]
```

### 10. 데이터 프레임을 Numpy 배열로 변환
<br>

```python
nd_df = df.values
```
<br>

---

# 2. Numpy

### 1. list 를 numpy로 변환
<br>

```python
beta = np.array[[1, 2, 3, 4]]
```

### 2. numpy 모양 변경
<br>

```python
beta = beta.reshape(4, 1)
```

### 3. 내적(행렬 곱)
<br>

```python
z = nd_df.dot(beta)
```
* nd_df가 m x n 행령, beta 가 n x m' 행렬이면 내적한 z 는 m x m' 행렬이 된다.
* nd_df의 열의 갯수와 beta의 행의 갯수가 같아야 한다.

