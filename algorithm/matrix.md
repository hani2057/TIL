# 2차원 배열

### 배열 순회

- n x m 배열의 n*m개의 모든 원소를 빠짐없이 조사하는 방법
- 행 우선 순회
    
    ```python
    for i in range(n):
        for j in range(m):
            Array[i][j] # 필요한 연산 수행
    ```
    
- 열 우선 순회
    
    ```python
    for i in range(n):
        for j in range(m):
            Array[i][j] # 필요한 연산 수행
    ```
    
- 지그재그 순회
    
    ```python
    for i in range(n):
        for j in range(m):
            Array[i][j + (m - 1 - 2*j) * (i % 2)] # 필요한 연산 수행
    # i % 2 가 1 즉 홀수행에 대해 Array[i][m - 1 -j]가 됨
    # i % 2 가 0 즉 짝수행에 대해 Array[i][j]가 됨
    ```
    

### 델타를 이용한 2차 배열 탐색

- 2차 배열의 한 좌표에서 인접 배열 요소를 탐색하는 방법
    
    ```python
    di = [0, 0, -1, 1] # 상하좌우
    dj = [-1, 1, 0, 0]
    for i in range(n):
        for j in range(m):
            for k in range(4):
                ni = i + di[k]
                nj = j + dj[k]
                if 0 <= ni < n and 0 <= nj < m: # 유효한 인덱스인지 체크
                    test(arr[ni][nj]
    ```
    
- 동일한 원리로 대각선 등에도 활용 가능

### 전치 행렬

```python
for i in range(n):
    for j in range(n):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
# 대각선 기준으로 우상단과 좌하단의 값을 대칭적으로 바꾸는 코드
```

```python
# 참고
lst = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
for i in lst:
    print(*i)

lst1 = list(map(list, zip(*lst)))
print(lst1)
for i in lst:
    print(*i)

lst2 = zip(*lst)[::-1]
for i in lst2:
    print(*i)

lst3 = list(zip(*lst[::-1]))
for i in lst3:
    print(*i)
```

### 배열의 부분집합(비트 연산자 사용)

- 비트 연산자
    - &: 비트 단위로 AND 연산
    - |: 비트 단위로 OR 연산
    - <<: 피연산자의 비트 열을 왼쪽으로 이동
    - >>: 피연산자의 비트 열을 오른쪽으로 이동
- 1 << n
    - n부터 1(2의 0승)까지 비트를 왼쪽으로 이동
    - 즉 1이 1개, 0이 n개가 되어 2의 n승이 됨
    - 부분집합의 수를 1 << n으로 나타낼 수 있음
- i & (1 << j)
    - i의 j번째 비트가 1인지 아닌지를 검사한다
    - 1111 & 1000 → 1000
- 간결하게 부분집합을 생성하는 방법
    
    ```python
    n = len(arr)
    
    for i in range(1 << n): # 모든 부분집합에 대해
        for j in range(n): # 원소의 수만큼 비트를 비교
            if i & (1 << j): # i의 j번 비트가 1인 경우
                print(arr[j], end=", ") # j번 원소 출력
        print()
    print()
    ```
    
    ```python
    n = len(arr)
    res = []
    for i in range(2**n):
        tmp_lst = []
        for j in range(n):
            if i & (1 << j):
                tmp_lst.append(arr[j])
            res.append(tmp_lst)
    for i in res:
        print(i)
    ```
