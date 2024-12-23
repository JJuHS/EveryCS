# 연결리스트

## 1. 개요

**연결리스트**란 메모리 상에서 비연속적으로 존재하는 데이터 구조로, 각 데이터(노드)가 다음 데이터의 주소를 포함하고 있다.  
배열과 달리 고정된 크기가 없고, 동적으로 크기를 조절할 수 있어 데이터 삽입과 삭제가 용이하다.  
하지만 순차 접근만 가능하여 탐색 속도가 느릴 수 있다.

### 1-0. array와 비교 및 시간 복잡도
| |array|linked list|
|--------|-------|----|
|**장점**|무작위 접근 가능|빠른 자료 삽입, 삭제, 자유로운 크기 조절|
|**단점**|느린 자료 삽입, 삭제, 크기 조절 불가능|순차 접근만 가능, 메모리 추가 할당 필요|

**시간복잡도**
- **탐색 (Search)**: O(N) (순차 접근 필요)
- **삽입 (Insert)**: O(1) (노드 추가 시)
- **삭제 (Delete)**: O(1) (노드 삭제 시)

### 1-1. 단순 연결 리스트

단순 연결 리스트는 각 노드가 데이터와 다음 노드의 주소(포인터)를 포함하는 구조다.  
단방향으로 연결되므로 후행 노드로의 접근은 쉽지만, 선행 노드로 접근하려면 리스트를 순차적으로 탐색해야 한다.

### 1-2. 이중 연결 리스트

이중 연결 리스트는 각 노드가 데이터, 이전 노드의 주소, 다음 노드의 주소를 포함한다.  
양방향으로 접근이 가능하여 탐색과 수정이 더 용이하지만, 단순 연결 리스트보다 메모리 사용량이 많다.

### 1-3. 원형 연결 리스트

원형 연결 리스트는 단순 연결 리스트의 확장으로, 마지막 노드가 다시 첫 번째 노드를 가리킨다.  
이로 인해 리스트의 끝을 알 수 없으며, 한 노드에서 모든 노드로 접근이 가능하하다.

---

## 2. 연결리스트의 동작

### 2-1. 데이터 삽입

데이터 삽입은 새로운 노드를 생성한 뒤 기존 노드의 포인터를 업데이트하는 방식으로 이루어진다.  
단순 연결 리스트에서는 다음 노드의 주소를 재설정하며, 이중 연결 리스트에서는 양쪽의 주소를 업데이트한다.

```python
# Python 단순 연결 리스트 삽입 예제
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

# 사용 예시
ll = LinkedList()
ll.insert(3)
ll.insert(5)
print(ll.head.data)  # 출력: 5
```

### 2-2. 데이터 삭제

데이터 삭제는 삭제할 노드의 포인터를 제거하거나 건너뛰는 방식으로 이루어진다.  
이중 연결 리스트에서는 이전 노드와 다음 노드의 포인터를 업데이트해야 한다.

```python
# Python 단순 연결 리스트 삭제 예제
class LinkedList:
    def delete(self, key):
        temp = self.head

        if temp is not None and temp.data == key:
            self.head = temp.next
            temp = None
            return

        prev = None
        while temp is not None and temp.data != key:
            prev = temp
            temp = temp.next

        if temp is None:
            return

        prev.next = temp.next
        temp = None

# 사용 예시
ll = LinkedList()
ll.insert(3)
ll.insert(5)
ll.delete(3)
```

---

## 3. 연결리스트 구현하기

### 정적 할당을 통해 연결리스트 구현

연결 리스트 구현의 기본은 노드 생성, 삽입, 삭제, 탐색이다.  
이를 통해 리스트 구조의 동작을 이해하여야 한다.

### 3-1. Java에서의 구현

Java에는 내장된 LinkedList 클래스가 있다.
#### (1) LinkedList 클래스 사용하기
```java
import java.util.LinkedList;

public class Main {
    public static void main(String[] args) {
        // Java의 기본 LinkedList 사용 예시
        LinkedList<Integer> list = new LinkedList<>();
        list.add(10); // 삽입
        list.add(20);
        list.add(30);

        System.out.println(list); // 출력: [10, 20, 30]

        list.remove(1); // 삭제
        System.out.println(list); // 출력: [10, 30]
    }
}
```

#### (2) 쌩?으로 구현하기기
```java
class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    Node head;

    public void insert(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
    }

    public void delete(int key) {
        Node temp = head, prev = null;
        if (temp != null && temp.data == key) {
            head = temp.next;
            return;
        }
        while (temp != null && temp.data != key) {
            prev = temp;
            temp = temp.next;
        }
        if (temp == null) return;
        prev.next = temp.next;
    }
}
```

### 3-2. Python에서의 구현

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def delete(self, key):
        temp = self.head

        if temp is not None and temp.data == key:
            self.head = temp.next
            temp = None
            return

        prev = None
        while temp is not None and temp.data != key:
            prev = temp
            temp = temp.next

        if temp is None:
            return

        prev.next = temp.next
        temp = None

    def display(self):
        temp = self.head
        while temp:
            print(temp.data, end=" -> ")
            temp = temp.next
        print("None")

# 사용 예시
ll = LinkedList()
ll.insert(10)
ll.insert(20)
ll.insert(30)
ll.display()  # 출력: 30 -> 20 -> 10 -> None
ll.delete(20)
ll.display()  # 출력: 30 -> 10 -> None
```

### 3-3. C++에서의 구현
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
    Node(int data) : data(data), next(nullptr) {}
};

class LinkedList {
    Node* head;

public:
    LinkedList() : head(nullptr) {}

    void insert(int data) {
        Node* newNode = new Node(data);
        newNode->next = head;
        head = newNode;
    }

    void deleteNode(int key) {
        Node* temp = head, *prev = nullptr;
        if (temp != nullptr && temp->data == key) {
            head = temp->next;
            delete temp;
            return;
        }
        while (temp != nullptr && temp->data != key) {
            prev = temp;
            temp = temp->next;
        }
        if (temp == nullptr) return;
        prev->next = temp->next;
        delete temp;
    }
};
```

### 4. 추천문제 
**연결리스트 기본**
[1406. 에디터 - 실버 2](https://www.acmicpc.net/problem/1406)
[1158. 요세푸스 문제 - 실버 4](https://www.acmicpc.net/problem/1158)
| 사실 요세푸스 문제는 굳이 연결리스트로 해결하는 문제는 아니지만, 연습을 위해 풀어본다면 좋을 것 같다.
## 응용
[23309. 철도 공사 - 골드 4](https://www.acmicpc.net/problem/23309)
[31857. 사탕 공장 - 골드 2](https://www.acmicpc.net/problem/31857)