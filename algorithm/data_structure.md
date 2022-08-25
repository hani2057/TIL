## DFS(Depth First Search)
- 완전탐색의 한 방법

```python
# 스택 사용 구현
stack = []
stack.append(s)

# 중복 방문 x
visited = set()
visited.add(s)

while stack:
    value = stack[-1]

		for next in Graph(value):
		    if next not in visited:
            visited.add(next)
		        stack.append(next)
		        break # 완전탐색에선 필요x / dfs는 필요
		else:
		    stack.pop()
```

```python
# 재귀 사용 구현
def dfs(value):
    # value를 가지고 동작
    for next in Graph(vlaue):
        if next not in visited:
            # next를 가지고 동작
						visited.add(next)
            def(next)
    return

# 좀 더 정리하면 아래와 같다
def dfs(value):
    visited.add(next)
    for next in Graph(value):
        if next not in visited:
            def(next)
```

<br>

## BFS(Breadth First Search)
- 완전탐색의 한 방법
- 파이썬은 collections.deque 모듈 사용하면 좋다.

```python
# 완전탐색
q = []

visited = set()
visited.add(s)

while q:
    value = q.popleft()
		for next in Graph(value):
		    if next not in visited:
            visited.add(next)
		        q.append(next)

# Queue는 방문처리하는 위치에 따라 효율이 매우 달라진다.
while q:
    value = q.popleft()
		visited.add(value) # 이렇게 할 경우 큐에 중복적으로 추가될 수 있음 
		for next in Graph(value):
		    if next not in visited:
		        q.append(next)

```

### Time complexity 비교

- list pop(0) : O(n)
- deque popleft : O(1)
- a in list : O(n)
- a in set : O(1)
- list[i][j] = 1 : O(1)