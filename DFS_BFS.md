### 1. 그래프 탐색 알고리즘

---

1. 탐색
    - 탐색(Serach)이란 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정
    - 대표적 그래프 탐색 알고리즘 : DFS, BFS

### 2. 자료구조

---

1. **스택**
    - 먼저 들어온 데이터가 나중에 나가는 자료구조
    - 입구와 출구가 동일한 형태
    - ex) 뒤로가기
    
    ```python
    stack = [] #리스트 자료형은 가장 오른쪽에 새로운 원소 삽입되므로 스택 가능
    stack.append(5)
    stack.append(2)
    stack.append(7)
    stack.pop() #가장 오른쪽의 원소 삭제
    stack.append(1)
    stack.append(4)
    stack.pop() #가장 오른쪽의 원소 삭제
    
    print(stack[::-1]) #스택의 최상단 원소(리스트 정방향의 반대)부터 출력
    print(stack) #스택의 최하단 원소부터 출력
    ```
    
2. 큐
    - 먼저 들어온 데이터가 먼저 나가는 형식의 자료구조
    - 입구와 출구가 모두 뚫려 있는 터널과 같은 형태
    - ex) 콜센터
    
    ```python
    from collections import deque 
    queue = deque() #큐 구현을 위한 deque 라이브러리 사용!
    queue.append(5)
    queue.append(2)
    queue.popleft()
    queue.append(3)
    queue.append(7)
    
    print(queue) #먼저 들어온 순서대로 출력
    queue.reverse() #역순으로 바꾸기
    print(queue) #나중에 들어온 원소부터 출력
    ```
    

### 3. 재귀 함수

---

1. **재귀 함수(Recursive Function)**
    - 자기 자신을 다시 호출하는 함수
    - 스택 자료구조 안에 함수가 담겨 컴퓨터 메모리에 올라감
    - 컴퓨터 메모리에는 한계가 있기 때문에, 재귀 깊이 제한이 존재함
    
    ```python
    def recursive_function():
    	print("재귀 함수를 호출합니다.") #해당 문자열을 무한히 출력
    	recursive_function()
    
    recursive_function()
    
    #어느정도 출력 후 최대 재귀 깊이 초과 메시지가 출력
    ```
    
2. **종료 조건**
    - 종료 조건을 명시해야 무한히 호출되는 것을 피할 수 있음
    
    ```python
    def recursive_function(i):
    	if i == 100:
    		return
    	print(i, "번째 재귀함수에서", i + 1, "번째 재귀함수를 호출합니다.")
    	recursive_function(i + 1)
    	print(i, "번째 재귀함수를 종료합니다.")
    
    recursive_function(1)
    ```
    
3. **팩토리얼 예시**
    - 0!과 1!은 1이므로, 이를 포함하여 종료 조건 명시
    - return에 n * (n -1) 점화식을 반영
    
    ```python
    def factorial(a):
        if a <= 1:
            return 1
        return a * factorial(a - 1)
    print(factorial(5))
    ```
    
4. **최대공약수 계산 예시 - 유클리드 호제법**
    - 유클리드 호제법
        - 두 개의 자연수에 대한 최대공약수를 구하는 대표적 알고리즘
        - 두 자연수 A, B에 대해 A % B를 R이라고 한다면, 
        (A와 B의 최대공약수) = (B와 R의 최대공약수)
    
    ```python
    def uclid(a, b):
        if a % b == 0: #a가 b의 배수가 되면 b가 최대공약수
            return b
        return uclid(b, a % b)
    ```
    
5. 유의 사항
    - 복잡한 알고리즘을 간결하게 작성할 수 있음
    - 모든 재귀함수는 반복문을 이용해 기능을 구현할 수 있음
        - 반복문보다 유리할 때도, 불리할 때도 있음
    - 컴퓨터가 함수를 연속적으로 호출하면 컴퓨터 메모리 내부의 스택 프레임에 쌓임
    - 스택을 사용해야 할 때 구현상 스택 라이브러리 대신에 재귀 함수를 이용하는 경우가 있음

### 3. DFS

---

1. DFS (Depth-First Search)
    - 깊이 우선 탐색
    - 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘
    - 스택 자료구조(또는 재귀함수)를 이용
    - 동작 과정
        1. 탐색 시작 노드를 스택에 삽입하고 방문 처리
        2. 스택의 최상단 노드에 방문하지 않은 인접한 노드가 하나라도 있으면 그 노드를 스택에 넣고 방문 처리
            
            → 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼냄
            
        3. 더 이상 b의 과정을 수행할 수 없을 때까지 반복
    
    ```python
    #각 노드가 연결된 정보를 표현 (2차원 리스트)
    graph = [[], [2, 3, 8], [1, 7], [1, 4, 5], [3, 5],
    [3, 4], [7], [2, 6, 8], [1, 7]]
    
    #각 노드가 방문된 정보를 표현 (1차원 리스트)
    visited = [False] * 9
    
    def dfs(graph, v, visited):
    	visited[v] = True
    	print(v, end = " ")
    	for i in graph[v]:
    		if not visited[i]:
    			dfs(graph, i, visited)
     
    ```
    

