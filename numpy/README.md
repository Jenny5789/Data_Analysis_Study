# 📐 NumPy Study Notes
>  NumPy 공식 문서를 따라가며 직접 작성한 한국어 학습 기록입니다.  
> A personal study log working through NumPy's official documentation in Korean.
 
---

## 📂 파일 구성 / File Structure
```
numpy/
├── The_basics.ipynb
├── Shape_Manipulation.ipynb
└── Copies_and_views.ipynb
```

---

## 📋 목차 / Table of Contents
1. [The Basics](#1-the-basics)
2. [Shape Manipulation](#2-shape-manipulation)
3. [Copies and Views](#3-copies-and-views)

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

| 종류 | 방법 | 객체 동일 | 데이터 공유 | 형태 독립 |
|:------:|:------:|:----------:|:------------:|:----------:|
| No copy | `b = a` | ✅ | ✅ | ❌ |
| View (얕은 복사) | `a.view()`, 슬라이싱 | ❌ | ✅ | ✅ |
| Deep copy | `a.copy()` | ❌ | ❌ | ✅ |

```python
b = a              # 동일 객체, b is a → True
c = a.view()       # 다른 객체지만 데이터 공유, c.base is a → True
d = a.copy()       # 완전히 독립된 복사본

# 메모리 절약 패턴
b = a[:100].copy()
del a              # 큰 배열 해제, b는 유지
```
> 💡 슬라이싱(`a[:]`)은 View를 반환하므로 값 변경 시 원본에 영향

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
