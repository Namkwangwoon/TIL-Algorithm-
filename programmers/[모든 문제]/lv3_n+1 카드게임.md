# n+1 카드게임
## 문제 설명
당신은 1~`n` 사이의 수가 적힌 카드가 하나씩 있는 카드 뭉치와 동전 `coin`개를 이용한 게임을 하려고 합니다. 카드 뭉치에서 카드를 뽑는 순서가 정해져 있으며, 게임은 다음과 같이 진행합니다.

1. 처음에 카드 뭉치에서 카드 `n/3`장을 뽑아 모두 가집니다. (`n`은 6의 배수입니다.) 당신은 카드와 교환 가능한 동전 `coin`개를 가지고 있습니다.
2. 게임은 1라운드부터 시작되며, 각 라운드가 시작할 때 카드를 두 장 뽑습니다. 카드 뭉치에 남은 카드가 없다면 게임을 종료합니다. 뽑은 카드는 카드 한 장당 동전 하나를 소모해 가지거나, 동전을 소모하지 않고 버릴 수 있습니다.
3. 카드에 적힌 수의 합이 `n+1`이 되도록 카드 두 장을 내고 다음 라운드로 진행할 수 있습니다. 만약 카드 두 장을 낼 수 없다면 게임을 종료합니다.

예를 들어 `n` = 12, `coin` = 4이고 [3, 6, 7, 2, 1, 10, 5, 9, 8, 12, 11, 4] 순서대로 카드를 뽑도록 카드 뭉치가 섞여 있습니다.

처음에 3, 6, 7, 2 카드 4장(= `n/3`)과 동전 4개(= `coin`)를 가지고 시작합니다. 다음 라운드로 진행하기 위해 내야 할 카드 두 장에 적힌 수의 합은 13(= `n+1`)입니다. 다음과 같은 방법으로 최대 5라운드까지 도달할 수 있습니다.

1. 1라운드에서 뽑은 카드 1, 10을 동전 두 개를 소모해서 모두 가집니다. 카드 3, 10을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 1, 2, 6, 7이고 동전이 2개 남습니다.
2. 2라운드에서 뽑은 카드 5, 9를 동전을 소모하지 않고 모두 버립니다. 카드 6, 7을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 1, 2고 동전이 2개 남습니다.
3. 3라운드에서 뽑은 카드 8, 12 중 동전 한 개를 소모해서 카드 12를 가집니다. 카드 1, 12을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 2이고 동전이 1개 남습니다.
4. 4라운드에서 뽑은 카드 11, 4 중 동전 한 개를 소모해서 카드 11을 가집니다. 카드 2, 11을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드와 동전은 없습니다.
5. 5라운드에서 카드 뭉치에 남은 카드가 없으므로 게임을 종료합니다.

처음에 가진 동전수를 나타내는 정수 `coin`과 카드를 뽑는 순서대로 카드에 적힌 수를 담은 1차원 정수 배열 `cards`가 매개변수로 주어질 때, 게임에서 도달 가능한 최대 라운드의 수를 return 하도록 solution 함수를 완성해 주세요.

## 제한사항
- 0 ≤ `coin` ≤ `n`
- 6 ≤ `cards`의 길이 = `n` < 1,000
  - `cards[i]`는 `i+1`번째로 뽑는 카드에 적힌 수를 나타냅니다.
  - 1 ≤ `cards[i]` ≤ `n`
  - `cards`의 원소는 중복되지 않습니다.
- `n`은 6의 배수입니다.

## 입출력 예
|coin|cards|result|
|-|-|-|
|4|[3, 6, 7, 2, 1, 10, 5, 9, 8, 12, 11, 4]|5|
|3|[1, 2, 3, 4, 5, 8, 6, 7, 9, 10, 11, 12]|2|
|2|[5, 8, 1, 2, 9, 4, 12, 11, 3, 10, 6, 7]|4|
|10|[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]|1|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

처음에 1, 2, 3, 4 카드 4장과 동전 3개를 가지고 시작합니다. 다음 라운드로 진행하기 위해 내야 할 카드 두 장에 적힌 수의 합은 13입니다. 다음과 같은 방법으로 최대 2라운드까지 도달할 수 있습니다.

