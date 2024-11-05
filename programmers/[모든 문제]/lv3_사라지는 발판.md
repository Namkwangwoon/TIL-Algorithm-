# 사라지는 발판
## 문제 설명
플레이어 A와 플레이어 B가 서로 게임을 합니다. 당신은 이 게임이 끝날 때까지 양 플레이어가 캐릭터를 몇 번 움직이게 될지 예측하려고 합니다.

각 플레이어는 자신의 캐릭터 하나를 보드 위에 올려놓고 게임을 시작합니다. 게임 보드는 1x1 크기 정사각 격자로 이루어져 있으며, 보드 안에는 발판이 있는 부분과 없는 부분이 있습니다. 발판이 있는 곳에만 캐릭터가 서있을 수 있으며, 처음 캐릭터를 올려놓는 곳은 항상 발판이 있는 곳입니다. 캐릭터는 발판이 있는 곳으로만 이동할 수 있으며, 보드 밖으로 이동할 수 없습니다. 밟고 있던 발판은 그 위에 있던 캐릭터가 다른 곳으로 이동하여 다른 발판을 밞음과 동시에 사라집니다. 양 플레이어는 번갈아가며 자기 차례에 자신의 캐릭터를 상하좌우로 인접한 4개의 칸 중에서 발판이 있는 칸으로 옮겨야 합니다.

다음과 같은 2가지 상황에서 패자와 승자가 정해지며, 게임이 종료됩니다.

- 움직일 차례인데 캐릭터의 상하좌우 주변 4칸이 모두 발판이 없거나 보드 밖이라서 이동할 수 없는 경우, 해당 차례 플레이어는 패배합니다.
- 두 캐릭터가 같은 발판 위에 있을 때, 상대 플레이어의 캐릭터가 다른 발판으로 이동하여 자신의 캐릭터가 서있던 발판이 사라지게 되면 패배합니다.

게임은 항상 플레이어 A가 먼저 시작합니다. 양 플레이어는 최적의 플레이를 합니다. 즉, 이길 수 있는 플레이어는 최대한 빨리 승리하도록 플레이하고, 질 수밖에 없는 플레이어는 최대한 오래 버티도록 플레이합니다. '이길 수 있는 플레이어'는 실수만 하지 않는다면 항상 이기는 플레이어를 의미하며, '질 수밖에 없는 플레이어'는 최선을 다해도 상대가 실수하지 않으면 항상 질 수밖에 없는 플레이어를 의미합니다. 최대한 오래 버틴다는 것은 양 플레이어가 캐릭터를 움직이는 횟수를 최대화한다는 것을 의미합니다.

아래 그림은 초기 보드의 상태와 각 플레이어의 위치를 나타내는 예시입니다.

