# 상담원 인원
## 문제 설명
현대모비스는 우수한 SW 인재 채용을 위해 상시로 채용 설명회를 진행하고 있습니다. 채용 설명회에서는 채용과 관련된 상담을 원하는 참가자에게 멘토와 1:1로 상담할 수 있는 기회를 제공합니다. 채용 설명회에는 멘토 `n`명이 있으며, 1~`k`번으로 분류되는 상담 유형이 있습니다. 각 멘토는 `k`개의 상담 유형 중 하나만 담당할 수 있습니다. 멘토는 자신이 담당하는 유형의 상담만 가능하며, 다른 유형의 상담은 불가능합니다. 멘토는 동시에 참가자 한 명과만 상담 가능하며, 상담 시간은 정확히 참가자가 요청한 시간만큼 걸립니다.

참가자가 상담 요청을 하면 아래와 같은 규칙대로 상담을 진행합니다.

- 상담을 원하는 참가자가 상담 요청을 했을 때, 참가자의 상담 유형을 담당하는 멘토 중 상담 중이 아닌 멘토와 상담을 시작합니다.
- 만약 참가자의 상담 유형을 담당하는 멘토가 모두 상담 중이라면, 자신의 차례가 올 때까지 기다립니다. **참가자가 기다린 시간은 참가자가 상담 요청했을 때부터 멘토와 상담을 시작할 때까지의 시간입니다.**
- 모든 멘토는 상담이 끝났을 때 자신의 상담 유형의 상담을 받기 위해 기다리고 있는 참가자가 있으면 즉시 상담을 시작합니다. 이때, **먼저 상담 요청한 참가자가 우선됩니다.**

참가자의 상담 요청 정보가 주어질 때, 참가자가 상담을 요청했을 때부터 상담을 시작하기까지 기다린 시간의 합이 최소가 되도록 각 상담 유형별로 멘토 인원을 정하려 합니다. **단, 각 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.**

예를 들어, 5명의 멘토가 있고 1~3번의 3가지 상담 유형이 있을 때 아래와 같은 참가자의 상담 요청이 있습니다.

**참가자의 상담 요청**
|참가자 번호|시각|상담 시간|상담 유형|
|-|-|-|-|
|1번 참가자|10분|60분|1번 유형|
|2번 참가자|15분|100분|3번|유형|
|3번 참가자|20분|30분|1번|유형|
|4번 참가자|30분|50분|3번 유형|
|5번 참가자|50분|40분|1번 유형|
|6번 참가자|60분|30분|2번 유형|
|7번 참가자|65분|30분|1번 유형|
|8번 참가자|70분|100분|2번 유형|

이때, 멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 25로 최소가 됩니다.

|1번 유형|2번 유형|3번 유형|
|-|-|-|
|2명|1명|2명|

**1번 유형**

1번 유형을 담당하는 멘토가 2명 있습니다.

- 1번 참가자가 상담 요청했을 때, 멘토#1과 10분~70분 동안 상담을 합니다.
- 3번 참가자가 상담 요청했을 때, 멘토#2와 20분~50분 동안 상담을 합니다.
- 5번 참가자가 상담 요청했을 때, 멘토#2와 50분~90분 동안 상담을 합니다.
- 7번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 5분 동안 기다리고 멘토#1과 70분~100분 동안 상담을 합니다.

**2번 유형**

2번 유형을 담당하는 멘토가 1명 있습니다.

- 6번 참가자가 상담 요청했을 때, 멘토와 60분~90분 동안 상담을 합니다.
- 8번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 6번 참가자의 상담이 끝날 때까지 20분 동안 기다리고 90분~190분 동안 상담을 합니다.

**3번 유형**

3번 유형을 담당하는 멘토가 2명 있습니다.

- 2번 참가자가 상담 요청했을 때, 멘토#1과 15분~115분 동안 상담을 합니다.
- 4번 참가자가 상담 요청했을 때, 멘토#2와 30분~80분 동안 상담을 합니다.

상담 유형의 수를 나타내는 정수 `k`, 멘토의 수를 나타내는 정수 `n`과 참가자의 상담 요청을 담은 2차원 정수 배열 `reqs`가 매개변수로 주어집니다. 멘토 인원을 적절히 배정했을 때 참가자들이 상담을 받기까지 기다린 시간을 모두 합한 값의 최솟값을 return 하도록 solution 함수를 완성해 주세요.

