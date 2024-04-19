# 양과 늑대
## 문제 설명
2진 트리 모양 초원의 각 노드에 늑대와 양이 한 마리씩 놓여 있습니다. 이 초원의 루트 노드에서 출발하여 각 노드를 돌아다니며 양을 모으려 합니다. 각 노드를 방문할 때 마다 해당 노드에 있던 양과 늑대가 당신을 따라오게 됩니다. 이때, 늑대는 양을 잡아먹을 기회를 노리고 있으며, 당신이 모은 양의 수보다 늑대의 수가 같거나 더 많아지면 바로 모든 양을 잡아먹어 버립니다. 당신은 중간에 양이 늑대에게 잡아먹히지 않도록 하면서 최대한 많은 수의 양을 모아서 다시 루트 노드로 돌아오려 합니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/f9770c70-d2da-49c8-8e97-3604801e345b)

예를 들어, 위 그림의 경우(루트 노드에는 항상 양이 있습니다) 0번 노드(루트 노드)에서 출발하면 양을 한마리 모을 수 있습니다. 다음으로 1번 노드로 이동하면 당신이 모은 양은 두 마리가 됩니다. 이때, 바로 4번 노드로 이동하면 늑대 한 마리가 당신을 따라오게 됩니다. 아직은 양 2마리, 늑대 1마리로 양이 잡아먹히지 않지만, 이후에 갈 수 있는 아직 방문하지 않은 모든 노드(2, 3, 6, 8번)에는 늑대가 있습니다. 이어서 늑대가 있는 노드로 이동한다면(예를 들어 바로 6번 노드로 이동한다면) 양 2마리, 늑대 2마리가 되어 양이 모두 잡아먹힙니다. 여기서는 0번, 1번 노드를 방문하여 양을 2마리 모은 후, 8번 노드로 이동한 후(양 2마리 늑대 1마리) 이어서 7번, 9번 노드를 방문하면 양 4마리 늑대 1마리가 됩니다. 이제 4번, 6번 노드로 이동하면 양 4마리, 늑대 3마리가 되며, 이제 5번 노드로 이동할 수 있게 됩니다. 따라서 양을 최대 5마리 모을 수 있습니다.

각 노드에 있는 양 또는 늑대에 대한 정보가 담긴 배열 `info`, 2진 트리의 각 노드들의 연결 관계를 담은 2차원 배열 `edges`가 매개변수로 주어질 때, 문제에 제시된 조건에 따라 각 노드를 방문하면서 모을 수 있는 양은 최대 몇 마리인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 2 ≤ `info`의 길이 ≤ 17
  - `info`의 원소는 0 또는 1 입니다.
  - info[i]는 i번 노드에 있는 양 또는 늑대를 나타냅니다.
  - 0은 양, 1은 늑대를 의미합니다.
  - info[0]의 값은 항상 0입니다. 즉, 0번 노드(루트 노드)에는 항상 양이 있습니다.
- `edges`의 세로(행) 길이 = `info`의 길이 - 1
  - `edges`의 가로(열) 길이 = 2
  - `edges`의 각 행은 [부모 노드 번호, 자식 노드 번호] 형태로, 서로 연결된 두 노드를 나타냅니다.
  - 동일한 간선에 대한 정보가 중복해서 주어지지 않습니다.
  - 항상 하나의 이진 트리 형태로 입력이 주어지며, 잘못된 데이터가 주어지는 경우는 없습니다.
  - 0번 노드는 항상 루트 노드입니다.
  
## 입출력 예
|info|edges|result|
|-|-|-|
|[0,0,1,1,1,0,1,0,1,0,1,1]|[[0,1],[1,2],[1,4],[0,8],[8,7],[9,10],[9,11],[4,3],[6,5],[4,6],[8,9]]|5|
|[0,1,0,1,1,0,1,0,0,1,0]|[[0,1],[0,2],[1,3],[1,4],[2,5],[2,6],[3,7],[4,8],[6,9],[9,10]]|5|