### 4. BFS

---

1. **BFS (Breadth-First Search)**
    - 너비 우선 탐색
    - 그래프에서 가까운 노드부터 우선적으로 탐색하는 알고리즘
    - 큐 자료구조 이용
    - 동작 과정
        1. 탐색 시작 노드를 큐에 삽입하고 방문 처리
        2. 큐에서 노드를 꺼낸 뒤, 해당 노드의 인접 노드 중 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리
            
            → 노드를 꺼냈으나 인접 노드가 없는 경우 무시
            
        3. 더이상 b의 과정을 수행할 수 없을 때까지 반복
    
    ```python
    # 각 노드가 연결된 정보를 표현 (2차원 리스트)
    graph = [[], [2, 3, 8], [1, 7], [1, 4, 5], [3, 5], [3, 4], [7], [2, 6, 8], [1, 7]]
    
    # 각 노드가 방문된 정보를 표현 (1차원 리스트)
    visited = [False] * 9
    
    from collections import deque
    def bfs(graph, start, visited):
        queue = deque([start])
        visited[start] = True
        while queue:
            v = queue.popleft()
            print(v, end=" ")
            for i in graph[v]:
                if not visited[i]:
                    queue.append(i)
                    visited[i] = True
    bfs(graph, 1, visited)
    ```
    

### 5. 문제 풀이

---

1. 음료수 얼려 먹기
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c20dda51-fa5b-4af2-94ca-a0a5ee05a7a3/Untitled.png)
    
    - DFS 활용
        1. 특정 지점의 주변 상, 하, 좌, 우를 살펴본 뒤 주변 지점에서 값이 0이면서 아직 방문하지 않은 지점을 방문
        2. 방문한 지점에서 다시 상, 하, 좌, 우를 상펴 방문을 진행하는 과정을 반복
            
            → 연결된 모든 지점 방문 가능
            
        3. 모든 노드에 대해 a ~b의 과정을 반복, 방문하지 않은 지점의 수를 카운트
    
    ```python
    n, m = map(int, input().split())
    
    #2차원 리스트의 맵 정보 입력 받기
    graph = []
    for i in range(n):
        graph.append(list(map(int, input())))
    
    # DFS로 특정 노드를 방문하고 연결된 모든 노드들도 방문
    def dfs(x, y):
        # 주어진 범위를 벗어나는 경우에는 즉시 종료
        if x <= -1 or x >= n or y <= -1 or y >= m:
            return False
        # 현재 노드를 아직 방문하지 않았다면
        if graph[x][y] == 0:
            graph[x][y] = 1
            dfs(x - 1, y)
            dfs(x, y - 1)
            dfs(x + 1, y)
            dfs(x, y + 1)
            return True #선행 변형 dfs를 모두 실행하고 난 뒤 return 실행됨
        return False
    
    # 모든 노드(위치)에 대해 음료수 채우기
    result = 0
    for i in range(n):
        for j in range(m):
            # 현재 위치에서 DFS 수행
            if dfs(i, j) == True:
                result += 1
    ```
    
2. 미로 탈출
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d82fe71a-adda-4998-bcb6-655f5574b017/Untitled.png)
    
    - BFS 활용
        - BFS는 시작 지점에서 가까운 노드부터 차례대로 그래프의 모든 노드를 탐색
    
    ```python
    from collections import deque
    
    n, m = map(int, input().split())
    
    graph = []
    for i in range(n):
        graph.append(list(map(int, input())))
    
    #방향 벡터 정의
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    
    def bfs(x, y):
        queue = deque()
        queue.append((x, y))
        # queue가 빌 때까지 반복
        while queue:
            # (x, y)를 큐에서 제거
            x, y = queue.popleft()
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if nx < 0 or nx >= n or ny < 0 or ny >= m:
                    continue
                if graph[nx][ny] == 0:
                    continue
                # 해당 노드를 처음 방문하는 경우에만 최단 거리를 기록
                if graph[nx][ny] == 1:
                    # n번째에 이동한 노드들은 모두 n의 값이 되게 함
                    graph[nx][ny] = graph[x][y] + 1
                    queue.append((nx, ny))
        return graph[n - 1][m - 1]
    
        print(bfs(0, 0))
    ```