## 제한사항
- 1 ≤ `k` ≤ 5
- `k` ≤ `n` ≤ 20
- 3 ≤ `reqs`의 길이 ≤ 300
  - `reqs`의 원소는 [`a`, `b`, `c`] 형태의 길이가 3인 정수 배열이며, `c`번 유형의 상담을 원하는 참가자가 `a`분에 `b`분 동안의 상담을 요청했음을 의미합니다.
  - 1 ≤ `a` ≤ 1,000
  - 1 ≤ `b` ≤ 100
  - 1 ≤ `c` ≤ `k`
  - `reqs`는 `a`를 기준으로 오름차순으로 정렬되어 있습니다.
  - `reqs` 배열에서 `a`는 중복되지 않습니다. 즉, 참가자가 상담 요청한 시각은 모두 다릅니다.

## 입출력 예
|k|n|reqs|result|
|-|-|-|-|
|3|5|[[10, 60, 1], [15, 100, 3], [20, 30, 1], [30, 50, 3], [50, 40, 1], [60, 30, 2], [65, 30, 1], [70, 100, 2]]|25|
|2|3|[[5, 55, 2], [10, 90, 2], [20, 40, 2], [50, 45, 2], [100, 50, 2]]|90|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

**참가자의 상담 요청**

|참가자 번호|시각|상담 시간|상담 유형|
|-|-|-|-|
|1번 참가자|5분|55분|2번 유형|
|2번 참가자|10분|90분|2번 유형|
|3번 참가자|20분|40분|2번 유형|
|4번 참가자|50분|45분|2번 유형|
|5번 참가자|100분|50분|2번 유형|

멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 90으로 최소가 됩니다.

|1번 유형|2번 유형|
|-|-|
|1명|2명|

**1번 유형**

1번 유형을 담당하는 멘토가 1명 있습니다. 1번 유형의 상담을 요청한 참가자가 없지만, 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.

**2번 유형**

2번 유형을 담당하는 멘토가 2명 있습니다.

- 1번 참가자가 상담 요청했을 때, 멘토#1과 5분~60분 동안 상담을 합니다.
- 2번 참가자가 상담 요청했을 때, 멘토#2와 10분~100분 동안 상담을 합니다.
- 3번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 40분 동안 기다리고 멘토#1과 60분~100분 동안 상담을 합니다. 1번 참가자의 상담이 끝났을 때 4번 참가자도 기다리고 있었지만, 먼저 상담 요청한 3번 참가자가 우선됩니다.
- 4번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 2번 참가자의 상담이 끝날 때까지 50분 동안 기다리고 멘토#2와 100분~145분 동안 상담을 합니다. 이때, 멘토#1과 상담할 수도 있지만 어느 멘토와 상담해도 상관없습니다.
- 5번 참가자가 상담 요청했을 때, 멘토#1과 100분~150분 동안 상담을 합니다.

따라서 90을 return 하면 됩니다.

