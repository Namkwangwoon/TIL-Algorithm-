# 네트워크
## 문제 설명
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

# 제한사항
- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

# 입출력 예
|n|computers|return|
|-|-|-|
|3|[[1, 1, 0], [1, 1, 0], [0, 0, 1]]|2|
|3|[[1, 1, 0], [1, 1, 1], [0, 1, 1]]|1|

# 입출력 예 설명
## 예제 #1
아래와 같이 2개의 네트워크가 있습니다.

![image](https://github.com/user-attachments/assets/c28cb95c-4cd7-47b3-ab14-4ae93c00dfa8)

## 예제 #2
아래와 같이 1개의 네트워크가 있습니다.

![image](https://github.com/user-attachments/assets/c300c947-0ba8-41c8-ab13-75637bf2f9df)

# 내 풀이
```python
from collections import defaultdict, deque

def solution(n, computers):
    graph = defaultdict(list)
    for i, computer in enumerate(computers):
        for j, comp in enumerate(computer):
            if comp==1:
                graph[i].append(j)
    
    visited = set()
    que = deque()
    answer = 0
    while len(visited)<n:
        for i in range(n):
            if i not in visited:
                que.append(i)
                break
        answer+=1
        while que:
            linked = que.popleft()
            visited.add(linked)
            for l in graph[linked]:
                if l not in visited:
                    que.append(l)
                    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.05ms, 10.3MB)
테스트 4 〉	통과 (0.09ms, 10.1MB)
테스트 5 〉	통과 (0.01ms, 10.2MB)
테스트 6 〉	통과 (0.20ms, 10.1MB)
테스트 7 〉	통과 (0.03ms, 10.2MB)
테스트 8 〉	통과 (0.13ms, 10.2MB)
테스트 9 〉	통과 (0.10ms, 10.2MB)
테스트 10 〉	통과 (0.09ms, 10.1MB)
테스트 11 〉	통과 (1.03ms, 10.2MB)
테스트 12 〉	통과 (0.39ms, 10.2MB)
테스트 13 〉	통과 (0.22ms, 10.2MB)
```
