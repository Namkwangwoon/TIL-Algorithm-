# 이중우선순위큐
## 문제 설명
이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

|명령어|수신 탑(높이)|
|-|-|
|I 숫자|큐에 주어진 숫자를 삽입합니다.|
|D 1|큐에서 최댓값을 삭제합니다.|
|D -1|큐에서 최솟값을 삭제합니다.|

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

## 제한사항
- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
  - 원소는 “명령어 데이터” 형식으로 주어집니다.
  - 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

## 입출력 예
|operations|return|
|-|-|
|["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"]|[0,0]|
|["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"]|[333, -45]|

## 입출력 예 설명
### 입출력 예 #1

- 16과 -5643을 삽입합니다.
- 최솟값을 삭제합니다. -5643이 삭제되고 16이 남아있습니다.
- 최댓값을 삭제합니다. 16이 삭제되고 이중 우선순위 큐는 비어있습니다.
- 우선순위 큐가 비어있으므로 최댓값 삭제 연산이 무시됩니다.
- 123을 삽입합니다.
- 최솟값을 삭제합니다. 123이 삭제되고 이중 우선순위 큐는 비어있습니다.

따라서 [0, 0]을 반환합니다.

## 입출력 예 #2

- -45와 653을 삽입후 최댓값(653)을 삭제합니다. -45가 남아있습니다.
- -642, 45, 97을 삽입 후 최댓값(97), 최솟값(-642)을 삭제합니다. -45와 45가 남아있습니다.
- 333을 삽입합니다.

이중 우선순위 큐에 -45, 45, 333이 남아있으므로, [333, -45]를 반환합니다.

# 내 풀이 1
```python
import heapq

def insert(heap, rev_heap, num):
    heapq.heappush(heap, num)
    heapq.heappush(rev_heap, num*-1)
    
    
def del_min(heap, rev_heap):
    if len(heap)==0:
        return
    num = heapq.heappop(heap)
    rev_heap.remove(num*-1)
    heapq.heapify(rev_heap)
    
    
def del_max(heap, rev_heap):
    if len(heap)==0:
        return
    num = heapq.heappop(rev_heap)
    heap.remove(num*-1)
    heapq.heapify(heap)
    
    
def solution(operations):
    heap, rev_heap = [], []
    for operation in operations:
        op1, op2 = operation.split()
        
        if op1=='I':   # Case 1: Insert
            insert(heap, rev_heap, int(op2))
        else:
            if op2.startswith('-'):   # Case 3: Del MIN
                del_min(heap, rev_heap)
            else:   # Case 2: Del MAX
                del_max(heap, rev_heap)
                
    if len(heap)==0:
        return [0, 0]
    else:
        m, M = heapq.heappop(heap), heapq.heappop(rev_heap)
        return [M*-1, m]
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.4MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.06ms, 10.5MB)
테스트 4 〉	통과 (0.01ms, 10.2MB)
테스트 5 〉	통과 (0.03ms, 10.4MB)
테스트 6 〉	통과 (0.03ms, 10.4MB)
테스트 7 〉	통과 (757.77ms, 16.3MB)
테스트 8 〉	통과 (0.04ms, 10.4MB)
테스트 9 〉	통과 (0.03ms, 10.4MB)
테스트 10 〉	통과 (0.02ms, 10.5MB)
```
# 내 풀이 2
```python
import heapq

def solution(operations):
    min_que = []
    max_que = []
    
    def insert(val):
        heapq.heappush(min_que, val)
        heapq.heappush(max_que, val*-1)
        
    def del_max():
        if max_que:
            max_val = heapq.heappop(max_que)
            min_que.remove(max_val*-1)
        
    def del_min():
        if min_que:
            min_val = heapq.heappop(min_que)
            max_que.remove(min_val*-1)
    
    for oper in operations:
        op, val = oper.split()
        val = int(val)
        if op=='I':
            insert(val)
        else:
            if val==1:
                del_max()
            else:
                del_min()
    if min_que:
        return [max_que[0]*-1, min_que[0]]
    else:
        return [0, 0]
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 9.25MB)
테스트 2 〉	통과 (0.03ms, 9.28MB)
테스트 3 〉	통과 (0.03ms, 9.27MB)
테스트 4 〉	통과 (0.03ms, 9.27MB)
테스트 5 〉	통과 (0.03ms, 9.48MB)
테스트 6 〉	통과 (0.03ms, 9.4MB)
테스트 7 〉	통과 (292.57ms, 15.5MB)
테스트 8 〉	통과 (0.02ms, 9.18MB)
테스트 9 〉	통과 (0.04ms, 9.24MB)
테스트 10 〉	통과 (0.03ms, 9.21MB)
```
