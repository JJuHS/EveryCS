## 최소신장트리, 최소스패닝트리 - Minimum Spanning Tree

### 개요

신장트리란 무엇일까?

신장트리(Spanning Tree)란 - **그래프에서 모든 정점을 포함하며, 모든 정점에 대한 최소한의 연결만을 가진 그래프**이다.

그래프라면서 왜 트리?? - 한 곳으로 도달하는 연결이 두 곳이상이 되는 경우에는, 즉, 사이클이 존재하는 경우, 최소한의 연결이라 볼 수 없기 때문에, 모든 곳에서 한 곳으로 이동하는 경우는 무조건 한 가지 경우, 즉 트리의 형태로 존재하게 된다.

최소신장트리? - 이러한 신장트리 중에서 간선의 가중치들의 합이 가장 작은 트리를 **최소신장트리(Minimum Spanning Tree)**라고 한다.

최소신장트리의 해법에는 두 가지, 프림알고리즘과 크루스칼알고리즘이 있다.

**알고리즘의 선택 기준**

보통의 경우 간선이 많은 경우에는 프림알고리즘, 간선이 적은 경우엔 크루스칼알고리즘을 사용하며, 둘 다 그리디 알고리즘의 일종이라고 볼 수 있다.

프림알고리즘은 O((V + E) log V)의 시간복잡도를 가지고, 크루스칼 알고리즘은 O(E log E)의 시간복잡도를 가진다.

~이 알고리즘과 해설을 보다보면, 다른 예외케이스가 있지 않을까 라고 생각할 수 있으나, 이 방법들이 최소신장트리를 찾을 수 있다는 매우 많은 증명이 존재한다.~

### 1\. 프림알고리즘

프림 알고리즘의 구현 아이디어는 **한 정점에서 시작하여 가장 작은 간선들만 찾아다니면서 트리를 완성하는 그리디 알고리즘**이다.

최소신장트리의 가장 중요한 점은 **우선순위큐**를 사용해야 한다는 것이다.

가장 가중치가 작게 형성된 트리를 먼저 적용시켜야 하기 때문이다.

1.  우선순위 큐를 사용하여 최소 가중치를 가진 간선을 선택하고
2.  방문하지 않은 정점을 선택하여 트리를 확장해간다.

```python
from heapq import heappop, heappush

def prim(start):
    global res
    q = []                    # 우선순위 큐
    MST = [0] * V             # 방문 여부 저장
    heappush(q, (0, start))   # 시작 노드 큐에 삽입 (가중치, 노드 번호)
    summation = 0             # MST의 가중치 합

    while q:
        weight, now = heappop(q)  # 현재 노드와 해당 노드로 가는 가중치를 꺼냄

        if MST[now]:              # 이미 방문한 노드
            continue

        MST[now] = 1              # 방문 처리
        summation += weight       # 가중치 합산

        # 인접 노드들을 확인하여 아직 MST에 포함되지 않은 노드들을 큐에 추가
        for next in range(V):
            if graph[now][next] and not MST[next]:  # 간선이 존재하고, 미방문일때
                heappush(q, (graph[now][next], next))  # 큐에 (가중치, 다음 노드 번호) 추가

    res = min(res, summation)  # 최소값 갱신


# 입력 받기
V, E = map(int, input().split())  # V: 정점의 수, E: 간선의 수
graph = [[0] * V for _ in range(V)]  # 인접 행렬로 그래프 초기화

for _ in range(E):
    s, e, w = map(int, input().split())  # s: 시작 정점, e: 끝 정점, w: 가중치
    graph[s][e] = w                      # 무방향 그래프이므로 양방향에 대해 가중치 저장
    graph[e][s] = w

res = 10 ** 6  # 최댓값으로 초기화

# 모든 정점에서 프림 알고리즘 실행
for i in range(V):
    prim(i)

print(res)  # 최소 신장 트리의 최소 가중치 합
```

### 2\. 크루스칼 알고리즘

크루스칼 알고리즘은 **가장 작은 가중치의 간선부터 선택하며, 사이클이 생기지 않도록 주의하면서 트리를 완성하는 그리디 알고리즘**이다.

이 알고리즘은 주로 유니온 파인드(Union-Find) 자료구조를 사용하여 사이클이 생기는지를 확인한다.

**구현단계**

1.  모든 간선을 가중치 기준으로 오름차순으로 정렬한다.
2.  정렬된 간선을 하나씩 선택하면서, 각 간선의 두 정점이 서로 다른 집합에 속해 있는지 확인한다.
3.  만약 두 정점이 다른 집합에 속해 있다면, 해당 간선을 선택하고 두 집합을 합친다.
4.  모든 간선에 대해 이 작업을 반복하며, 선택한 간선이 V−1개가 되면 최소 신장 트리가 완성된다.

```python
V, E = map(int, input().split())    # 정점V, 간선E
edges = []  # 간선 정보

for _ in range(V):
    s, e, w = map(int, input().split())  # 시작 정점(s), 끝 정점(e), 가중치(w)
    edges.append([s, e, w]) 

# 간선을 가중치(w)를 기준으로 오름차순 정렬
edges.sort(key=lambda x: x[2])

sum_weight = 0  # 최소 스패닝 트리의 총 가중치를
parents = [i for i in range(V)]  # 각 정점의 부모가 저장된 리스트, 초기에는 자기 자신을 부모로 설정

# 부모 찾기
def union_find(x):
    if parents[x] == x:  # 부모가 자기 자신인 경우
        return x
    # 경로 압축(Path Compression)을 수행하면서 부모 찾기
    parents[x] = union_find(parents[x])
    return parents[x]

# 두 정점을 하나의 집합으로 합치기
def union_set(x, y):
    # 각 정점의 부모를 찾습니다.
    x, y = union_find(x), union_find(y)
    # 부모가 같으면 이미 같은 집합
    if x == y:
        return
    # 부모가 다를 경우 작은 번호를 부모로 설정합니다.
    if x < y:
        parents[y] = x
    else:
        parents[x] = y

# 모든 간선에 대하여 MST 확인하기.
for s, e, w in edges:
    # 두 정점이 이미 같은 집합에 속해 있으면(사이클이 발생하면) skip
    if union_find(s) == union_find(e):
        continue
    # 두 정점을 같은 집합으로 합치기
    union_set(s, e)
    # 가중치 최신화
    sum_weight += w

# 최소 스패닝 트리의 총 가중치
print(sum_weight)
```

### 3\. 추천 문제

G4 : [1197.최소 스패닝 트리](https://www.acmicpc.net/problem/1197), [1922.네트워크 연결](https://www.acmicpc.net/problem/1922), [1647\. 도시 분할 계획](https://www.acmicpc.net/problem/1647), [6497.전력난](https://www.acmicpc.net/problem/6497)

G3 : [4386.별자리 만들기](https://www.acmicpc.net/problem/4386), [1774.우주신과의 교감](https://www.acmicpc.net/problem/1774), [14621.나만 안되는 연애](https://www.acmicpc.net/problem/14621)