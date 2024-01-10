# 게임 맵 최단거리
## 문제 설명
ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/803a11c3-8315-48eb-9af1-053e92b007d2)

위 그림에서 검은색 부분은 벽으로 막혀있어 갈 수 없는 길이며, 흰색 부분은 갈 수 있는 길입니다. 캐릭터가 움직일 때는 동, 서, 남, 북 방향으로 한 칸씩 이동하며, 게임 맵을 벗어난 길은 갈 수 없습니다.

아래 예시는 캐릭터가 상대 팀 진영으로 가는 두 가지 방법을 나타내고 있습니다.

- 첫 번째 방법은 11개의 칸을 지나서 상대 팀 진영에 도착했습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/afd7f7d9-18e1-4edf-b0af-383e79de44f1)

- 두 번째 방법은 15개의 칸을 지나서 상대팀 진영에 도착했습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/c9484297-4cec-4d26-a4fa-8e73da28f301)

위 예시에서는 첫 번째 방법보다 더 빠르게 상대팀 진영에 도착하는 방법은 없으므로, 이 방법이 상대 팀 진영으로 가는 가장 빠른 방법입니다.

만약, 상대 팀이 자신의 팀 진영 주위에 벽을 세워두었다면 상대 팀 진영에 도착하지 못할 수도 있습니다. 예를 들어, 다음과 같은 경우에 당신의 캐릭터는 상대 팀 진영에 도착할 수 없습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/d7403409-9b4a-4fae-9dae-cf632bb633e7)

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

## 제한사항
- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.
  - n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.
- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.
- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.

## 입출력 예
|maps|answer|
|-|-|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,1],[0,0,0,0,1]]|11|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,0],[0,0,0,0,1]]|-1|

## 입출력 예 설명
### 입출력 예 #1
주어진 데이터는 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/90b34a3d-fa47-4137-8eb4-4b38fe9d979c)

캐릭터가 적 팀의 진영까지 이동하는 가장 빠른 길은 다음 그림과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/90f5c329-1d81-489d-882b-4ab2321c39dd)

따라서 총 11칸을 캐릭터가 지나갔으므로 11을 return 하면 됩니다.

### 입출력 예 #2
문제의 예시와 같으며, 상대 팀 진영에 도달할 방법이 없습니다. 따라서 -1을 return 합니다.

# 내 풀이1
- 최단 경로만 찾으면 되므로, BFS를 써야겠다 생각
```python
from collections import deque
        
def solution(maps):
    visited = [[0 for i in range(len(maps[0]))] for i in range(len(maps))]
    
    q = deque([(visited, (0, 0), 0)])
    
    while q:
        vis, pos, dist = q.popleft()
        vis[pos[0]][pos[1]] = 1
        dist+=1
        
        if pos[0]==len(maps)-1 and pos[1]==len(maps[0])-1:
            return dist
        
        if pos[0]+1<len(maps) and vis[pos[0]+1][pos[1]]==0 and maps[pos[0]+1][pos[1]]==1:
            q.append((vis.copy(), (pos[0]+1, pos[1]), dist))        
        if pos[1]+1<len(maps[0]) and vis[pos[0]][pos[1]+1]==0 and maps[pos[0]][pos[1]+1]==1:
            q.append((vis.copy(), (pos[0], pos[1]+1), dist))
        if pos[0]-1>=0 and vis[pos[0]-1][pos[1]]==0 and maps[pos[0]-1][pos[1]]==1:
            q.append((vis.copy(), (pos[0]-1, pos[1]), dist))        
        if pos[1]-1>=0 and vis[pos[0]][pos[1]-1]==0 and maps[pos[0]][pos[1]-1]==1:
            q.append((vis.copy(), (pos[0], pos[1]-1), dist))
            
    return -1
```
정확성  테스트
```
테스트 1 〉	통과 (0.08ms, 10.2MB)
테스트 2 〉	통과 (0.04ms, 10.2MB)
테스트 3 〉	통과 (0.07ms, 10.1MB)
테스트 4 〉	통과 (0.06ms, 10.2MB)
테스트 5 〉	통과 (0.07ms, 10.2MB)
테스트 6 〉	통과 (0.10ms, 10.2MB)
테스트 7 〉	통과 (0.15ms, 10.2MB)
테스트 8 〉	통과 (0.04ms, 10.2MB)
테스트 9 〉	통과 (0.14ms, 10.3MB)
테스트 10 〉	통과 (0.15ms, 10.2MB)
테스트 11 〉	통과 (0.07ms, 10.3MB)
테스트 12 〉	통과 (0.04ms, 10.3MB)
테스트 13 〉	통과 (0.06ms, 10.3MB)
테스트 14 〉	통과 (0.06ms, 10.2MB)
테스트 15 〉	통과 (0.08ms, 10.3MB)
테스트 16 〉	통과 (0.04ms, 10.4MB)
테스트 17 〉	통과 (0.10ms, 10.3MB)
테스트 18 〉	통과 (0.03ms, 10.3MB)
테스트 19 〉	통과 (0.02ms, 10.3MB)
테스트 20 〉	통과 (0.01ms, 10.4MB)
테스트 21 〉	통과 (0.02ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	실패 (시간 초과)
테스트 2 〉	실패 (시간 초과)
테스트 3 〉	실패 (시간 초과)
테스트 4 〉	실패 (시간 초과)
```
- 효율성 테스트를 하나도 통과하지 못함,,
- 재귀 시도?
- Visited 때문인가?

