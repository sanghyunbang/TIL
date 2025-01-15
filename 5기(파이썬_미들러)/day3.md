# 📝 선분 위의 점 문제 정리

## 1️⃣ 문제 설명
**선분 위의 점 (백준)**

- **제한사항:**
  - 시간 제한: 1초
  - 메모리 제한: 256MB
- **문제:**
  1차원 좌표상의 점 `N개`와 선분 `M개`가 주어질 때, 각각의 선분 위에 점이 몇 개 있는지 구하는 프로그램을 작성하시오.
- **입력 조건:**
  - 첫째 줄: 점의 개수 `N`과 선분의 개수 `M` (1 ≤ N, M ≤ 100,000)
  - 둘째 줄: 점의 좌표 (중복 없음)
  - 셋째 줄~: 선분의 시작점과 끝점
  - 모든 좌표는 1,000,000,000보다 작거나 같은 자연수
- **출력 조건:**
  - 각 선분마다 포함된 점의 개수 출력

### ✨ 예제 입력
```
5 5
1 3 10 20 30
1 10
20 60
3 30
2 15
4 8
```

### ✨ 예제 출력
```
3
2
4
2
0
```

---

## 2️⃣ 코드를 작성하며 했던 실수
```python
input_data = """5 5
1 3 10 20 30
1 10
20 60
3 30
2 15
4 8"""

data = input_data.strip().split("\n")

# 입력 처리
num_dot, num_lines = map(int, data[0].split())
dot_location = list(map(int, data[1].split()))

list_lines = []

for i in range(num_lines):
  int_list = list(map(int, data[2+i].split()))
  list_lines.append(int_list)

print(num_dot)
print(num_lines)
print(dot_location)
print(list_lines)

for i in range(num_lines):
  start_index = 0
  end_index = num_dot - 1

  while list_lines[i][0] > dot_location[start_index]:  # 시작부터
    start_index += 1
    if start_index > num_dot:
      break

  while list_lines[i][1] < dot_location[end_index]:  # 끝자리부터
    end_index -= 1
    if start_index < 0:
      break

  print(end_index - start_index + 1)
```

### 🛑 문제점
- `break` 사용이 **해당 반복문 멈출 수 있음**. 잘못된 위치에서 사용하면 로직 흐름에 영향을 줄 수 있음.
- 논리적인 오류로 인해 출력값이 예상과 다르게 나올 가능성.

---

## 3️⃣ 정답 코드
### (1) `bisect` 모듈 사용 코드
```python
import bisect

data = """5 5
1 3 10 20 30
1 10
20 60
3 30
2 15
4 8"""

data = data.strip().split("\n")

# 입력 처리
num_dot, num_lines = map(int, data[0].split())
dot_location = list(map(int, data[1].split()))
dot_location.sort()

list_lines = []
for i in range(num_lines):
    list_lines.append(list(map(int, data[2+i].split())))

for line in list_lines:
    start, end = line

    # 이진 탐색으로 범위 계산
    start_index = bisect.bisect_left(dot_location, start)
    end_index = bisect.bisect_right(dot_location, end) - 1

    if start_index <= end_index:
        print(end_index - start_index + 1)
    else:
        print(0)
```

### (2) 직접 구현한 이진 탐색 코드
```python
def lower_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left + right) // 2
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left

def upper_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left + right) // 2
        if arr[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left - 1

# 입력 처리
num_dot, num_lines = map(int, data[0].split())
dot_location = list(map(int, data[1].split()))
dot_location.sort()

list_lines = []
for i in range(num_lines):
    list_lines.append(list(map(int, data[2+i].split())))

for line in list_lines:
    start, end = line

    # 이진 탐색으로 범위 계산
    start_index = lower_bound(dot_location, start)
    end_index = upper_bound(dot_location, end)

    if start_index <= end_index:
        print(end_index - start_index + 1)
    else:
        print(0)
```

---

## 4️⃣ 교훈 및 요약
### 🔑 배운 점
- **`break` 사용 주의:**
  - `break`는 잘못 사용하면 예상치 못한 반복문 종료를 초래할 수 있음.
- **이진 탐색 활용:**
  - 정렬된 배열에서 값의 범위를 빠르게 찾는 데 유용.
  - `bisect` 모듈을 사용하거나 직접 구현 가능.

### 🛠️ 시간 복잡도 비교
| 방법                | 시간 복잡도               |
|---------------------|---------------------------|
| `while` 루프 사용  | `O(num_dot * num_lines)`  |
| `bisect` 사용      | `O(num_lines * log(num_dot))` |
| 직접 구현한 이진 탐색 | `O(num_lines * log(num_dot))` |

---

## 5️⃣ 결론
- 이진 탐색은 선형 탐색보다 훨씬 효율적입니다.
- `bisect` 모듈을 사용할 수 없을 때 직접 구현해도 동일한 효율을 얻을 수 있습니다.
