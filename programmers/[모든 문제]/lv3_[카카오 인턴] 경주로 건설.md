# [카카오 인턴] 경주로 건설
## 문제 설명
![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/0f8d28be-5eda-4410-a071-d57f219ae7dc)

건설회사의 설계사인 `죠르디`는 고객사로부터 자동차 경주로 건설에 필요한 견적을 의뢰받았습니다.

제공된 경주로 설계 도면에 따르면 경주로 부지는 `N x N` 크기의 정사각형 격자 형태이며 각 격자는 `1 x 1` 크기입니다.

설계 도면에는 각 격자의 칸은 `0` 또는 `1` 로 채워져 있으며, `0`은 칸이 비어 있음을 `1`은 해당 칸이 벽으로 채워져 있음을 나타냅니다.

경주로의 출발점은 (0, 0) 칸(좌측 상단)이며, 도착점은 (N-1, N-1) 칸(우측 하단)입니다. 죠르디는 출발점인 (0, 0) 칸에서 출발한 자동차가 도착점인 (N-1, N-1) 칸까지 무사히 도달할 수 있게 중간에 끊기지 않도록 경주로를 건설해야 합니다.

경주로는 상, 하, 좌, 우로 인접한 두 빈 칸을 연결하여 건설할 수 있으며, 벽이 있는 칸에는 경주로를 건설할 수 없습니다.

이때, 인접한 두 빈 칸을 상하 또는 좌우로 연결한 경주로를 `직선 도로` 라고 합니다.

또한 두 `직선 도로`가 서로 직각으로 만나는 지점을 `코너` 라고 부릅니다.

건설 비용을 계산해 보니 `직선 도로` 하나를 만들 때는 100원이 소요되며, `코너`를 하나 만들 때는 500원이 추가로 듭니다.

죠르디는 견적서 작성을 위해 경주로를 건설하는 데 필요한 최소 비용을 계산해야 합니다.


예를 들어, 아래 그림은 `직선 도로` 6개와 `코너` 4개로 구성된 임의의 경주로 예시이며, 건설 비용은 6 x 100 + 4 x 500 = 2600원 입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/06cc1e0a-9674-4c6b-a044-76f6a3b851ab)

또 다른 예로, 아래 그림은 직선 도로 4개와 코너 1개로 구성된 경주로이며, 건설 비용은 4 x 100 + 1 x 500 = 900원 입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/df690196-e091-4ffd-b7e5-89a070ce7fb1)

도면의 상태(0은 비어 있음, 1은 벽)을 나타내는 2차원 배열 board가 매개변수로 주어질 때, 경주로를 건설하는데 필요한 최소 비용을 return 하도록 solution 함수를 완성해주세요.

## [제한사항]
- board는 2차원 정사각 배열로 배열의 크기는 3 이상 25 이하입니다.
- board 배열의 각 원소의 값은 0 또는 1 입니다.
  - 도면의 가장 왼쪽 상단 좌표는 (0, 0)이며, 가장 우측 하단 좌표는 (N-1, N-1) 입니다.
  - 원소의 값 0은 칸이 비어 있어 도로 연결이 가능함을 1은 칸이 벽으로 채워져 있어 도로 연결이 불가능함을 나타냅니다.
- board는 항상 출발점에서 도착점까지 경주로를 건설할 수 있는 형태로 주어집니다.
- 출발점과 도착점 칸의 원소의 값은 항상 0으로 주어집니다.

## 입출력 예
|board|result|
|-|-|
|[[0,0,0],[0,0,0],[0,0,0]]|900|
[[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0],[0,0,0,0,0,1,0,0],[0,0,0,0,1,0,0,0],[0,0,0,1,0,0,0,1],[0,0,1,0,0,0,1,0],[0,1,0,0,0,1,0,0],[1,0,0,0,0,0,0,0]]|3800|
|[[0,0,1,0],[0,0,0,0],[0,1,0,1],[1,0,0,0]]|2100|
|[[0,0,0,0,0,0],[0,1,1,1,1,0],[0,0,1,0,0,0],[1,0,0,1,0,1],[0,1,0,0,0,1],[0,0,0,0,0,0]]|3200|

## 입출력 예에 대한 설명
### 입출력 예 #1

본문의 예시와 같습니다.

### 입출력 예 #2

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/1fed8129-0406-4195-a26d-fc396ca160df)

위와 같이 경주로를 건설하면 `직선 도로` 18개, `코너` 4개로 총 3800원이 듭니다.

### 입출력 예 #3

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/08742b88-8b05-409e-8a82-4b67968e375e)

위와 같이 경주로를 건설하면 `직선 도로` 6개, `코너` 3개로 총 2100원이 듭니다.

### 입출력 예 #4

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/5f28aefe-ef81-4e31-a7ca-650833bd6145)

붉은색 경로와 같이 경주로를 건설하면 `직선 도로` 12개, `코너` 4개로 총 3200원이 듭니다.

만약, 파란색 경로와 같이 경주로를 건설한다면 `직선 도로` 10개, `코너` 5개로 총 3500원이 들며, 더 많은 비용이 듭니다.

