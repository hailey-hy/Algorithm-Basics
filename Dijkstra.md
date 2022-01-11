### 1. 최단 경로 알고리즘

---

- 가장 짧은 경로를 찾는 알고리즘
- 문제 상황 유형
    1. 한 지점에서 다른 한 지점까지의 최단 경로
    2. 한 지점에서 다른 모든 지점까지의 최단 경로
    3. 모든 지점에서 다른 모든 지점까지의 최단 경로
- 지점: 노드
- 지점간 연결 도로: 간선

### 2. 다익스트라 최단 경로 알고리즘

---

1. 개요
    - 특정한 노드에서 출발하여 다른 모든 노드로 가는 최단 경로를 계산
    - 음의 간선이 없을 때 정상적으로 동작함
    - 그리디 알고리즘으로 분류됨 → 매 상황에서 가장 비용이 적은 노드를 선택해 임의의 과정을 반복함
        - cf. 최단 경로 알고리즘은 다이나믹 프로그래밍의 원리가 적용됨
2. 동작 과정
    1. 출발 노드를 설정
    2. 최단 거리 테이블을 초기화
    3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택
    4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb620d80-2ee2-46f9-b4e6-f56017b39567/Untitled.png)
    
3. 특징
    - 그리디 알고리즘
    - 단계를 거치며 한 번 처리된 노드의 최단 거리는 고정되어 더 이상 바뀌지 않음
        
        → 한 단계당 하나의 노드에 대한 최단 거리를 확실히 찾는 것으로 이해할 수 있음
        
    - 다익스트라 알고리즘을 수행한 뒤 테이블에는 각 노드까지의 최단 거리 정보가 저장됨
        
        → 완벽한 형태의 최단 경로를 구하기 위해서는 소스코드에 추가적인 기능을 더 넣을 것
        
4. 구현
    - 단계마다 방문하지 않은 노드 중, 최단거리가 가장 짧은 노드 선택을 위해 매 단계마다 1차원 테이블의 모든 원소를 확인(순차 탐색)함
    
    ```python
    import sys
    input = sys.stdin.readline
    INF = int(1e9) #무한을 의미하는 값으로 10억을 설정
    
    # 노드의 개수, 간선의 개수를 입력받기
    n, m = map(int, input().split())
    # 시작 노드의 번호를 입력받기
    start = int(input())
    # 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만들기
    graph = [[] for i in range(n + 1)]
    # 방문한 적이 있는지 체크하는 목적의 리스트를 만들기
    visited = [False] * (n + 1)
    # 최단 거리 테이블을 모두 무한으로 초기화
    distance = [INF] * (n + 1)
    
    # 모든 간선 정보를 입력받기
    for _ in range(m):
        a, b, c = map(int, input().split())
        # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
        graph[a].append((b, c))
    
    # 방문하지 않은 노드 중에서, 가장 최단거리가 짧은 노드의 번호를 반환
    def get_smallest_node():
        min_value = INF
        index = 0 #가장 최단 거리가 짧은 노드(인덱스)
        for i in range(1, n + 1):
            if distance[i] < min_value and not visited[i]:
                min_value = distance[i]
                index = i
        return index
    
    def dijkstra(start):
        #시작 노드에 대해서 초기화
        distance[start] = 0
        visited[start] = True
        for j in graph[start]:
            distance[j[0]] = j[1]
        # 시작 노드를 제외한 전체 n - 1개의 노드에 대해 반복
        for i in range(n - 1):
            # 현재 최단 거리가 가장 짧은 노드를 꺼내서, 방문 처리
            now = get_smallest_node()
            visited[now] = True
            # 현재 노드와 연결된 다른 노드를 확인
            for j in graph[now]:
                cost = distance[now] + j[1]
                # 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
                if cost < distance[j[0]]:
                    distance[j[0]] = cost
    
    # 다익스트라 알고리즘을 수행
    dijkstra(start)
    
    # 모든 노드로 가기 위한 최단 거리를 출력
    for i in range(1, n + 1):
        # 도달할 수 없는 경우, 무한(INFINITY)이라고 출력
        if distance[i] == INF:
            print("INFINITY")
        # 도달할 수 있는 경우 거리를 출력
        else:
            print(distance[i])
    ```
    

### 3. 우선순위 큐

---

- 우선순위가 가장 높은 데이터를 가장 먼저 삭제하는 자료구조
- 여러 개의 물건 데이터를 자료구조에 넣었다가 가치가 높은 물건 데이터부터 꺼내서 확인해야 하는 경우 이용
- Python 등 대부분의 프로그래밍 언어에서 표준 라이브러리 형태로 지원

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0349fa22-29c7-4f21-84d5-631fb370208b/Untitled.png)

