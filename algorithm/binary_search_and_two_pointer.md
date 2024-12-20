# 이분탐색과 투포인터

**목차**
1. 이분탐색  
    1-1. 개요  
    1-2. 알고리즘 원리  
    1-3. 예시  
    1-4. 시간복잡도  
    1-5. Bisect 라이브러리  
    1-6. 추천문제     

2. 투포인터  
    2-1. 개요  
    2-2. 알고리즘 원리  
    2-3. 예시  
    2-4. 시간복잡도  
    2-5. 추천문제  

3. 이분탐색과 투포인터 비교

# 이분탐색과 투포인터

## 1. 이분탐색

### 1-1. 개요

이분탐색(Binary Search)은 **정렬된 배열**에서 특정 값을 찾을 때 사용되는 효율적인 탐색 알고리즘이다.  
배열의 중간 값을 기준으로 탐색 범위를 절반으로 줄여가며 값을 찾는다.

### 1-2. 알고리즘 원리

1. 배열의 **중간 값**을 선택한다.  
2. 중간 값이 찾고자 하는 값과 같으면 탐색을 종료한다.  
3. 중간 값이 찾고자 하는 값보다 크면 **왼쪽 절반**을 탐색한다.  
4. 중간 값이 찾고자 하는 값보다 작으면 **오른쪽 절반**을 탐색한다.  
5. 이 과정을 반복하여 탐색 범위가 비어있으면 값이 존재하지 않는다.  

### 1-3. 예시

**문제**: 배열에서 숫자 7 찾기

```python
# 이분탐색 함수
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# 예제 배열
arr = [1, 3, 5, 7, 9, 11, 13]
target = 7
result = binary_search(arr, target)

if result != -1:
    print(f"{target}의 인덱스 : {result}")
else:
    print(f"{target} NOT FOUND")
```

**출력:**
```
7의 인덱스 : 3
```

### 1-4. 시간복잡도

- **시간복잡도**: O(log N)
    - 매번 탐색 범위를 절반으로 줄이기 때문에 로그 시간에 탐색이 가능하다.

### 1-5. Bisect 라이브러리

Python의 `bisect` 라이브러리는 이분탐색을 쉽게 수행할 수 있는 유틸리티를 제공한다.

#### 주요 함수
- `bisect_left(arr, x)`: x가 삽입될 가장 왼쪽 인덱스를 반환한다.  
- `bisect_right(arr, x)`: x가 삽입될 가장 오른쪽 인덱스를 반환한다.  

#### 예시 문제

**문제**: 정렬된 배열에 숫자 4를 삽입할 수 있는 위치를 찾기

```python
import bisect

# 예제 배열
arr = [1, 3, 5, 7, 9]

# 삽입할 값
x = 4

# bisect_left를 사용하여 삽입 위치 찾기
index = bisect.bisect_left(arr, x)
print(f"{x}를 삽입할 위치는 인덱스 : {index}")

# 실제로 값을 삽입
arr.insert(index, x)
print("삽입 이후 배열:", arr)
```

**출력:**
```
4를 삽입할 위치는 인덱스 : 2
삽입 이후 배열: [1, 3, 4, 5, 7, 9]
```

---

### 1-6. 추천 문제
백준 [1920. 수 찾기(실버4)](https://www.acmicpc.net/problem/1920), [2805. 나무 자르기(실버2)](https://www.acmicpc.net/problem/2805), [2110. 공유기 설치(골드4)](https://www.acmicpc.net/problem/2110), [1300. K번째 수(골드 1)](https://www.acmicpc.net/problem/1300)

## 2. 투포인터

### 2-1. 개요

투포인터(Two Pointers)는 두 개의 포인터를 사용해 배열을 탐색하면서 조건을 만족하는 구간이나 값을 찾는 알고리즘이다.  
**정렬된 배열**에서 활용되며, 구간 합이나 특정 조건을 만족하는 쌍을 찾을 때 유용하다.

### 2-2. 알고리즘 원리

1. **두 개의 포인터**를 배열의 시작과 끝에 배치한다.  
2. 포인터를 이동시키면서 조건을 확인한다.  
3. 조건을 만족하면 결과를 기록하고, 그렇지 않으면 포인터를 조정한다.  
4. 포인터가 교차할 때까지 이 과정을 반복한다.  

### 2-3. 예시

**문제**: 정렬된 배열에서 합이 10이 되는 두 수를 찾기

```python
# 투포인터 함수
def two_sum(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return (arr[left], arr[right])
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return None

# 예제 배열
arr = [1, 2, 4, 5, 7, 8, 9]
target = 10
result = two_sum(arr, target)

if result:
    print(f"합이 {target}이 되는 두 수: {result}")
else:
    print("조건을 만족하는 두 수가 없음")
```

**출력:**
```
합이 10이 되는 두 수: (2, 8)
```

### 2-4. 시간복잡도

- **시간복잡도**: O(N)
    - 배열을 한 번만 순회하기 때문에 선형 시간에 해결할 수 있음

---

### 2-5. 추천 문제
백준 [3273. 두 수의 합(실버3)](https://www.acmicpc.net/problem/3273), [2470. 두 용액(골드5)](https://www.acmicpc.net/problem/2470), [1806. 부분합(골드4)](https://www.acmicpc.net/problem/1806), [1644. 소수의 연속합(골드3)](https://www.acmicpc.net/problem/1644)

## 3. 이분탐색과 투포인터 비교

| **비교 항목**       | **이분탐색**                            | **투포인터**                         |
|---------------------|-----------------------------------------|--------------------------------------|
| **알고리즘 조건**    | 정렬된 배열                                                      |
| **동작 방식**        | 중간 값을 기준으로 범위를 절반씩 줄임    | 두 포인터를 양쪽에서 이동하며 탐색   |
| **시간복잡도**       | O(log N)                              | O(N)                                 |
| **장점**            | 빠른 탐색 속도                         | 간단한 구현 및 연속 구간 탐색에 유리  |
| **단점**            | 삽입/삭제 시 정렬 필요                  | 특정 조건을 만족하는 문제에만 적합    |
| **사용 사례**        | 특정 값 탐색, Lower/Upper Bound 찾기   | 구간 합, 특정 합을 만족하는 쌍 찾기   |

이분탐색은 **정확한 값**이나 **인덱스**를 찾을 때 유리하고, 투포인터는 **구간이나 쌍**을 찾을 때 적합하다.
