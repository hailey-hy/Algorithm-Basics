### 1. 다이나믹 프로그래밍

---

1. 다이나믹 프로그래밍
    - 메모리를 적절히 사용하여 수행 시간 효율성을 비약적으로 향상시키는 방법
    - 이미 계산된 결과는 별도의 메모리 영역에 저장하여 다시 계산하지 않도록 함
    - 다이나믹 프로그래밍의 구현은 탑다운과 보텀업으로 구성
    - 동적 계획법과 동의어지만, "다이나믹"에 별다른 의미 없음
    - cf. 일반적 프로그래밍 분야에서의 "동적(Dynamic)"의 의미
        - 자료구조에서의 동적 할당(Dynamic Allocation): 프로그램이 실행되는 도중 실행에 필요한 메모리를 할당하는 기법

### 2. 다이나믹 프로그래밍의 조건

---

1. 최적 부분 구조 (Optimal Substructure)
    - 큰 문제를 작은 문제로 나눌 수 있으며, 작은 문제의 답을 모아서 큰 문제를 해결할 수 있다
2. 중복되는 부분 문제 (Overlapping Subproblem)
    - 동일한 작은 문제를 반복적으로 해결해야 함

### 3. 응용

---

1. 피보나치 수열
    - 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...
    - n항과 n+1항의 합이 n+2항이 되는 수열
    - 점화식
    - 재귀 함수를 통한 구현
    
    ```python
    def fibo(x):
        if x == 1 or x == 2:
            return 1
        return fibo(x - 1) + fibo(x - 2)
    
    print(fibo(4))
    ```  
    - 시간 복잡도 분석
        - O(2**n)

### 4. 탑다운(메모이제이션) 방식

---

1. 메모이제이션 (Memoization)
    - 다이나믹 프로그래밍을 구현하는 방법 중 하나
    - 한 번 게산한 결과를 메모리 공간에 메모하는 기법
    - 같은 문제를 다시 호출하면 메모했던 결과를 그대로 가져옴
    - 값을 기록해 놓는다는 점에서 캐싱(Caching)이라고도 함
2. 보텀업 방식과의 차이
    - 다이나믹 프로그래밍의 전형적 형태는 보텀업
    - 보텀업 방식은 반복문을 사용
    - 결과 저장용 리스트는 DP 테이블이라고 부름
    - 메모이제이션은 이전에 계산된 결과를 일시적으로 기록해 놓는 넓은 개념을 의미

```python
d = [0] * 100 #DP 테이블

def fibo(x):
    if x == 1 or x == 2:
        return 1
    if d[x] != 0:
        return d[x]
    d[x] = fibo(x - 1) + fibo(x - 2)
    return d[x]
print(fibo(99))
```

- 동작 분석

```python
d = [0] * 100 

def fibo(x):
		print("f(" + str(x) + ")", end=" ")
    if x == 1 or x == 2:
        return 1
    if d[x] != 0:
        return d[x]
    d[x] = fibo(x - 1) + fibo(x - 2)
    return d[x]
print(fibo(6))

	#실행 결과: f(6) f(5) f(4) f(3) f(2) f(1) f(2) f(3) f(4)
```

### 5. 보텀업 방식

---

```python
d = [0] * 100 #DP 테이블

d[1] = 1
d[2] = 2
n = 99

for i in range(3, n + 1):
    d[i] = d[i - 1] + d[i - 2]

print(d[n])
```

### 6. 분할 정복과의 차이

---

- 다이나믹 프로그래밍과 분할 정복은 모두 최적 부분 구조를 가질 때 사용 가능
- 주요 차이점: 부분 문제의 중복
    - 분할 정복 문제에서는 동일한 부분 문제가 반복적으로 제시되지 않음
 
### 7. 다이나믹 프로그래밍 문제에 접근하는 법

---

1. 주어진 문제가 어떤 유형인지 파악하기
    - 그리디, 구현, 완전 탐색의 아이디어로 접근 가능한지 확인
    - 다른 알고리즘으로 풀이 방법이 떠오르지 않을 경우 다이나믹 프로그래밍을 고려
2. 재귀 함수로 비효율적인 완전 탐색 프로그램을 작성한 뒤, 작은 문제에서 구한 답이 큰 문제에서 그대로 사용될 수 있으면 코드를 개선
3. 일반적인 코딩 테스트 수준에서는 기본 유형의 다이나믹 프로그래밍 문제가 출제되므로 겁먹지 않기

### 8. 문제

---

1. 개미 전사

- 문제 해결 아이디어
   
    ```python
    n = int(input())
    array = list(map(int, input().split()))
    
    d = [0] * 100
    
    d[0] = array[0]
    d[1] = max(array[0], array[1])
    for i in range(2, n):
        d[i] = max(d[i - 1], d[i - 2] + array[i])
    
    print(d[n - 1])
    ```
    
1. **1로 만들기 ⭐️**
      
    ```python
    x = int(input())
    
    d = [0] * 30001
    #d[0] = 0, d[1] = 0, d[2] = 1, d[3] = 1, d[4] = 2, d[5] = 1, d[6] = 2, d[7] = 3
    
    for i in range(2, x + 1):
        **d[i] = d[i - 1] + 1**
        if i % 2 == 0:
            d[i] = min(d[i], d[i // 2] + 1)
        if i % 3 == 0:
            d[i] = min(d[i], d[i // 3] + 1)
        if i % 5 == 0:
            d[i] = min(d[i], d[i // 5] + 1)
    
    print(d[x])
    ```
    
2. **효율적인 화폐 구성 ⭐️**
       
    - 해결 아이디어
   
    ```python
    n, m = map(int, input().split())
    
    d = [10001] * (m + 1)
    
    array = []
    for i in range(n):
        array.append(int(input()))
    
    d[0] = 0
    for i in range(n):
        for j in range(array[i], m + 1):
            if d[j - array[i]] != 10001:
                d[j] = min(d[j], d[j - array[i]] + 1)
    
    if d[m] == 10001:
        print(-1)
    else:     
        print(d[m])
    ```
    
    1. 금광

        - 해결 아이디어
            
            ```python
            for tc in range(int(input())):
                n, m = map(int, input().split())
                array = list(map(int, input().split()))
            
                dp = []
                index = 0
                for i in range(n):
                    dp.append(array[index:index + m])
                    index += m
            
            for j in range(1, m):
                for i in range(n):
                    if i == 0: left_up = 0
                    else: left_up = dp[i - 1][j - 1]
            
                    if i == n - 1: left_down = 0
                    else: left_down = dp[i + 1][j - 1]
                        
                    left = dp[i][j - 1]
                    dp[i][j] = dp[i][j] + max(left_up, left_down, left)
            
            result = 0
            for i in range(n) :
                result = max(result, dp[i][m - 1])
            print(result)
            ```
            
    2. 병사 배치하기
        
        - 해결 아이디어
            - 가장 긴 증가하는 부분 수열 (Longest Increasing Subsequence, LIS)
  ]   
        ```python
        n = int(input())
        array = list(map(int, input().split()))
        # 순서를 뒤집어 "최장 증가 부분 수열 문제로 변환"
        array.reverse()
        
        # 다이나믹 프로그래밍을 위한 1차원 DP 테이블 초기화
        dp = [1] * n
        
        # 가장 긴 증가하는 부분 수열 (LIS) 알고리즘 수행
        for i in range(1, n):
            for j in range(0, i):
                if array[j] < array[i]:
                    dp[i] = max(dp[i], dp[j] + 1)
        
        # 열외해야 하는 병사의 최소 수를 출력
        print(n - max(dp))
        ```