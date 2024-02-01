# 섬 연결하기
## 문제 설명
n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

## 제한사항

- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 `((n-1) * n) / 2`이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

## 입출력 예

|n|costs|return|
|-|-|-|
|4|[[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]]|4|

## 입출력 예 설명

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/564ce481-25a8-47e2-9ef3-c2be54c0df56)

# 다른 사람의 풀이 1
- cost가 낮은 경로부터 넣으면서 모든 노드를 지나는지 항상 확인하면 되겠다 생각했지만, 두 개 이상의 그룹(예: (1-2-3) (4-5))을 처리하지 못했다.
- 결국 다른 사람의 풀이를 참고
```python
def solution(n, costs):
    costs.sort(key=lambda x: x[2])
    answer = 0
    island = set([0])
    
    while len(island)!=n:
        for cost in costs:
            a, b, c = cost
            if a in island and b in island:
                continue
            elif a in island or b in island:
                island.update([a, b])
                answer += c
                break
    
    return answer
```
- 첫 번쩨 while 루프에서는, 0번 노드와 연결된 최소 그래프를 찾음
- 두 번째 while 루프부터, 만들어진 최소 그래프를 토대로 전체 최소 그래프로 확장
- Kruskal 알고리즘이란게 있는데, 그것와 유사하다

# 다른 사람의 풀이 2
- Kruskal 알고리즘을 사용한 풀이
```python
def ancestor(node, parents):
    if parents[node] == node:
        return node
    else:
        return ancestor(parents[node], parents)

def solution(n, costs):
    answer = 0
    edges = sorted([(x[2], x[0], x[1]) for x in costs])
    parents = [i for i in range(n)]
    bridges = 0
    for w, f, t in edges:
        if ancestor(f, parents) != ancestor(t, parents):
            answer += w
            parents[ancestor(f, parents)] = t
            bridges += 1
        if bridges == n - 1:
            break
    return answer
```
- cost가 작은 경로부터 보면서, 연결된 노드들로 그래프 생성
  - 노드의 연결은, 최 상단 부모 노드(parents)를 갖게 함으로써 표현 (= 최 상단 부모 노드가 같으면 두 노드들은 연결되어 있다.)
  - 이미 연결되어 있는 그래프는 다시 연결할 필요가 없다. (최 상단 부모 노드가 다른 노드들만 연결한다)
