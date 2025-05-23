# 🚀 Hash Table vs List (해시 테이블 vs 리스트)

이 문서에서는 **Hash Table (Python의 `set`)**과 **List (리스트)**를 비교합니다. 저장 방식, 검색 메커니즘, 그리고 성능 차이에 대해 살펴보겠습니다. 또한 충돌 해결 방식도 코드 예제로 설명합니다.

---

## 1. 리스트(List)는 어떻게 저장하고 검색하나요? 📝

### 📦 저장 방식
- **리스트**는 배열 기반 구조로, 요소들이 메모리에 순차적으로 저장됩니다.
- 추가된 순서대로 데이터가 저장됩니다.

### 🔍 검색 방식
- 리스트는 **처음부터 끝까지 하나씩 확인**하면서 값을 검색합니다.
- 이러한 방식은 **선형 탐색(Linear Search)**이라고 하며, 시간 복잡도는 **O(N)**입니다.

### 코드 예제
```python
my_list = [3, 5, 7, 9]

# Searching for a value in a list
if 7 in my_list:
    print("Found")
else:
    print("Not Found")
```
- **최악의 경우**: 찾고자 하는 값이 리스트의 마지막에 있거나 없을 때, 리스트 전체를 확인해야 합니다.

---

## 2. 집합(Set, 해시 테이블)은 어떻게 저장하고 검색하나요? 🔑

### 📦 저장 방식
- `Set`은 **해시 테이블(Hash Table)**을 사용합니다. 각 요소는 **해시 함수(Hash Function)**를 통해 고유한 메모리 위치로 매핑됩니다.
- 해시 함수는 데이터를 입력받아 고유한 숫자(해시값)를 반환합니다.

### 🔍 검색 방식
- 값을 검색할 때도 해시 함수를 사용하여 **바로 해당 위치로 이동**합니다.
- 이 방식은 매우 빠르며, 평균 시간 복잡도는 **O(1)**입니다.

### 코드 예제
```python
my_set = {3, 5, 7, 9}

# Searching for a value in a set
if 7 in my_set:
    print("Found")
else:
    print("Not Found")
```
- 리스트와 달리, `set`은 하나씩 비교하지 않고, **해시값**을 계산하여 해당 위치를 직접 참조합니다.

---

## 3. 리스트와 집합 비교 ⚖️

| ✨ 특징               | 리스트(List)                 | 집합(Set)                   |
|----------------------|-----------------------------|-----------------------------|
| **저장 방식**          | 순차적 저장 (배열 기반)        | 해시 테이블 기반              |
| **검색 방식**          | 선형 탐색 (O(N))             | 직접 접근 (O(1) 평균)         |
| **순서 유지**          | 유지함                        | 유지하지 않음                  |
| **중복 허용**          | 허용함                        | 허용하지 않음                  |

---

## 4. 해시 충돌(Hash Collision)과 해결 방법 🛠️

### 🔄 해시 충돌이란?
- 두 개 이상의 데이터가 동일한 해시값을 가질 때 발생합니다. 예를 들어:
  - `hash(7) = 303`
  - `hash(17) = 303`
- 이러한 경우, 두 데이터는 동일한 슬롯에 저장됩니다.

### 💡 Python의 충돌 해결 방법: 개별 체이닝(Separate Chaining)
- 충돌이 발생하면, 해당 슬롯에서 **연결 리스트(Linked List)**를 사용하여 값을 저장합니다.
- 슬롯에 추가적인 공간을 확보하여, 동일한 해시값을 가진 데이터를 함께 저장합니다.

### 코드 예제 (충돌 해결)
```python
# Simplified hash function: value % 5
def simple_hash(value):
    return value % 5

# Example elements
values = [7, 12, 17]
hash_table = {i: [] for i in range(5)}

# Storing values in the hash table
for value in values:
    hash_value = simple_hash(value)
    hash_table[hash_value].append(value)

print("Hash Table:", hash_table)
```
출력 결과:
```
Hash Table: {0: [], 1: [], 2: [7, 12, 17], 3: [], 4: []}
```
위 예제에서, 모든 값이 동일한 슬롯(2번 슬롯)에 저장되며 연결 리스트로 관리됩니다.

---

## 5. 리스트와 집합 성능 비교 실험 🧪

다음 코드는 리스트와 집합에서 특정 값을 검색할 때의 성능 차이를 보여줍니다.

### 코드 예제
```python
import time

# 대량의 데이터 준비
n = 10**6
list_data = list(range(n))
set_data = set(range(n))

# 리스트 검색 시간 측정
start_time = time.time()
print(n in list_data)  # 리스트의 마지막 값을 검색
end_time = time.time()
print("List search time:", end_time - start_time)

# 집합 검색 시간 측정
start_time = time.time()
print(n in set_data)  # 집합의 마지막 값을 검색
end_time = time.time()
print("Set search time:", end_time - start_time)
```

### 결과 예시
```
False
List search time: 0.12
False
Set search time: 0.00001
```
- 리스트는 요소를 하나씩 확인하므로 시간이 더 오래 걸립니다.
- 집합은 해시값으로 직접 접근하므로 매우 빠릅니다.

---

## 6. 시간 초과 문제 해결 코드 🛠️

다음은 리스트 대신 집합을 사용하여 시간 초과 문제를 해결한 코드입니다:

### 코드 예제(문제풀이 코드)
```python
import sys

# 표준 입력 한꺼번에 읽기
input_data = sys.stdin.read().strip().split("\n")

# 입력 처리
T = int(input_data[0])
N_list = []
M_list = []
Watch_list = []
Remember_list = []

for _ in range(T):
    N_list.append(int(input_data[4 * _ + 1]))
    M_list.append(int(input_data[4 * _ + 3]))
    Watch_list.append(set(map(int, input_data[4 * _ + 2].split())))  # set으로 저장
    Remember_list.append(list(map(int, input_data[4 * _ + 4].split())))

# 답안 출력
for num in range(T):
    watch_set = Watch_list[num]
    remember_list = Remember_list[num]

    for value in remember_list:
        if value in watch_set:
            print(1)
        else:
            print(0)
```

### 주요 변경점
1. **`Watch_list`를 `set`으로 변환**:
   - 리스트 대신 집합을 사용하여 탐색 속도를 O(1)로 개선.
2. **탐색 과정 최적화**:
   - `value in watch_set`으로 빠르게 존재 여부 확인.

---

## 7. 핵심 요약 🎯

1. **성능**:
   - 리스트: 검색 시간 복잡도 O(N) (요소 수에 비례)
   - 집합: 검색 시간 복잡도 O(1) (평균적으로 상수 시간)

2. **사용 사례**:
   - 리스트를 사용:
     - 요소 순서를 유지해야 할 때
     - 중복 값이 필요할 때
   - 집합을 사용:
     - 빠른 검색이 필요할 때
     - 중복을 허용하지 않을 때

3. **충돌 해결**:
   - Python의 `set`은 **개별 체이닝** 방식을 사용하여 해시 충돌을 해결합니다.

4. **시간 초과 문제 해결**:
   - 집합을 사용해 탐색 속도를 최적화하여 큰 데이터셋에서도 효율적으로 동작합니다.

---

