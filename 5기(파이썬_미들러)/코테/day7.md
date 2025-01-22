# ▶️ 숫바꽃지에서 제출: BFS 통해 결정

## ✨ **Problem Statement**

- 수빈이는 동생과 숫바꽃지를 하고 있습니다.
  
  현재의 수빈이 위치 \(\(N\)\) 및 동생의 위치 \(\(K\)\) 가 주어지면, 수빈이가 동생을 밝을 수 있는 것은 현재 테스트과 항상적으로 반해 확인해야 합니다.

**Constraints**
- \( 0 \leq N, K \leq 100,000 \)
- 수빈이가 \(X\) 위치에서 이동할 것:
  1. \( X - 1 \)
  2. \( X + 1 \)
  3. \( 2 \times X \)

---

## 해당 문제에 대한 나의 풀이 

### **1. My Initial Solution --> 오답**

```python
import sys
from collections import deque

# 텍스트 입력 처리및 공유합니다.
data = sys.stdin.read().strip().split()

mover, basis_point = map(int, data)
graph = {}
visited = set()  # 방문한 노드 검증

buffered_point = basis_point * 2

# 노드 결차 생성하는 함수
def tree_gen_iterative(start_point, graph, exception):
    stack = [start_point]

    while stack:
        point = stack.pop()

        if point <= 0 or point in visited:
            continue

        visited.add(point)

        if point not in graph:
            graph[point] = []

        if point % 2 == 0:
            new_node_minus = point - 1
            new_node_plus = point + 1
            new_node_div = point // 2

            for new_node in [new_node_minus, new_node_plus, new_node_div]:
                if new_node > 0 and new_node <= exception and new_node not in visited:
                    graph[point].append(new_node)
                    if new_node not in graph:
                        graph[new_node] = []
                    graph[new_node].append(point)
                    stack.append(new_node)
        else:
            new_node_minus = point - 1
            new_node_plus = point + 1

            for new_node in [new_node_minus, new_node_plus]:
                if new_node > 0 and new_node <= exception and new_node not in visited:
                    graph[point].append(new_node)
                    if new_node not in graph:
                        graph[new_node] = []
                    graph[new_node].append(point)
                    stack.append(new_node)

    return graph

result_graph = tree_gen_iterative(buffered_point, graph, buffered_point)

def shortest_path(tree, start, target):
    queue = deque([(start, [start])])
    visited = set()

    while queue:
        current, path = queue.popleft()

        if current == target:
            return path

        visited.add(current)

        for neighbor in tree[current]:
            if neighbor not in visited:
                queue.append((neighbor, path + [neighbor]))

    return None

answer_path = shortest_path(result_graph, basis_point, mover)

if answer_path:
    answer = len(answer_path) - 1
else:
    answer = -1

print(answer)
```

---



### **2. 최종 최적화된 코드**

**💡 BFS Implementation**
```python
from collections import deque

def find_min_time(n, k):
    if n == k:
        return 0

    max_range = 100000
    visited = [False] * (max_range + 1) # 0번째 인덱스를 사용 안하고, 1부터 시작할 거라 +1
    queue = deque([(n, 0)]) # n은 시작지점, 0은 현재시간

    while queue:
        current, time = queue.popleft()

        for next_pos in (current - 1, current + 1, current * 2): # 현재 위치에서 이동가능한 모든 경로 / 해당 경로, 동일 시간
            if 0 <= next_pos <= max_range and not visited[next_pos]:
                if next_pos == k:
                    return time + 1

                visited[next_pos] = True
                queue.append((next_pos, time + 1))

# Example usage
n, k = 5, 17
print(find_min_time(n, k))  # Output: 4
```

## (설명) BFS와 최단 시간 보장 원리

## BFS의 탐색 방식
BFS(너비 우선 탐색)는 **모든 경로를 같은 깊이(단계)**에서 탐색한 뒤, 더 깊은 단계로 넘어갑니다.  
즉, BFS는 가장 먼저 도달한 경로가 최단 경로임을 보장합니다.