# 내 풀이 1
```python
from collections import defaultdict, deque
from itertools import combinations

def make_req_se(reqs, req_se):
    for req in reqs:
        req_se[req[-1]].append((req[0], req[0]+req[1]))
    
def fill_times(times, req_se, n, k):
    for K in range(1, k+1): # 유형 번호
        require = req_se[K]
        for mentor in range(1, n+1):    # 멘토의 수
            wait = 0
            end_times = sorted([r[1] for r in require[:mentor]])
            for i in range(mentor, len(require), mentor):    # 요청의 끊음 단위
                end_point = min(i+mentor, len(require))
                req = require[i:end_point]
                print(req)
                new_end_times = []
                for j, r in enumerate(req): # 마지막 끊음 단위의 끝나는 시각으로부터 기다리는 시간 계산
                    if end_times[j]>r[0]:
                        wait+=end_times[j]-r[0]
                        new_end_times.append(end_times[j] + r[1]-r[0])
                    else:
                        new_end_times.append(r[1])
                end_times = sorted(new_end_times)
            times[K][mentor] = wait
    

def solution(k, n, reqs):
    times = [[0]*(n+1) for _ in range(k+1)]
    req_se = defaultdict(list)
    
    make_req_se(reqs, req_se)
    l_req_type = len(req_se)    # 요청이 한 번이라도 들어온 상담 유형 수
    zero_types = k-l_req_type   # 요청이 한 번도 안들어온 상담 유형 수
    types = list(req_se.keys())
    print(req_se)
    
    fill_times(times, req_se, n, k)
    print(times)
    
    if l_req_type==1:
        return times[types[0]][n-zero_types]
    min_waits = float('inf')
    remain_mentors = n-zero_types
    # 요청이 있는 상담 유형에 상담사들을 배치하는 모든 경우의 수
    for comb in combinations([i for i in range(1, remain_mentors)], l_req_type-1):
        comb=sorted([0]+list(comb)+[remain_mentors])
        splits = [comb[i+1]-comb[i] for i in range(len(comb)-1)]
        print(splits)
        cur_waits = 0
        for i, split in enumerate(splits):
            cur_waits+=times[types[i]][split]
        min_waits = min(min_waits, cur_waits)
    return min_waits
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.3MB)
테스트 2 〉	통과 (0.07ms, 10.3MB)
테스트 3 〉	통과 (0.09ms, 10.4MB)
테스트 4 〉	통과 (0.11ms, 10.3MB)
테스트 5 〉	실패 (0.50ms, 10.5MB)
테스트 6 〉	실패 (0.76ms, 10.3MB)
테스트 7 〉	통과 (1.08ms, 10.3MB)
테스트 8 〉	통과 (1.74ms, 10.5MB)
테스트 9 〉	실패 (4.69ms, 10.3MB)
테스트 10 〉	실패 (15.23ms, 10.4MB)
테스트 11 〉	실패 (14.40ms, 10.2MB)
테스트 12 〉	실패 (14.44ms, 10.4MB)
테스트 13 〉	실패 (15.61ms, 10.3MB)
테스트 14 〉	실패 (16.65ms, 10.3MB)
테스트 15 〉	실패 (15.34ms, 10.3MB)
테스트 16 〉	실패 (15.03ms, 10.3MB)
테스트 17 〉	실패 (14.63ms, 10.3MB)
테스트 18 〉	실패 (14.39ms, 10.4MB)
테스트 19 〉	실패 (14.85ms, 10.3MB)
테스트 20 〉	실패 (16.88ms, 10.3MB)
```
- 테스트 케이스는 통과했지만, 결과적으로 실패했다.
- 복잡한 과정들을 나름 모듈화 했지만, 어딘가에서 실수한 것으로 보인다.
- 결국 다른 사람의 풀이를 참고

# 다른 사람의 풀이
```python
from heapq import heappush, heappop
from itertools import combinations_with_replacement, permutations


def solution(k, n, reqs):
    def cal_wait_time(waitings, n):  # 유형 별 waiting_list에 n명의 상담 원이 있을 때 대기 시간 계산
        total_time = 0
        counsel_list = []
        for _ in range(n):
            heappush(counsel_list, 0)
        for start, duration in waitings:
            prev_end = heappop(counsel_list)  # 자리가 생기는 시간
            if start > prev_end:  # 바로 상담 가능
                heappush(counsel_list, duration)
            else:
                wait_time = prev_end - start
                total_time += wait_time
                heappush(counsel_list, duration + wait_time)
        return total_time

    result = 1e9

    waiting_list = [[] for _ in range(k)]
    for req in reqs:
        waiting_list[req[2] - 1].append([req[0], req[0] + req[1]])

    cases = set()
    for comb in combinations_with_replacement([i for i in range(1, n - k + 2)], r=k):
        if sum(comb) == n:
            for perm in permutations(comb, k):
                cases.add(perm)

    for case in cases:
        time = 0
        for i in range(k):
            time += cal_wait_time(waiting_list[i], case[i])
        result = min(result, time)

    return result
```
- 전략
  - 내 풀이와 비슷하게, 모든 경우의 수 계산
  - 내 풀이와 대부분 비슷하지만, 특정 상담 유형에서 상담원 수에 따른 대기 시간을 계산할 때 heapq를 사용
    - 내 풀이처럼, 상담원 수의 단위대로 끊는 것 보다 훨씬 정확하다.
    - 상담 유형의 참가 인원이 항상 상담원 수로 나눠 떨어지지 않기 때문
  - **itertools.combinations_with_replacement()** : 중복이 있는 combinations
    - ex.
      ```python
      combinations_with_replacement([1, 2, 3], 2)

      >> (1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)
      ```
    - 상담이 들어온 유형들에 대해, 각 유형에 상담원을 배치하는 모든 경우의 수를 구할 때 유리함

