# 프로세스
## 문제 설명
운영체제의 역할 중 하나는 컴퓨터 시스템의 자원을 효율적으로 관리하는 것입니다. 이 문제에서는 운영체제가 다음 규칙에 따라 프로세스를 관리할 경우 특정 프로세스가 몇 번째로 실행되는지 알아내면 됩니다.

```
1. 실행 대기 큐(Queue)에서 대기중인 프로세스 하나를 꺼냅니다.
2. 큐에 대기중인 프로세스 중 우선순위가 더 높은 프로세스가 있다면 방금 꺼낸 프로세스를 다시 큐에 넣습니다.
3. 만약 그런 프로세스가 없다면 방금 꺼낸 프로세스를 실행합니다.
  3.1 한 번 실행한 프로세스는 다시 큐에 넣지 않고 그대로 종료됩니다.
```
예를 들어 프로세스 4개 [A, B, C, D]가 순서대로 실행 대기 큐에 들어있고, 우선순위가 [2, 1, 3, 2]라면 [C, D, A, B] 순으로 실행하게 됩니다.

현재 실행 대기 큐(Queue)에 있는 프로세스의 중요도가 순서대로 담긴 배열 `priorities`와, 몇 번째로 실행되는지 알고싶은 프로세스의 위치를 알려주는 `location`이 매개변수로 주어질 때, 해당 프로세스가 몇 번째로 실행되는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- `priorities`의 길이는 1 이상 100 이하입니다.
  - `priorities`의 원소는 1 이상 9 이하의 정수입니다.
  - `priorities`의 원소는 우선순위를 나타내며 숫자가 클 수록 우선순위가 높습니다.
- `location`은 0 이상 (대기 큐에 있는 프로세스 수 - 1) 이하의 값을 가집니다.
  - `priorities`의 가장 앞에 있으면 0, 두 번째에 있으면 1 … 과 같이 표현합니다.

## 입출력 예
|priorities|location|return|
|-|-|-|
|[2, 1, 3, 2]|2|1|
|[1, 1, 9, 1, 1, 1]|0|5|

## 입출력 예 설명
### 예제 #1

문제에 나온 예와 같습니다.

### 예제 #2

6개의 프로세스 [A, B, C, D, E, F]가 대기 큐에 있고 중요도가 [1, 1, 9, 1, 1, 1] 이므로 [C, D, E, F, A, B] 순으로 실행됩니다. 따라서 A는 5번째로 실행됩니다.

# 내 풀이
```python
from collections import deque

def solution(priorities, location):
    q = deque([(i, p) for i, p in enumerate(priorities)])
    
    count = 1
    while q:
        max_p = max(q, key=lambda x: x[1])
        i, p = q.popleft()
        
        if p==max_p[1]:
            if i==location:
                return count
            else:
                count+=1
        else:
            q.append((i, p))
```
정확성  테스트
```
테스트 1 〉	통과 (0.70ms, 10.4MB)
테스트 2 〉	통과 (3.15ms, 10.2MB)
테스트 3 〉	통과 (0.19ms, 10.1MB)
테스트 4 〉	통과 (0.09ms, 10.3MB)
테스트 5 〉	통과 (0.01ms, 10.1MB)
테스트 6 〉	통과 (0.49ms, 10MB)
테스트 7 〉	통과 (0.33ms, 10.2MB)
테스트 8 〉	통과 (1.90ms, 10.2MB)
테스트 9 〉	통과 (0.04ms, 10.2MB)
테스트 10 〉	통과 (0.30ms, 10.2MB)
테스트 11 〉	통과 (1.51ms, 10.2MB)
테스트 12 〉	통과 (0.08ms, 10.2MB)
테스트 13 〉	통과 (1.32ms, 10.3MB)
테스트 14 〉	통과 (0.02ms, 10.3MB)
테스트 15 〉	통과 (0.03ms, 10.1MB)
테스트 16 〉	통과 (0.18ms, 10MB)
테스트 17 〉	통과 (2.64ms, 10.1MB)
테스트 18 〉	통과 (0.03ms, 10.2MB)
테스트 19 〉	통과 (1.69ms, 10.2MB)
테스트 20 〉	통과 (0.26ms, 10.1MB)
```
- 문제 그래도 queue를 사용
- 처음 `priority`의 index를 기억함과 동시에, 항상 queue의 max priority값을 알아야 함

# 다른 사람의 풀이
```python
def solution(priorities, location):
    queue =  [(i,p) for i,p in enumerate(priorities)]
    answer = 0
    while True:
        cur = queue.pop(0)
        if any(cur[1] < q[1] for q in queue):
            queue.append(cur)
        else:
            answer += 1
            if cur[0] == location:
                return answer
```
- `any()`를 통해, 하나라도 현재 priority보다 큰 priority가 큐 안에 있으면 다시 append
- **`pop(0)`보다 `popleft()`의 시간 복잡도가 더 낮으므로, deque를 쓰는 것이 좋다!**
