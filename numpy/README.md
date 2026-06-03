# 📐 NumPy — The Basics

> NumPy 공식 문서의 **"The basics"** 섹션을 직접 실습하며 한국어 주석으로 정리한 노트입니다.  
> A hands-on study notebook following NumPy's official "The Basics" documentation, annotated in Korean.

---

## 📋 목차 / Table of Contents

1. [ndarray 속성](#1-ndarray-속성--ndarray-attributes)
2. [배열 생성](#2-배열-생성--array-creation)
3. [배열 출력](#3-배열-출력--printing-arrays)
4. [기본 연산](#4-기본-연산--basic-operations)
5. [유니버설 함수](#5-유니버설-함수--universal-functions)
6. [인덱싱 & 슬라이싱](#6-인덱싱--슬라이싱--indexing--slicing)

---

## 📂 파일 구성 / File Structure

```
numpy/
└── The_basics.ipynb    # 본 노트 / This notebook
```

---

## 🗂️ 내용 요약 / Summary

### 1. ndarray 속성 / ndarray Attributes

| 속성 | 설명 | 예시 (3×4 배열) |
|------|------|----------------|
| `ndim` | 차원 수 (축의 개수) | `2` |
| `shape` | 각 차원의 크기 | `(3, 4)` |
| `size` | 전체 요소 수 | `12` |
| `dtype` | 요소의 자료형 | `int64` |
| `itemsize` | 요소 하나의 바이트 크기 | `8` |
| `data` | 실제 데이터가 저장된 메모리 버퍼 | — |

---

### 2. 배열 생성 / Array Creation

```python
np.array([2, 3, 4])                      # 리스트로 1차원 배열 생성
np.array([(1.5, 2, 3), (4, 5, 6)])       # 2차원 배열 생성
np.zeros((3, 4))                         # 0으로 초기화
np.ones((2, 3, 4), dtype=np.int16)       # 1로 초기화 (3차원)
np.empty((2, 3))                         # 초기화 없이 메모리 할당
np.arange(10, 30, 5)                     # 범위 배열 생성
np.linspace(0, 2, 9)                     # 균등 간격 배열 생성
np.fromfunction(f, (5, 4), dtype=int)    # 함수로 배열 생성
```

---

### 3. 배열 출력 / Printing Arrays

- 요소 수가 1000개를 초과하면 중간이 `...`으로 생략됨
- `np.set_printoptions(threshold=sys.maxsize)` 로 전체 출력 가능

---

### 4. 기본 연산 / Basic Operations

```python
# 요소별 연산 (Element-wise)
a - b
b ** 2
10 * np.sin(a)
a < 35          # 불리언 배열 반환

# 행렬 연산 (Matrix)
A * B           # 요소별 곱 (element-wise product)
A @ B           # 행렬 곱 (matrix multiplication)
A.dot(B)        # A @ B 와 동일

# 집계 함수 (Aggregation)
a.sum()
a.min()
a.max()
b.sum(axis=0)       # 열 방향 합계
b.min(axis=1)       # 행 방향 최솟값
b.cumsum(axis=1)    # 행 방향 누적 합계
```

> 💡 **dtype 업캐스팅**: `int + float → float`, 반대 방향은 불가

---

### 5. 유니버설 함수 / Universal Functions

배열의 모든 요소에 수학 함수를 **요소별로** 적용하는 함수

```python
np.exp(B)       # e^B
np.sqrt(B)      # √B
np.add(B, C)    # B + C (요소별)
```

---

### 6. 인덱싱 & 슬라이싱 / Indexing & Slicing

```python
b[2, 3]         # 스칼라 인덱싱 — 단일 값 반환
b[0:5, 1]       # 슬라이싱 — 0~4행의 1열
b[:, 1]         # 모든 행의 1열
b[1:3, :]       # 1~2행 전체
b[-1]           # 마지막 행

# Ellipsis (...)
c[1, ...]       # c[1, :, :] 와 동일
c[..., 2]       # c[:, :, 2] 와 동일

# 반복 순회
for row in b:           # 행 단위 순회
for elem in b.flat:     # 요소 단위 순회 (1차원화)

# 역순 슬라이싱
a[::-1]         # 배열 뒤집기
```

---

## 🛠️ 실행 환경 / Environment

| 항목 | 버전 |
|------|------|
| Python | 3.13.9 |
| NumPy | — |
| 실행 환경 | Jupyter Notebook |

---

## 📖 참고 / Reference

- [NumPy 공식 문서 — The Basics](https://numpy.org/doc/stable/user/quickstart.html)