# 내 풀이
```python
from collections import deque

def solution(board):
    l = len(board)
    q = deque([])
    direcs = {'U':(-1,0), 'D':(1,0), 'L':(0,-1), 'R':(0,1)}
    oppos = {'U':'D', 'D':'U', 'L':'R', 'R':'L'}
    
    board[0][0]=-1
    
    if board[0][1]==0:
        board[0][1]=100
        q.append((0, 1, 'R', 100))
    if board[1][0]==0:
        board[1][0]=100
        q.append((1, 0, 'D', 100))
    
    while q:
        x, y, d, cost = q.popleft()
        
        corners = list(oppos.keys())
        corners.remove(d)
        corners.remove(oppos[d])
        
        for i, di in enumerate([d]+corners):
            if i==0: c = cost+100
            else: c = cost+600
            
            x_dir, y_dir = direcs[di]
            if x+x_dir>=0 and x+x_dir<l and y+y_dir>=0 and y+y_dir<l and board[x+x_dir][y+y_dir]!=1:
                if board[x+x_dir][y+y_dir]==0 or board[x+x_dir][y+y_dir]>=c:
                    board[x+x_dir][y+y_dir]=c
                    q.append((x+x_dir, y+y_dir, di, c))
        
    return board[-1][-1]
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.4MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.02ms, 10.1MB)
테스트 4 〉	통과 (0.05ms, 10.1MB)
테스트 5 〉	통과 (0.04ms, 10.2MB)
테스트 6 〉	통과 (0.24ms, 10.3MB)
테스트 7 〉	통과 (0.27ms, 10.3MB)
테스트 8 〉	통과 (0.23ms, 10.2MB)
테스트 9 〉	통과 (0.28ms, 10.2MB)
테스트 10 〉	통과 (0.50ms, 10.2MB)
테스트 11 〉	통과 (2.50ms, 10.4MB)
테스트 12 〉	통과 (2.71ms, 10.1MB)
테스트 13 〉	통과 (0.17ms, 10.2MB)
테스트 14 〉	통과 (0.21ms, 10.3MB)
테스트 15 〉	통과 (0.66ms, 10.3MB)
테스트 16 〉	통과 (0.84ms, 10.1MB)
테스트 17 〉	통과 (1.79ms, 10.2MB)
테스트 18 〉	통과 (2.53ms, 10.2MB)
테스트 19 〉	통과 (1.87ms, 10.1MB)
테스트 20 〉	통과 (1.15ms, 10.1MB)
테스트 21 〉	통과 (0.76ms, 10.2MB)
테스트 22 〉	통과 (0.07ms, 10.4MB)
테스트 23 〉	통과 (0.06ms, 10.2MB)
테스트 24 〉	통과 (0.12ms, 10.1MB)
테스트 25 〉	실패 (0.04ms, 10.2MB)
```
- 전략
  - 맵 상에서 `(N-1, N-1)`까지 최소 비용으로 도달하는 문제이므로, DP+BFS로 풀어야겠다 생각
    - DFS vs BFS 중에 고민했지만, 한 경로의 최소 비용을 구하기 위해서는 상하좌우 네 방향의 비용을 고려해야 하므로 BFS로 낮은 인덱스부터 써내려가는게 적합하다 생각
  - 이동할 때에는 직선이나 코너를 고려하기 위해, 이전 방향을 고려하여 직선은 +100 코너는 +600
    - 해당 위치로 더 적은 비용으로 도달할 때 마다 update & 큐에 넣음
      - 이 때, board[x][y] <= cost로 해야함 (특정 board 칸이 같은 cost일 때 그 곳에 들어온 방향에 따라 다음 칸의 cost에 영향을 미칠 수 있기 때문)
- 마지막 케이스에서만 실패해서 결국 다른 사람의 풀이 참고,,
- 다른 사람의 풀이도 내 풀이와 전략은 동일했지만, 어디였는지 내 풀이의 오류를 결국 찾지 못했다

# 다른 사람의 풀이
```python
from collections import deque

dx = [0, 0, 1, -1]
dy = [1, -1, 0, 0]

def bfs(board):
    n = len(board)
    price = [[int(1e9)] * n for _ in range(n)]
    price[0][0] = 0

    queue = deque()
    queue.append((0, 0, 0, 5))  # (시작X, 시작Y, 시작Cost, 시작방향)

    while queue:
        x, y, c, z = queue.popleft()

        if x == n - 1 and y == n - 1:
            continue

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            nz = i

            if nx < 0 or nx >= n or ny < 0 or ny >= n:
                continue
            if board[nx][ny] == 1:
                continue
            
            if z==5:
                nc = c + 100

            elif nz == z:
                nc = c + 100
                
            else:
                nc = c + 600

            if nc <= price[nx][ny]:
                price[nx][ny] = nc
                queue.append((nx, ny, nc, i))

    return price[-1][-1]


def solution(board):
    n = len(board)
    answer = bfs(board)
    return answer
```
- 내 풀이랑 전략이 동일
- 다른 점이라면
  - board가 아닌 price라는 새로운 배열을 또 정의
  - board의 칸에 들어온 방향으로 다시 나가는 경우까지 고려
- 이 풀이도 운이 좋았을 뿐, dx, dy의 순서를 바꾸면 원래는 틀린 풀이이다..
- 따라서 맨 처음 시작 방향을 `(1,0)` `(0,1)`로 나누어서 더 작은 값을 리턴해야함!
```python
from collections import deque

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]


def bfs(board, dir):
    n = len(board)
    price = [[int(1e9)] * n for _ in range(n)]
    price[0][0] = 0

    queue = deque()
    queue.append((0, 0, 0, dir))  # (시작X, 시작Y, 시작Cost, 시작방향)

    while queue:
        x, y, c, z = queue.popleft()

        if x == n - 1 and y == n - 1:
            continue

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            nz = i

            if nx < 0 or nx >= n or ny < 0 or ny >= n:
                continue
            if board[nx][ny] == 1:
                continue

            if nz == z:
                nc = c + 100
            else:
                nc = c + 600

            if nc < price[nx][ny]:
                price[nx][ny] = nc
                queue.append((nx, ny, nc, i))

    return price[-1][-1]


def solution(board):
    answer = min(bfs(board, 3), bfs(board, 1))
    return answer
```
