# 다익스트라 (Dijkstra)

## 1. 개요

**다익스트라** 알고리즘은 가중치가 양수인 그래프에서 특정 시작점에서 다른 모든 정점까지의 최단 경로를 구하는 알고리즘이다.  

## 2. 알고리즘의 원리

1. 시작 정점을 기준으로 초기 거리를 설정하며, 시작 정점의 거리는 0으로 설정한다.
2. 방문하지 않은 정점 중에서 가장 가까운 정점을 선택한다.
3. 선택한 정점을 기준으로 인접한 정점들의 거리를 갱신한다.
4. 모든 정점이 반복될 때까지 반복한다.

## 3. 구현

### 3-1. 구현 1 - 인접 행렬 방식

#### 설명

1. 동작 방식
    - 그래프를 인접 행렬로 표현하며, 각 정점 간의 연결 여부와 가중치를 행렬로 나타냄
    - 방문 여부를 저장하는 배열과 현재까지의 최소 서리를 저장하는 배열 사용
    - 모든 정점을 순회하며, 최소 거리를 갱신

2. 시간복잡도
    - 시간복잡도는 O(V ^ 2)이다.

#### 코드

1. 파이썬 코드
```python
import sys

def dijkstra(graph, start):
    n = len(graph)
    visited = [False] * n
    distance = [sys.maxsize] * n

    distance[start] = 0

    for _ in range(n):
        # 방문하지 않은 노드 중 최단 거리 노드 선택
        min_distance = sys.maxsize
        min_index = -1
        for i in range(n):
            if not visited[i] and distance[i] < min_distance:
                min_distance = distance[i]
                min_index = i

        visited[min_index] = True

        # 선택한 노드를 기준으로 거리 갱신
        for j in range(n):
            if graph[min_index][j] != 0 and not visited[j]:
                new_distance = distance[min_index] + graph[min_index][j]
                if new_distance < distance[j]:
                    distance[j] = new_distance

    return distance

# 그래프 입력 (인접 행렬)
graph = [
    [0, 2, 0, 1, 0],
    [2, 0, 3, 2, 0],
    [0, 3, 0, 0, 1],
    [1, 2, 0, 0, 1],
    [0, 0, 1, 1, 0]
]

start_node = 0
distances = dijkstra(graph, start_node)
print(distances)
```

2. C++ 코드
```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

void dijkstra(vector<vector<int>> &graph, int start) {
    int n = graph.size();
    vector<int> distance(n, INT_MAX);
    vector<bool> visited(n, false);

    distance[start] = 0;

    for (int i = 0; i < n; i++) {
        int min_distance = INT_MAX, min_index = -1;

        for (int j = 0; j < n; j++) {
            if (!visited[j] && distance[j] < min_distance) {
                min_distance = distance[j];
                min_index = j;
            }
        }

        visited[min_index] = true;

        for (int j = 0; j < n; j++) {
            if (graph[min_index][j] != 0 && !visited[j]) {
                int new_distance = distance[min_index] + graph[min_index][j];
                if (new_distance < distance[j]) {
                    distance[j] = new_distance;
                }
            }
        }
    }

    cout << "최단 거리: ";
    for (int d : distance) {
        cout << d << " ";
    }
    cout << endl;
}

int main() {
    vector<vector<int>> graph = {
        {0, 2, 0, 1, 0},
        {2, 0, 3, 2, 0},
        {0, 3, 0, 0, 1},
        {1, 2, 0, 0, 1},
        {0, 0, 1, 1, 0}
    };

    int start_node = 0;
    dijkstra(graph, start_node);

    return 0;
}
```

3. JAVA 코드
```java
import java.util.Arrays;

public class Dijkstra {
    public static void dijkstra(int[][] graph, int start) {
        int n = graph.length;
        int[] distance = new int[n];
        boolean[] visited = new boolean[n];

        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[start] = 0;

        for (int i = 0; i < n; i++) {
            int minDistance = Integer.MAX_VALUE;
            int minIndex = -1;

            for (int j = 0; j < n; j++) {
                if (!visited[j] && distance[j] < minDistance) {
                    minDistance = distance[j];
                    minIndex = j;
                }
            }

            visited[minIndex] = true;

            for (int j = 0; j < n; j++) {
                if (graph[minIndex][j] != 0 && !visited[j]) {
                    int newDistance = distance[minIndex] + graph[minIndex][j];
                    if (newDistance < distance[j]) {
                        distance[j] = newDistance;
                    }
                }
            }
        }

        System.out.println("최단 거리: " + Arrays.toString(distance));
    }

    public static void main(String[] args) {
        int[][] graph = {
            {0, 2, 0, 1, 0},
            {2, 0, 3, 2, 0},
            {0, 3, 0, 0, 1},
            {1, 2, 0, 0, 1},
            {0, 0, 1, 1, 0}
        };

        int startNode = 0;
        dijkstra(graph, startNode);
    }
}
```

