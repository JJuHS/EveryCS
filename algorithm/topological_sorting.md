# 위상정렬 (Topological Sorting)

## 1. 개요

위상정렬(Topological Sorting)은 방향성 비순환 그래프(DAG, Directed Acyclic Graph)에서 노드들을 선형으로 정렬하는 방법이다.  
이 정렬은 그래프의 모든 간선 `(u, v)`에 대해 노드 `u`가 노드 `v`보다 앞에 오도록 정렬한다.  

위상정렬은 순서가 필요한 작업 스케줄링, 컴파일러의 의존성 분석, 데이터 처리 파이프라인 등 다양한 분야에서 활용된다.

## 2. 알고리즘의 원리

위상정렬은 두 가지 주요 알고리즘을 사용한다:  

1. **Kahn의 알고리즘 (BFS 기반)**: 진입 차수(indegree)를 기반으로 정렬한다.  
2. **깊이 우선 탐색 (DFS 기반)**: 각 노드를 방문한 후, 역순으로 정렬한다.  

위상정렬이 가능한 그래프는 **사이클이 없는 방향 그래프 (DAG)**여야 한다.  만약 사이클이 존재한다면 위상정렬을 수행할 수 없다.  

## 3. 위상정렬의 수행과정

1. **진입 차수가 0인 노드**를 찾는다.  
2. 이 노드를 결과 리스트에 추가하고, 해당 노드에서 나가는 간선을 제거한다.  
3. 새로운 진입 차수가 0인 노드를 찾고, 2번 과정을 반복한다.  
4. 모든 노드를 방문할 때까지 이 과정을 반복한다.  

그래프의 모든 노드를 방문하기 전에 진입 차수가 0인 노드가 없다면 **사이클이 존재**하는 것!  

## 4. 위상정렬의 유형

### 4-1. Kahn의 알고리즘 (BFS 기반)

Kahn의 알고리즘은 진입 차수가 0인 노드를 큐에 넣고, BFS 방식으로 노드를 처리하면서 위상정렬을 수행한다.  

#### 예시 문제
**문제**: 다음 그래프를 위상정렬하기

```
1 → 2 → 4
1 → 3 → 4
```

#### 예시 코드
```python
from collections import deque

# 그래프 정의
n = 4
graph = {1: [2, 3], 2: [4], 3: [4], 4: []}
indegree = [0] * (n + 1)

# 진입 차수 계산
for nodes in graph.values():
    for node in nodes:
        indegree[node] += 1

# 진입 차수가 0인 노드를 큐에 삽입
queue = deque([i for i in range(1, n + 1) if indegree[i] == 0])
result = []

# Kahn's 알고리즘 수행
while queue:
    current = queue.popleft()
    result.append(current)
    for neighbor in graph[current]:
        indegree[neighbor] -= 1
        if indegree[neighbor] == 0:
            queue.append(neighbor)

print("위상정렬 결과:", result)
```

#### 출력
```
위상정렬 결과: [1, 2, 3, 4]
```

### 4-2. 깊이 우선 탐색 (DFS 기반)

DFS 기반의 위상정렬은 노드를 방문한 후, 재귀 호출이 끝날 때 해당 노드를 스택에 넣어 정렬을 수행한다.

#### 예시 문제
**문제**: 다음 그래프를 위상정렬하기

```
5 → 2
5 → 0
4 → 0
4 → 1
2 → 3
3 → 1
```

#### 예시 코드
```python
from collections import defaultdict

# 그래프 정의
graph = defaultdict(list)
graph[5].extend([2, 0])
graph[4].extend([0, 1])
graph[2].append(3)
graph[3].append(1)

visited = set()
stack = []

def dfs(v):
    visited.add(v)
    for neighbor in graph[v]:
        if neighbor not in visited:
            dfs(neighbor)
    stack.append(v)

for node in range(6):
    if node not in visited:
        dfs(node)

print("위상정렬 결과:", stack[::-1])
```

#### 출력
```
위상정렬 결과: [5, 4, 2, 3, 1, 0]
```

### 4-3. Parallel 알고리즘

Parallel 위상정렬은 여러 노드를 동시에 처리하는 알고리즘으로, 주로 병렬 컴퓨팅 환경에서 사용된다.  
이 방법은 Kahn의 알고리즘을 확장하여 여러 작업을 병렬로 처리한다.

#### 특징
1. **여러 진입 차수가 0인 노드**를 동시에 처리한다.  
2. **병렬 처리**를 통해 수행 시간을 줄일 수 있다.  

이 알고리즘은 **대규모 작업 스케줄링**이나 **병렬 컴파일러**에서 사용된다.  

## 5. 최단 거리 찾기에서 적용

위상정렬은 DAG에서 최단 거리 문제를 해결하는 데 사용될 수 있다.  

### 과정
1. 그래프를 위상정렬한다.  
2. 시작 노드로부터의 거리를 기록한다.  
3. 위상정렬 순서대로 각 노드를 확인하며, 인접 노드로의 거리를 갱신한다.  

#### 예시 코드
```python
from collections import deque

# 그래프 정의
n = 6
graph = {0: [(1, 5), (2, 3)], 1: [(3, 6)], 2: [(3, 7), (4, 4)], 3: [(5, 2)], 4: [(5, 1)], 5: []}
indegree = [0] * n

# 진입 차수 계산
for u in graph:
    for v, _ in graph[u]:
        indegree[v] += 1

# 위상정렬 수행
queue = deque([i for i in range(n) if indegree[i] == 0])
distance = [0] * n

while queue:
    current = queue.popleft()
    for neighbor, weight in graph[current]:
        indegree[neighbor] -= 1
        distance[neighbor] = max(distance[neighbor], distance[current] + weight)
        if indegree[neighbor] == 0:
            queue.append(neighbor)

print("최단 거리:", distance)
```

#### 출력
```
최단 거리: [0, 5, 3, 12, 7, 14]
```

## 6. 특이한 점

- **사이클 감지**: 위상정렬 중 진입 차수가 0인 노드가 없으면 사이클이 존재한다.
- **유일한 정렬이 아닐 수 있음**: DAG에 대해 위상정렬 결과는 유일하지 않을 수 있다.
