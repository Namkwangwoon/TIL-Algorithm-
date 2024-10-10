# 등대
## 문제 설명
인천 앞바다에는 1부터 `n`까지 서로 다른 번호가 매겨진 등대 `n`개가 존재합니다. 등대와 등대 사이를 오가는 뱃길이 `n-1`개 존재하여, 어느 등대에서 출발해도 다른 모든 등대까지 이동할 수 있습니다. 등대 관리자 윤성이는 전력을 아끼기 위하여, 이 중 몇 개의 등대만 켜 두려고 합니다. 하지만 등대를 아무렇게나 꺼버리면, 뱃길을 오가는 배들이 위험할 수 있습니다. 한 뱃길의 양쪽 끝 등대 중 적어도 하나는 켜져 있도록 등대를 켜 두어야 합니다.

예를 들어, 아래 그림과 같이 등대 8개와 7개의 뱃길들이 있다고 합시다. 이 경우 1번 등대와 5번 등대 두 개만 켜 두어도 모든 뱃길은 양쪽 끝 등대 중 하나가 켜져 있으므로, 배들은 안전하게 운항할 수 있습니다.

![image](https://github.com/user-attachments/assets/04d5715b-08b9-43b2-8fd0-939bedff1883)

등대의 개수 `n`과 각 뱃길이 연결된 등대의 번호를 담은 이차원 배열 `lighthouse`가 매개변수로 주어집니다. 윤성이가 켜 두어야 하는 등대 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 2 ≤ `n` ≤ 100,000
- `lighthouse`의 길이 = `n – 1`
  - `lighthouse` 배열의 각 행 `[a, b]`는 `a`번 등대와 `b`번 등대가 뱃길로 연결되어 있다는 의미입니다.
    - 1 ≤ `a` ≠ `b` ≤ `n`
    - 모든 등대는 서로 다른 등대로 이동할 수 있는 뱃길이 존재하도록 입력이 주어집니다.

## 입출력 예
|n|lighthouse|result|
|-|-|-|
|8|[[1, 2], [1, 3], [1, 4], [1, 5], [5, 6], [5, 7], [5, 8]]|2|
|10|[[4, 1], [5, 1], [5, 6], [7, 6], [1, 2], [1, 3], [6, 8], [2, 9], [9, 10]]|3|

# 입출력 예 설명
## 입출력 예 #1

- 본문에서 설명한 예시입니다.

### 입출력 예 #2

- 뱃길은 아래 그림과 같이 연결되어 있습니다. 윤성이가 이중 1, 6, 9번 등대 3개만 켜 두어도 모든 뱃길은 양쪽 끝 등대 중 하나가 켜져 있게 되고, 이때의 등대 개수 3개가 최소가 됩니다.

![image](https://github.com/user-attachments/assets/3e7bc74b-b01e-41a0-935e-a429ebcd96f1)

# 내 풀이 1
```python
from collections import defaultdict

def search(node, light, graph, visited):
    num_light = light*1
    
    to_visit = [n for n in graph[node] if not visited[n]]
    
    # Leaf node
    if not to_visit:
        return num_light
    
    # Not leaf node
    for n in to_visit:
        visited[n]=True
        new_light = search(n, True, graph, visited)
        if light:  # 현재 light이면, 자식 노드는 light이든 아니든 상관 없다 (더 적은 light 개수로)
            new_light = min(new_light, search(n, False, graph, visited))
        visited[n]=False
        num_light += new_light
        
    return num_light
        

def solution(n, lighthouse):
    graph = defaultdict(list)
    for lh in lighthouse:
        a, b = lh
        graph[a].append(b)
        graph[b].append(a)
    visited = [False]*(n+1)
    visited[1]=True
    
    # 1번 노드가 등대가 아닌 경우
    a = search(1, False, graph, visited)
    
    # 1번 노드가 등대인 경우
    b = search(1, True, graph, visited)
    
    return min(a, b)
```
정확성  테스트
```
테스트 1 〉	실패 (시간 초과)
테스트 2 〉	통과 (1055.55ms, 41.4MB)
테스트 3 〉	통과 (9302.80ms, 41.4MB)
테스트 4 〉	통과 (8954.52ms, 41.4MB)
테스트 5 〉	통과 (6014.08ms, 41.5MB)
테스트 6 〉	통과 (1443.60ms, 41.4MB)
테스트 7 〉	통과 (1180.68ms, 41.4MB)
테스트 8 〉	실패 (시간 초과)
테스트 9 〉	실패 (시간 초과)
테스트 10 〉	실패 (시간 초과)
테스트 11 〉	실패 (시간 초과)
테스트 12 〉	통과 (8800.49ms, 18.7MB)
테스트 13 〉	통과 (1255.93ms, 12.6MB)
테스트 14 〉	통과 (0.01ms, 10.2MB)
테스트 15 〉	통과 (46.82ms, 10.6MB)
테스트 16 〉	통과 (1299.63ms, 11.2MB)
```
- 답은 맞추는 것 같았지만 시간초과가 발생했다.
- 결국 다른 사람의 풀이를 참고

# 다른 사람의 풀이
```python
import sys
from collections import defaultdict
sys.setrecursionlimit(1000001)

A = defaultdict(list)
vis = [False] * 1000001

# 자신을 포함한 subtree에서, 내가 켜졌을 때의 최소 점등 등대 개수와
# 내가 꺼졌을 때의 최소 점등 등대 개수를 반환합니다.
def dfs(u):
    vis[u] = True
    if not A[u]:
        # u가 leaf라면 내가 켜졌을 떄의 최소 점등 등대 개수는 1
        # 내가 꺼졌을 때의 최소 점등 등대 개수는 0
        return 1, 0
    
    # u가 leaf가 아니라면
    on, off = 1, 0
    for v in [v for v in A[u] if not vis[v]]:
        # 내가 켜졌다면 child들은 켜지든 꺼지든 상관 없습니다. -> 킨 것과 끈 것중 최소값을 취함
        # 내가 꺼졌다면 child들은 무조건 켜져야 합니다.
        # 이 점을 생각해서 leaf들의 정보를 취합, 정리합니다.
        child_on, child_off = dfs(v)
        on += min(child_on, child_off)
        off += child_on
    return on, off

def solution(n, lighthouse):
    for u, v in lighthouse:
        A[u].append(v)
        A[v].append(u)
        
    on, off = dfs(1)
    return min(on, off)
```
- 풀이의 방법은 매우 비슷했다. (같은 dfs)
- 현재 노드의 등대가 켜졌을 때, 자식 노드의 등대가 꺼진 경우 vs 켜진 경우 비교할 때,
  - 나의 풀이는 visited를 사용해 백트래킹 하며 두 번의 재귀를 사용했지만,
  - 지금 풀이는 한 번의 재귀로 자식 노드의 두 경우를 한 번에 도출

# 내 풀이 2
```python
from collections import defaultdict
import sys
sys.setrecursionlimit(1000000)

def search(node, graph, visited):
    to_visit = [n for n in graph[node] if not visited[n]]
    
    if not to_visit:
        return 0, 1
    
    cur_dark, cur_light = 0, 1
    for n in to_visit:
        visited[n] = True
        min_, max_ = search(n, graph, visited)
        cur_dark += max_
        cur_light += min(min_, max_)
        
    return cur_dark, cur_light
        

def solution(n, lighthouse):
    graph = defaultdict(list)
    for lh in lighthouse:
        a, b = lh
        graph[a].append(b)
        graph[b].append(a)
    
    # 1번 노드부터 시작
    visited = [False]*(n+1)
    visited[1]=True
    visited[0]=True
    min_, max_ = search(1, graph, visited)
    
    return min(min_, max_)
```
- search에서 한 번의 호출로 현재 노드가 light or not일 때의 두 경우를 한 번에 도출
- `sys.setrecursionlimit`으로 재귀 횟수 제한을 늘리는 과정도 필요!

# 다른 사람의 풀이 2
```python
import sys
from collections import defaultdict
sys.setrecursionlimit(1000001)

A = defaultdict(list)
vis = [False] * 1000001

# 자신을 포함한 subtree에서, 내가 켜졌을 때의 최소 점등 등대 개수와
# 내가 꺼졌을 때의 최소 점등 등대 개수를 반환합니다.
def dfs(u):
    vis[u] = True
    if not A[u]:
        # u가 leaf라면 내가 켜졌을 떄의 최소 점등 등대 개수는 1
        # 내가 꺼졌을 때의 최소 점등 등대 개수는 0
        return 1, 0
    
    # u가 leaf가 아니라면
    on, off = 1, 0
    for v in [v for v in A[u] if not vis[v]]:
        # 내가 켜졌다면 child들은 켜지든 꺼지든 상관 없습니다. -> 킨 것과 끈 것중 최소값을 취함
        # 내가 꺼졌다면 child들은 무조건 켜져야 합니다.
        # 이 점을 생각해서 leaf들의 정보를 취합, 정리합니다.
        child_on, child_off = dfs(v)
        on += min(child_on, child_off)
        off += child_on
    return on, off

def solution(n, lighthouse):
    for u, v in lighthouse:
        A[u].append(v)
        A[v].append(u)
        
    on, off = dfs(1)
    return min(on, off)
```
- 전체적인 코드 흐름은 똑같지만, dp 개념의 배열을 사용해서 더 쉬운 코드를 작성
