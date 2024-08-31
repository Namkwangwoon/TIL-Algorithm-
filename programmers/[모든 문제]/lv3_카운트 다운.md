# 카운트 다운
## 문제 설명
프로그래머스 다트 협회에서는 매년마다 새로운 특수 룰으로 다트 대회를 개최합니다. 이번 대회의 룰은 "카운트 다운"으로 "제로원" 룰의 변형 룰입니다.

"카운트 다운"은 게임이 시작되면 무작위로 점수가 정해지고, 다트를 던지면서 점수를 깎아서 정확히 0점으로 만드는 게임입니다. 단, 남은 점수보다 큰 점수로 득점하면 버스트가 되며 실격 합니다.

다음 그림은 다트 과녁입니다.

![image](https://github.com/user-attachments/assets/3d3ef7a8-9702-4e74-87cc-6e263a5b918c)

다트 과녁에는 1 부터 20 까지의 수가 하나씩 있고 각 수마다 "싱글", "더블", "트리플" 칸이 있습니다. "싱글"을 맞히면 해당 수만큼 점수를 얻고 "더블"을 맞히면 해당 수의 두 배만큼 점수를 얻고 "트리플"을 맞히면 해당 수의 세 배만큼 점수를 얻습니다. 가운데에는 "불"과 "아우터 불"이 있는데 "카운트 다운" 게임에서는 구분 없이 50점을 얻습니다.

대회는 토너먼트로 진행되며 한 게임에는 두 선수가 참가하게 됩니다. 게임은 두 선수가 교대로 한 번씩 던지는 라운드 방식으로 진행됩니다. 가장 먼저 0점을 만든 선수가 승리하는데 만약 두 선수가 같은 라운드에 0점을 만들면 두 선수 중 "싱글" 또는 "불"을 더 많이 던진 선수가 승리하며 그마저도 같다면 선공인 선수가 승리합니다.

다트 실력에 자신 있던 종호는 이 대회에 출전하기로 하였습니다. 최소한의 다트로 0점을 만드는 게 가장 중요하고, 그러한 방법이 여러가지가 있다면 "싱글" 또는 "불"을 최대한 많이 던지는 방법을 선택해야 합니다.

목표 점수 `target`이 매개변수로 주어졌을 때 최선의 경우 던질 다트 수와 그 때의 "싱글" 또는 "불"을 맞춘 횟수(합)를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

## 제한사항
- 1 ≤ `target` ≤ 100,000

## 입출력 예
|target|result|
|-|-|
|21|[1,0]|
|58|[2,2]|

## 입출력 예 설명
### 입출력 예 #1
7 트리플로 21점을 만들 수 있습니다.

### 입출력 예 #2
불 + 8 싱글로 58점을 만들 수 있습니다.

# 내 풀이 1
```python
from collections import deque

def check_bull(rounds, remain_target, bull, single, double, triple, q):
    if remain_target>=50:
        q.append((rounds+1, remain_target-50, bull+1, single, double, triple))
        
def check_single(rounds, remain_target, bull, single, double, triple, q):
    div = remain_target//1
    score = min(div, 20)
    q.append((rounds+1, remain_target-score*1, bull, single+1, double, triple))
    
def check_double(rounds, remain_target, bull, single, double, triple, q):
    div = remain_target//2
    score = min(div, 20)
    q.append((rounds+1, remain_target-score*2, bull, single, double+1, triple))
    
def check_triple(rounds, remain_target, bull, single, double, triple, q):
    div = remain_target//3
    score = min(div, 20)
    q.append((rounds+1, remain_target-score*3, bull, single, double, triple+1))

def solution(target):
    answer = []
    # [round, target, bull, single, double, triple]
    q = deque([(0, target, 0, 0, 0, 0)])
    min_round = None
    
    while True:
        rounds, remain_target, bull, single, double, triple = q.popleft()
        if remain_target==0:
            return [rounds, bull+single]
        
        if remain_target>=50:
            check_bull(rounds, remain_target, bull, single, double, triple, q)
        
        check_single(rounds, remain_target, bull, single, double, triple, q)
        
        check_double(rounds, remain_target, bull, single, double, triple, q)
        
        check_triple(rounds, remain_target, bull, single, double, triple, q)
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	실패 (시간 초과)
테스트 3 〉	실패 (시간 초과)
테스트 4 〉	실패 (시간 초과)
테스트 5 〉	실패 (시간 초과)
테스트 6 〉	실패 (시간 초과)
테스트 7 〉	실패 (시간 초과)
테스트 8 〉	실패 (시간 초과)
테스트 9 〉	실패 (시간 초과)
테스트 10 〉	실패 (시간 초과)
테스트 11 〉	실패 (시간 초과)
테스트 12 〉	실패 (시간 초과)
테스트 13 〉	실패 (시간 초과)
테스트 14 〉	실패 (시간 초과)
테스트 15 〉	실패 (시간 초과)
테스트 16 〉	실패 (시간 초과)
테스트 17 〉	실패 (시간 초과)
테스트 18 〉	실패 (시간 초과)
테스트 19 〉	실패 (시간 초과)
테스트 20 〉	실패 (시간 초과)
테스트 21 〉	통과 (0.02ms, 10.4MB)
테스트 22 〉	통과 (0.01ms, 10.3MB)
테스트 23 〉	통과 (0.01ms, 10.3MB)
테스트 24 〉	통과 (0.01ms, 10.2MB)
테스트 25 〉	통과 (0.01ms, 10.4MB)
```
- 전략
  - 가장 최적의 다트를 던지는 경우를 구하면 되기 때문에, 처음에 BFS로 접근
  - 매 라운드마다 불, 싱글, 더블, 트리플을 던지는 경우의 수를 모두 check 하며 탐색했다.
  - 각 점수의 최솟값을 체크하지 않고, 모든 경우의 수를 체크해서 + 탐색이라 시간 초과가 났다.
    - 테스트 해보니, 점수의 최솟값을 체크해도 똑같이 시간초과가 난다.

# 다른 사람의 풀이
```python
from collections import deque
MAX = float('inf')

def solution(target):
    dp = [[MAX, -MAX] for _ in range(target+1)]
    
    q = deque([(0, 0, 0)])
    
    while q :
        score, dart, single = q.popleft()
        
        for i in range(1, 21) :
            for j in range(1, 4) :
                _score = i*j + score
                if _score > target :
                    continue
                
                _single = single + 1 if j == 1 else single
                if dp[_score][0] > dart + 1  or dp[_score][0] == dart + 1 and dp[_score][1] < _single :
                    dp[_score] = [dart+1, _single]
                    q.append((_score, dart+1, _single))
                    
        if score + 50 > target :
            continue
        if dp[score + 50][0] > dart + 1 or dp[score+50][0] == dart + 1 and dp[score + 50][1] < single + 1 :
            dp[score+50] = [dart + 1, single + 1]
            q.append((score+50, dart+1, single+1))
        
    return dp[-1]
```
- 전략
  - dp를 이용해 처음부터 다트를 던지는 경우의 수들을 체크하며, 최적의 값이 갱신될 때마다 que에 넣어가면서 체크
    - `score`: 득점 수, `dart`: 던진 횟수, `single`: 싱글과 불의 횟수
  - [{총 던진 횟수}, -1*{불+싱글 횟수}] 의 값이 작을수록 최적의 경우이다.

# 내 풀이 2
```python
from collections import deque

def solution(target):
    # [던진 횟수, -1*(불+싱글)]
    dp = [[float('inf'), -1*float('inf')] for _ in range(target+1)]
    
    # score, nums, bull_single
    q = deque([(0, 0, 0)])
    
    while q:
        score, nums, bull_single = q.popleft()
        
        ## single, double, triple을 1~20회 더하며 더 최적의 값 확인
        for shot in range(1, 21):
            for multi in range(1, 4):
                new_score = score + shot*multi
                
                if new_score>target:
                    continue
                    
                new_bull_single = bull_single+1 if multi==1 else bull_single
                if dp[new_score] > [nums+1, -1*new_bull_single]:
                    dp[new_score] = [nums+1, -1*new_bull_single]
                    q.append((new_score, nums+1, new_bull_single))
        
        # Bull을 대해 더 최적의 값 확인
        if score+50>target:
            continue
        new_score = score+50
        if [nums+1, -1*(bull_single+1)] < dp[new_score]:
            dp[new_score] = [nums+1, -1*(bull_single+1)]
            q.append((score+50, nums+1, bull_single+1))
        
        
    return [dp[-1][0], dp[-1][1]*-1]
```
- 다른 사람의 풀이 1을 참고하여, dp로 해결