## 입출력 예 설명
### 입출력 예 #1

문제의 예시와 같습니다.

### 입출력 예 #2

주어진 입력은 다음 그림과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/1b8f5c27-7b61-4039-ad3b-a8590f8d99f1)

0번 - 2번 - 5번 - 1번 - 4번 - 8번 - 3번 - 7번 노드 순으로 이동하면 양 5마리 늑대 3마리가 됩니다. 여기서 6번, 9번 노드로 이동하면 양 5마리, 늑대 5마리가 되어 양이 모두 잡아먹히게 됩니다. 따라서 늑대에게 잡아먹히지 않도록 하면서 최대로 모을 수 있는 양은 5마리입니다.

# 내 풀이1
```python
from collections import defaultdict
import heapq

def dfs(node, visited, new_visit, sheeps, graph, count, info):
    for n in graph[node]:
        if new_visit[n]==False:
            new_visit[n] = True
            if visited[n]==True:
                dfs(n, visited, new_visit, sheeps, graph, 1, info)
            else:
                if info[n]==0:
                    heapq.heappush(sheeps, (count, n))
                    dfs(n, visited, new_visit, sheeps, graph, 1, info)
                else:
                    dfs(n, visited, new_visit, sheeps, graph, count+1, info)
    

def solution(info, edges):
    answer, s_count = 1, 1
    graph = defaultdict(list)
    sheeps = []
    heapq.heapify(sheeps)
    visited = [True]+[False for _ in range(len(info)-1)]
    
    edges.sort()
    for e in edges:
        graph[e[0]].append(e[1])
        graph[e[1]].append(e[0])
    
    while True:
    # for i in range(2):
        new_visit = [True]+[False for _ in range(len(info)-1)]
        dfs(0, visited, new_visit, sheeps, graph, 1, info)
        if not sheeps:
            return s_count
        dist, new_node = heapq.heappop(sheeps)
        answer -= dist-2
        if answer<=0:
            return s_count
        else:
            s_count+=1
        visited[new_node]=True
        sheeps = []
        heapq.heapify(sheeps)
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	실패 (0.16ms, 10.2MB)
테스트 3 〉	실패 (0.04ms, 10.4MB)
테스트 4 〉	통과 (0.02ms, 10.1MB)
테스트 5 〉	실패 (0.07ms, 10.3MB)
테스트 6 〉	실패 (0.05ms, 10.3MB)
테스트 7 〉	실패 (0.05ms, 10.3MB)
테스트 8 〉	실패 (0.08ms, 10.3MB)
테스트 9 〉	실패 (0.10ms, 10.2MB)
테스트 10 〉	실패 (0.13ms, 10.2MB)
테스트 11 〉	통과 (0.11ms, 10.4MB)
테스트 12 〉	실패 (0.09ms, 10.2MB)
테스트 13 〉	실패 (0.11ms, 10.3MB)
테스트 14 〉	실패 (0.15ms, 10.1MB)
테스트 15 〉	실패 (0.05ms, 10.2MB)
테스트 16 〉	실패 (0.11ms, 10.3MB)
테스트 17 〉	실패 (0.13ms, 10.4MB)
테스트 18 〉	통과 (0.04ms, 10.4MB)
```
- 전략
  - 트리 구조를 띄고는 있지만, 꼭 트리로 만들 필요는 없었다.
    - 데이터들의 크기에 따라 노드로 정리하는것도 아니었고, 그저 최대 두 개의 연결선을 갖는 그래프로 봤다.
  - `info`의 길이, 즉, 노드의 갯수가 17개 이하로 정해졌기 때문에, 꽤 많은 반복문을 쓸 수 있다.
  - dfs를 무조건 쓸 생각이였다.
  - 현재까지 방문한 노드들을 기억하고, 현재 방문한 노드로 부터 최소 거리로 양까지 갈 수 있는 노드들을 heap으로 구하는 과정을 반복
  - 양이 더이상 남지 않거나, 현재 방문한 노드들로부터 최소 거리의 양까지 갔을때 늑대가 같거나 더 많아지면 종료