# 내 풀이 2
```python
from collections import defaultdict
from itertools import combinations
import heapq

def make_req_se(reqs, req_se):
    for req in reqs:
        req_se[req[-1]].append((req[0], req[0]+req[1]))
    
def fill_times(times, req_se, n, k):
    for K in range(1, k+1): # 유형 번호
        require = req_se[K]
        for mentor in range(1, n+1):    # 멘토의 수
            heap = []
            wait = 0
            for m in range(min(mentor, len(require))):
                heapq.heappush(heap, require[m][1])
            for req in require[mentor:]:
                end_time = heapq.heappop(heap)
                if end_time>req[0]:
                    wait+=end_time-req[0]
                    heapq.heappush(heap, end_time+req[1]-req[0])
                else:
                    heapq.heappush(heap, req[1])
            times[K][mentor] = wait
                    
    

def solution(k, n, reqs):
    times = [[0]*(n+1) for _ in range(k+1)]
    req_se = defaultdict(list)
    
    make_req_se(reqs, req_se)
    l_req_type = len(req_se)    # 요청이 한 번이라도 들어온 상담 유형 수
    zero_types = k-l_req_type   # 요청이 한 번도 안들어온 상담 유형 수
    types = list(req_se.keys())
    
    fill_times(times, req_se, n, k)
    
    if l_req_type==1:
        return times[types[0]][n-zero_types]
    min_waits = float('inf')
    remain_mentors = n-zero_types
    # 요청이 있는 상담 유형에 상담사들을 배치하는 모든 경우의 수
    for comb in combinations([i for i in range(1, remain_mentors)], l_req_type-1):
        comb = [0]+list(comb)+[remain_mentors]
        splits = [comb[i+1]-comb[i] for i in range(len(comb)-1)]
        cur_waits = 0
        for i, split in enumerate(splits):
            cur_waits+=times[types[i]][split]
        min_waits = min(min_waits, cur_waits)
    return min_waits
```
정확성  테스트
```
테스트 1 〉	통과 (0.05ms, 10.2MB)
테스트 2 〉	통과 (0.07ms, 10.3MB)
테스트 3 〉	통과 (0.07ms, 10.2MB)
테스트 4 〉	통과 (0.08ms, 10.4MB)
테스트 5 〉	통과 (0.18ms, 10.2MB)
테스트 6 〉	통과 (0.54ms, 10.2MB)
테스트 7 〉	통과 (0.29ms, 10.3MB)
테스트 8 〉	통과 (0.85ms, 10.2MB)
테스트 9 〉	통과 (4.42ms, 10.4MB)
테스트 10 〉	통과 (9.18ms, 10.4MB)
테스트 11 〉	통과 (8.80ms, 10.2MB)
테스트 12 〉	통과 (8.73ms, 10.4MB)
테스트 13 〉	통과 (8.80ms, 10.2MB)
테스트 14 〉	통과 (9.84ms, 10.4MB)
테스트 15 〉	통과 (8.64ms, 10.3MB)
테스트 16 〉	통과 (15.91ms, 10.2MB)
테스트 17 〉	통과 (8.70ms, 10.3MB)
테스트 18 〉	통과 (9.13ms, 10.4MB)
테스트 19 〉	통과 (9.62ms, 10.2MB)
테스트 20 〉	통과 (9.21ms, 10.3MB)
```
- 내 풀이1에서 각 유형별 상담원 수에 따른 대기 시간을 계산할 때 heapq로 바꿔줬더니 통과함
- 전략
  - 모든 상담들을 유형별로 정리
  - 각 상담 유형들에 대해, 배치된 상담원 수에 따른 대기 시간들을 `times`에 모두 저장
  - 요청된 상담들에 따라, 요청이 없었던 유형에 상담원을 1명씩 배치하고, 나머지 유형들에 대한 모든 경우의 수를 구함
  - 가장 짧은 대기 시간을 출력
