# 카드 짝 맞추기
## 문제 설명
게임 개발자인 `베로니`는 개발 연습을 위해 다음과 같은 간단한 카드 짝맞추기 보드 게임을 개발해 보려고 합니다.

게임이 시작되면 화면에는 카드 16장이 뒷면을 위로하여 `4 x 4` 크기의 격자 형태로 표시되어 있습니다. 각 카드의 앞면에는 카카오프렌즈 캐릭터 그림이 그려져 있으며, 8가지의 캐릭터 그림이 그려진 카드가 각기 2장씩 화면에 무작위로 배치되어 있습니다.

유저가 카드를 2장 선택하여 앞면으로 뒤집었을 때 같은 그림이 그려진 카드면 해당 카드는 게임 화면에서 사라지며, 같은 그림이 아니라면 원래 상태로 뒷면이 보이도록 뒤집힙니다. 이와 같은 방법으로 모든 카드를 화면에서 사라지게 하면 게임이 종료됩니다.

게임에서 카드를 선택하는 방법은 다음과 같습니다.

- 카드는 `커서`를 이용해서 선택할 수 있습니다.
  - 커서는 4 x 4 화면에서 유저가 선택한 현재 위치를 표시하는 "굵고 빨간 테두리 상자"를 의미합니다.
- 커서는 [Ctrl] 키와 방향키에 의해 이동되며 키 조작법은 다음과 같습니다.
  - 방향키 ←, ↑, ↓, → 중 하나를 누르면, 커서가 누른 키 방향으로 1칸 이동합니다.
  - [Ctrl] 키를 누른 상태에서 방향키 ←, ↑, ↓, → 중 하나를 누르면, 누른 키 방향에 있는 가장 가까운 카드로 한번에 이동합니다.
    - 만약, 해당 방향에 카드가 하나도 없다면 그 방향의 가장 마지막 칸으로 이동합니다.
  - 만약, 누른 키 방향으로 이동 가능한 카드 또는 빈 공간이 없어 이동할 수 없다면 커서는 움직이지 않습니다.
- 커서가 위치한 카드를 뒤집기 위해서는 [Enter] 키를 입력합니다.
  - [Enter] 키를 입력해서 카드를 뒤집었을 때
    - 앞면이 보이는 카드가 1장 뿐이라면 그림을 맞출 수 없으므로 두번째 카드를 뒤집을 때 까지 앞면을 유지합니다.
    - 앞면이 보이는 카드가 2장이 된 경우, 두개의 카드에 그려진 그림이 같으면 해당 카드들이 화면에서 사라지며, 그림이 다르다면 두 카드 모두 뒷면이 보이도록 다시 뒤집힙니다.

"베로니"는 게임 진행 중 카드의 짝을 맞춰 몇 장 제거된 상태에서 카드 앞면의 그림을 알고 있다면, 남은 카드를 모두 제거하는데 필요한 키 조작 횟수의 최솟값을 구해 보려고 합니다. 키 조작 횟수는 방향키와 [Enter] 키를 누르는 동작을 각각 조작 횟수 `1`로 계산하며, [Ctrl] 키와 방향키를 함께 누르는 동작 또한 조작 횟수 `1`로 계산합니다.



다음은 카드가 몇 장 제거된 상태의 게임 화면에서 커서를 이동하는 예시입니다.

아래 그림에서 빈 칸은 이미 카드가 제거되어 없어진 칸을 의미하며, 그림이 그려진 칸은 카드 앞 면에 그려진 그림을 나타냅니다.

