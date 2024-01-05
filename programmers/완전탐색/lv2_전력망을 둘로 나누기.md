# 전력망을 둘로 나누기
## 문제 설명
n개의 송전탑이 전선을 통해 하나의 트리 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.

송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- n은 2 이상 100 이하인 자연수입니다.
- wires는 길이가 `n-1`인 정수형 2차원 배열입니다.
  - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
  - 1 ≤ v1 < v2 ≤ n 입니다.
  - 전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

## 입출력 예
|n|wires|result|
|-|-|-|
|9|[[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]]|3|
|4|[[1,2],[2,3],[3,4]]|0|
|7|[[1,2],[2,7],[3,7],[3,4],[4,5],[6,7]]|1|
## 입출력 예 설명
### 입출력 예 #1

- 다음 그림은 주어진 입력을 해결하는 방법 중 하나를 나타낸 것입니다.
![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/51faa84f-f9ab-4da9-b183-66e5754892e9)
- 4번과 7번을 연결하는 전선을 끊으면 두 전력망은 각 6개와 3개의 송전탑을 가지며, 이보다 더 비슷한 개수로 전력망을 나눌 수 없습니다.
- 또 다른 방법으로는 3번과 4번을 연결하는 전선을 끊어도 최선의 정답을 도출할 수 있습니다.

### 입출력 예 #2

- 다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/dcb8d572-a86d-4150-a1fc-c038cd320186)
- 2번과 3번을 연결하는 전선을 끊으면 두 전력망이 모두 2개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

### 입출력 예 #3

- 다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/d5829169-6618-4d1d-897d-79da211ddd88)
- 3번과 7번을 연결하는 전선을 끊으면 두 전력망이 각각 4개와 3개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

# 내 풀이
```python
from collections import deque

def bfs(node, n, graph):
    visited = [0] * (n+1)
    queue = deque([node])
    
    visited[node]=1
    count=1
    while queue:
        togo = queue.popleft()
        for go in graph[togo]:
            if visited[go]==0:
                visited[go]=1
                queue.append(go)
                count+=1
    
    return count
    

def solution(n, wires):
    graph = [[] for _ in range(n+1)]
    min_val = n
    
    for wire in wires:
        graph[wire[0]].append(wire[1])
        graph[wire[1]].append(wire[0])
        
    for rem_wire in wires:
        graph[rem_wire[0]].remove(rem_wire[1])
        graph[rem_wire[1]].remove(rem_wire[0])
        
        graph_1 = bfs(rem_wire[0], n, graph)
        graph_2 = bfs(rem_wire[1], n, graph)
        
        diff = abs(graph_1-graph_2)
        if min_val > diff:
            min_val = diff
        
        graph[rem_wire[0]].append(rem_wire[1])
        graph[rem_wire[1]].append(rem_wire[0])
        
    
    return min_val
```
정확성  테스트
```
테스트 1 〉	통과 (2.93ms, 10.2MB)
테스트 2 〉	통과 (2.55ms, 10.4MB)
테스트 3 〉	통과 (2.22ms, 10.2MB)
테스트 4 〉	통과 (2.32ms, 10.4MB)
테스트 5 〉	통과 (2.63ms, 10.4MB)
테스트 6 〉	통과 (0.03ms, 10.3MB)
테스트 7 〉	통과 (0.02ms, 10.2MB)
테스트 8 〉	통과 (0.14ms, 10.3MB)
테스트 9 〉	통과 (0.12ms, 10.3MB)
테스트 10 〉	통과 (2.30ms, 10.1MB)
테스트 11 〉	통과 (2.62ms, 10.2MB)
테스트 12 〉	통과 (2.43ms, 10.3MB)
테스트 13 〉	통과 (2.67ms, 10.2MB)
```
- 자료구조를 쓰지 않고 풀어보려고 하다가 모르겠어서 다른 사람의 코드를 참고함
- BFS(혹은 DFS)로 풀 수 있음
- 예전 문제에서 재귀 방식의 DFS를 사용해 봤다면, 이번에는 queue를 사용한 BFS로 풂 (from collections import deque)
  - deque의 append(), popleft() 사용

### collections.deque
- list와 비슷
- appendleft(), pop()으로 queue 구현 가능 (append(), pop()으로 stack 구현 가능, popleft()도 가능)
- extend(), extendleft()를 통해 iterable 데이터(list, str, tuple) 추가 가능
- rotate()를 통해 요소들을 값 만큼 회전 가능
 ```python
  deq = deque(['a', 'b', 'c', 'd', 'e'])
  deq.rotate(2)
    >> ['d', 'e', 'a', 'b', 'c']
  deq.rotate(-1)
    >> ['b', 'c', 'd', 'e', 'a']
```
# 내 풀이2
- 연습을 위해 재귀를 사용해서도 풀어봄
```python
def bfs(node, new_graph, visited):
    visited[node] = 1
    count = 1
    
    for n in new_graph[node]:
        if visited[n]==0:
            count += bfs(n, new_graph, visited)
    
    return count

def solution(n, wires):
    graph = [[] for _ in range(n+1)]
    min_abs = n
    
    for wire in wires:
        graph[wire[0]].append(wire[1])
        graph[wire[1]].append(wire[0])
    
    for wire in wires:
        new_graph = graph.copy()
        
        new_graph[wire[0]].remove(wire[1])
        new_graph[wire[1]].remove(wire[0])
        
        a = bfs(wire[0], new_graph, [0 for _ in range(n+1)])
        b = bfs(wire[1], new_graph, [0 for _ in range(n+1)])
        
        diff = abs(a-b)
        if min_abs > diff:
            min_abs = diff
        
        new_graph[wire[0]].append(wire[1])
        new_graph[wire[1]].append(wire[0])
        
    return min_abs
```
정확성  테스트
```
테스트 1 〉	통과 (3.56ms, 10.2MB)
테스트 2 〉	통과 (4.50ms, 10.2MB)
테스트 3 〉	통과 (2.69ms, 10.4MB)
테스트 4 〉	통과 (2.48ms, 10.2MB)
테스트 5 〉	통과 (3.49ms, 10.2MB)
테스트 6 〉	통과 (0.02ms, 10.2MB)
테스트 7 〉	통과 (0.02ms, 10.2MB)
테스트 8 〉	통과 (0.15ms, 10.2MB)
테스트 9 〉	통과 (0.22ms, 10.2MB)
테스트 10 〉	통과 (4.42ms, 10.2MB)
테스트 11 〉	통과 (2.81ms, 10.2MB)
테스트 12 〉	통과 (4.18ms, 10.1MB)
테스트 13 〉	통과 (2.46ms, 10.1MB)
```
