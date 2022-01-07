### 1. 이진 탐색 알고리즘

---

1. 이진 탐색
    - 정렬되어 있는 리스트에서 특정 원소를 빠르게 찾을 수 있도록 하는 탐색 알고리즘
    - **정렬되어 있는 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법**
        - 시작점, 끝점, 중간점을 이용하여 탐색 범위를 설정
    - cf. 순차 탐색
        - 리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 확인하는 방법
    - 동작 원리
        1. 시작점은 첫번째 인덱스, 끝점은 가장 끝 인덱스로 설정, 중간점은 중간값에서 소수점 이하를 제거한 인덱스로 설정
        2. 찾고자 하는 값과 중간점을 비교해 중간점이 더 작다면 중간점을 끝점으로 하고, 중간점이 더 크다면 중간점을 시작점으로 바꿈
        3. b를 반복
    - 시간 복잡도
        - 단계마다 탐색 범위를 2로 나누는 것과 동일하므로 O(logN)
    
    ```python
    #재귀적 구현
    
    def binary_search(array, target, start, end):
        if start > end:
            return None
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        elif array[mid] > target:
            return binary_search(array, target, start, mid - 1)
        elif array[mid] < target:
            return binary_search(array, target, mid + 1, end)
    
    n, target = map(int, input().split())
    array = list(map(int, input().split()))
    
    result = binary_search(array, target, 0, n - 1)
    if result == None:
        print("원소가 존재하지 않습니다.")
    else:
        print(result + 1)
    ```
    
    ```python
    #반복문 구현
    
    def binary_code(array, target, start, end):
        while start <= end:
            mid = (start + end) // 2
            if array[mid] == target:
                return mid
            elif array[mid] > target:
                end = mid - 1
            elif array[mid] < target:
                start = mid + 1
        return None
    
    n, target = map(int, input().split())
    array = list(map(int, input().split()))
    
    result = binary_code(array, target, 0, n - 1)
    if result == None:
        print("X")
    else:
        print(result + 1)
    ```
    

### 2. 이진 탐색 라이브러리

---

1. bisect_left(a, x)
    - 정렬된 순서를 유지하면서 배열 a에 x를 삽입할 가장 왼쪽 인덱스를 반환
2. bisect_right(a, x)
    - 정렬된 순서를 유지하면서 배열 a에 x를 삽입할 가장 오른쪽 인덱스를 반환
    
    ```python
    from bisect import bisect_left, bisect_right
    
    a = [1, 2, 4, 4, 8]
    x = 4
    
    print(bisect_left(a, x)) #2
    print(bisect_right(a, x)) #4
    ```
    

### 3. 응용

---

1. 값이 특정 범위에 속하는 데이터 개수 구하기
    
    ```python
    from bisect import bisect_left, bisect_right
    
    def count_by_range(a, left_value, right_value):
        right_index = bisect_right(a, right_value)
        left_index = bisect_right(a, left_value)
        return right_index - left_index
    
    a = [1, 2, 3, 3, 3, 3, 4, 4, 8, 9]
    
    print(count_by_range(a, 4, 4)) # 2
    print(count_by_range(a, -1, 3)) # 6
    ```
    
2.  파라메트릭 서치 (Parametric Search)
    - 최적화 문제를 결정 문제(Yes/No)로 바꾸어 해결하는 기법
        - ex) 특정한 조건을 만족하는 가장 알맞은 값을 빠르게 찾는 최적화 문제
    - 일반적으로 코딩 테스트에서 파라메트릭 서치 문제는 이진 탐색을 이용해 해결할 수 있음

### 4. 문제

---

1. 떡볶이 떡 만들기
    
    1. 시작점: 떡을 자르지 않는 0부터 시작
    끝점: 가장 긴 떡의 길이
    중간점: 시작점 + 끝점 // 2
    2. 시작점부터 떡을 자른 길이를 합해 필요한 떡의 크기와 비교
    
    ```python
    N, M = map(int, input().split()) #4, 6
    array = list(map(int, input().split())) #[19, 15, 10, 17]
    
    start = 0
    end = max(array)
    
    result = 0
    while start <= end:
        total = 0
        mid = (start + end) // 2
        for x in array:
            if x > mid:
                total += x - mid
        if total < M:
            end = mid - 1
        else:
            result = mid
            start = mid + 1
    print(result)
    ```
    
2. 정렬된 배열에서 특정 수의 개수 구하기

```python
from bisect import bisect_right, bisect_left

N, x = map(int, input().split()) #4, 6
array = list(map(int, input().split())) #[19, 15, 10, 17]

result =  bisect_right(array, x) - bisect_left(array, x)

if result > 0:
    print(result)
else:
    print(-1)
```