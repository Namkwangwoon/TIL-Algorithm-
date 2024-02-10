# 순위
## 문제 설명
n명의 권투선수가 권투 대회에 참여했고 각각 1번부터 n번까지 번호를 받았습니다. 권투 경기는 1대1 방식으로 진행이 되고, 만약 A 선수가 B 선수보다 실력이 좋다면 A 선수는 B 선수를 항상 이깁니다. 심판은 주어진 경기 결과를 가지고 선수들의 순위를 매기려 합니다. 하지만 몇몇 경기 결과를 분실하여 정확하게 순위를 매길 수 없습니다.

선수의 수 n, 경기 결과를 담은 2차원 배열 results가 매개변수로 주어질 때 정확하게 순위를 매길 수 있는 선수의 수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 선수의 수는 1명 이상 100명 이하입니다.
- 경기 결과는 1개 이상 4,500개 이하입니다.
- results 배열 각 행 [A, B]는 A 선수가 B 선수를 이겼다는 의미입니다.
- 모든 경기 결과에는 모순이 없습니다.

## 입출력 예
|n|results|return|
|-|-|-|
|5|[[4, 3], [4, 2], [3, 2], [1, 2], [2, 5]]|2|

## 입출력 예 설명
2번 선수는 [1, 3, 4] 선수에게 패배했고 5번 선수에게 승리했기 때문에 4위입니다.

5번 선수는 4위인 2번 선수에게 패배했기 때문에 5위입니다.

# 내 풀이
- 엄청난 삽질 끝에 2시간 만에 풀었다,,
```python
def solution(n, results):
    
    strong = [set() for i in range(n+1)]
    weak = [set() for i in range(n+1)]
    
    for [s, w] in results:
        strong[w].add(s)
        strong[w].update(strong[s])
        weak[s].add(w)
        weak[s].update(weak[w])
        
        for ss in strong[s]:
            weak[ss].update(weak[s])
        for ww in weak[w]:
            strong[ww].update(strong[w])
    
    answer = 0
    
    for s, w in zip(strong, weak):
        if len(s)+len(w)==n-1:
            answer+=1
            
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.05ms, 10.2MB)
테스트 4 〉	통과 (0.02ms, 10.3MB)
테스트 5 〉	통과 (1.34ms, 10.2MB)
테스트 6 〉	통과 (2.47ms, 10.3MB)
테스트 7 〉	통과 (20.12ms, 10.4MB)
테스트 8 〉	통과 (63.13ms, 10.8MB)
테스트 9 〉	통과 (113.72ms, 11.1MB)
테스트 10 〉	통과 (140.10ms, 11.3MB)
```
- 그래프 문제이지만, 방향을 가지며, 그 방향으로 이어지는 모든 케이스를 고려해야 함
- 정확하게 순위를 매길 수 있는 선수는, **나를 제외한 나머지가 나보다 강한지 약한지를 모두 알 때**
- 따라서, 나보다 강한 상대를 나타내는 집합 & 나보다 약한 상대를 나타내는 집합 두 개를 활용
- 주의할점은, 나보다 강한 상대보다 더 강한 상대는 나보다 당연히 강하고, 나보다 약한 상대보다 더 약한 상대는 나보다 당연히 약하다.
  - 위 조건을 계속 업데이트 해줌 (나 뿐만 아니라, 다른 사람들도 업데이트)

# 다른 사람의 풀이 1
```python
from collections import defaultdict
def solution(n, results):
    answer = 0
    win, lose = defaultdict(set), defaultdict(set)
    for result in results:
            lose[result[1]].add(result[0])
            win[result[0]].add(result[1])

    for i in range(1, n + 1):
        for winner in lose[i]: win[winner].update(win[i])
        for loser in win[i]: lose[loser].update(lose[i])

    for i in range(1, n+1):
        if len(win[i]) + len(lose[i]) == n - 1: answer += 1
    return answer
```
- 기본적인 원리는 매우 비슷
- 먼저 results들로 정리해 놓고, 차례로 돌면서 업데이트
- Results의 원소 하나가 그래프에서 하나의 경로이므로, 어쨋든 경로를 타고 타고 가서 가능하다.
- 내 풀이보다 훨씬 효율적임

# 다른 사람의 풀이 2
```python
def solution(n, results):
    total = [['?' for i in range(n)] for j in range(n)]

    for i in range(n):
        total[i][i] = 'SELF'

    for result in results:
        total[result[0]-1][result[1]-1] = 'WIN'
        total[result[1]-1][result[0]-1] = 'LOSE'

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if total[i][k] == 'WIN' and total[k][j] == 'WIN':
                    total[i][j] = 'WIN'
                elif total[i][k] == 'LOSE' and total[k][j] == 'LOSE':
                    total[i][j] = 'LOSE'

    answer = 0

    for i in total:
        if '?' not in i:
            answer += 1

    return answer
```
- 플로이드 워셜
  - 원래는 모든 최단 경로를 구하는 알고리즘 (여기서는 모든 경로에 대해 모든 경우의 수의 win/lose 결정)
  - 삼중 for문을 통해 최대한 모든 win/lose를 구함
  - ? 가 없는 선수를 count
  - 효율성이 약간 떨어지긴 함
  - 장점
    - 다익스트라는 하나의 정점에서 다른 모든 정점까지의 최단 거리를 구하는 반면,
    - 플로이드-워셜은 모든 노드 간 최단 경로를 구함 & 음의 간선도 사용 가능
