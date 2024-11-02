# 수레 움직이기
## 문제 설명
`n` x `m` 크기 격자 모양의 퍼즐판이 주어집니다.

퍼즐판에는 빨간색 수레와 파란색 수레가 하나씩 존재합니다. 각 수레들은 자신의 시작 칸에서부터 자신의 도착 칸까지 이동해야 합니다.
모든 수레들을 각자의 도착 칸으로 이동시키면 퍼즐을 풀 수 있습니다.

당신은 각 턴마다 **반드시 모든 수레를 상하좌우로 인접한 칸 중 한 칸으로 움직여야 합니다.** 단, 수레를 움직일 때는 아래와 같은 규칙이 있습니다.

- 수레는 벽이나 격자 판 밖으로 움직일 수 없습니다.
- 수레는 자신이 방문했던 칸으로 움직일 수 없습니다.
- 자신의 도착 칸에 위치한 수레는 움직이지 않습니다. 계속 해당 칸에 고정해 놓아야 합니다.
- 동시에 두 수레를 같은 칸으로 움직일 수 없습니다.
- 수레끼리 자리를 바꾸며 움직일 수 없습니다.

예를 들어, 아래 그림처럼 `n` = 3, `m` = 2인 퍼즐판이 있습니다.

![image](https://github.com/user-attachments/assets/35088959-ab48-4a50-9a52-79a129e4fc0e)


- 속이 빨간색인 원은 빨간색 수레를 나타냅니다.
- 속이 파란색인 원은 파란색 수레를 나타냅니다.
- 테두리가 빨간색인 원은 빨간색 수레의 도착 칸을 나타냅니다.
- 테두리가 파란색인 원은 파란색 수레의 도착 칸을 나타냅니다.

위 퍼즐판은 아래와 같은 순서로 3턴만에 풀 수 있습니다.

![image](https://github.com/user-attachments/assets/bbd766a4-0ec2-4dc3-bf9b-4826f148fb85)


- 빨간색 사선이 처진 칸은 빨간색 수레가 방문했던 칸을 나타냅니다. 규칙에 따라 빨간색 수레는 빨간색 사선이 처진 칸(방문했던 칸)으로는 이동할 수 없습니다.
- 파란색 사선이 처진 칸은 파란색 수레가 방문했던 칸을 나타냅니다. 규칙에 따라 파란색 수레는 파란색 사선이 처진 칸(방문했던 칸)으로는 이동할 수 없습니다.

![image](https://github.com/user-attachments/assets/e4ebe2e6-a239-427e-bf4e-4b50cd31da48)


- 위처럼 동시에 수레를 같은 칸으로 움직일 수는 없습니다.

퍼즐판의 정보를 나타내는 2차원 정수 배열 `maze`가 매개변수로 주어집니다. 퍼즐을 푸는데 필요한 턴의 최솟값을 return 하도록 solution 함수를 완성해 주세요. 퍼즐을 풀 수 없는 경우 0을 return 해주세요.

## 제한사항
- 1 ≤ `maze`의 길이 (= 세로 길이) ≤ 4
  - 1 ≤ `maze[i]`의 길이 (= 가로 길이) ≤ 4
  - `maze[i][j]`는 0,1,2,3,4,5 중 하나의 값을 갖습니다.

|maze[i][j]|의미|
|-|-|
|0|빈칸|
|1|빨간 수레의 시작 칸|
|2|파란 수레의 시작 칸|
|3|빨간 수레의 도착 칸|
|4|파란 수레의 도착 칸|
|5|벽|
  - 빨간 수레의 시작 칸, 빨간 수레의 도착 칸, 파란 수레의 시작 칸, 파란 수레의 도착 칸은 퍼즐판에 1개씩 존재합니다.

## 입출력 예
|maze|result|
|-|-|
|[[1, 4], [0, 0], [2, 3]]|3|
|[[1, 0, 2], [0, 0, 0], [5, 0 ,5], [4, 0, 3]]|7|
|[[1, 5], [2, 5], [4, 5], [3, 5]]|0|
|[[4, 1, 2, 3]]|0|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

![image](https://github.com/user-attachments/assets/657322b6-31e3-424d-b32f-b3f26f4130c6)

7턴만에 퍼즐을 풀 수 있습니다. 다른 방법으로도 퍼즐을 풀 수 있지만 7턴보다 빠르게 풀 수는 없습니다.

### 입출력 예 #3

![image](https://github.com/user-attachments/assets/0e8d2bd6-e19b-491e-b284-58285945dd3c)

다음 턴에 파란색 수레가 파란색 수레의 도착 칸에 위치한 후 고정되어 빨간색 수레가 빨간색 수레의 도착 칸에 도착할 수 없게 됩니다.
퍼즐을 풀 수 없으므로 0을 return 해야 합니다.

### 입출력 예 #4

![image](https://github.com/user-attachments/assets/312f4240-dd5f-4db5-a450-e95a8a8d17f8)

수레는 서로 위치를 바꾸면서 움직일 수 없으므로 퍼즐을 풀 수 없습니다. 따라서 0을 return 해야 합니다.

# 내 풀이
```python
from collections import deque
import copy
import sys
sys.setrecursionlimit(1000000)

dr = [0, 1, 0, -1]
dc = [1, 0, -1, 0]

def check_out_and_wall(r, c, R, C, maze):
    if (not 0<=r<R) or (not 0<=c<C) or (maze[r][c]==5):
        return True
    else:
        return False


def dfs(red_cur, blue_cur, red_end, blue_end, maze, red_visited, blue_visited, R, C, count):
    rr, rc = red_cur
    br, bc = blue_cur
    min_count = R*C+1
    
    # 도착점에 도착하면 자리 고정
    if (rr, rc)==red_end: RDR, RDC = [0], [0]
    else: RDR, RDC = dr, dc
    if (br, bc)==blue_end: BDR, BDC = [0], [0]
    else: BDR, BDC = dr, dc
        
    for rdr, rdc in zip(RDR, RDC):
        cur_rr, cur_rc = rr+rdr, rc+rdc
        # 빨간색 혼자서 확인 조건
        if check_out_and_wall(cur_rr, cur_rc, R, C, maze) or ((rr,rc)!=(cur_rr, cur_rc) and ((cur_rr, cur_rc) in red_visited)):
            continue
        for bdr, bdc in zip(BDR, BDC):
            cur_br, cur_bc = br+bdr, bc+bdc
            # 파란색 혼자서 확인 조건
            if check_out_and_wall(cur_br, cur_bc, R, C, maze) or ((br,bc)!=(cur_br, cur_bc) and ((cur_br, cur_bc) in blue_visited)):
                continue
            
            # 빨간색+파란색 확인 조건
            if ((cur_rr,cur_rc)==(cur_br,cur_bc)) or ((cur_rr,cur_rc)==(br,bc) and (cur_br,cur_bc)==(rr,rc)):
                continue
                
            if (cur_rr,cur_rc)==red_end and (cur_br,cur_bc)==blue_end:
                return count+1
            
            new_red_visited = copy.deepcopy(red_visited)
            new_red_visited.add((cur_rr, cur_rc))
            new_blue_visited = copy.deepcopy(blue_visited)
            new_blue_visited.add((cur_br, cur_bc))
            
            min_count = min(min_count, dfs((cur_rr, cur_rc), (cur_br, cur_bc), red_end, blue_end, maze, new_red_visited, new_blue_visited, R, C, count+1))
        
    return min_count


def solution(maze):
    red_visited = set()
    blue_visited = set()
    red_start, red_end, blue_start, blue_end = 0, 0, 0, 0
    R, C = len(maze), len(maze[0])
    
    for i in range(R):
        for j in range(C):
            if maze[i][j]==1:
                red_start = (i,j)
            elif maze[i][j]==2:
                blue_start = (i,j)
            elif maze[i][j]==3:
                red_end = (i,j)
            elif maze[i][j]==4:
                blue_end = (i,j)
    red_visited.add(red_start)
    blue_visited.add(blue_start)
    
    answer = dfs(red_start, blue_start, red_end, blue_end, maze, red_visited, blue_visited, R, C, 0)
    return answer if answer!=R*C+1 else 0
```
정확성  테스트
```
테스트 1 〉	통과 (0.07ms, 10.3MB)
테스트 2 〉	통과 (0.05ms, 10.3MB)
테스트 3 〉	통과 (0.07ms, 10.4MB)
테스트 4 〉	통과 (0.07ms, 10.3MB)
테스트 5 〉	통과 (0.01ms, 10.4MB)
테스트 6 〉	통과 (8.76ms, 10.5MB)
테스트 7 〉	통과 (22.77ms, 10.4MB)
테스트 8 〉	통과 (98.47ms, 10.4MB)
테스트 9 〉	통과 (1121.95ms, 10.4MB)
테스트 10 〉	통과 (0.83ms, 10.5MB)
테스트 11 〉	통과 (0.24ms, 10.4MB)
테스트 12 〉	통과 (99.24ms, 10.5MB)
테스트 13 〉	통과 (4.14ms, 10.5MB)
테스트 14 〉	통과 (0.95ms, 10.3MB)
테스트 15 〉	통과 (0.55ms, 10.3MB)
테스트 16 〉	통과 (0.64ms, 10.4MB)
테스트 17 〉	통과 (0.62ms, 10.3MB)
테스트 18 〉	통과 (0.49ms, 10.3MB)
테스트 19 〉	통과 (2.80ms, 10.4MB)
테스트 20 〉	통과 (1.37ms, 10.3MB)
```
- 풀긴 풀었지만, 시작점을 `visited`에 넣고 시작하는 것을 까먹어서, 시간을 꽤 오래 썼다.. 주의하자
- 전략
  - 칸의 크기 범위가 작으므로, 시간초과는 생각하지 않았다.
  - 필요한 최소 턴을 구하면 되므로 BFS가 맞지만, 재귀를 쓰기 위해 DFS로 접근
  - `visited`는 `set()`로 구성
  - 도착해서 고정되는 경우는, 방향을 `[0.0]` 하나만 살펴보도록 함
  - 체크해야 하는 조건 5개를 두 부류로 나눔
    - 혼자 체크하는 조건
      - 판을 벗어나거나 벽인 경우
      - (도착점에 도착해서 이동하지 않은 경우를 제외하고) 방문했던 칸을 재방문한 경우
    - 같이 확인해야 하는 조건
      - 서로 같은 칸을 방문하는 경우
      - 서로 교차하는 경우
      
# 다른 사람의 풀이
```python
from collections import deque
import copy

def solution(maze):
    m,n = len(maze), len(maze[0])

    # Find start point
    red,blue = [],[]
    for i in range(m):
        for j in range(n):
            if maze[i][j] == 1:
                red = [i,j]
            elif maze[i][j] == 2:
                blue = [i,j]
    
    # Direction U,D,L,R
    dr = [-1, 1, 0, 0]
    dc = [0, 0, -1, 1]
    
    red_reached,blue_reached = False, False
    vr_,vb_ = [[[False for _ in range(n)] for _ in range(m)] for _ in range(2)]

    # First snapshot
    path = deque()
    path.append(red + blue + [vr_] + [vb_] + [0])

    # Start BFS
    while path:
        rr,rc,br,bc,vr_temp,vb_temp,count = path.popleft()
        
        vr = copy.deepcopy(vr_temp)
        vb = copy.deepcopy(vb_temp)
        vr[rr][rc] = True
        vb[br][bc] = True

        # End condition
        if maze[rr][rc] == 3 and maze[br][bc] == 4: 
            return count
            break
        # Select payoad not to move
        else:
            if maze[rr][rc] == 3:
                red_reached = True
                blue_reached = False
            elif maze[br][bc] == 4:
                red_reached = False
                blue_reached = True
            else: 
                red_reached = False
                blue_reached = False

        # seeking path
        for i in range(4):
            if blue_reached:
                r = [rr+dr[i], rc+dc[i]]
                if is_bounded(r,m,n):
                    if not (is_overlapped(r,(br,bc)) or is_wall(r,maze) or is_visited(r,vr)):
                        path.append([r[0],r[1],br,bc,vr,vb,count+1])
            elif red_reached:
                b = [br+dr[i], bc+dc[i]]
                if is_bounded(b,m,n):
                    if not (is_overlapped(b,(rr,rc)) or is_wall(b,maze) or is_visited(b,vb)):
                        path.append([rr,rc,b[0],b[1],vr,vb,count+1])
            else:
                r = [rr+dr[i], rc+dc[i]]
                if is_bounded(r,m,n):
                    if not (is_wall(r,maze) or is_visited(r,vr)):
                        for j in range(4):
                            b = [br+dr[j], bc+dc[j]]
                            if is_bounded(b,m,n):
                                if not (is_overlapped(r,b) or is_switched(r,(br,bc),b,(rr,rc)) or is_wall(b,maze) or is_visited(b,vb)):
                                    path.append([r[0],r[1],b[0],b[1],vr,vb,count+1])
    return 0

# Conditions
def is_bounded(x,m,n):
    return 0 <= x[0] < m and 0 <= x[1] < n

def is_wall(x,map):
    return map[x[0]][x[1]] == 5

def is_overlapped(x,y):
    return (x[0],x[1]) == (y[0],y[1])

def is_switched(x,y,z,w):
    return (x[0],x[1]) == (y[0],y[1]) and (z[0],z[1]) == (w[0],w[1])

def is_visited(n,v):
    return v[n[0]][n[1]]
```
- BFS로 풀었다.
- 나머지 풀이들은 대부분 비슷