![image](https://github.com/user-attachments/assets/f84271aa-859a-41b9-9797-a2be34de1ec2)

위와 같은 경우, 플레이어 A는 실수만 하지 않는다면 항상 이길 수 있습니다. 따라서 플레이어 A는 이길 수 있는 플레이어이며, B는 질 수밖에 없는 플레이어입니다. 다음은 A와 B가 최적의 플레이를 하는 과정을 나타냅니다.

1. 플레이어 A가 초기 위치 (1, 0)에서 (1, 1)로 이동합니다. **플레이어 A가 (0, 0)이나 (2, 0)으로 이동할 경우 승리를 보장할 수 없습니다. 따라서 무조건 이길 방법이 있는 (1, 1)로 이동합니다.**
2. 플레이어 B는 (1, 1)로 이동할 경우, 바로 다음 차례에 A가 위 또는 아래 방향으로 이동하면 발판이 없어져 패배하게 됩니다. **질 수밖에 없는 플레이어는 최대한 오래 버티도록 플레이하기 때문에 (1, 1)로 이동하지 않습니다.** (1, 2)에서 위쪽 칸인 (0, 2)로 이동합니다.
3. A가 (1, 1)에서 (0, 1)로 이동합니다.
4. B에게는 남은 선택지가 (0, 1)밖에 없습니다. 따라서 (0, 2)에서 (0, 1)로 이동합니다.
5. A가 (0, 1)에서 (0, 0)으로 이동합니다. 이동을 완료함과 동시에 B가 서있던 (0, 1)의 발판이 사라집니다. B가 패배합니다.
6. 만약 과정 2에서 B가 아래쪽 칸인 (2, 2)로 이동하더라도 A는 (2, 1)로 이동하면 됩니다. 이후 B가 (2, 1)로 이동, 다음 차례에 A가 (2, 0)으로 이동하면 B가 패배합니다.

위 예시에서 양 플레이어가 최적의 플레이를 했을 경우, 캐릭터의 이동 횟수 합은 5입니다. 최적의 플레이를 하는 방법은 여러 가지일 수 있으나, 이동한 횟수는 모두 5로 같습니다.

게임 보드의 초기 상태를 나타내는 2차원 정수 배열 `board`와 플레이어 A의 캐릭터 초기 위치를 나타내는 정수 배열 `aloc`, 플레이어 B의 캐릭터 초기 위치를 나타내는 정수 배열 `bloc`이 매개변수로 주어집니다. 양 플레이어가 최적의 플레이를 했을 때, 두 캐릭터가 움직인 횟수의 합을 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 1 ≤ `board`의 세로 길이 ≤ 5
- 1 ≤ `board`의 가로 길이 ≤ 5
- `board`의 원소는 0 또는 1입니다.
  - 0은 발판이 없음을, 1은 발판이 있음을 나타냅니다.
  - 게임 보드의 좌측 상단 좌표는 (0, 0), 우측 하단 좌표는 (`board`의 세로 길이 - 1, `board`의 가로 길이 - 1)입니다.
- `aloc`과 `bloc`은 각각 플레이어 A의 캐릭터와 플레이어 B의 캐릭터 초기 위치를 나타내는 좌표값이며 [r, c] 형태입니다.
  - r은 몇 번째 행인지를 나타냅니다.
  - 0 ≤ r < `board`의 세로 길이
  - c는 몇 번째 열인지를 나타냅니다.
  - 0 ≤ c < `board`의 가로 길이
  - 초기 보드의 `aloc`과 `bloc` 위치는 항상 발판이 있는 곳입니다.
  - `aloc`과 `bloc`이 같을 수 있습니다.
- 상대 플레이어의 캐릭터가 있는 칸으로 이동할 수 있습니다.

## 입출력 예
|board|aloc|bloc|result|
|-|-|-|-|
|[[1, 1, 1], [1, 1, 1], [1, 1, 1]]|[1, 0]|[1, 2]|5|
|[[1, 1, 1], [1, 0, 1], [1, 1, 1]]|[1, 0]|[1, 2]|4|
|[[1, 1, 1, 1, 1]]|[0, 0]|[0, 4]|4|
|[[1]]|[0, 0]|[0, 0]|0|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

주어진 조건을 그림으로 나타내면 아래와 같습니다.

![image](https://github.com/user-attachments/assets/a2681501-353a-41ba-8edc-0f6f71126dbf)

항상 이기는 플레이어는 B, 항상 지는 플레이어는 A입니다.

다음은 B가 이기는 방법 중 하나입니다.

1. A가 (1, 0)에서 (0, 0)으로 이동
2. B가 (1, 2)에서 (2, 2)로 이동
3. A가 (0, 0)에서 (0, 1)로 이동
4. B가 (2, 2)에서 (2, 1)로 이동
5. A가 (0, 1)에서 (0, 2)로 이동
6. B가 (2, 1)에서 (2, 0)으로 이동
7. A는 더 이상 이동할 수 없어 패배합니다.

위와 같이 플레이할 경우 이동 횟수 6번 만에 게임을 B의 승리로 끝낼 수 있습니다.

B가 다음과 같이 플레이할 경우 게임을 더 빨리 끝낼 수 있습니다. 이길 수 있는 플레이어는 최대한 빨리 게임을 끝내려 하기 때문에 위 방법 대신 아래 방법을 선택합니다.

1. A가 (1, 0)에서 (0, 0)으로 이동
2. B가 (1, 2)에서 (0, 2)로 이동
3. A가 (0, 0)에서 (0, 1)로 이동
4. B가 (0, 2)에서 (0, 1)로 이동
5. A는 더 이상 이동할 수 없어 패배합니다.

위와 같이 플레이할 경우 이동 횟수 4번 만에 게임을 B의 승리로 끝낼 수 있습니다. 따라서 4를 return 합니다.

### 입출력 예 #3

양 플레이어는 매 차례마다 한 가지 선택지밖에 고를 수 없습니다. 그 결과, (0, 2)에서 어디로도 이동할 수 없는 A가 패배합니다. 양 플레이어가 캐릭터를 움직인 횟수의 합은 4입니다.

### 입출력 예 #4

게임을 시작하는 플레이어 A가 처음부터 어디로도 이동할 수 없는 상태입니다. 따라서 A의 패배이며, 이동 횟수의 합은 0입니다.

# 내 풀이 1
```python
import sys
sys.setrecursionlimit(100000)
import copy

dr = [0, 1, 0, -1]
dc = [1, 0, -1, 0]

def move_possible(R, C, board):
    len_R, len_C = len(board), len(board[0])
    
    if 0<=R<len_R and 0<=C<len_C and board[R][C]==1:
        return True
    else:
        return False


def dfs(turn, board, aloc, bloc, N):
    r, c = aloc if turn==0 else bloc
    move=False
    answers = []
    
    if board[r][c]==0:
        answers += [(1-turn, N)]
        return answers
    for i in range(4):
        R, C = r+dr[i], c+dc[i]
        if move_possible(R, C, board):
            move=True
            new_board = copy.deepcopy(board)
            new_board[r][c] = 0
            aloc, bloc = ((R, C), bloc) if turn==0 else (aloc, (R, C))
            answers += dfs(1-turn, new_board, aloc, bloc, N+1)
    if not move:
        answers += [(1-turn, N)]
    return answers

    
def solution(board, aloc, bloc):
    tmp_answer = 0
    answers = sorted(dfs(0, board, aloc, bloc, 0))
    for i, answer in enumerate(answers):
        if answer[0]==1:
            if i==0: return answer[1]
            else: return tmp_answer
        tmp_answer = answer[1]
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.4MB)
테스트 2 〉	통과 (0.05ms, 10.3MB)
테스트 3 〉	통과 (0.22ms, 10.5MB)
테스트 4 〉	통과 (0.76ms, 10.2MB)
테스트 5 〉	실패 (0.75ms, 10.2MB)
테스트 6 〉	통과 (0.04ms, 10.3MB)
테스트 7 〉	통과 (0.01ms, 10.4MB)
테스트 8 〉	통과 (0.11ms, 10.3MB)
테스트 9 〉	실패 (0.69ms, 10.3MB)
테스트 10 〉	통과 (0.56ms, 10.2MB)
테스트 11 〉	통과 (0.13ms, 10.3MB)
테스트 12 〉	통과 (1.06ms, 10.3MB)
테스트 13 〉	실패 (0.15ms, 10.5MB)
테스트 14 〉	실패 (0.03ms, 10.2MB)
테스트 15 〉	실패 (0.85ms, 10.4MB)
테스트 16 〉	통과 (1.00ms, 10.5MB)
테스트 17 〉	실패 (159.22ms, 10.5MB)
테스트 18 〉	실패 (452.55ms, 10.9MB)
테스트 19 〉	실패 (47.44ms, 10.5MB)
테스트 20 〉	실패 (14.71ms, 10.4MB)
테스트 21 〉	실패 (2347.13ms, 13.3MB)
테스트 22 〉	실패 (3.84ms, 10.3MB)
테스트 23 〉	통과 (0.96ms, 10.4MB)
테스트 24 〉	실패 (0.16ms, 10.4MB)
테스트 25 〉	통과 (3.29ms, 10.4MB)
테스트 26 〉	통과 (1.29ms, 10.3MB)
테스트 27 〉	통과 (0.11ms, 10.2MB)
테스트 28 〉	통과 (0.14ms, 10.5MB)
테스트 29 〉	실패 (0.43ms, 10.3MB)
테스트 30 〉	통과 (0.07ms, 10.4MB)
```
- 결론적으로는, 문제를 잘못 이해해서 비슷하게는 풀었지만 절반 조금 넘게 맞았다.
- "이길 수 있는 플레이어는 최대한 빨리 승리하도록 플레이하고, 질 수 밖에 없는 플레이어는 최대한 오래 버티도록 플레이함"을 넘겨 짚었다.
  - 모든 경우의 수를 구한 다음, A가 이기는 경우는 제일 큰 횟수, B가 이기는 경우는 제일 작은 횟수만 출력하는 되는 줄 알았다.
 
# 다른 사람의 풀이
```python
dy = [-1, 1, 0, 0]
dx = [0, 0, -1, 1]
INF = 987654321


def solution(board, aloc, bloc):
    return solve(board, aloc[0], aloc[1], bloc[0], bloc[1])[1]


def in_range(board, y, x):
    if y < 0 or y >= len(board) or x < 0 or x >= len(board[0]):
        return False
    return True


def is_finished(board, y, x):
    for i in range(4):
        ny = y + dy[i]
        nx = x + dx[i]
        if in_range(board, ny, nx) and board[ny][nx]:
            return False
    return True


def solve(board, y1, x1, y2, x2):
    # can_win, turn
    if is_finished(board, y1, x1):
        return [False, 0]

    # 서로 두 위치가 같을 때 이번 턴에 움직이면 무조건 이기므로
    if y1 == y2 and x1 == x2:
        return [True, 1]

    min_turn = INF
    max_turn = 0
    can_win = False

    # dfs
    for i in range(4):
        ny = y1 + dy[i]
        nx = x1 + dx[i]
        if not in_range(board, ny, nx) or not board[ny][nx]:
            continue

        board[y1][x1] = 0
        result = solve(board, y2, x2, ny, nx)  # 차례가 바뀌기 때문에 위치를 바꿔준다.
        board[y2][x2] = 1

        # 이 시점에서는 result[0]이 False여야만 현재 턴에서 내가 이길 수 있다.
        if not result[0]:
            can_win = True
            min_turn = min(min_turn, result[1])
        elif not can_win:
            max_turn = max(max_turn, result[1])

    turn = min_turn if can_win else max_turn

    return [can_win, turn + 1]
```
- 전략
  - 같은 DFS이다.
  - 문제의 핵심은, **"이길 수 있는 플레이어"** 와 **"질 수 밖에 없는 플레이어"** 이다.
    - DFS로 미리 앞을 내다봤을 때, 지는 경우밖에 없는 경우는 최대한 횟수를 늘리고, 이길 수 있는 경우가 하나라도 있으면 최대한 횟수를 줄여야 함
    - 승패가 결정하는 두 가지 경우의 수가 나타날 때 까지 탐색한 다음, 하나라도 이길 경우의 수가 있으면 min_turn, 질 수 밖에 없으면 max_turn을 리턴

# 내 풀이 2
```python
import sys
sys.setrecursionlimit(1000000)
import copy

dr = [0, 1, 0, -1]
dc = [1, 0, -1, 0]

def movable(board, r, c):
    if 0<=r<len(board) and 0<=c<len(board[0]) and board[r][c]==1:
        return True
    else:
        return False


def dfs(board, cur_r, cur_c, next_r, next_c):
    # 현재 움직일 수 없음: 현재 플레이어 패배
    for i in range(4):
        new_r, new_c = cur_r+dr[i], cur_c+dc[i]
        if movable(board, new_r, new_c): break
    else: return [False, 0]
    
    # 현재 움직일 수 있고, 두 플레이어가 같은 위치에 있음: 현재 플레이어 승리
    if cur_r==next_r and cur_c==next_c: return [True, 1]

    winnable, min_turn, max_turn = False, float('inf'), 0
    
    for i in range(4):
        new_r, new_c = cur_r+dr[i], cur_c+dc[i]
        if not movable(board, new_r, new_c): continue
        
        new_board = copy.deepcopy(board)
        new_board[cur_r][cur_c] = 0
        result = dfs(new_board, next_r, next_c, new_r, new_c)
        
        # 상대가 이길 가능성이 없는 경우 (내가 무조건 이기는 경우)
        if not result[0]:
            winnable = True
            min_turn = min(min_turn, result[1])
        # 상대가 이길 가능성이 있고, 내가 이길 가능성은 없는 경우 (내가 무조건 지는 경우)
        elif not winnable:
            max_turn = max(max_turn, result[1])

    return [winnable, min_turn+1 if winnable else max_turn+1]

    
def solution(board, aloc, bloc):
    return dfs(board, *aloc, *bloc)[1]
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.4MB)
테스트 2 〉	통과 (0.05ms, 10.3MB)
테스트 3 〉	통과 (0.23ms, 10.2MB)
테스트 4 〉	통과 (0.67ms, 10.4MB)
테스트 5 〉	통과 (0.80ms, 10.4MB)
테스트 6 〉	통과 (0.03ms, 10.3MB)
테스트 7 〉	통과 (0.01ms, 10.2MB)
테스트 8 〉	통과 (0.11ms, 10.3MB)
테스트 9 〉	통과 (0.60ms, 10.4MB)
테스트 10 〉	통과 (0.45ms, 10.3MB)
테스트 11 〉	통과 (0.13ms, 10.3MB)
테스트 12 〉	통과 (0.84ms, 10.1MB)
테스트 13 〉	통과 (0.15ms, 10.3MB)
테스트 14 〉	통과 (0.05ms, 10.3MB)
테스트 15 〉	통과 (0.79ms, 10.4MB)
테스트 16 〉	통과 (0.92ms, 10.3MB)
테스트 17 〉	통과 (167.53ms, 10.3MB)
테스트 18 〉	통과 (410.46ms, 10.2MB)
테스트 19 〉	통과 (48.18ms, 10.3MB)
테스트 20 〉	통과 (16.88ms, 10.3MB)
테스트 21 〉	통과 (2408.14ms, 10.2MB)
테스트 22 〉	통과 (3.79ms, 10.3MB)
테스트 23 〉	통과 (0.62ms, 10.4MB)
테스트 24 〉	통과 (0.00ms, 10.3MB)
테스트 25 〉	통과 (2.34ms, 10.2MB)
테스트 26 〉	통과 (2.51ms, 10.3MB)
테스트 27 〉	통과 (0.16ms, 10.2MB)
테스트 28 〉	통과 (0.22ms, 10.2MB)
테스트 29 〉	통과 (0.79ms, 10.2MB)
테스트 30 〉	통과 (0.03ms, 10.3MB)
```
- 다시 풀어도 어려웠다..
