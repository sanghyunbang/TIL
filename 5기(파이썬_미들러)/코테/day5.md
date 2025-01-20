# 🧪 두 용액 문제 정리

## 📜 문제 설명 (Problem Description)

### 문제 배경
- KOI 부설 과학연구소에서는 산성(음수 특성값)과 알칼리성(양수 특성값) 용액을 혼합해 **특성값의 합이 0에 가장 가까운 조합**을 찾는 연구를 진행 중입니다.
- 용액의 특성값은 정수로 주어지며, 산성은 음수, 알칼리성은 양수로 구분됩니다.
- 혼합된 용액의 특성값은 두 용액의 특성값 합으로 계산됩니다.

### 요구사항
1. 두 용액을 혼합한 경우, 특성값 합이 0에 가장 가까운 조합을 찾습니다.
2. 출력은 항상 **오름차순**이어야 합니다.
3. 입력 조건:
   - \( N \) (용액 개수): \( 2 \leq N \leq 100,000 \)
   - 특성값 범위: \(-1,000,000,000 \leq x \leq 1,000,000,000\)
   - 특성값은 모두 **서로 다른** 것입니다.

---

## 💡 문제 해결 방법 (Solution Approach)

### 1⃣ **투 포인터 방식 (Two Pointer Method)**

#### **개념**
- 투 포인터(Two Pointer)는 정렬된 리스트에서 두 포인터(`left`와 `right`)를 양 끝에 배치하고, 특정 조건에 따라 포인터를 이동하면서 최적의 해를 찾는 알고리즘입니다.
- 시간 복잡도가 \( O(N \log N) \)로 효율적이며, 이 문제와 같은 정렬된 리스트 기반의 최적화 문제에 적합합니다.

#### **방식**
1. 리스트를 정렬합니다.
2. 포인터 `left`는 리스트의 첫 번째 요소, `right`는 마지막 요소를 검사합니다.
3. 두 포인터가 검사하는 합을 계산하여 다음을 수행합니다:
   - 합이 0보다 작으면: `left`를 오른쪽으로 이동 (더 큰 값을 탐색).
   - 합이 0보다 크면: `right`를 왼쪽으로 이동 (더 작은 값을 탐색).
   - 합이 정확히 0이면 현재 값을 최적해로 반환합니다.
4. 합의 절대값이 가장 작은 조합을 계속 갱신합니다.

#### **장점**
- 모든 조합을 한 번의 루프에서 효율적으로 탐색 가능합니다.
- 코드가 간결하여 이해하기 쉽습니다.

---

### 2⃣ **이분 탐색 방식 (Binary Search Method)**

#### **개념**
- 음수와 양수 리스트를 분리하고, 음수 리스트의 각 값에 대해 양수 리스트에서 가장 가까운 값을 **이분 탐색**으로 찾습니다.
- 이분 탐색은 특정 값에 대해 상한(`ceil`)과 하한(`floor`)을 빠르게 찾는 데 유용합니다.

#### **방식**
1. 리스트를 정렬하고 음수와 양수 리스트를 분리합니다.
2. 음수 리스트의 각 값에 대해 양수 리스트에서 이분 탐색으로 가장 가까운 값을 찾습니다:
   - `bisect_left`를 사용하여 삽입 위치를 찾습니다.
   - 해당 위치의 값과 바로 이전 값(경계값)을 빠르게 탐색하며 계속 비교합니다.
3. 같은 부호끼리의 조합(음수-음수, 양수-양수)도 해당합니다.

#### **장점**
- 특정 값과 가장 가까운 포인터를 찾는 데 유용합니다.
- 투 포인터 방식과 비슷한 효율적인 탐색을 제공합니다.

---

## 🔎 내가 실수했던 부분과 원인 (Mistakes in Original Code)

