### 1. 정렬

---

1. 정렬 (Sorting)
    - 데이터를 특정한 기준에 따라 순서대로 나열하는 것
    - 일반적으로 문제 상황에 따라 적절한 정렬 알고리즘이 공식처럼 사용됨

### 2. 선택 정렬 알고리즘

---

- 처리되지 않은 데이터 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복
- 매번 선형 탐색을 진행하는 것과 동일
- 이중 반복문을 이용해 구현

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    min_index = i #스와핑할 변수 만들어놓기
    for j in range(i + 1, len(array)): #가장 작은 원소를 찾는 과정
        if array[min_index] > array[j]:
            min_index = j
    # 스와핑할 변수와 가장 작은 원소 스와프
    array[i], array[min_index] = array[min_index], array[i]
print(array)
```

- 시간 복잡도
    - N번만큼 가장 작은 수를 찾아 맨 앞으로 보내야 함
    - N + (N - 1) + (N - 2) + ... + 2 → (N**2 + N - 2) / 2
        
        → O(N**2)
        

### 3. 삽입 정렬 알고리즘

---

- 처리되지 않은 데이터를 하나씩 골라 적절한 위치에 삽입
- 선택 정렬에 비해 일반적으로 더 효율적으로 동작
- 동작 예시
    1. 첫 번째 데이터는 정렬되어있다고 가정, 두번째 데이터가 첫 번째 데이터의 왼쪽 또는 오른쪽으로 들어갈지 판단
    2. 다음 데이터가 첫번째 데이터의 왼쪽/오른쪽, 또는 머물러 있기 중 선택

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    for j in range(i, 0, -1): #인덱스 i부터 1까지 1씩 감소하며 반복
        if array[j] < array[j - 1]:
            array[j], array[j - 1] = array[j - 1], array[j]
        else: #자기보다 작은 데이터를 만나면 그 위치에서 멈춤
            break

print(array)
```

- 시간 복잡도
    - O(N**2)
    - 반복문이 두 번 중첩되어 사용
    - 그러나 현재 리스트의 데이터가 거의 정렬되어 있으면 O(N)의 시간 복잡도를 가짐

### 4. 퀵 정렬

---

- 기준 데이터를 설정하고, 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 방법
- 일반적 상황에서 가장 많이 사용되는 정렬 알고리즘
- 병합 정렬과 함께 대부분의 프로그래밍 언어의 정렬 라이브러리의 근간이 되는 알고리즘
- 가장 기본적인 퀵 정렬은 첫 번째 데이터를 기준 데이터(Pivot)로 설정
- 동작 원리
    1. 첫번째 원소를 기준으로 하여 왼쪽에서 피벗값보다 큰 데이터를, 오른쪽에서 피벗값보다 작은 데이터를 선택하여 서로 바꿈
    2. a를 반복 수행하다 위치가 엇갈리는 경우, 피벗과 작은 데이터의 위치를 서로 변경(분할)
    3. 분할 이후 왼쪽/오른쪽을 각각의 배열로 판단하여 퀵 정렬 수행
    - 재귀적, 수행될 때마다 정렬 범위가 좁아짐
- 시간 복잡도
    - 이상적인 경우 분할이 절반씩 일어난다면 → O(N * logN)
    - 최악의 경우(이미 정렬된 배열에 대해 첫번째 원소를 피벗으로 삼을 때) → O(N ** 2)
        - 매번 두번째 데이터를 기준으로 분할이 일어나게 됨
        - 표준 라이브러리를 이용하면 N * logN을 보장

```python
#정석적 풀이

array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]
# 1 =   [5, 4, 9, 0, 3, 1, 6, 2, 7, 8]
# 2 =   [5, 4, 2, 0, 3, 1, 6, 9, 7, 8]
# 3 =   [1, 4, 2, 0, 3, 5, 6, 9, 7, 8]

def quick_sort(array, start, end):
    if start >= end: #원소가 1개일 경우 종료
        return
    pivot = start
    left = start + 1
    right = end
    while(left <= right):
        # 피벗보다 큰 데이터를 찾을 때까지 반복
        while(left <= end and array[left] <= array[pivot]):
            left += 1
        # 피벗보다 작은 데이터를 찾을 때까지 반복
        while(right > start and array[right] >= array[pivot]):
            right -= 1 
        if left > right:
            array[right], array[pivot] = array[pivot, array[right]]
        else:
            array[left], array[right] = array[right], array[left]
    quick_sort(array, start, right - 1)
    quick_sort(array, right + 1, end)

quick_sort(array, 0, len(array) - 1)
print(array)
```

