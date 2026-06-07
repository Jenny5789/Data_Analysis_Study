# 📐 NumPy Study Notes
>  NumPy 공식 문서를 따라가며 직접 작성한 학습 기록입니다.  
> A personal study log working through NumPy's official documentation.
 
---

## 📂 파일 구성 / File Structure
```
numpy/
├── The_basics.ipynb
├── Shape_Manipulation.ipynb
├── Copies_and_views.ipynb
├── Advanced_indexing.ipynb
└── Tricks_and_tips.ipynb
```

---

## 📋 목차 / Table of Contents
1. [The Basics](#1-the-basics)
2. [Shape Manipulation](#2-shape-manipulation)
3. [Copies and Views](#3-copies-and-views)
4. [Advanced Indexing and Index Tricks](#4-advanced-indexing-and-index-tricks)
5. [Tricks and Tips](#5-tricks-and-tips)

---

## 📝 내용 요약 / Summary

### 1. The Basics

#### ndarray 속성 / Attributes
| 속성 | 설명 | 예시 (3×4 배열) |
|:------:|:------:|:----------------:|
| `ndim` | 차원 수 (축의 개수) | `2` |
| `shape` | 각 차원의 크기 | `(3, 4)` |
| `size` | 전체 요소 수 | `12` |
| `dtype` | 요소의 자료형 | `int64` |
| `itemsize` | 요소 하나의 바이트 크기 | `8` |
| `data` | 실제 데이터가 저장된 메모리 버퍼 | — |

#### 배열 생성 / Array Creation
```python
np.array([2, 3, 4])                      # 리스트로 1차원 배열 생성
np.zeros((3, 4))                         # 0으로 초기화
np.ones((2, 3, 4), dtype=np.int16)       # 1로 초기화 (3차원)
np.empty((2, 3))                         # 초기화 없이 메모리 할당
np.arange(10, 30, 5)                     # 범위 배열 생성
np.linspace(0, 2, 9)                     # 균등 간격 배열 생성
np.fromfunction(f, (5, 4), dtype=int)    # 함수로 배열 생성
```

#### 기본 연산 / Basic Operations
```python
A * B            # 요소별 곱 (element-wise)
A @ B            # 행렬 곱 (matrix multiplication)
a.sum(axis=0)    # 열 방향 합계
a.min(axis=1)    # 행 방향 최솟값
a.cumsum(axis=1) # 행 방향 누적 합계
```
> 💡 **dtype 업캐스팅**: `int + float → float`, 반대 방향은 불가

#### 인덱싱 & 슬라이싱 / Indexing & Slicing
```python
b[2, 3]              # 스칼라 인덱싱
b[0:5, 1]            # 슬라이싱
c[1, ...]            # c[1, :, :] 와 동일 (Ellipsis)
c[..., 2]            # c[:, :, 2] 와 동일
for row in b:        # 행 단위 순회
for elem in b.flat:  # 요소 단위 순회
```

---

### 2. Shape Manipulation

#### 형태 변환 / Reshaping
```python
a.ravel()          # 1차원으로 평탄화 (view 반환)
a.reshape(6, 2)    # 새로운 형태로 변환 (원본 유지)
a.resize((2, 6))   # 원본 자체를 변환 (in-place)
a.reshape(3, -1)   # 열 수를 NumPy가 자동 계산
a.T                # 전치 (transpose)
```
> 💡 `reshape` vs `resize`: `reshape`은 새 배열 반환, `resize`는 원본 변경

#### 배열 결합 / Stacking
```python
np.vstack((a, b))         # 수직 결합 (행 방향)
np.hstack((a, b))         # 수평 결합 (열 방향)
np.column_stack((a, b))   # 1D 배열을 열로 결합 → 2D
np.r_[1:4, 0, 4]          # 범위와 값을 이어 붙여 1D 배열 생성
```
> 💡 1D 배열 기준: `hstack`은 이어 붙이기, `column_stack`은 열로 세우기

---

### 3. Copies and Views

#### 복사 종류 비교 / Copy Types

| 종류 | 방법 | 객체 동일 | 데이터 공유 | 형태 독립 |
|:------:|:------:|:----------:|:------------:|:----------:|
| No copy | `b = a` | ✅ | ✅ | ❌ |
| Shallow copy | `a.view()`, 슬라이싱 | ❌ | ✅ | ✅ |
| Deep copy | `a.copy()` | ❌ | ❌ | ✅ |

#### No copy
```python
b = a            # 새 객체 생성 없음, b is a → True
# Python은 가변 객체를 참조로 전달 → 함수 호출도 복사 없음
def f(x):
    print(id(x))
id(a) == id(f(a))    # True (동일 객체)
```

#### Shallow copy
```python
c = a.view()           # 다른 객체지만 데이터 공유
c is a                 # False
c.base is a            # True → c는 a의 데이터를 바라봄
c.flags.owndata        # False → 데이터 소유권 없음

c = c.reshape((2, 6))  # c의 형태만 변경, a.shape는 유지
c[0, 4] = 1234         # a의 데이터도 변경됨

# 슬라이싱은 Shallow copy를 반환
s = a[:, 1:3]
s[:] = 10              # a의 데이터도 변경됨
# 주의: s = 10 (변수 재할당) ≠ s[:] = 10 (데이터 직접 변경)
```

#### Deep copy
```python
d = a.copy()     # 완전히 독립된 새 배열
d is a           # False
d.base is a      # False → 데이터 공유 없음
d[0, 0] = 9999   # a에 영향 없음

# 메모리 절약 패턴: 큰 배열의 일부만 유지할 때
b = a[:100].copy()
del a            # a의 메모리 해제 가능
# b = a[:100] 만 했다면 a를 참조하므로 del a 후에도 a 메모리 유지됨
```

> 💡 슬라이싱(`a[:]`)은 Shallow copy를 반환하므로 값 변경 시 원본에 영향
>
> 💡 `c.base is a`로 Shallow copy 여부 확인 가능, `c.flags.owndata`로 데이터 소유권 확인 가능

---

### 4. Advanced Indexing and Index Tricks

#### 배열 인덱스로 인덱싱 / Indexing with Arrays of Indices
```python
a = np.arange(12)**2
i = np.array([1, 1, 3, 8, 5])
a[i]                         # 인덱스 배열로 선택 → [1, 1, 9, 64, 25]

j = np.array([[3, 4], [9, 7]])
a[j]                         # j의 shape 그대로 반환 → [[9, 16], [81, 49]]

# 다차원 배열에서 여러 축 동시 인덱싱
a = np.arange(12).reshape(3, 4)
i = np.array([[0, 1], [1, 2]])
j = np.array([[2, 1], [3, 3]])
a[i, j]      # → [[2, 5], [7, 11]]
a[i, 2]      # 두 번째 축은 스칼라 → [[2, 6], [6, 10]]
a[:, j]      # 첫 번째 축 전체 + j 인덱싱 → shape (3, 2, 2)

# 인덱스 배열을 tuple로 묶어야 올바르게 동작
l = (i, j)
a[l]         # a[i, j]와 동일 → [[2, 5], [7, 11]]
# a[np.array([i, j])] 는 에러 (3D 배열로 해석됨)

# 시계열에서 최댓값 인덱스 활용
ind = data.argmax(axis=0)              # 각 시리즈별 최댓값의 행 인덱스
time_max = time[ind]
data_max = data[ind, range(data.shape[1])]

# 인덱스 배열로 값 할당
a[[1, 3, 4]] = 0             # 지정 인덱스에 값 할당
a[[0, 0, 2]] = [1, 2, 3]     # 중복 인덱스는 마지막 값 유지 → [2, 1, 3, ...]
a[[0, 0, 2]] += 1            # 중복 인덱스에 += 는 1회만 적용
```
> 💡 인덱스 배열이 여러 축에 걸칠 경우, 반드시 **같은 shape**이어야 함

#### 불리언 배열로 인덱싱 / Indexing with Boolean Arrays
```python
a = np.arange(12).reshape(3, 4)
b = a > 4
a[b]         # 조건 True인 요소만 1D로 반환 → [5, 6, 7, 8, 9, 10, 11]
a[b] = 0     # 조건 True인 요소에 값 할당

# 1D 불리언 배열로 행/열 선택
b1 = np.array([False, True, True])           # 행 선택
b2 = np.array([True, False, True, False])    # 열 선택
a[b1, :]     # 2, 3행 선택
a[:, b2]     # 1, 3열 선택
a[b1, b2]    # 각 True 위치 쌍 → [4, 10]
```
> 💡 불리언 배열의 길이는 해당 축의 크기와 **일치**해야 함

#### ix_() 함수 / The ix_() Function
```python
a = np.array([2, 3, 4, 5])
b = np.array([8, 5, 4])
c = np.array([5, 4, 6, 8, 3])
ax, bx, cx = np.ix_(a, b, c)
# ax.shape → (4,1,1), bx.shape → (1,3,1), cx.shape → (1,1,5)

result = ax + bx * cx        # Broadcasting으로 (4,3,5) 결과 배열
result[3, 2, 4]              # → 17
a[3] + b[2] * c[4]           # → 17 (동일)
```
> 💡 `np.ix_()`는 Broadcasting을 활용해 중간 배열 생성 없이 벡터 조합 계산 가능

---

### 5. Tricks and Tips

#### 자동 reshape / "Automatic" Reshaping
```python
a = np.arange(30)
b = a.reshape((2, -1, 3))    # -1은 NumPy가 자동 계산 → (2, 5, 3)
```

#### 벡터 스태킹 / Vector Stacking
```python
x = np.arange(0, 10, 2)
y = np.arange(5)
np.vstack([x, y])    # 수직 결합 → shape (2, 5)
np.hstack([x, y])    # 수평 결합 → shape (10,)
```

#### 히스토그램 / Histograms
```python
# matplotlib: 직접 플롯
plt.hist(v, bins=50, density=True)

# NumPy: 데이터만 계산 (플롯 없음)
n, bins = np.histogram(v, bins=50, density=True)
plt.plot(.5 * (bins[1:] + bins[:-1]), n)
```
> 💡 `np.histogram`은 데이터만 반환, `plt.hist`는 자동 플롯까지 수행

---

## 🛠️ 실행 환경 / Environment
| 항목 | 버전 |
|:------:|:------:|
| Python | 3.13.9 |
| NumPy | 2.3.5 |
| 실행 환경 | Jupyter Notebook |

---

## 📖 참고 / Reference
- [NumPy 공식 문서 — Quickstart](https://numpy.org/doc/stable/user/quickstart.html)
