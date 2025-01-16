# 기타 레슨 문제 풀이 정리

## 📝 문제 설명
- **목표**: 주어진 강의 순서를 유지하면서, 강의들을 M개의 블루레이에 나눠 담고, **블루레이 크기의 최솟값**을 구하라.
- **조건**:
  1. 각 블루레이의 크기는 동일해야 한다.
  2. 블루레이 개수는 최대 M개.
  3. 강의 순서를 바꿀 수 없다.

### 입력 형식
1. 첫 줄: 강의의 개수 `N` (1 ≤ N ≤ 100,000)과 블루레이 개수 `M` (1 ≤ M ≤ N).
2. 둘째 줄: 강의 길이를 나타내는 `N`개의 자연수 (각 값 ≤ 10,000).

### 출력 형식
- 가능한 **블루레이 크기 중 최솟값**.

### 예제 입력/출력
#### 입력:
```plaintext
9 3
1 2 3 4 5 6 7 8 9
```
#### 출력:
```plaintext
17
```

---

## 🔑 풀이 핵심 개념

### 1. **이분 탐색 (Binary Search)**
- **범위 설정**:
  - 최소 크기: 가장 긴 강의의 길이 (`max(강의 길이)`).
  - 최대 크기: 모든 강의 길이의 합 (`sum(강의 길이)`).
- **탐색 목표**:
  - 가능한 블루레이 크기 중 최소값을 탐색.

### 2. **구간 나누기 (Greedy Partitioning)**
- 현재 블루레이의 크기를 `X`로 가정하고, 강의 순서를 유지하면서 블루레이에 강의를 배치.
- **규칙**:
  - 현재 블루레이 용량이 `X`를 초과하면 새로운 블루레이를 시작.
  - 블루레이 개수가 `M`을 초과하면 `X`는 불가능한 값.

---

## 📜 Python 코드
```python
# 입력 처리
n, m = map(int, input().split())
lectures = list(map(int, input().split()))

# 이분 탐색 범위 설정
min_value = max(lectures)
max_value = sum(lectures)
result = 0

# 이분 탐색
while min_value <= max_value:
    mid_value = (min_value + max_value) // 2
    
    # 구간 나누기
    current_sum = 0
    count = 1  # 블루레이 개수

    for lecture in lectures:
        if current_sum + lecture > mid_value:
            count += 1  # 새로운 블루레이 사용
            current_sum = lecture  # 현재 강의를 새로운 블루레이에 추가
        else:
            current_sum += lecture

    # 블루레이 개수가 조건을 만족하는지 확인
    if count <= m:
        result = mid_value  # 가능한 값 저장
        max_value = mid_value - 1
    else:
        min_value = mid_value + 1

print(result)
```

---

## 📚 풀이 과정 설명

### 1. **이분 탐색 범위 설정**
- 블루레이 크기의 가능한 범위를 설정:
  - 최소값: 가장 긴 강의의 길이 (`max(lectures)`)
  - 최대값: 모든 강의를 하나의 블루레이에 담은 경우 (`sum(lectures)`)

### 2. **이분 탐색 진행**
- `mid_value = (min_value + max_value) // 2`로 중간값 설정.
- `mid_value`를 기준으로 블루레이에 강의를 배치한 후, 다음을 확인:
  - 블루레이 개수가 `M` 이하인지.

### 3. **구간 나누기 로직**
- 강의를 순서대로 탐색하며, 현재 블루레이의 크기가 `mid_value`를 초과하면 새 블루레이를 사용.
- 최종 블루레이 개수를 계산하고, 이 값이 `M` 이하인지 판단.

### 4. **범위 조정**
- 블루레이 개수가 `M` 이하라면:
  - `result`를 업데이트하고, `max_value`를 줄임 (`max_value = mid_value - 1`).
- 블루레이 개수가 `M`을 초과하면:
  - `min_value`를 늘림 (`min_value = mid_value + 1`).

---

## 🧠 유의점 및 실수 방지

### 1. **인덱스 처리 실수**
- `current_sum + lecture > mid_value`에서 **조건을 잘못 설정**하면 논리가 틀어질 수 있습니다.

### 2. **최소/최대 범위 설정 실수**
- `min_value = max(lectures)`와 `max_value = sum(lectures)`를 반드시 올바르게 설정해야 합니다. 그렇지 않으면 탐색이 비효율적이거나 잘못된 결과를 도출할 수 있습니다.

### 3. **블루레이 개수 초과 여부 확인**
- 블루레이 개수 `count`를 정확히 계산해야 하며, 마지막 강의를 처리하지 않으면 논리가 틀릴 수 있습니다.

### 4. **이분 탐색 종료 조건**
- `while min_value <= max_value` 조건이 올바르게 설정되지 않으면 무한 루프에 빠질 수 있습니다.

---

## 📖 주요 개념 정리
### 이분 탐색
- **정의**: 정렬된 범위에서 값을 효율적으로 탐색하는 방법.
- **시간 복잡도**: \(O(\log(	ext{range}))\)

### 구간 나누기 (Partitioning)
- **정의**: 특정 기준을 기준으로 데이터를 여러 구간으로 나누는 방법.
- **적용 사례**: 블루레이 문제, 화물 적재 문제 등.

---

## 🔍 참고
- 시간 복잡도: \(O(N \cdot \log(	ext{sum(lectures)} - 	ext{max(lectures)}))\)
- Greedy와 Binary Search를 결합한 효율적인 탐색 방법.
