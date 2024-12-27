# 우선순위 큐 (Priority Queue)

## 1. 개요

우선순위 큐는 각 원소에 우선순위를 부여하여, 우선순위가 높은 원소가 먼저 처리되도록 하는 데이터 구조  
일반적인 큐는 FIFO(First In, First Out) 방식이지만, 우선순위 큐는 우선순위를 기준으로 순서가 정해짐.  

주로 힙을 사용하여 구현되며, 배열이나 연결 리스트를 통해서도 만들 수 있음.

---

## 2. 우선순위 큐의 특성

### 1. 원소의 입, 출

- **삽입**: 새로운 원소를 큐에 추가
- **삭제**: 가장 높은 우선순위를 가진 원소를 큐에서 제거
- **조회**: 현재 가장 높은 우선순위를 가진 원소를 반환

### 2. 배열, 연결리스트, 힙을 통한 우선순위 큐

| 구현 방식    | 삽입 시간복잡도 | 삭제 시간복잡도 | 공간복잡도 | 특징                                       |
|--------------|------------------|------------------|------------|--------------------------------------------|
| 배열 (정렬X) | O(1)             | O(N)             | O(N)       | 삽입은 빠르지만 삭제 시 전체를 탐색해야 함. |
| 배열 (정렬O) | O(N)             | O(1)             | O(N)       | 삽입 시 정렬이 필요하지만 삭제가 빠름.     |
| 연결 리스트   | O(N)             | O(1)             | O(N)       | 삽입 시 정렬을 유지해야 함.                |
| 힙           | O(log N)         | O(log N)         | O(N)       | 삽입, 삭제 모두 효율적이며 구현도 용이함. |

#### 중요
힙과 우선순위 큐는 다른 개념이다.  
    - 힙은 자료 구조이며
    - 우선순위 큐는 자료구조(배열, 연결리스트가 될 수도 있음)을 이용한 알고리즘 구현 방식이다.

---

## 3. 배열을 이용한 우선순위 큐 구현

### 1. 구현

```python
class PriorityQueueArray:
    def __init__(self):
        self.queue = []

    def insert(self, data):
        self.queue.append(data)

    def delete(self):
        if not self.queue:
            return None
        max_index = 0
        for i in range(len(self.queue)):
            if self.queue[i] > self.queue[max_index]:
                max_index = i
        return self.queue.pop(max_index)

    def peek(self):
        if not self.queue:
            return None
        return max(self.queue)
```

### 2. 연산 별 동작 방식 및 시간 복잡도

#### 1. 추가
- 데이터를 단순히 배열의 끝에 삽입함.
- **시간 복잡도**: O(1)

#### 2. 삭제
- 배열에서 가장 큰 값을 찾고, 해당 값을 제거함.
- **시간 복잡도**: O(N)

#### 3. 조회
- 배열에서 가장 큰 값을 탐색함.
- **시간 복잡도**: O(N)

---

## 4. 연결 리스트를 이용한 우선순위 큐 구현

### 1. 구현

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class PriorityQueueLinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        if not self.head or self.head.data < data:
            new_node.next = self.head
            self.head = new_node
        else:
            current = self.head
            while current.next and current.next.data >= data:
                current = current.next
            new_node.next = current.next
            current.next = new_node

    def delete(self):
        if not self.head:
            return None
        max_data = self.head.data
        self.head = self.head.next
        return max_data

    def peek(self):
        if not self.head:
            return None
        return self.head.data
```

### 2. 연산 별 동작 방식 및 시간 복잡도

#### 1. 추가
- 노드를 삽입하며 정렬 상태를 유지함.
- **시간 복잡도**: O(N)

#### 2. 삭제
- 가장 앞에 있는 노드를 제거함.
- **시간 복잡도**: O(1)

#### 3. 조회
- 가장 앞에 있는 노드를 반환함.
- **시간 복잡도**: O(1)

---

## 5. 힙을 이용한 우선순위 큐 구현

### 1. 구현

```python
import heapq

class PriorityQueueHeap:
    def __init__(self):
        self.heap = []

    def insert(self, data):
        heapq.heappush(self.heap, -data)

    def delete(self):
        if not self.heap:
            return None
        return -heapq.heappop(self.heap)

    def peek(self):
        if not self.heap:
            return None
        return -self.heap[0]
```

### 2. 연산 별 동작 방식 및 시간 복잡도

#### 1. 추가
- 힙에 원소를 삽입하며 힙 특성을 유지함.
- **시간 복잡도**: O(log N)

#### 2. 삭제
- 힙에서 루트 노드를 제거하며 힙 특성을 유지함.
- **시간 복잡도**: O(log N)

#### 3. 조회
- 힙의 루트 노드를 반환함.
- **시간 복잡도**: O(1)