- 타겟으로 정한 양까지의 방문을 기억할 때, 타겟 양까지 가는 길의 늑대들을 visited로 체크하는 것을 빼먹은 것 같다.
- 결국 다른 사람의 풀이를 참고

# 다른 사람의 풀이1
```python
def solution(info, edges):
    visited = [False for _ in range(len(info))]
    answer = set([])
    
    def dfs(sheep, wolf):
        if sheep>wolf:
            answer.add(sheep)
        else:
            return
        
        for e in edges:
            p, c = e
            if visited[p]==True and visited[c]==False:
                visited[c]=True
                if info[c]==0:
                    dfs(sheep+1, wolf)
                else:
                    dfs(sheep, wolf+1)
                visited[c]=False
        
    visited[0]=True
    dfs(1, 0)
    
    return max(answer)
```
- 전략
  - 내 풀이와 동일하게, dfs로 접근 + 백트래킹
  - 모든 연결선에 대한 dfs를 반복적으로 진행하여, 모든 가지 + 모든 경우의 수에 대한 dfs를 진행
    - 늑대가 양과 같거나 더 많아지면 다시 되돌아오도록 함
  - 양을 최대로 방문한 수를 기억
- dfs()의 for e in edges는 매 dfs마다 모든 연결선들을 체크하므로 약간 비효율적
  - 방문한 노드들과 관련있는 연결선들만 체크하도록 다시 코드를 짜봤다.

# 내 풀이2
```python
from collections import defaultdict

def solution(info, edges):
    graph = defaultdict(list)
    answer = 1
    visited = [True]+[False for _ in range(len(info)-1)]
    
    for e in edges:
        graph[e[0]].append(e[1])
    
    
    def dfs(sheep, wolf, node):
        nonlocal answer
        
        if wolf>=sheep:
            return
        else:
            answer = max(answer, sheep)
        
        for i, v in enumerate(visited):
            if v==True:
                for n in graph[i]:
                    if visited[n]==False:
                        visited[n]=True
                        if info[n]==0:
                            dfs(sheep+1, wolf, n)
                        else:
                            dfs(sheep, wolf+1, n)
                        visited[n]=False
        
    
    dfs(1, 0, 0)
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.20ms, 10.3MB)
테스트 3 〉	통과 (0.01ms, 10.2MB)
테스트 4 〉	통과 (0.01ms, 10.1MB)
테스트 5 〉	통과 (1.28ms, 10.3MB)
테스트 6 〉	통과 (0.33ms, 10.3MB)
테스트 7 〉	통과 (0.09ms, 10.2MB)
테스트 8 〉	통과 (0.11ms, 10.1MB)
테스트 9 〉	통과 (0.59ms, 10.2MB)
테스트 10 〉	통과 (8.73ms, 10.3MB)
테스트 11 〉	통과 (0.18ms, 10.3MB)
테스트 12 〉	통과 (1.54ms, 10.2MB)
테스트 13 〉	통과 (0.04ms, 10.2MB)
테스트 14 〉	통과 (0.10ms, 10.3MB)
테스트 15 〉	통과 (0.72ms, 10.2MB)
테스트 16 〉	통과 (1.19ms, 10.1MB)
테스트 17 〉	통과 (21.14ms, 10.3MB)
테스트 18 〉	통과 (0.50ms, 10.3MB)
```
- 전략
  - dfs + 백트래킹은 동일
  - 먼저 edge들을 그래프 형태로 만든 다음, dfs시에는 현재 방문한 노드들을 기반으로 백트래킹을 진행
  - 각 가지들에서 계속 뻗어 나가며, 늑대가 양과 같거나 더 커지면 되돌아옴
- 더 효율적으로 방문하려다가 이중 for문이 되버려서 오히려 호율이 떨어지는 경우도 생기긴 하는 것 같다.
