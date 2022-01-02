### 1. 그리디 알고리즘

---

1. **그리디 알고리즘**
    - 현재 상황에서 지금 당장 좋은 것만 고르는 방법
    - 문제 해결을 위한 최소한의 아이디어를 떠올릴 수 있는 능력 필요
    - 정당성 분석이 중요 → 가장 좋아보이는 것을 반복해도 최적의 해를 구할 수 있는가?
2. **문제 1: 거스름돈**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07f7b828-9469-4f90-a8ee-5b60054c6ba8/Untitled.png)
    
    - 가장 큰 화폐 단위부터 돈을 거슬러 줄 것
        - 해답 예시
            
            ```python
            n = int(input())
            cnt = 0
            array = [500, 100, 50, 10]
            for coin in array:
            	cnt += n // coin
            	n %= coin
            print(count)
            ```
            
    - 큰 화폐 단위가 항상 작은 화폐 단위의 배수가 되므로, 작은 단위를 종합해 다른 해가 나올 수 없기 때문
    - 시간 복잡도: 동전의 종류가 N일때, O(N)
3. **문제 2: 1이 될 때까지**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f20ce29b-0b51-4935-9fd4-ac65168ad0f3/Untitled.png)
    
    - 최대한 많이 나누기 수행
        
        ```python
        n, k = map(int, input().split())
        result = 0
        while True:
        	target = (n // k) * k #n에서 가장 가까운 k로 나누어 떨어지는 수
        	result += (n - target)
        	n = target
        	if n < k:
        		break
        	result += 1
        	n //= k
        result += (n - 1)
        print(result)
        ```
        
    - 시간 복잡도: log(N)
4. **문제 3: 곱하기 혹은 더하기**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5e2ed15-cd0b-4389-9d41-f9b05e81a8c5/Untitled.png)
    
    - 두 수 중 하나라도 1이하라면 더하고, 두 수가 모두 2 이상이면 곱하기
    
    ```python
    S = input()
    total = int(S[0])
    for i in range(1, len(S)):
        if int(S[i]) > 1 and total > 1:
            total *= int(S[i])
        elif int(S[i]) > 0 or total > 0:
            total += int(S[i])
        else:
            pass
    print(total)
    ```
    
5. **문제 4: 모험가 길드**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29f74427-3af6-4bf8-a453-b624b947a2b7/Untitled.png)
    
    ```python
    n = int(input())
    data = list(map(int, input().split())
    data.sort()
    result = 0
    count = 0
    for i in data:
    	count += 1
    	if count >= i:
    		result += 1
    		count = 0
    print(result)
    ```
    

### 2. 구현 알고리즘

---

1. **구현**
    - 머리속에 있는 알고리즘을 소스코드로 바꾸는 과정
        - 알고리즘은 간단하지만 코드가 지나치게 김
        - 실수 연산과 특정 소수점 자리 출력
        - 문자열을 특정 기준에 따라 끊어 처리
        - 적절한 라이브러리를 찾아 사용
2. **2차원 공간과 행렬**
    - 일반적으로 알고리즘 문제에서 2차원 공간 (이중 for문)은 행렬의 의미로 사용됨
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e78019b-e1ff-4a77-ad09-bad7a87af089/Untitled.png)
    
3. 문제 1: 상하좌우
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5981f5ed-91fd-4e17-8fb2-629ceb38739e/Untitled.png)
    
    - 일련의 명령에 따라 개체를 차례대로 이동시키는 시뮬레이션 유형
    - 해답 예시
        
        ```python
        n = int(input())
        x, y = 1, 1
        plans = input().split()
        
        dx = [0, 0, -1, -1]
        dy = [-1, 1, 0, 0]
        move_types = ["L", "R", "U", "D"]
        
        for plan in plans:
            for i in range(len(move_types)):
                if plan == move_types[i]:
                    nx = x + dx[i]
                    ny = y + dy[i]
            if nx < 1 or ny < 1 or nx > n or ny > n:
                continue
            x, y = nx, ny
        print(x, y)
        ```
        
4. 문제 2: 시각
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80ab89fc-863f-4eb7-9cfc-c21a1b17a51e/Untitled.png)
    
    - 가능한 경우의 수를 모두 검사해보는 완전 탐색(Brute Forcing) 유형
    
    ```python
    h = int(input())
    count = 0
    for i in range(h + 1):
    	for j in range(60):
    		for k in range(60):
    			if "3" in str(i) + str(j) + str(k)
    				count += 1
    print(count)
    ```
    
5. 문제 3: 왕실의 나이트
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fda965f0-486a-48db-b97d-b61144dde792/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/681e7e83-49cf-49ec-905b-2e5f20c89034/Untitled.png)
    
    - 요구사항대로 충실히 구현
    - 리스트를 이용해 8가지 방향에 대한 방향 벡터를 정의
    
    ```python
    input_data = input()
    row = int(input_data[1])
    column = int(ord(input_data[0])) - int(ord("a")) + 1
    
    steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, 2), (-2, 1)]
    result = 0
    for step in steps:
        next_row = row + step[0]
        next_column = column + step[1]
        if next_row >= 1 and next_row < 8 and next_column >= 1 and next_column <= 8:
            result += 1
    print(result)
    ```
    
6. 문제 4: 문자열 재정렬
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c6f2e1f-7973-4bde-b05b-7ebba80ade80/Untitled.png)
    
    ```python
    input_data = input()
    array = []
    total = 0
    for data in input_data:
        try:
            total += int(data)
        except:
            array.append(data)
    array.sort()
    if total != 0:
        array.append(str(total))
    print("".join(array))
    ```