```python
#파이썬적 풀이

array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array):
    if len(array) <= 1:
        return array
    pivot = array[0] # 피벗은 첫 번째 원소
    tail = array[1:] # 피벗(첫 번째 원소)을 제외한 리스트

    left_side = [x for x in tail if x <= pivot]
    right_side = [x for x in tail if x > pivot]

    return quick_sort(left_side) + [pivot] + quick_sort(right_side)

print(quick_sort(array))
```

### 5. 계수 정렬

---

- 특정 조건이 부합할 때만 사용할 수 있으나, 매우 빠르게 동작하는 정렬 알고리즘
    - 데이터의 크기 범위가 제한되어 정수 형태로 표현할 수 있을 때 사용 가능
- 데이터의 개수가 N, 데이터(양수) 중 최댓값이 K일 때,  
최악의 경우에도 수행 시간 O(N + K)를 보장
- 동작 원리
    1. 가장 작은 데이터부터 가장 큰 데이터까지의 범위가 모두 담길 수 있도록 리스트 생성
    2. 범위 리스트에 데이터를 카운트
    3. 범위 리스트의 첫 번째 데이터부터 카운트 값만큼 반복하여 출력

```python
 # 모든 원소의 값이 0보다 크거나 같다고 가정
array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5, 2]

#  모든 범위를 포함하는 리스트 선언 (모든 값은 0으로 초기화)
count = [0] * (max(array) + 1)

for i in range(len(array)):
    count[array[i]] += 1 # 각 데이터에 해당하는 인덱스의 값 증가

for i in range(len(count)):
    for j in range(count[i]):
        print(i, end=" ")
```

- 시간 복잡도
    - O(N + K) (공간 복잡도도 같음)
- 심각한 비효율성을 초래할 수 있음
    - 0, 999999로 데이터가 두개 존재하는 경우
- 동일 값을 가지는 여러 데이터가 등장할 때 가장 효과적으로 사용할 수 있음
    - 성적의 경우, 해당 점수를 맞은 사람이 여러 명일 수 있으므로 계수 정렬이 효과적임

### 6. 정렬 알고리즘 비교

---

1. 알고리즘별 시간, 공간 복잡도 비교

- 표준 정렬 라이브러리로 사용할 경우 최악의 경우에도 O(N * logN)으로 동작하도록 되어 있음
1. 선택 정렬과 기본 정렬 라이브러리 수행 시간 비교
    
    ```python
    # 선택 정렬 수행시간
    
    from random import randint
    import time
    
    array = []
    for _ in range(10000):
        array.append(randint(1, 100))
    
    # 선택 정렬 프로그램 성능 측정
    start_time = time.time()
    
    for i in range(len(array)):
        min_index = i
        for j in range(i + 1, len(array)):
            if array[min_index] > array[j]:
                min_index = j
        array[i], array[min_index] = array[min_index], array [i]
    
    # 측정 종료
    end_time = time.time()
    
    # 수행 시간 출력
    print("선택 정렬 성능 측정:", end_time - start_time)
    ```
    
    ```python
    # 기본 정렬 수행시간
    
    from random import randint
    import time
    
    array = []
    for _ in range(10000):
        array.append(randint(1, 100))
    
    # 선택 정렬 프로그램 성능 측정
    start_time = time.time()
    
    array.sort()
    
    # 측정 종료
    end_time = time.time()
    
    # 수행 시간 출력
    print("기본 정렬 성능 측정:", end_time - start_time)
    ```
    

### 7. 문제

---

1. 문제 1: 두 배열의 원소 교체
    
    ```python
    N, K = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    
    A.sort() #오름차순 정렬
    B.sort(reverse=True) #내림차순 정렬
    
    for i in range(K):
        if A[i] < B[i]:
            A[i], B[i] = B[i], A[i]
        else: #최대 K번 바꿀 수 있으므로, 크거나 같다면 탈출
            break
        
    print(sum(A))
    ```