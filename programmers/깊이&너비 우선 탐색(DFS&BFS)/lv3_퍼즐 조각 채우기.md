# 퍼즐 조각 채우기
## 문제 설명
테이블 위에 놓인 퍼즐 조각을 게임 보드의 빈 공간에 적절히 올려놓으려 합니다. 게임 보드와 테이블은 모두 각 칸이 1x1 크기인 정사각 격자 모양입니다. 이때, 다음 규칙에 따라 테이블 위에 놓인 퍼즐 조각을 게임 보드의 빈칸에 채우면 됩니다.

- 조각은 한 번에 하나씩 채워 넣습니다.
- 조각을 회전시킬 수 있습니다.
- 조각을 뒤집을 수는 없습니다.
- 게임 보드에 새로 채워 넣은 퍼즐 조각과 인접한 칸이 비어있으면 안 됩니다.

다음은 퍼즐 조각을 채우는 예시입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/1f719e91-7bfe-4126-9d64-c14b86285213)


위 그림에서 왼쪽은 현재 게임 보드의 상태를, 오른쪽은 테이블 위에 놓인 퍼즐 조각들을 나타냅니다. 테이블 위에 놓인 퍼즐 조각들 또한 마찬가지로 [상,하,좌,우]로 인접해 붙어있는 경우는 없으며, 흰 칸은 퍼즐이 놓이지 않은 빈 공간을 나타냅니다. 모든 퍼즐 조각은 격자 칸에 딱 맞게 놓여있으며, 격자 칸을 벗어나거나, 걸쳐 있는 등 잘못 놓인 경우는 없습니다.

이때, 아래 그림과 같이 3,4,5번 조각을 격자 칸에 놓으면 규칙에 어긋나므로 불가능한 경우입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/3db3f9c0-f4c6-4ca9-91ae-a94b8e4a6e68)

- 3번 조각을 놓고 4번 조각을 놓기 전에 위쪽으로 인접한 칸에 빈칸이 생깁니다.
- 5번 조각의 양 옆으로 인접한 칸에 빈칸이 생깁니다.

다음은 규칙에 맞게 최대한 많은 조각을 게임 보드에 채워 넣은 모습입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/0eb67da2-faf3-4ceb-a976-50bc49c56224)


최대한 많은 조각을 채워 넣으면 총 14칸을 채울 수 있습니다.

현재 게임 보드의 상태 `game_board`, 테이블 위에 놓인 퍼즐 조각의 상태 `table`이 매개변수로 주어집니다. 규칙에 맞게 최대한 많은 퍼즐 조각을 채워 넣을 경우, 총 몇 칸을 채울 수 있는지 return 하도록 solution 함수를 완성해주세요.

제한사항
- 3 ≤ `game_board`의 행 길이 ≤ 50
- `game_board`의 각 열 길이 = `game_board`의 행 길이
  - 즉, 게임 보드는 정사각 격자 모양입니다.
  - `game_board`의 모든 원소는 0 또는 1입니다.
  - 0은 빈칸, 1은 이미 채워진 칸을 나타냅니다.
  - 퍼즐 조각이 놓일 빈칸은 1 x 1 크기 정사각형이 최소 1개에서 최대 6개까지 연결된 형태로만 주어집니다.
- `table`의 행 길이 = `game_board`의 행 길이
- `table`의 각 열 길이 = `table`의 행 길이
  - 즉, 테이블은 `game_board`와 같은 크기의 정사각 격자 모양입니다.
  - `table`의 모든 원소는 0 또는 1입니다.
  - 0은 빈칸, 1은 조각이 놓인 칸을 나타냅니다.
  - 퍼즐 조각은 1 x 1 크기 정사각형이 최소 1개에서 최대 6개까지 연결된 형태로만 주어집니다.
- `game_board`에는 반드시 하나 이상의 빈칸이 있습니다.
- `table`에는 반드시 하나 이상의 블록이 놓여 있습니다.

## 입출력 예
|game_board|table|result|
|-|-|-|
|[[1,1,0,0,1,0],[0,0,1,0,1,0],[0,1,1,0,0,1],[1,1,0,1,1,1],[1,0,0,0,1,0],[0,1,1,1,0,0]]|[[1,0,0,1,1,0],[1,0,1,0,1,0],[0,1,1,0,1,1],[0,0,1,0,0,0],[1,1,0,1,1,0],[0,1,0,0,0,0]]|14|
|[[0,0,0],[1,1,0],[1,1,1]]|[[1,1,1],[1,0,0],[0,0,0]]|0|

## 입출력 예 설명
### 입출력 예 #1

입력은 다음과 같은 형태이며, 문제의 예시와 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/2669e5df-a4d2-4786-84e8-d503ed8132e5)


