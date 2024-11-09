# 고고학 최고의 발견
## 문제 설명
고고학자인 혜선은 오래전부터 성궤의 행방을 추적해왔습니다. 그동안 그의 연구는 주류 학자들로부터 인정받지 못했었지만, 혜선이는 포기하지 않고 조사를 계속했고 마침내 성궤의 행방을 알아내었습니다.

그러나 오래전 누군가로부터 봉인된 성궤는 특별한 잠금장치에 의해 보호되고 있었습니다. 잠금장치는 일종의 퍼즐과 연결되어 퍼즐을 해결하면 열리는 것으로 보입니다.

퍼즐은 시계들이 행렬을 이루는 구조물인데 하나의 시계에 시곗바늘은 하나씩만 있습니다. 각 시곗바늘은 시계방향으로만 돌릴 수 있고 한 번의 조작으로 90도씩 돌릴 수 있습니다. 시계들은 기계장치에 의해 연결되어 있어 어떤 시계의 시곗바늘을 돌리면 그 시계의 상하좌우로 인접한 시계들의 시곗바늘도 함께 돌아갑니다. 행렬의 모서리에 위치한 시계의 시곗바늘을 돌리는 경우에는 인접한 세 시계의 시곗바늘들이 함께 돌아가며, 꼭짓점에 위치한 시계의 시곗바늘을 돌리는 경우에는 인접한 두 시계의 시곗바늘들이 함께 돌아갑니다.

각 시계는 12시, 3시, 6시, 9시 방향 중의 한 방향을 가리키고 있습니다. 각 시계의 시곗바늘을 적절히 조작하여 모든 시곗바늘이 12시 방향을 가리키면 퍼즐이 해결되어 성궤를 봉인하고 있는 잠금장치가 열릴 것입니다.

노후화된 퍼즐 기계장치가 걱정되었던 혜선은 가능한 최소한의 조작으로 퍼즐을 해결하려고 합니다. 시곗바늘들의 행렬 `clockHands`가 매개변수로 주어질 때, 퍼즐을 해결하기 위한 최소한의 조작 횟수를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 2 ≤ `clockHands`의 길이 ≤ 8
- `clockHands`는 2차원 배열이며 행과 열의 크기가 동일합니다.
- 0 ≤ `clockHands`의 원소 ≤ 3
- `clockHands[i]`의 원소들은 시곗바늘의 방향을 나타내며 0은 12시 방향, 1은 3시 방향, 2는 6시 방향, 3은 9시 방향을 나타냅니다.
- 해결 가능한 퍼즐만 주어집니다.

## 입출력 예
|clockHands|result|
|-|-|
|[[0,3,3,0],[3,2,2,3],[0,3,2,0],[0,3,3,3]]|3|

##입출력 예 설명
## 입출력 예 #1
2행 2열의 시곗바늘, 2행 3열의 시곗바늘, 4행 3열의 시곗바늘을 각각 한 번씩 돌리면 최소 조작 횟수로 퍼즐을 해결할 수 있습니다.

![image](https://github.com/user-attachments/assets/a63cdacd-afb9-473e-ad21-b33c67ec975e)

# 다른 사람의 풀이
```python
from copy import deepcopy

dx = [0, -1, 1, 0, 0]
dy = [0, 0, 0, 1, -1]

def rotate(x, y, lst, rt) :
    length = len(lst)
    for k in range(5) :
        ax, ay = x + dx[k], y + dy[k]
        if -1 < ax < length and -1 < ay < length :
            lst[ay][ax] = ( lst[ay][ax] + rt ) % 4

def solution(clockHands):
    answer = float('inf')
    length = len(clockHands)
    
    for i in range(4 ** length) :
        tmp = 0
        tmp_clock = deepcopy(clockHands)
        for j in range(length) :
            rt = i % 4 ** ( j + 1 ) // 4 ** j
            if rt == 0 :
                continue
                
            rotate(j, 0, tmp_clock, rt)
            tmp += rt
        
        for y in range(1, length) :
            for x in range(length) :
                if tmp_clock[y-1][x] == 0 :
                    continue
                rt = 4 - tmp_clock[y-1][x]
                rotate(x, y, tmp_clock, rt)
                tmp += rt
        if sum(tmp_clock[-1]) == 0 : 
            answer = min(answer, tmp)
        
    return answer
```
- 행-렬의 갯수가 8 이하로 정해진 것을 보고, 모든 경우의 수를 세는 것 같았지만, 결국 풀이를 떠올리지 못하고 다른 사람의 풀이를 참고했다.
- 전략
  - 각 칸을 4번 이상 회전시키는 것은 그 횟수에서 4를 뺀 회전과 결과가 같으므로 무의미하다.
  - 따라서, 각 칸을 1~4번 회전시키는 경우의수를 봐야 하지만, $4^{64}$는 너무 크므로, 경우의 수를 줄여야 함
  - 첫 번째 줄의 경우의 수를 정하면, 그 결과에 의해 두 번째 줄의 회전 횟수가 정해지고, 이로 인해, 계속 다음 줄의 경우가 정해진다.
    - 첫 번째 줄의 시계를 회전시킬 수 있는 회전은 첫 번째 줄과 두 번째 줄의 시계에 의해서만 가능하다.
    - 따라서 첫 번째 줄의 회전이 정해지면, 남은 12시가 아닌 시곗바늘은 두 번째 줄의 시계 회전에 의해 정해져야 함!
  - 첫 번째 줄을 회전시키는 모든 경우의 수 $4^{N}$을 product에 의해 만들고 회전시킴
  - 이후 첫 번째 줄의 결과를 보며 두 번째 줄을 회전시키고, 두 번째 줄의 결과를 보며 세 번째 줄을, ... 마지막 줄 까지 회전시킴
  - 만약 모든 회전을 했을 때, 마지막 줄이 12시 방향이 아닌 것이 존재하면, 이 경우는 불가능한 경우임.
  - 가능한 모든 경우의 수를 세었을 때, 가장 적게 회전시킨 수를 카운트

# 내 풀이
```python
from itertools import product
from copy import deepcopy

dr = [0, 0, 0, -1, 1]
dc = [0, -1, 1, 0, 0]

def rotate(board, r, c, rot_num, N):
    for i in range(5):
        R, C = r+dr[i], c+dc[i]
        if 0<=R<N and 0<=C<N:
            board[R][C] = (board[R][C]+rot_num)%4


def solution(clockHands):
    N = len(clockHands)
    answer = float('inf')
    
    for case in product(range(4), repeat=N):
        new_board = deepcopy(clockHands)
        nums = sum(case)
        for i in range(N):
            rotate(new_board, 0, i, case[i], N)
            
        for i in range(1,N):
            for j in range(N):
                if new_board[i-1][j]:
                    rot_num = 4-new_board[i-1][j]
                    nums += rot_num
                    rotate(new_board, i, j, rot_num, N)
        if sum(new_board[-1]): continue
        answer = min(answer, nums)
        
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.31ms, 10.3MB)
테스트 2 〉	통과 (0.37ms, 10.3MB)
테스트 3 〉	통과 (9.40ms, 10.4MB)
테스트 4 〉	통과 (1586.48ms, 10.3MB)
테스트 5 〉	통과 (1492.99ms, 10.4MB)
테스트 6 〉	통과 (8210.95ms, 10.3MB)
테스트 7 〉	통과 (8223.07ms, 10.4MB)
테스트 8 〉	통과 (7692.10ms, 10.3MB)
테스트 9 〉	통과 (7708.47ms, 10.4MB)
테스트 10 〉	통과 (7853.66ms, 10.3MB)
```
