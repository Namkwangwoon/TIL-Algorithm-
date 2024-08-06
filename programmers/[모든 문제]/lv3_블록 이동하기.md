# 블록 이동하기
## 문제 설명
로봇개발자 "무지"는 한 달 앞으로 다가온 "카카오배 로봇경진대회"에 출품할 로봇을 준비하고 있습니다. 준비 중인 로봇은 `2 x 1` 크기의 로봇으로 "무지"는 **"0"** 과 **"1"** 로 이루어진 `N x N` 크기의 지도에서 `2 x 1` 크기인 로봇을 움직여 **(N, N)** 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 **(1, 1)** 로 하며 지도 내에 표시된 숫자 **"0"** 은 빈칸을 **"1"** 은 벽을 나타냅니다. 로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 로봇은 처음에 아래 그림과 같이 좌표 **(1, 1)** 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.

![image](https://github.com/user-attachments/assets/8460a28c-33f3-4b3c-ad17-87c9323bc731)

로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 예를 들어, 위 그림에서 오른쪽으로 한 칸 이동한다면 **(1, 2), (1, 3)** 두 칸을 차지하게 되며, 아래로 이동한다면 **(2, 1), (2, 2)** 두 칸을 차지하게 됩니다. 로봇이 차지하는 두 칸 중 어느 한 칸이라도 **(N, N)** 위치에 도착하면 됩니다.

로봇은 다음과 같이 조건에 따라 회전이 가능합니다.

![image](https://github.com/user-attachments/assets/42b7bd33-a0bd-451d-88aa-90c021b99a7b)

위 그림과 같이 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

**"0"** 과 **"1"** 로 이루어진 지도인 board가 주어질 때, 로봇이 **(N, N)** 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- board의 한 변의 길이는 5 이상 100 이하입니다.
- board의 원소는 0 또는 1입니다.
- 로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
- 로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

## 입출력 예
|board|result|
|-|-|
|[[0, 0, 0, 1, 1],[0, 0, 0, 1, 0],[0, 1, 0, 1, 1],[1, 1, 0, 0, 1],[0, 0, 0, 0, 0]]|7|

## 입출력 예에 대한 설명
문제에 주어진 예시와 같습니다.

로봇이 오른쪽으로 한 칸 이동 후, (1, 3) 칸을 축으로 반시계 방향으로 90도 회전합니다. 다시, 아래쪽으로 3칸 이동하면 로봇은 (4, 3), (5, 3) 두 칸을 차지하게 됩니다. 이제 (5, 3)을 축으로 시계 방향으로 90도 회전 후, 오른쪽으로 한 칸 이동하면 (N, N)에 도착합니다. 따라서 목적지에 도달하기까지 최소 7초가 걸립니다.

# 내 풀이
- 전형적인 구현 문제이지만, 시간이 너무 오래걸려서 결국 다른 사람의 풀이를 참고했다.
```python
from collections import deque

direc_r = [0, 1]
direc_c = [1, 0]

dr = [0, 1, 0, -1]
dc = [1, 0, -1, 0]

def check_move(r, c, direc, board, visited, N, move):
    new_que = deque([])
    
    for i in range(4):  # 네 방향으로 이동 가능한지 check
        if 0<=r+dr[i]<N and 0<=c+dc[i]<N and 0<=r+dr[i]+direc_r[direc]<N and 0<=c+dc[i]+direc_c[direc]<N:  # 범위 안의 인덱스
            if board[r+dr[i]][c+dc[i]]==0 and board[r+dr[i]+direc_r[direc]][c+dc[i]+direc_c[direc]]==0:  # board 값이 0
                if not (r+dr[i], c+dc[i], direc) in visited:  # 방문하지 않은 위치
                    new_que.append((r+dr[i], c+dc[i], direc, move+1))
                    visited.add((r+dr[i], c+dc[i], direc))

    return new_que


def check_rotate(r, c, direc, board, visited, N, move):
    new_que = deque([])

    if direc==0:
        for i, minus_plus in enumerate([-1, 1]):  # 위, 아래
            if 0<=r-1+i and r+i<N:  # 범위 안의 인덱스
                if board[r+minus_plus][c]==0 and board[r+minus_plus][c+1]==0:  # board 값이 0
                    for j in range(2):
                        if (r-1+i, c+j, direc+1) not in visited:
                            new_que.append((r-1+i, c+j, direc+1, move+1))
                            visited.add((r-1+i, c+j, direc+1))
        
    else:
        for i, minus_plus in enumerate([-1, 1]):  # 왼쪽, 오른쪽
            if 0<=c-1+i and c+i<N:  # 범위 안의 인덱스
                if board[r][c+minus_plus]==0 and board[r+1][c+minus_plus]==0:  # board 값이 0
                    for j in range(2):
                        if (r+j, c-1+i, direc-1) not in visited:
                            new_que.append((r+j, c-1+i, direc-1, move+1))
                            visited.add((r+j, c-1+i, direc-1))
    
    return new_que
    
    
def solution(board):
    N = len(board)
    que = deque([(0, 0, 0, 0)])
    visited = set([(0, 0, 0)])
    
    while que:
        r, c, direc, move = que.popleft()
        
        if r+dr[direc]==N-1 and c+dc[direc]==N-1:
            return move
        
        que += check_move(r, c, direc, board, visited, N, move)
        
        que += check_rotate(r, c, direc, board, visited, N, move)
```
- 전략
  - 최소의 움직임으로 **(N, N)** 에 도달해야 하므로, BFS
  - 로봇은 가로 형태일 경우 왼쪽을 기준으로, 세로 형태일 경우 위쪽을 기준으로 좌표, 방향을 설정하여 중복을 방지
  - 이동 및 회전이 가능한 조건을 꼼꼼히 체크하고, 가능한 경우를 que에 넣어줌과 동시에 visited에 표시
  - 핵심은, visited를 어떻게 구성할지이다.
    - 로봇이 두 칸을 차지하고, 방향도 존재하므로, 행렬 형태의 visited를 사용하는 것은 복잡하다.
    - 다른 사람의 풀이에서 참고했듯, set()를 활용하여 해시 형태로 visited 사용 가능
      - 로봇의 위치 및 방향을 set()에 추가 및 포함 여부를 확인하면서, visited를 체크