![image](https://github.com/user-attachments/assets/4aca15a8-5186-40ad-9aa8-85c368362c06)

예시에서 커서는 두번째 행, 첫번째 열 위치에서 시작하였습니다.

![image](https://github.com/user-attachments/assets/ceeafb0d-44c3-4d4d-bcfb-6e39efba40da)

[Enter] 입력, ↓ 이동, [Ctrl]+→ 이동, [Enter] 입력 = 키 조작 4회

![image](https://github.com/user-attachments/assets/0d719ac9-e378-4d6f-94d7-d8061b8622c6)

[Ctrl]+↑ 이동, [Enter] 입력, [Ctrl]+← 이동, [Ctrl]+↓ 이동, [Enter] 입력 = 키 조작 5회

![image](https://github.com/user-attachments/assets/ced7aad0-71cc-45fe-9d07-bd9a555de926)

[Ctrl]+→ 이동, [Enter] 입력, [Ctrl]+↑ 이동, [Ctrl]+← 이동, [Enter] 입력 = 키 조작 5회

위와 같은 방법으로 커서를 이동하여 카드를 선택하고 그림을 맞추어 카드를 모두 제거하기 위해서는 총 14번(방향 이동 8번, [Enter] 키 입력 6번)의 키 조작 횟수가 필요합니다.

## [문제]
현재 카드가 놓인 상태를 나타내는 2차원 배열 board와 커서의 처음 위치 r, c가 매개변수로 주어질 때, 모든 카드를 제거하기 위한 키 조작 횟수의 최솟값을 return 하도록 solution 함수를 완성해 주세요.

## [제한사항]
- board는 4 x 4 크기의 2차원 배열입니다.
- board 배열의 각 원소는 0 이상 6 이하인 자연수입니다.
  - 0은 카드가 제거된 빈 칸을 나타냅니다.
  - 1 부터 6까지의 자연수는 2개씩 들어있으며 같은 숫자는 같은 그림의 카드를 의미합니다.
  - 뒤집을 카드가 없는 경우(board의 모든 원소가 0인 경우)는 입력으로 주어지지 않습니다.
- r은 커서의 최초 세로(행) 위치를 의미합니다.
- c는 커서의 최초 가로(열) 위치를 의미합니다.
- r과 c는 0 이상 3 이하인 정수입니다.
- 게임 화면의 좌측 상단이 (0, 0), 우측 하단이 (3, 3) 입니다.

## [입출력 예]
|board|r|c|result|
|-|-|-|-|
|[[1,0,0,3],[2,0,0,0],[0,0,0,2],[3,0,1,0]]|1|0|14|
|[[3,0,0,2],[0,0,1,0],[0,1,0,0],[2,0,0,3]]|0|1|16|

## 입출력 예에 대한 설명
### 입출력 예 #1
문제의 예시와 같습니다.

### 입출력 예 #2
입력으로 주어진 게임 화면은 아래 그림과 같습니다.

![image](https://github.com/user-attachments/assets/c678806e-6b69-4c0a-af24-0a32011f5c51)

위 게임 화면에서 모든 카드를 제거하기 위한 키 조작 횟수의 최솟값은 16번 입니다.

# 내 풀이
```python
from collections import defaultdict
import sys, copy
sys.setrecursionlimit(1000000)

# U, L, D, R
dr = [-1, 0, 1, 0]
dc = [0, 1, 0, -1]

def find_min_dist(board, from_, to_):
    direc = []
    if from_[0]>to_[0]: direc.append(0)
    if from_[1]<to_[1]: direc.append(1)
    if from_[0]<to_[0]: direc.append(2)
    if from_[1]>to_[1]: direc.append(3)
    if len(direc)>1: direc = [direc, list(reversed(direc))]
    else: direc = [direc]
    l_direc = len(direc)
    
    target_r, target_c = to_
    min_whole_count = float('inf')
    
    for direc_order in direc:  # 두 방향 이동 순서
        cur_r, cur_c = from_
        whole_count = 0
        for i, d in enumerate(direc_order):  # 하나의 방향에 대해 이동
            count = 0
            jump_count = 0
            drr, dcc = dr[d], dc[d]
            while ((cur_r==target_r)*1+(cur_c==target_c)*1!=i+3-l_direc) and (0<=cur_r+drr<4 and 0<=cur_c+dcc<4):
                cur_r+=drr
                cur_c+=dcc
                if board[cur_r][cur_c]!=0 or not (0<=cur_r+drr<4 and 0<=cur_c+dcc<4):
                    jump_count+=1
                    count=0
                else: count+=1
            whole_count += count+jump_count
        min_whole_count = min(min_whole_count, whole_count)
        
    return min_whole_count


def dfs(board, card_pos, visited, r, c, remain_count, num, ind, move_count, N):
    move_count+=1
    new_pos = card_pos[num][ind]
    # print((r, c), "-->", new_pos)
    move_count+=find_min_dist(board, (r, c), new_pos)
    r, c = new_pos
    remain_count-=1
    board[r][c]=0
    visited[num][ind] = True
    
    if remain_count==0:  # 끝점
        # print("end!")
        return [move_count]
    
    if False in visited[num]:  # 나머지 같은 숫자를 찾아갈 때
        return dfs(copy.deepcopy(board), card_pos, copy.deepcopy(visited), r, c, remain_count, num, 1-ind, move_count, N)
    else:  # 새로운 숫자를 찾아갈 때
        answer = []
        for new_num in range(1, N+1):  # 카드의 숫자
            for new_ind in range(2):  # 같은 숫자 카드의 순서
                if not visited[new_num][new_ind]:
                    # print(new_num, new_ind)
                    answer += dfs(copy.deepcopy(board), card_pos, copy.deepcopy(visited), r, c, remain_count, new_num, new_ind, move_count, N)
        return answer


def solution(board, r, c):
    card_pos = defaultdict(list)  # 숫자별 카드 위치
    for i in range(4):
        for j in range(4):
            if board[i][j]!=0:
                card_pos[board[i][j]].append((i,j))
    N = len(card_pos)
    visited = [[False]*2 for _ in range(N+1)]  # 카드 뒤집음 여부
    remain_count = N*2
    move_count=0
    answer = []
    
    # while True:
    for num in range(1, N+1):  # 카드의 숫자
    # for num in range(1, 2):  # 카드의 숫자
        for ind in range(2):  # 같은 숫자 카드의 순서
            answer += dfs(copy.deepcopy(board), card_pos, copy.deepcopy(visited), r, c, remain_count, num, ind, move_count, N)
    return min(answer)
```
- 구현 문제
- 겨우겨우 두 시간만에 풀었던 것 같다
- 전략
  - 최소 조작 횟수는 절대 알 수 없으므로, 모든 경우를 고려해야 한다. (값의 범위가 전부 작음)
  - 각 숫자마다 카드가 두 개가 있으므로, 이중 for문으로 모든 경우의 수 고려
  - [Ctrl] + 이동은 이동 중에 숫자(카드)를 만나거나 끝점에 도달했을 때
    - 주목할 점은, 칸이 최대 4칸밖에 안되므로 Ctrl 이동으로 갔다가 역방향으로 되돌아오는 방법으로 이득을 취할 수 없음
  - 다른 사람의 풀이를 보니, permutation으로 모든 경우의 수를 만든 다음 각각의 이동 횟수를 세어도 됨!
