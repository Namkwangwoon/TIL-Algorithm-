# 입국심사
## 문제 설명
n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
- 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
- 심사관은 1명 이상 100,000명 이하입니다.

## 입출력 예
|n|times|return|
|-|-|-|
|6|[7, 10]|28|

## 입출력 예 설명
가장 첫 두 사람은 바로 심사를 받으러 갑니다.

7분이 되었을 때, 첫 번째 심사대가 비고 3번째 사람이 심사를 받습니다.

10분이 되었을 때, 두 번째 심사대가 비고 4번째 사람이 심사를 받습니다.

14분이 되었을 때, 첫 번째 심사대가 비고 5번째 사람이 심사를 받습니다.

20분이 되었을 때, 두 번째 심사대가 비지만 6번째 사람이 그곳에서 심사를 받지 않고 1분을 더 기다린 후에 첫 번째 심사대에서 심사를 받으면 28분에 모든 사람의 심사가 끝납니다.

# 내 풀이
```python
from collections import deque

def solution(n, times):
    l = len(times)
    answer = max(times) * n
    q = deque([[0 for i in range(l)]])
    
    while q:
        print(q)
        ent = q.popleft()
        
        for i in range(l):
            new_ent = ent.copy()
            new_ent[i] += 1
            if sum(new_ent)==n:
                answer = min(answer, max([x*y for x, y in zip(times, new_ent)]))
            elif new_ent not in q:
                q.append(new_ent)
    
    return answer
```
- 이분탐색을 쓰지 않고 BFS로 풀어보고자 했는데, 테스트 케이스에서는 통과했지만, 시간초과
- 결국 다른 사람들의 이분 탐색 코드를 참고하여 코드를 작성했다.

```python
def solution(n, times):
    left, right = 1, max(times)*n
    answer = 0
    
    while left<=right:
        mid = (left+right) // 2
        
        people = 0
        for time in times:
            people += mid//time
        
        if people>=n:
            answer = mid
            right = mid-1
        else:
            left = mid+1
            
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.1MB)
테스트 2 〉	통과 (0.10ms, 10.2MB)
테스트 3 〉	통과 (4.15ms, 10.3MB)
테스트 4 〉	통과 (319.17ms, 14.1MB)
테스트 5 〉	통과 (514.49ms, 14.3MB)
테스트 6 〉	통과 (300.60ms, 14.2MB)
테스트 7 〉	통과 (580.09ms, 14.2MB)
테스트 8 〉	통과 (678.23ms, 14.1MB)
테스트 9 〉	통과 (0.03ms, 10.2MB)
```
- answer=mid 하는 조건이 제일 중요해보임
- 위 조건문처럼, people>n과 people=n 경우를 묶어야, 그 다음 루프에서 people<n이 되어 answer가 다른 값으로 바뀌지 않음
