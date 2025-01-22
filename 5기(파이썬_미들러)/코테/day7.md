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
    visited = [False] * (max_range + 1)
    queue = deque([(n, 0)])

    while queue:
        current, time = queue.popleft()

        for next_pos in (current - 1, current + 1, current * 2):
            if 0 <= next_pos <= max_range and not visited[next_pos]:
                if next_pos == k:
                    return time + 1

                visited[next_pos] = True
                queue.append((next_pos, time + 1))

# Example usage
n, k = 5, 17
print(find_min_time(n, k))  # Output: 4
```

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