### 4. 힙

---

- 우선순위 큐를 구현하기 위해 사용하는 자료구조 중 하나
- 최소 힙 (Min Heap)과 최대 힙 (Max Heap)이 있음
- 다익스트라 최단 경로 알고리즘을 포함해 다양한 알고리즘에서 사용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f3cf12fc-c603-4cfc-b8d3-3fcac09e3c60/Untitled.png)

```python
import heapq

# 오름차순 힙 정렬 (Heap Sort)
def heapsort(iterable):
    h = []
    result = []
    # 모든 원소를 차례대로 힙에 삽입
    for value in iterable:
        heapq.heappush(h, value) #내림차순 -> heapq.heappush(h, -value)
    # 힙에 삽입된 모든 원소를 차례대로 꺼내어 담기
    for i in range(len(h)):
        result.append(heapq.heappop(h)) #내림차순 -> result.append(-heapq.heappop(h))
    return result

result = heapsort([1, 3, 5, 7, 9, 2, 4, 6, 8, 0])
print(result)
```

- 시간 복잡도
    - O(Nlogn)

### 5. 힙을 활용한 개선된 다익스트라 알고리즘 구현 방법

---

- 단계마다 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택하기 위해 힙 자료구조를 이용
- 다익스트라 알고리즘이 동작하는 기본 원리는 동일
    - 현재 가장 가까운 노드를 저장해 놓기 위해 힙 자료구조를 추가적으로 이용한다는 점이 다름
    - 현재의 최단 거리가 가장 짧은 노드를 선택해야 하므로 최소 힙을 사용

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) #무한을 의미하는 값으로 10억을 설정

# 노드의 개수, 간선의 개수를 입력받기
n, m = map(int, input().split())
# 시작 노드 번호를 입력받기
start = int(input())
# 각 노드에 연결되어 있는 노드에 대한 정보를 답는 리스트를 만들기
graph = [[] for i in range(n + 1)]
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF] * (n + 1)

# 모든 간선 정보를 입력받기
for _ in range(m):
    a, b, c = map(int, input().split())
    #  a번 노드에서 b번 노드로 가는 비용이 c라는 의미
    graph[a].append((b, c))

def dijkstra(start):
    q = []
    # 시작 노드로 가기 위한 최단 거리는 0으로 설정하여, 큐에 삽입
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q: # 큐가 비어있지 않다면
        # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
        dist, now = heapq.heappop(q)
        # 현재 노드가 이미 처리된 적이 있는 노드라면 무시
        if distance[now] < dist:
            continue
        # 현재 노드와 연결된 다른 인접한 노드들을 확인
        for i in graph[now]:
            cost = dist + i[1]
            # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))

# 다익스트라 알고리즘을 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리를 출력
for i in range(1, n + 1):
    # 도달할 수 없는 경우, 무한(INFINITY)이라고 출력
    if distance[i] == INF:
        print("INFINITY")
    # 도달할 수 있는 경우 거리를 출력
    else:
        print(distance[i])
```

- 시간 복잡도
    - 힙 자료구조를 이용할 경우 O(ElogV)
    - 노드의 개수 V이상은 처리되지 않으며, 우선순위 큐에서 꺼낸 노드와 연결된 다른 노드들을 확인하는 총 횟수는 최대 간선의 개수(E)만큼 수행될 수 있음

### 6. 플로이드 워셜 알고리즘

---

1. **플로이드 워셜 알고리즘**
    - 모든 노드에서 다른 모든 노드까지의 최단 경로를 모두 계산
    - 플로이드 워셜 (Floyd - Warshall) 알고리즘은 다익스트라 알고리즘과 마찬가지로 단계별로 거쳐 가는 노드를 기준으로 알고리즘을 수행
        - 단, 매 단계마다 방문하지 않은 노드 중 최단 거리를 갖는 노드를 찾는 과정이 필요하지 않음
    - 플로이드 워셜은 2차원 테이블에 최단 거리 정보를 저장함
    - 다이나믹 프로그래밍 유형에 속함
2. **구현**
- 각 단계마다 특정한 노드 k를 거쳐 가는 경우를 확인
    - a → b의 최단 거리보다, a → k → b 로 가는 거리가 더 짧은지 검사
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f178e442-0d55-4d84-881b-9df82f33a5ff/Untitled.png)
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec618477-941f-4bfe-9bd6-09a7209b6cb3/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c62b3990-46a4-45a6-afbc-d896cf98beab/Untitled.png)

```python
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수 및 간선의 개수를 입력받기
n = int(input())
m = int(input())
# 2차원 리스트(그래프 표현)를 만드록, 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대하나 정보를 입력 받아, 그 값으로 초기화
for _ in range(m):
    # A에서 B로 가는 비용은 C라고 설정
    a, b, c = map(int, input().split())
    graph[a][b] = c