### **문제 1: 불필요한 음수/양수 분리 및 복잡한 계산**
- 기존 코드에서 음수와 양수 리스트를 분리한 뒤 각각의 조합을 다른 계산을 통해 처리하고, 이후 음수-양수 조합을 추가로 계산했습니다.
- 이 방식은 비효율적이며 코드 복잡성을 증가시켰습니다.

**실제 오류 예시:**
- 입력: `[-2, 4, -99, -1, 98]`
- 문제 상황:
  - 음수 리스트와 양수 리스트를 분리하여 `start_Minus_gap`과 `start_Plus_gap`을 계산했지만, 최적의 음수-양수 조합(예: `-99`와 `98`)이 고려되지 않음.
  - 결과적으로 최적 해를 놓칠 가능성이 존재.

---

### **문제 2: 이분 탐색의 경계값 처리 오류**
- 기존 코드에서 `min_index`와 `max_index` 경계를 잘못 처리하여 잘못된 결과가 발생할 가능성이 있었습니다.
- 특히 이분 탐색 후 `mid_index`가 리스트 경계에 도달했을 때, 인덱스를 초과하거나 비정확한 값과 비교하는 문제가 있었습니다.

**실제 오류 예시:**
- 입력: `[-1000000000, -1, 1, 1000000000]`
- 문제 상황:
  - 이분 탐색으로 음수 `-1`에 대해 양수 리스트의 `1`을 찾았으나, 경계값 검증 없이 계산하여 논리적 오류 발생.

---

## 🛠️ 해결 코드 (Refactored Solution)

### **투 포인터 방식**
```python
import sys

# 입력 처리
data = sys.stdin.read().strip().split("\n")
N = int(data[0])
liquid = list(map(int, data[1].split()))
liquid.sort()

# 투 포인터 초기화
left, right = 0, N - 1
best_gap = float('inf')
best_pair = (0, 0)

while left < right:
    mix = liquid[left] + liquid[right]

    # 최적값 갱신
    if abs(mix) < best_gap:
        best_gap = abs(mix)
        best_pair = (liquid[left], liquid[right])

    # 포인터 이동
    if mix < 0:
        left += 1
    elif mix > 0:
        right -= 1
    else:
        break

# 결과 출력
print(*best_pair)
```

### **이분 탐색 방식**
```python
import sys
from bisect import bisect_left

# 입력 처리
data = sys.stdin.read().strip().split("\n")
N = int(data[0])
liquid = list(map(int, data[1].split()))
liquid.sort()

# 음수와 양수 리스트 분리
negatives = [x for x in liquid if x < 0]
positives = [x for x in liquid if x > 0]

# 최적값 저장
best_gap = float('inf')
best_pair = (0, 0)

# 음수-양수 조합 탐색
for neg in negatives:
    idx = bisect_left(positives, -neg)

    for i in [idx - 1, idx]:
        if 0 <= i < len(positives):
            mix = neg + positives[i]
            if abs(mix) < best_gap:
                best_gap = abs(mix)
                best_pair = (neg, positives[i])

# 같은 부호끼리의 조합 탐색
for lst in [negatives, positives]:
    for i in range(len(lst) - 1):
        mix = lst[i] + lst[i + 1]
        if abs(mix) < best_gap:
            best_gap = abs(mix)
            best_pair = (lst[i], lst[i + 1])

# 결과 출력
print(*sorted(best_pair))
```

---

## 🚀 결론 (Conclusion)
- **투 포인터 방식**: 직관적이고 간결하며, 정렬된 리스트에서 모든 조합을 효율적으로 탐색 가능.
- **이분 탐색 방식**: 특정 값과 가장 가까운 요소를 찾는 데 유리하나, 경계값 처리와 같은 추가적인 복잡성 존재.

**내 실수에서 배운 점**:
1. 복잡한 로직을 단순화하면 문제를 더 효율적으로 해결할 수 있음.
2. 이분 탐색의 경계값 검증은 필수적.
3. 문제 요구사항(오름차순 출력 등)을 항상 명확히 이해하고 구현해야 함. 😊
