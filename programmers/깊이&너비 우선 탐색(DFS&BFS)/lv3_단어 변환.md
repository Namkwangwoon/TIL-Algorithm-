# 단어 변환
## 문제 설명
두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```
예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

## 입출력 예
|begin|target|words|return|
|-|-|-|-|
|"hit"|"cog"|["hot", "dot", "dog", "lot", "log", "cog"]|4|
|"hit"|"cog"|["hot", "dot", "dog", "lot", "log"]|0|

## 입출력 예 설명
### 예제 #1
문제에 나온 예와 같습니다.

### 예제 #2
target인 "cog"는 words 안에 없기 때문에 변환할 수 없습니다.

# 내 풀이
```python
from collections import deque

def solution(begin, target, words):
    q = deque([(0, begin)])
    l = len(begin)
    
    if target not in words:
        return 0
    
    while q:
        cnt, curr = q.popleft()
        
        if curr==target:
            return cnt
        
        for word in words:
            same_count=0
            for w, c in zip(word, curr):
                if w==c:
                    same_count += 1
            if same_count == l-1:
                q.append((cnt+1, word))
    
    return 0
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	통과 (0.05ms, 10.2MB)
테스트 3 〉	통과 (4.64ms, 10.3MB)
테스트 4 〉	통과 (0.01ms, 10.2MB)
테스트 5 〉	통과 (0.00ms, 10.2MB)
```
- 최소의 단계만 return 하면 되므로, bfs
- 변환할 수 있는 조건 1, 2를 검사할 때, for문을 사용하여 비효율적이라 생각했지만 우선 진행했다.

# 다른 사람의 풀이
```python
from collections import deque


def get_adjacent(current, words):
    for word in words:
        if len(current) != len(word):
            continue

        count = 0
        for c, w in zip(current, word):
            if c != w:
                count += 1

        if count == 1:
            yield word


def solution(begin, target, words):
    dist = {begin: 0}
    queue = deque([begin])

    while queue:
        current = queue.popleft()

        for next_word in get_adjacent(current, words):
            if next_word not in dist:
                dist[next_word] = dist[current] + 1
                queue.append(next_word)

    return dist.get(target, 0)
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.4MB)
테스트 2 〉	통과 (0.09ms, 10.2MB)
테스트 3 〉	통과 (0.43ms, 10.3MB)
테스트 4 〉	통과 (0.01ms, 10.1MB)
테스트 5 〉	통과 (0.01ms, 10.2MB)
```
- 같은 bfs를 사용
- 'dist'를 통해서 visited 처럼 queue에 단어가 중복으로 들어가는 것 방지
  - 안 될 것 같다고 생각했지만, 어차피 bfs + 최소 경로만 찾으면 되기 때문에, 결국 dist에는 최소의 경로들만 들어가게 됨