# 내 풀이2
- visited 대신, maps의 칸에 최단 경로를 넣어버리는 방법
```python
from collections import deque

def solution(maps):
    q = deque([((0,0), 1)])
    
    while q:
        pos, dist = q.popleft()
        maps[pos[0]][pos[1]] = dist
        
        if pos[0]+1<len(maps) and maps[pos[0]+1][pos[1]]==1:
            q.append(((pos[0]+1, pos[1]), dist+1))
        if pos[1]+1<len(maps[0]) and maps[pos[0]][pos[1]+1]==1:
            q.append(((pos[0], pos[1]+1), dist+1))
        if pos[0]-1>=0 and maps[pos[0]-1][pos[1]]==1:
            q.append(((pos[0]-1, pos[1]), dist+1))
        if pos[1]-1>=0 and maps[pos[0]][pos[1]-1]==1:
            q.append(((pos[0], pos[1]-1), dist+1))
            
    if maps[-1][-1]!=1:
        return maps[-1][-1]
    else:
        return -1
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.3MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.07ms, 10.3MB)
테스트 4 〉	통과 (0.04ms, 10.3MB)
테스트 5 〉	통과 (0.03ms, 10.3MB)
테스트 6 〉	통과 (0.04ms, 10.3MB)
테스트 7 〉	통과 (0.09ms, 10.2MB)
테스트 8 〉	통과 (0.04ms, 10.3MB)
테스트 9 〉	통과 (0.09ms, 10.4MB)
테스트 10 〉	통과 (0.13ms, 10.3MB)
테스트 11 〉	통과 (0.05ms, 10.1MB)
테스트 12 〉	통과 (0.02ms, 10.2MB)
테스트 13 〉	통과 (0.07ms, 10.3MB)
테스트 14 〉	통과 (0.03ms, 10.3MB)
테스트 15 〉	통과 (0.05ms, 10.2MB)
테스트 16 〉	통과 (0.02ms, 10.3MB)
테스트 17 〉	통과 (0.05ms, 10.1MB)
테스트 18 〉	통과 (0.01ms, 10.1MB)
테스트 19 〉	통과 (0.01ms, 10.2MB)
테스트 20 〉	통과 (0.00ms, 10.3MB)
테스트 21 〉	통과 (0.01ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	실패 (시간 초과)
테스트 2 〉	실패 (시간 초과)
테스트 3 〉	실패 (시간 초과)
테스트 4 〉	실패 (시간 초과)
```
- 시간이 좀 줄긴 했지만, 효율성 테스트는 여전히 실패
- 결국 다른 사람의 코드를 참고,,

