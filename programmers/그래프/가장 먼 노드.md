# 가장 먼 노드
## 문제 설명
n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

## 입출력 예
|n|vertex|return|
|-|-|-|
|6|[[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]|3|

## 입출력 예 설명
예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/9de2737a-6d34-48a4-84dd-df35bde830e5)

# 내 풀이 1
- 첫 번째 시도는, vertex로 그래프를 만들어서, 만든 그래프 결과로 BFS 방식으로 거리를 계산
```python
from collections import deque

def solution(n, edge):
    maps = [[0 for i in range(n+1)] for j in range(n+1)]
    nodes = [0 for i in range(n+1)]
    nodes[0], nodes[1] = -1, -1
    
    for e in edge:
        maps[e[0]][e[1]] = 1
        maps[e[1]][e[0]] = 1
    
    q = deque([(1, 0)])
    
    while 0 in nodes and q:
        num, dist = q.popleft()
        
        dist+=1
        
        for i, m in enumerate(maps[num]):
            if m==1 and nodes[i]==0:
                nodes[i] = dist
                q.append((i, dist))
    
    return nodes.count(max(nodes))
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.2MB)
테스트 2 〉	통과 (0.07ms, 10.1MB)
테스트 3 〉	통과 (0.09ms, 10MB)
테스트 4 〉	통과 (0.82ms, 10.2MB)
테스트 5 〉	실패 (21.00ms, 10.4MB)
테스트 6 〉	실패 (8.59ms, 10.7MB)
테스트 7 〉	실패 (151.93ms, 15.4MB)
테스트 8 〉	실패 (시간 초과)
테스트 9 〉	실패 (99.67ms, 17.9MB)
```
- 그래프를 만드는 과정에서 (n+1) * (n+1) 그래프를 만들었는데, 이 과정에서 시간초과가 발생한 듯 보임
- 그래프를 만드는 과정, 찾는 과정에서 모두 $O(n^2)$

# 내 풀이 2
- 두 번째 시도는, vertex를 반복적으로 돌면서, dist들을 업데이트 하는 방식 사용
```python
def solution(n, edge):
    
    edge.sort(key=lambda x: min(x))
    dists = [-1 for i in range(n+1)]
    dists[0], dists[1] = 0, 0
    count=1
    
    while -1 in dists:
        new_dist = dists.copy()
        for e1, e2 in edge:
            if -1 in [dists[e1], dists[e2]] and dists[e1]+dists[e2]!=-2:
                if dists[e1]==-1: new_dist[e1]=count
                if dists[e2]==-1: new_dist[e2]=count
        dists = new_dist
        count+=1
    
    return dists.count(max(dists))
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.2MB)
테스트 2 〉	통과 (0.03ms, 10.3MB)
테스트 3 〉	통과 (0.06ms, 10.2MB)
테스트 4 〉	통과 (0.41ms, 10.3MB)
테스트 5 〉	통과 (18.04ms, 10.5MB)
테스트 6 〉	통과 (4.81ms, 10.8MB)
테스트 7 〉	통과 (80.40ms, 15.7MB)
테스트 8 〉	실패 (시간 초과)
테스트 9 〉	통과 (62.96ms, 18.2MB)
```
- 하나의 케이스에서 실패
- 그래프가 하나의 선처럼 쭉 연결된 형태에서는 비효율적이다.

# 내 풀이 3
- 결국 다른 사람의 풀이를 참고했는데, 내 풀이1과 유사하지만, 그래프를 만드는 과정에서 매우 효율적이였다.
```python
from collections import deque

def solution(n, edge):
    graph = [[] for j in range(n+1)]
    nodes = [-1 for i in range(n+1)]
    nodes[0], nodes[1] = 0, 0
    
    for v in edge:
        graph[v[0]].append(v[1])
        graph[v[1]].append(v[0])
        
    q = deque([1])
    while q and -1 in nodes:
        node = q.popleft()
        for g in graph[node]:
            if nodes[g]==-1:
                nodes[g]=nodes[node]+1
                q.append(g)
    
    return nodes.count(max(nodes))
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.1MB)
테스트 2 〉	통과 (0.04ms, 10MB)
테스트 3 〉	통과 (0.03ms, 10.2MB)
테스트 4 〉	통과 (0.23ms, 10.1MB)
테스트 5 〉	통과 (0.84ms, 10.5MB)
테스트 6 〉	통과 (3.37ms, 10.8MB)
테스트 7 〉	통과 (71.80ms, 16.7MB)
테스트 8 〉	통과 (30.27ms, 20.4MB)
테스트 9 〉	통과 (41.34ms, 20MB)
```
- 효율적인 그래프 형성 & 서치를 하면서, 최악의 케이스에서 효율성 확보
