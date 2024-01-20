# 여행경로
## 문제 설명
주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

## 입출력 예
|tickets|return|
|-|-|
|[["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]|["ICN", "JFK", "HND", "IAD"]|
|[["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]]|["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"]|

## 입출력 예 설명
### 예제 #1

["ICN", "JFK", "HND", "IAD"] 순으로 방문할 수 있습니다.

### 예제 #2

["ICN", "SFO", "ATL", "ICN", "ATL", "SFO"] 순으로 방문할 수도 있지만 ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] 가 알파벳 순으로 앞섭니다.

# 내 풀이
- 가능한 경로 중에서 알파벳 순으로 빠른 경로를 찾는 문제이지만, 미리 sorting을 해놓고 알파벳이 가장 빠른 순서인 경로부터 찾으면 되므로, DFS
- 우선 stack을 사용한 DFS
```python
def solution(tickets):
    answer, stack = ["ICN"], []
    tickets = sorted(tickets, key=lambda x: x[1], reverse=True)
    
    for i, tic in enumerate(tickets):
        if tic[0]=="ICN":
            stack.append((answer+tic[1:], tickets[:i]+tickets[i+1:]))
    
    while stack:
        ans, tic = stack.pop()
        
        for i, t in enumerate(tic):
            if ans[-1]==t[0]:
                new_ans, new_tic = ans.copy(), tic.copy()
                new_ans.append(t[1])
                new_tic.pop(i)
                if len(new_tic)==0: return new_ans
                stack.append((new_ans, new_tic))
```
정확성  테스트
```
테스트 1 〉	통과 (0.06ms, 10.1MB)
테스트 2 〉	통과 (0.02ms, 10.1MB)
테스트 3 〉	통과 (0.01ms, 10.2MB)
테스트 4 〉	통과 (0.01ms, 10.1MB)
```
- 재귀를 사용한 DFS
```python
def dfs(answer, tickets):
    
    for i, tic in enumerate(tickets):
        if answer[-1]==tic[0]:
            new_ans, new_tic = answer.copy(), tickets.copy()
            new_ans.append(tic[1])
            new_tic.pop(i)
            if len(new_tic)==0: return new_ans
            ans = dfs(new_ans, new_tic)
            if ans: return ans
    

def solution(tickets):
    answer = ["ICN"]
    tickets = sorted(tickets, key=lambda x: x[1])
    
    return dfs(answer, tickets)
```
정확성  테스트
```
테스트 1 〉	통과 (0.06ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.2MB)
테스트 3 〉	통과 (0.01ms, 10.2MB)
테스트 4 〉	통과 (0.01ms, 10.2MB)
```

# 다른 사람의 풀이
```python
from collections import defaultdict 

def dfs(graph, N, key, footprint):
    print(footprint)

    if len(footprint) == N + 1:
        return footprint

    for idx, country in enumerate(graph[key]):
        graph[key].pop(idx)

        tmp = footprint[:]
        tmp.append(country)

        ret = dfs(graph, N, country, tmp)

        graph[key].insert(idx, country)

        if ret:
            return ret


def solution(tickets):
    answer = []

    graph = defaultdict(list)

    N = len(tickets)
    for ticket in tickets:
        graph[ticket[0]].append(ticket[1])
        graph[ticket[0]].sort()

    answer = dfs(graph, N, "ICN", ["ICN"])

    return answer
```
- DFS를 쓴 것은 같은
- collections.defaultdict를 사용하여, tickets의 모든 티켓들을 dict['from'] = ['to1', 'to2', ...]로 정리 후 알파벳 순서대로 sorting 해준 후 이어줌
### collections.defaultdict

```python
from collections import defaultdict

dic = defaultdict(<자료형 or lambda: >)
```

- 선언한 `<자료형 or lambda: >` 의 기본값(ex: 0, [], (), '')으로 딕셔너리의 초기값으로 지정 가능
- 숫자, 리스트, 세트, 문자열 등으로 초기화 가능
