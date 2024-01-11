# 네트워크
## 문제 설명
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

## 제한사항
- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

## 입출력 예
|n|computers|return|
|-|-|-|
|3|[[1, 1, 0], [1, 1, 0], [0, 0, 1]]|2|
|3|[[1, 1, 0], [1, 1, 1], [0, 1, 1]]|1|

## 입출력 예 설명
### 예제 #1
아래와 같이 2개의 네트워크가 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/860611a1-6f1a-4e78-9fe9-f298a39459c9)

### 예제 #2
아래와 같이 1개의 네트워크가 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/b6cadadc-f7cc-404d-afe1-4633f621f788)

# 내 풀이
```python
def solution(n, computers):
    l = len(computers)
    nodes = [(l-i-1, l-i-1) for i in range(l)]
    answer = 0
    graph = [set([0])]
    
    while nodes:
        n = nodes.pop()
        
        for i, c in enumerate(computers[n[1]]):
            if c==1:
                nodes.append((n[1], i))
                computers[n[1]][i] = 0
                computers[i][n[1]] = 0
                
        for i, g in enumerate(graph):
            if g & set(n):
                graph[i] = g | set(n)
                break
        else:
            graph.append(set(n))
            
    return len(graph)
```
정확성  테스트
```
테스트 1 〉	통과 (0.04ms, 10.1MB)
테스트 2 〉	통과 (0.03ms, 10.2MB)
테스트 3 〉	통과 (0.14ms, 10.2MB)
테스트 4 〉	통과 (0.29ms, 10.3MB)
테스트 5 〉	통과 (0.01ms, 10.3MB)
테스트 6 〉	통과 (0.80ms, 10.2MB)
테스트 7 〉	통과 (0.06ms, 10.2MB)
테스트 8 〉	통과 (0.46ms, 10.2MB)
테스트 9 〉	통과 (0.45ms, 10.1MB)
테스트 10 〉	통과 (0.27ms, 10.3MB)
테스트 11 〉	통과 (2.38ms, 10.2MB)
테스트 12 〉	통과 (3.47ms, 10.1MB)
테스트 13 〉	통과 (0.75ms, 10.3MB)
```
- DFS
  - 그래프의 모든 경우의수를 봐야 함
  - 나중에 두 개의 그래프가 합쳐지는 불상사를 막으려면, 하나의 노드에서 가지가 계속 뻗어나가듯이 확장
- 모든 연결(자신-자신 포함)을 우선 담고, DFS 방식으로 가지가 뻗어나가듯이 그래프 확장
- 마지막에는 그래프의 갯수 출력

# 다른 사람의 풀이
```python
def solution(n, computers):
    answer = 0
    visited = [0 for i in range(n)]
    def dfs(computers, visited, start):
        stack = [start]
        while stack:
            j = stack.pop()
            if visited[j] == 0:
                visited[j] = 1
            # for i in range(len(computers)-1, -1, -1):
            for i in range(0, len(computers)):
                if computers[j][i] ==1 and visited[i] == 0:
                    stack.append(i)
    i=0
    while 0 in visited:
        if visited[i] ==0:
            dfs(computers, visited, i)
            answer +=1
        i+=1
    return answer
```
- DFS는 동일
- visited를 정의해서, 0번 노드부터 차례로 DFS, 이 과정에서 방문한 노드는 visited로 체크
- 다음 DFS 부터는 visisted를 참고하여 방문하지 않은 노드에서 DFS