1. 1라운드에서 뽑은 카드 5, 8을 동전 두 개를 소모해서 모두 가집니다. 카드 5, 8을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 1, 2, 3, 4이고 동전이 1개 남습니다.
2. 2라운드에서 뽑은 카드 6, 7중 남은 동전 한 개로 어떤 카드를 골라도 카드에 적힌 수의 합이 13이 되도록 카드 두 장을 낼 수 없으므로 게임이 종료됩니다.

따라서 2를 return 하면 됩니다.

### 입출력 예 #3

처음에 5, 8, 1, 2 카드 4장과 동전 2개를 가지고 시작합니다. 다음 라운드로 진행하기 위해 내야 할 카드 두 장에 적힌 수의 합은 13입니다. 다음과 같은 방법으로 최대 4라운드까지 도달할 수 있습니다.

1. 1라운드에서 뽑은 카드 9, 4를 동전을 소모하지 않고 모두 버립니다. 카드 5, 8을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 1, 2고 동전이 2개 남습니다.
2. 2라운드에서 뽑은 카드 12, 11를 동전 두 개를 소모해서 모두 가집니다. 카드 1, 12를 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드는 2, 11이고 남은 동전이 없으므로 더 이상 카드를 추가로 가질 수 없습니다.
3. 3라운드에서 뽑은 카드 3, 10을 모두 버립니다. 카드 2, 11을 내고 다음 라운드로 진행합니다. 이때 손에 남은 카드와 동전은 없습니다.
4. 4라운드에서 뽑은 카드 6, 7을 모두 버립니다. 카드에 적힌 수의 합이 13이 되도록 카드 두 장을 낼 수 없으므로 게임이 종료됩니다.

따라서 4를 return 하면 됩니다.

### 입출력 예 #4

처음에 1, 2, 3, 4, 5, 6 카드 6장과 동전 10개를 가지고 시작합니다. 다음 라운드로 진행하기 위해 내야 할 카드 두 장에 적힌 수의 합은 19입니다. 1라운드에서 카드 7, 8 두 장을 모두 가져도 합이 19가 되도록 카드 두 장을 낼 수 없으므로 최대 1라운드까지만 도달할 수 있습니다.

따라서 1을 return 하면 됩니다.

# 내 풀이 1
```python
from collections import defaultdict
def solution(coin, cards):
    n = len(cards)
    index = {num:i for i,num in enumerate(cards)}
    round_coin = defaultdict(list)  # 필요한 최소 round - 필요 코인 수
    for i in range(1,n//2+1):
        ind_1, ind_2 = max(0, (index[i]-n//3)//2+1), max(0,(index[n+1-i]-n//3)//2+1)
        min_ind, max_ind = min(ind_1, ind_2), max(ind_1, ind_2)
        if max_ind==0:  # 둘 다 처음 뽑은 카드일 경우
            round_coin[0].append(0)
        elif min_ind==0:  # 둘 중 하나가 처음 뽑은 카드일 경우
            round_coin[max_ind].append(1)
        else:  # 둘 다 라운드마다 뽑을 경우
            round_coin[max_ind].append(2)
        
    stacked_coins = round_coin[0] if 0 in round_coin else []
    max_round = (2*n//3)//2
    rounds = 1
    for r in range(1, max_round+1):
        if r in round_coin:
            stacked_coins += round_coin[r]
            stacked_coins = sorted(stacked_coins)
        if len(stacked_coins)<r or sum(stacked_coins[:r])>coin:
            return rounds
        elif len(stacked_coins)>=r and sum(stacked_coins[:r])==coin:
            return rounds+1
        rounds+=1
    return rounds
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.1MB)
테스트 2 〉	통과 (0.02ms, 10.3MB)
테스트 3 〉	통과 (0.02ms, 10.3MB)
테스트 4 〉	통과 (0.05ms, 10.3MB)
테스트 5 〉	통과 (0.06ms, 10.4MB)
테스트 6 〉	실패 (0.03ms, 10.1MB)
테스트 7 〉	실패 (0.10ms, 10.2MB)
테스트 8 〉	통과 (0.18ms, 10.2MB)
테스트 9 〉	통과 (0.24ms, 10.1MB)
테스트 10 〉	통과 (0.22ms, 10.1MB)
테스트 11 〉	실패 (0.62ms, 10.3MB)
테스트 12 〉	통과 (0.85ms, 10.2MB)
테스트 13 〉	통과 (0.92ms, 10.5MB)
테스트 14 〉	통과 (1.44ms, 10.3MB)
테스트 15 〉	통과 (1.55ms, 10.4MB)
테스트 16 〉	통과 (1.33ms, 10.2MB)
테스트 17 〉	실패 (2.17ms, 10.2MB)
테스트 18 〉	통과 (2.41ms, 10.2MB)
테스트 19 〉	통과 (0.65ms, 10.2MB)
테스트 20 〉	통과 (0.63ms, 10.3MB)
```
- 전략
  - 각 라운드마다, 더했을 때 `n+1`이 되는 두 수가 현재까지 등장해야 코인을 써서 활용 가능
  - 짝이 정해져 있으므로, `1`~`n//2` 까지만 보고 해당 짝이 몇 라운드에서 짝을 완성하는지 + 해당 짝은 몇 개의 코인을 소모하는지 체크
  - 현재까지 등장한 짝들 중, 코인을 적게 소모하는 짝을 사용해야만 라운드를 최대한으로 갈 수 있음
  - 매 라운드마다 등장한 짝들을 update하여, 가장 작은 round 갯수 만큼의 코인을 소모했을 때 갖고 있는 coin 갯수보다 작으면 가능하도록 함