다음 코드를 살펴봅시다:
```python
if next_pos == k:
    return time + 1
```
이 부분이 최단 시간 보장의 핵심입니다. 이를 더 구체적으로 살펴보면:

### 1. BFS에서 큐를 사용하는 이유
BFS는 큐(deque)를 사용해 탐색합니다.  
큐는 FIFO(First In, First Out) 방식으로 작동하기 때문에, 먼저 삽입된 노드(경로)는 먼저 처리됩니다.

### 2. BFS 탐색 순서
현재 노드에서 이동 가능한 모든 경로를 계산하고, 동일한 시간 단계에서 탐색합니다.  
예를 들어:
- 첫 번째 단계에서 `n`에서 출발.
- 두 번째 단계에서 `n-1`, `n+1`, `2*n` 모두 탐색.
- 세 번째 단계에서 다시 각 노드의 가능한 이동 탐색.

### 3. 최단 시간 보장의 이유
BFS는 먼저 발견된 경로가 가장 짧은 경로임을 보장합니다.  
만약 `next_pos`가 목표 지점 `k`에 도달하면, 해당 단계의 `time + 1`을 반환합니다.  
더 깊은 단계로 넘어가기 전에 정답을 반환하기 때문에 최단 시간을 보장합니다.

---

## 예시로 이해하기
BFS를 층별로 탐색하는 구조로 생각하면 쉽게 이해할 수 있습니다.  
예를 들어, `n=5`, `k=17`이라면 탐색 과정은 다음과 같이 진행됩니다:

### 1단계: 시작 지점 5
- 가능한 이동: 4, 6, 10
- 큐: `[4, 6, 10]`

### 2단계: 4, 6, 10에서 탐색
- `4 → 3, 5(이미 방문), 8`
- `6 → 5(이미 방문), 7, 12`
- `10 → 9, 11, 20`
- 큐: `[3, 8, 7, 12, 9, 11, 20]`

### 3단계: 3, 8, 7, ...에서 탐색
- 만약 17에 도달하면 즉시 반환.

BFS는 각 단계에서 모든 노드를 탐색하고 가장 먼저 도달한 경로가 최단 경로이기 때문에 추가 탐색 없이 결과를 반환합니다.

---

## 왜 최단 시간이 보장될까?
1. BFS는 모든 경로를 같은 시간 단계에서 탐색하므로, 더 짧은 경로를 놓치지 않습니다.
2. 만약 `next_pos == k`라면, BFS가 해당 경로에 도달한 순간 최단 시간임을 보장합니다.
3. 더 깊은 단계(더 긴 시간)의 탐색을 진행하기 전에 결과를 반환합니다.

---

## 결론
이 코드에서 `return time + 1`은 `k`에 도달한 최초의 시간을 반환합니다.  
BFS의 탐색 순서 덕분에 이 값은 항상 최단 시간임을 보장합니다.

---

## ✨ **Key Concepts**

### **BFS (Breadth-First Search)**
- BFS is ideal for finding the shortest path in an unweighted graph.
- It explores all nodes at the current depth before moving on to the next level.

#### **Algorithm Steps**:
1. Initialize a queue with the starting node.
2. Use a `visited` structure to prevent revisiting nodes.
3. While the queue is not empty:
   - Dequeue the front node.
   - Explore all its neighbors.
   - If a neighbor is the target node, return the path or time.

### **Complexity Analysis**
- **Time Complexity**: \(O(V + E)\), where \(V\) is the number of vertices and \(E\) is the number of edges.
- **Space Complexity**: \(O(V)\) for the visited array and queue.

---

## 🌟 **Conclusion**

1. Initial graph generation using `tree_gen_iterative` was inefficient and led to higher time and space complexity.
2. The optimized BFS solution directly calculates the shortest path without pre-generating the graph.
3. BFS is a robust algorithm for solving shortest path problems in unweighted graphs like this one.

---