# 다른 사람의 풀이
```python
from collections import deque

def solution(maps):
    x_move = [1, 0, -1, 0]
    y_move = [0, 1, 0, -1]

    x_h, y_h = (len(maps[0]), len(maps))
    queue = deque([(0, 0, 1)])

    while queue:
        x, y, d = queue.popleft()

        for i in range(4):
            nx = x + x_move[i]
            ny = y + y_move[i]

            if nx > -1 and ny > -1 and nx < x_h and ny < y_h:
                if maps[ny][nx] == 1 or maps[ny][nx] > d + 1:
                    maps[ny][nx] = d + 1
                    if nx == x_h - 1 and ny == y_h - 1:
                        return d + 1

                    queue.append((nx, ny, d + 1))

    return -1
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.03ms, 10.1MB)
테스트 4 〉	통과 (0.03ms, 10.2MB)
테스트 5 〉	통과 (0.03ms, 10.4MB)
테스트 6 〉	통과 (0.07ms, 10.2MB)
테스트 7 〉	통과 (0.07ms, 10.1MB)
테스트 8 〉	통과 (0.03ms, 10.4MB)
테스트 9 〉	통과 (0.06ms, 10.3MB)
테스트 10 〉	통과 (0.11ms, 10.3MB)
테스트 11 〉	통과 (0.03ms, 10.1MB)
테스트 12 〉	통과 (0.03ms, 10.1MB)
테스트 13 〉	통과 (0.03ms, 10.2MB)
테스트 14 〉	통과 (0.03ms, 10.1MB)
테스트 15 〉	통과 (0.03ms, 10.3MB)
테스트 16 〉	통과 (0.02ms, 10.4MB)
테스트 17 〉	통과 (0.04ms, 10.3MB)
테스트 18 〉	통과 (0.01ms, 10.4MB)
테스트 19 〉	통과 (0.01ms, 10.2MB)
테스트 20 〉	통과 (0.01ms, 10.2MB)
테스트 21 〉	통과 (0.01ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (13.51ms, 10.3MB)
테스트 2 〉	통과 (3.46ms, 10.3MB)
테스트 3 〉	통과 (8.48ms, 10.2MB)
테스트 4 〉	통과 (5.20ms, 10.2MB)
```
- 같은 BFS인거 같음
- 어떤게 효율성을 높여줬는지 알아봄

# 내 풀이2-1
```python
from collections import deque

def solution(maps):
    q = deque([((0,0), 1)])
    maps_w, maps_h = len(maps), len(maps[0])
    
    while q:
        pos, dist = q.popleft()
        
        if pos[0]+1<maps_w and maps[pos[0]+1][pos[1]]==1:
            maps[pos[0]+1][pos[1]] = dist+1
            q.append(((pos[0]+1, pos[1]), dist+1))
        if pos[1]+1<maps_h and maps[pos[0]][pos[1]+1]==1:
            maps[pos[0]][pos[1]+1] = dist+1
            q.append(((pos[0], pos[1]+1), dist+1))
        if pos[0]-1>=0 and maps[pos[0]-1][pos[1]]==1:
            maps[pos[0]-1][pos[1]] = dist+1
            q.append(((pos[0]-1, pos[1]), dist+1))
        if pos[1]-1>=0 and maps[pos[0]][pos[1]-1]==1:
            maps[pos[0]][pos[1]-1] = dist+1
            q.append(((pos[0], pos[1]-1), dist+1))
            
    if maps[-1][-1]!=1:
        return maps[-1][-1]
    else:
        return -1
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.4MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.04ms, 10.3MB)
테스트 4 〉	통과 (0.03ms, 10.2MB)
테스트 5 〉	통과 (0.03ms, 10.4MB)
테스트 6 〉	통과 (0.03ms, 10.2MB)
테스트 7 〉	통과 (0.05ms, 10.3MB)
테스트 8 〉	통과 (0.06ms, 10.4MB)
테스트 9 〉	통과 (0.04ms, 10.3MB)
테스트 10 〉	통과 (0.05ms, 10.3MB)
테스트 11 〉	통과 (0.02ms, 10.4MB)
테스트 12 〉	통과 (0.03ms, 10.3MB)
테스트 13 〉	통과 (0.03ms, 10.4MB)
테스트 14 〉	통과 (0.03ms, 10.3MB)
테스트 15 〉	통과 (0.02ms, 10.3MB)
테스트 16 〉	통과 (0.01ms, 10.3MB)
테스트 17 〉	통과 (0.04ms, 10.3MB)
테스트 18 〉	통과 (0.01ms, 10.4MB)
테스트 19 〉	통과 (0.01ms, 10.4MB)
테스트 20 〉	통과 (0.00ms, 10.2MB)
테스트 21 〉	통과 (0.01ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (9.12ms, 10.2MB)
테스트 2 〉	통과 (2.12ms, 10.2MB)
테스트 3 〉	통과 (5.37ms, 10.2MB)
테스트 4 〉	통과 (3.66ms, 10.4MB)
```
- q.append() 전에 maps에 최단 경로를 적어줌
- **BFS에서 다음 가지까지 가기 전에 경로를 적어줌으로써 불필요한 search를 줄임**