### 3-2. 구현 2 - 우선순위 큐 사용

#### 설명

1. 동작 방식
    - 우선순위 큐에 정점의 최소 거리를 저장
    - 순회를 할 때, 우선순위 큐에서 최소 거리에 해당하는 정점과 경로를 꺼내어 찾는다.

2. 시간복잡도
    - O((V + E) * log V)

#### 코드

1. 파이썬 코드
```python
import heapq

def dijkstra(graph, start):
    n = len(graph)
    distance = [float('inf')] * n
    distance[start] = 0
    pq = [(0, start)]  # (거리, 노드)

    while pq:
        dist, node = heapq.heappop(pq)

        if dist > distance[node]:
            continue

        for neighbor, weight in graph[node]:
            new_distance = dist + weight
            if new_distance < distance[neighbor]:
                distance[neighbor] = new_distance
                heapq.heappush(pq, (new_distance, neighbor))

    return distance

# 그래프 입력 (인접 리스트)
graph = [
    [(1, 2), (3, 1)],  # 0번 노드
    [(0, 2), (2, 3), (3, 2)],  # 1번 노드
    [(1, 3), (4, 1)],  # 2번 노드
    [(0, 1), (1, 2), (4, 1)],  # 3번 노드
    [(2, 1), (3, 1)]   # 4번 노드
]

start_node = 0
distances = dijkstra(graph, start_node)
print(distances)
```

2. C++ 코드
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

void dijkstra(const vector<vector<pair<int, int>>> &graph, int start) {
    int n = graph.size();
    vector<int> distance(n, INT_MAX);
    distance[start] = 0;

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.push({0, start});

    while (!pq.empty()) {
        int dist = pq.top().first;
        int node = pq.top().second;
        pq.pop();

        if (dist > distance[node]) continue;

        for (auto &neighbor : graph[node]) {
            int next_node = neighbor.first;
            int weight = neighbor.second;
            int new_distance = dist + weight;

            if (new_distance < distance[next_node]) {
                distance[next_node] = new_distance;
                pq.push({new_distance, next_node});
            }
        }
    }

    cout << "최단 거리: ";
    for (int d : distance) {
        cout << d << " ";
    }
    cout << endl;
}

int main() {
    vector<vector<pair<int, int>>> graph = {
        {{1, 2}, {3, 1}},  // 0번 노드
        {{0, 2}, {2, 3}, {3, 2}},  // 1번 노드
        {{1, 3}, {4, 1}},  // 2번 노드
        {{0, 1}, {1, 2}, {4, 1}},  // 3번 노드
        {{2, 1}, {3, 1}}   // 4번 노드
    };

    int start_node = 0;
    dijkstra(graph, start_node);

    return 0;
}
```

3. JAVA 코드

```java
import java.util.*;

public class Dijkstra {
    public static void dijkstra(List<List<int[]>> graph, int start) {
        int n = graph.size();
        int[] distance = new int[n];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[start] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[]{0, start});

        while (!pq.isEmpty()) {
            int[] node = pq.poll();
            int dist = node[0];
            int currentNode = node[1];

            if (dist > distance[currentNode]) continue;

            for (int[] neighbor : graph.get(currentNode)) {
                int nextNode = neighbor[0];
                int weight = neighbor[1];
                int newDistance = dist + weight;

                if (newDistance < distance[nextNode]) {
                    distance[nextNode] = newDistance;
                    pq.offer(new int[]{newDistance, nextNode});
                }
            }
        }

        System.out.print("최단 거리: ");
        for (int d : distance) {
            System.out.print(d + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        List<List<int[]>> graph = new ArrayList<>();
        graph.add(Arrays.asList(new int[]{1, 2}, new int[]{3, 1}));  // 0번 노드
        graph.add(Arrays.asList(new int[]{0, 2}, new int[]{2, 3}, new int[]{3, 2}));  // 1번 노드
        graph.add(Arrays.asList(new int[]{1, 3}, new int[]{4, 1}));  // 2번 노드
        graph.add(Arrays.asList(new int[]{0, 1}, new int[]{1, 2}, new int[]{4, 1}));  // 3번 노드
        graph.add(Arrays.asList(new int[]{2, 1}, new int[]{3, 1}));  // 4번 노드

        int startNode = 0;
        dijkstra(graph, startNode);
    }
}
```

## 4. 추천 문제
백준   
[실버 2 - 11724. 연결 요소의 개수](https://www.acmicpc.net/problem/11724)  
[골드 5 - 13549. 숨바꼭질 3](https://www.acmicpc.net/problem/13549)  
[골드 4 - 1753. 최단경로](https://www.acmicpc.net/problem/1753)  
[골드 2 - 9370. 미확인 도착지](https://www.acmicpc.net/problem/9370)  