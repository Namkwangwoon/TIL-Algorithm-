# 야근 지수
## 문제 설명
회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

## 제한 사항
- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.

## 입출력 예
|works|n|result|
|-|-|-|
|[4, 3, 3]|4|12|
|[2, 1, 2]|1|6|
|[1,1]|3|0|

## 입출력 예 설명
### 입출력 예 #1
n=4 일 때, 남은 일의 작업량이 [4, 3, 3] 이라면 야근 지수를 최소화하기 위해 4시간동안 일을 한 결과는 [2, 2, 2]입니다. 이 때 야근 지수는 22 + 22 + 22 = 12 입니다.

### 입출력 예 #2
n=1일 때, 남은 일의 작업량이 [2,1,2]라면 야근 지수를 최소화하기 위해 1시간동안 일을 한 결과는 [1,1,2]입니다. 야근지수는 12 + 12 + 22 = 6입니다.

### 입출력 예 #3

남은 작업량이 없으므로 피로도는 0입니다.

# 내 풀이 1
```python
import heapq

def solution(n, works):
    heap = list(map(lambda x: x*-1, works))
    heapq.heapify(heap)
    
    for _ in range(n):
        num = heapq.heappop(heap)
        if num==0:
            return 0
        num+=1
        heapq.heappush(heap, num)
    
    answer = 0
    for h in heap:
        answer += h**2
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.3MB)
테스트 2 〉	통과 (0.01ms, 10.1MB)
테스트 3 〉	통과 (0.05ms, 10.2MB)
테스트 4 〉	통과 (0.02ms, 10.4MB)
테스트 5 〉	통과 (0.02ms, 10.2MB)
테스트 6 〉	통과 (0.01ms, 10.4MB)
테스트 7 〉	통과 (0.01ms, 10.2MB)
테스트 8 〉	통과 (0.33ms, 10.1MB)
테스트 9 〉	통과 (0.52ms, 10.1MB)
테스트 10 〉	통과 (0.01ms, 10.2MB)
테스트 11 〉	통과 (0.01ms, 10.1MB)
테스트 12 〉	통과 (0.02ms, 10MB)
테스트 13 〉	통과 (0.01ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (395.17ms, 10.2MB)
테스트 2 〉	통과 (392.99ms, 10.4MB)
```

# 내 풀이 2
```python
import heapq

def solution(n, works):
    rev_works = []
    
    for work in works:
        heapq.heappush(rev_works, work*-1)
    
    for _ in range(n):
        if not rev_works: break
        max_work = heapq.heappop(rev_works)
        max_work += 1
        if max_work!=0: heapq.heappush(rev_works, max_work)
        
    answer = 0
    for work in rev_works:
        answer += work**2
        
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.04ms, 9.18MB)
테스트 2 〉	통과 (0.01ms, 9.22MB)
테스트 3 〉	통과 (0.01ms, 9.24MB)
테스트 4 〉	통과 (0.01ms, 9.23MB)
테스트 5 〉	통과 (0.01ms, 9.24MB)
테스트 6 〉	통과 (0.01ms, 9.2MB)
테스트 7 〉	통과 (0.01ms, 9.3MB)
테스트 8 〉	통과 (0.34ms, 9.26MB)
테스트 9 〉	통과 (0.51ms, 9.27MB)
테스트 10 〉	통과 (0.01ms, 9.11MB)
테스트 11 〉	통과 (0.01ms, 9.22MB)
테스트 12 〉	통과 (0.01ms, 9.25MB)
테스트 13 〉	통과 (0.01ms, 9.31MB)
```
효율성  테스트
```
테스트 1 〉	통과 (399.70ms, 9.29MB)
테스트 2 〉	통과 (415.16ms, 9.2MB)
```