- 실패한 원인은, 매 라운드마다 짝을 지워주지 않았기 때문
  - 지워지지 않았던 짝이 다음 round에서 sorted() 했을 때 가장 뒤로 밀려나버리면, 결과가 꼬인다.
  
# 다른 사람의 풀이
```python
from collections import deque

def solution(coin, cards):
    N = len(cards)
    initial = set(cards[:N//3])
    not_yet = deque(cards[N//3:])
    possible = set()

    def update_possible():
        if not_yet:
            possible.add(not_yet.popleft())
            possible.add(not_yet.popleft())

    # source에서 숫자 하나를 고르고, 그 숫자에 맞는 쌍을 target에서 찾습니다.
    # 찾는 데 성공했다면 True를 반환하고, 실패했다면 False를 반환합니다.
    def remove_pair(source: set, target: set) -> bool:
        nonlocal coin, round
        for x in list(source):
            if N+1-x in target:
                source.remove(x)
                target.remove(N+1-x)
                return True
        return False

    round = 1
    while not_yet:
        update_possible()
        if remove_pair(initial, initial):
            round += 1
        elif coin >= 1 and remove_pair(initial, possible):
            coin -= 1
            round += 1
        elif coin >= 2 and remove_pair(possible, possible):
            coin -= 2
            round += 1
        else:
            break

    return round
```
- 실제 과정들을 똑같이 구현
- 우선순위대로 라운드를 진행해 나감
  1. coin을 소모하지 않는 짝
  2. coin을 1개 소모하는 짝
  3. coin을 2개 소모하는 짝
- 위 우선순위로, 각 라운드 당 한 개의 짝을 제거, 제거하지 못하면 라운드는 종료된다.