### 입출력 예 #2

블록의 회전은 가능하지만, 뒤집을 수는 없습니다.

# 내 풀이
- 빈 공간 및 퍼즐을 인식하기 위해서는 DFS로 찾아야한다는 것만 떠올리고, 어떻게 풀지에 대해서는 떠올리지 못했다
- 문제를 잘 읽어야 하는게, 예시의 불가능한 그림에서 3,4 번 처럼 넣는 것이 가능한 줄 알았다. (알았으면 좀 더 쉽게 인식했을지도..?)
- 다른 사람의 접근법만을 참고해서 내 풀이를 작성했다
```python
import numpy as np

def grouping(maps, l, find):
    stack = []
    groups = []
    
    for i in range(l):
        for j in range(l):
            if maps[i][j]==find:
                group = []
                stack.append((i, j))
                maps[i][j] = 1-find
                while stack:
                    x, y = stack.pop()
                    group.append([x, y])
                    
                    if x-1>=0 and maps[x-1][y]==find:
                        maps[x-1][y]=1-find
                        stack.append([x-1, y])
                    if y-1>=0 and maps[x][y-1]==find:
                        maps[x][y-1]=1-find
                        stack.append([x, y-1])
                    if x+1<l and maps[x+1][y]==find:
                        maps[x+1][y]=1-find
                        stack.append([x+1, y])
                    if y+1<l and maps[x][y+1]==find:
                        maps[x][y+1]=1-find
                        stack.append([x, y+1])
                np_group = np.array(group)
                groups.append(sorted((np_group - [min(np_group[:,0]), min(np_group[:,1])]).tolist()))
                
    return groups


def rotate(puzzle):
    rot_puzzles = [puzzle]
    for i in range(3):
        puz = rot_puzzles[-1]
        rot = []
        for (x, y) in puz:
            rot.append([-y, x])
        
        np_rot = np.array(rot)
        rot = sorted((np_rot - [min(np_rot[:,0]), min(np_rot[:,1])]).tolist())
        rot_puzzles.append(rot)
    
    return rot_puzzles
        

def solution(game_board, table):
    l = len(game_board)
    
    holes = grouping(game_board, l, 0)
    frags = grouping(table, l, 1)
    find=0
    answer=0
    
    for hole in holes:
        find=0
        for frag in frags:
            for rot_frag in rotate(frag):
                if hole==rot_frag:
                    answer += len(hole)
                    frags.remove(frag)
                    find=1
                    break
            if find==1:
                break
                
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (4.19ms, 28.5MB)
테스트 2 〉	통과 (2.26ms, 28.2MB)
테스트 3 〉	통과 (1.95ms, 28.4MB)
테스트 4 〉	통과 (2.02ms, 28.7MB)
테스트 5 〉	통과 (1.69ms, 28.5MB)
테스트 6 〉	통과 (39.49ms, 28.3MB)
테스트 7 〉	통과 (33.37ms, 28.3MB)
테스트 8 〉	통과 (42.12ms, 28.5MB)
테스트 9 〉	통과 (26.18ms, 28.3MB)
테스트 10 〉	통과 (305.84ms, 28.7MB)
테스트 11 〉	통과 (1023.65ms, 28.5MB)
테스트 12 〉	통과 (453.51ms, 28.3MB)
테스트 13 〉	통과 (486.71ms, 28.4MB)
테스트 14 〉	통과 (3.13ms, 28.6MB)
테스트 15 〉	통과 (0.31ms, 28.4MB)
테스트 16 〉	통과 (0.60ms, 28.2MB)
테스트 17 〉	통과 (1.09ms, 28.2MB)
테스트 18 〉	통과 (0.59ms, 28.1MB)
테스트 19 〉	통과 (0.20ms, 28.7MB)
테스트 20 〉	통과 (0.21ms, 28.3MB)
테스트 21 〉	통과 (0.17ms, 28.3MB)
테스트 22 〉	통과 (0.16ms, 28.3MB)
```
풀이방법
1. 먼저 빈공간(holes)과 퍼즐들(frags)을 좌표로 표현 후, 각 좌표에 대한 min 값으로 나눠주어, 원점에 맞닿도록 함
  - ex. [[2, 3], [1, 6], [3, 2]] ->  -(1, 2)  -> [[1, 1], [0, 4], [2, 0]]
  - holes의 각 좌표들은 sorting까지 진행 (나중에 ==를 하기 위해)
2. holes와 frags를 하나씩 비교하면서, frags를 3번 회전시킨 4개의 rot_frag와 비교하여, == 진행 후 같으면 좌표의 갯수를 저장
  - rot_frags 4개 각각은 sorting됨
