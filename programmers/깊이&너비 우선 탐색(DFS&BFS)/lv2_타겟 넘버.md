# 타겟 넘버
## 문제 설명
n개의 음이 아닌 정수들이 있습니다. 이 정수들을 순서를 바꾸지 않고 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```
사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

## 입출력 예
|numbers|target|return|
|-|-|-|
|[1, 1, 1, 1, 1]|3|5|
|[4, 1, 2, 1]|4|2|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

```
+4+1-2+1 = 4
+4-1+2-1 = 4
```
- 총 2가지 방법이 있으므로, 2를 return 합니다.

# 내 풀이
## Queue를 사용
```python
from collections import deque

def solution(numbers, target):
    answer=0
    q = deque([(-1, 0)])
    
    while q:
        to, sums = q.popleft()
        if to==len(numbers)-1 and sums==target:
            answer+=1
        if to<len(numbers)-1:
            q.append((to+1, sums+numbers[to+1]))
            q.append((to+1, sums-numbers[to+1]))
            
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (1135.95ms, 99.8MB)
테스트 2 〉	통과 (1061.20ms, 99.4MB)
테스트 3 〉	통과 (0.79ms, 10.4MB)
테스트 4 〉	통과 (3.10ms, 10.6MB)
테스트 5 〉	통과 (28.51ms, 12.5MB)
테스트 6 〉	통과 (1.47ms, 10.3MB)
테스트 7 〉	통과 (1.46ms, 10.3MB)
테스트 8 〉	통과 (7.07ms, 10.7MB)
```
## 재귀를 사용
```python
def dfs(numbers, target, index, sums):
    if index==len(numbers)-1:
        if target==sums:
            return 1
        else:
            return 0
    
    return dfs(numbers, target, index+1, sums+numbers[index+1]) + dfs(numbers, target, index+1, sums-numbers[index+1])

def solution(numbers, target):
    return dfs(numbers, target, -1, 0)
```
정확성  테스트
```
테스트 1 〉	통과 (473.59ms, 10.2MB)
테스트 2 〉	통과 (430.42ms, 10.3MB)
테스트 3 〉	통과 (0.42ms, 10.2MB)
테스트 4 〉	통과 (1.92ms, 10.1MB)
테스트 5 〉	통과 (12.67ms, 10.2MB)
테스트 6 〉	통과 (0.86ms, 10.2MB)
테스트 7 〉	통과 (0.44ms, 10.2MB)
테스트 8 〉	통과 (3.62ms, 10.1MB)
```