# 내 풀이 2
```python
from collections import deque

def update_possible(possible, left):
    for _ in range(2):
        possible.add(left.popleft())
        
def remove(source, target, n):
    for s in list(source):
        if n+1-s in target:
            source.remove(s)
            target.remove(n+1-s)
            return True
    return False
    

def solution(coin, cards):
    n = len(cards)
    init = set(cards[:n//3])
    possible = set()
    left = deque(cards[n//3:])
    
    rounds = 1
    while left:
        update_possible(possible, left)
        
        # 0 coin
        if remove(init, init, n):
            rounds+=1
        # 1 coin
        elif coin>=1 and remove(init, possible, n):
            rounds+=1
            coin-=1
        # 2 coin
        elif coin>=2 and remove(possible, possible, n):
            rounds+=1
            coin-=2
        else:
            break
    return rounds
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.1MB)
테스트 3 〉	통과 (0.02ms, 10.1MB)
테스트 4 〉	통과 (0.02ms, 10.2MB)
테스트 5 〉	통과 (0.03ms, 10.2MB)
테스트 6 〉	통과 (0.03ms, 10.2MB)
테스트 7 〉	통과 (0.03ms, 10.4MB)
테스트 8 〉	통과 (0.09ms, 10.2MB)
테스트 9 〉	통과 (0.27ms, 10.1MB)
테스트 10 〉	통과 (0.35ms, 10.1MB)
테스트 11 〉	통과 (0.42ms, 10.2MB)
테스트 12 〉	통과 (2.53ms, 10.3MB)
테스트 13 〉	통과 (5.70ms, 10.1MB)
테스트 14 〉	통과 (5.28ms, 10.2MB)
테스트 15 〉	통과 (7.98ms, 10.2MB)
테스트 16 〉	통과 (8.36ms, 10.3MB)
테스트 17 〉	통과 (5.36ms, 10.1MB)
테스트 18 〉	통과 (9.55ms, 10.2MB)
테스트 19 〉	통과 (0.66ms, 10.2MB)
테스트 20 〉	통과 (0.15ms, 10.1MB)
```
- 다른 사람의 풀이를 참고하여 풀이

# 내 풀이 3
```python
from collections import defaultdict, deque
def solution(coin, cards):
    n = len(cards)
    index = {num:i for i,num in enumerate(cards)}
    round_coin = defaultdict(list)  # 필요한 최소 round - 필요 코인 수
    for i in range(1,n//2+1):
        ind_1, ind_2 = max(0, (index[i]-n//3)//2+1), max(0,(index[n+1-i]-n//3)//2+1)
        min_ind, max_ind = min(ind_1, ind_2), max(ind_1, ind_2)
        if max_ind==0:  # 둘 다 처음 뽑은 카드일 경우
            round_coin[0].append(0)
        elif min_ind==0:  # 둘 중 하나가 처음 뽑은 카드일 경우
            round_coin[max_ind].append(1)
        else:  # 둘 다 라운드마다 뽑을 경우
            round_coin[max_ind].append(2)
        
    stacked_coins = round_coin[0] if 0 in round_coin else []
    max_round = (2*n//3)//2
    rounds = 1
    for r in range(1, max_round+1):
        if r in round_coin:  # 해당 라운드에 생성되는 짝이 있을 경우
            stacked_coins += round_coin[r]
            stacked_coins = sorted(stacked_coins)
        if stacked_coins:  # 사용할 수 있는 짝이 있는 경우
            if stacked_coins[0]<=coin:  # 사용할 짝이 코인으로 커버 가능한지
                coin-=stacked_coins[0]
                stacked_coins = stacked_coins[1:]
                rounds+=1
            else:
                return rounds
        else:
            return rounds
    return rounds
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.2MB)
테스트 2 〉	통과 (0.03ms, 10.2MB)
테스트 3 〉	통과 (0.03ms, 10.4MB)
테스트 4 〉	통과 (0.04ms, 10.2MB)
테스트 5 〉	통과 (0.05ms, 10.2MB)
테스트 6 〉	통과 (0.05ms, 10.2MB)
테스트 7 〉	통과 (0.14ms, 10.3MB)
테스트 8 〉	통과 (0.15ms, 10.2MB)
테스트 9 〉	통과 (0.22ms, 10.2MB)
테스트 10 〉	통과 (0.21ms, 10.2MB)
테스트 11 〉	통과 (1.25ms, 10.2MB)
테스트 12 〉	통과 (1.13ms, 10.3MB)
테스트 13 〉	통과 (1.14ms, 10.2MB)
테스트 14 〉	통과 (0.88ms, 10.4MB)
테스트 15 〉	통과 (1.33ms, 10.3MB)
테스트 16 〉	통과 (1.30ms, 10.3MB)
테스트 17 〉	통과 (1.75ms, 10.3MB)
테스트 18 〉	통과 (2.06ms, 10.4MB)
테스트 19 〉	통과 (0.84ms, 10.2MB)
테스트 20 〉	통과 (0.66ms, 10.4MB)
```
- 내 풀이 1에서 분석했던 문제점을 고쳐봤더니 통과했다.