# 점화식에 따라 플로이드 워셜 알고리즘을 수행
for k in range(1, n + 1): # k: 거쳐가는 노드
    for a in range(1, n + 1): # a: 출발 노드
        for b in range(1, n + 1): # b: 도착 노드
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

# 수행된 결과를 출력
for a in range(1, n + 1): 
    for b in range(1, n + 1):
        # 도달할 수 없는 경우, 무한(INFINIY)라고 출력
        if graph[a][b] == INF:
            print("INFINIY", end=" ")
        # 도달할 수 있는 경우 거리를 출력
        else:
            print(graph[a][b], end=" ")

    print()
```

1. **시간 복잡도**
    - 노드 개수가 N개일 때, 알고리즘상으로 N번의 단계를 수행
        - 각 단계마다 O(N**2)의 연산을 통해 현재 노드를 거쳐 가는 모든 경로를 고려
    - 총 시간복잡도: O(N**3)

### 7. 문제

---

1. 전보

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e68260b-c113-42d2-8859-cfcfff2ec90b/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5da8452e-fe99-4fc6-865d-6c87e088f9d7/Untitled.png)

- 해결 아이디어
    - 한 도시에서 다른 도시까지의 최단 거리 문제로 치환 가능

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수, 간선의 개수, 시작 노드를 입력받기
n, m, start = map(int, input().split())
# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만들기
graph = [[] for i in range(n + 1)]
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF] * (n + 1)

# 모든 간선 정보를 입력받기
for _ in range(m):
    x, y, z = map(int, input().split())
    # X번 노드에서 Y번 노드로 가는 비용이 Z라는 의미
    graph[x].append((y, z))

def dijkstra(start):
   q = []
   # 시작 노드로 가기 위한 최단 경로는 0으로 설정하여, 큐에 삽입
   heapq.heappush(q, (0, start))
   distance[start] = 0
   while q: # 큐가 비어있지 않다면
        # 가장 최단 거리가 짧은 노드에 대한 정보를 꺼내기
        dist, now = heapq.heappop(q)
        if distance[now] < dist:
            continue
        # 현재 노드와 연결된 다른 인접한 노드들을 확인
        for i in graph[now]:
            cost = dist + i[1]
            # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))

# 다익스트라 알고리즘을 수행
dijkstra(start)

# 도달할 수 있는 노드의 개수
count = 0
# 도달할 수 있는 노드 중에서, 가장 멀리 있는 노드와의 최단 거리
max_distance = 0
for d in distance:
    # 도달할 수 있는 노드인 경우
    if d != 1e9:
        count += 1
        max_distance = max(max_distance, d)

# 시작 노드는 제외해야 하므로 count - 1을 출력
print(count - 1, max_distance)
```

1. 미래 도시
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2cb444bd-19d1-4325-af8b-51e598d52fa1/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99b810da-7144-44bf-85a4-af675c233402/Untitled.png)
    
    ```python
    INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정
    
    # 노드의 개수 및 간선의 개수를 입력받기
    n, m = map(int, input().split())
    # 2차원 리스트(그래프 표현)를 만들고, 모든 값을 무한으로 초기화
    graph = [[INF] * (n + 1) for _ in range(n + 1)]
    
    # 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            if a == b:
                graph[a][b] = 0
    
    # 각 간선에 대한 정보를 입력 받아, 그 값으로 초기화
    for _ in range(m):
        # A와 B가 서로에게 가는 비용은 1이라고 설정
        a, b = map(int, input().split())
        graph[a][b] = 1
        graph[b][a] = 1
    
    # 거쳐 갈 노드 X와 최종 목적지 노드 K를 입력받기
    x, k = map(int, input().split())
    
    # 점화식에 따라 플로이드 워셜 알고리즘을 수행
    for k in range(1, n + 1):
        for a in range(1, n + 1):
            for b in range(1, n + 1):
                graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])
    
    # 수행된 결과를 출력
    distance = graph[1][k] + graph[k][x]
    
    # 도달할 수 없는 경우, -1을 출력
    if distance >= 1e9:
        print("-1")
    # 도달할 수 있다면, 최단 거리를 출력
    else:
        print(distance)
    ```