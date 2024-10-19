# 주사위 고르기
## 문제 설명
A와 B가 `n`개의 주사위를 가지고 승부를 합니다. 주사위의 6개 면에 각각 하나의 수가 쓰여 있으며, 주사위를 던졌을 때 각 면이 나올 확률은 동일합니다. 각 주사위는 1 ~ `n`의 번호를 가지고 있으며, 주사위에 쓰인 수의 구성은 모두 다릅니다.

A가 먼저 `n / 2`개의 주사위를 가져가면 B가 남은 `n / 2`개의 주사위를 가져갑니다. 각각 가져간 주사위를 모두 굴린 뒤, 나온 수들을 모두 합해 점수를 계산합니다. 점수가 더 큰 쪽이 승리하며, **점수가 같다면 무승부**입니다.

A는 자신이 **승리할 확률**이 가장 높아지도록 주사위를 가져가려 합니다.

다음은 `n` = 4인 예시입니다.

|주사위|구성|
|-|-|
|#1|[1, 2, 3, 4, 5, 6]|
|#2|[3, 3, 3, 3, 4, 4]|
|#3|[1, 3, 3, 4, 4, 4]|
|#4|[1, 1, 4, 4, 5, 5]|

- 예를 들어 A가 주사위 #1, #2를 가져간 뒤 6, 3을 굴리고, B가 주사위 #3, #4를 가져간 뒤 4, 1을 굴린다면 A의 승리입니다. (6 + 3 > 4 + 1)

A가 가져가는 주사위 조합에 따라, 주사위를 굴린 1296가지 경우의 승패 결과를 세어보면 아래 표와 같습니다.

|A의 주사위|승|무|패|
|-|-|-|-|
|#1, #2|596|196|504|
|#1, #3|560|176|560|
|#1, #4|616|184|496|
|#2, #3|496|184|616|
|#2, #4|560|176|560|
|#3, #4|504|196|596|

A가 승리할 확률이 가장 높아지기 위해선 주사위 #1, #4를 가져가야 합니다.

주사위에 쓰인 수의 구성을 담은 2차원 정수 배열 `dice`가 매개변수로 주어집니다. 이때, 자신이 승리할 확률이 가장 높아지기 위해 A가 골라야 하는 주사위 번호를 **오름차순**으로 1차원 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요. 승리할 확률이 가장 높은 주사위 조합이 유일한 경우만 주어집니다.

## 제한사항
- 2 ≤ `dice`의 길이 = `n` ≤ 10
  - `n`은 2의 배수입니다.
  - `dice[i]`는 `i+1`번 주사위에 쓰인 6개의 수를 담고 있습니다.
  - `dice[i]`의 길이 = 6
  - 1 ≤ `dice[i]`의 원소 ≤ 100
  
## 입출력 예
|dice|result|
|-|-|
|[[1, 2, 3, 4, 5, 6], [3, 3, 3, 3, 4, 4], [1, 3, 3, 4, 4, 4], [1, 1, 4, 4, 5, 5]]|[1, 4]|
|[[1, 2, 3, 4, 5, 6], [2, 2, 4, 4, 6, 6]]|[2]|
|[[40, 41, 42, 43, 44, 45], [43, 43, 42, 42, 41, 41], [1, 1, 80, 80, 80, 80], [70, 70, 1, 1, 70, 70]]|[1, 3]|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

|주사위|구성|
|-|-|
|#1|[1, 2, 3, 4, 5, 6]|
|#2|[2, 2, 4, 4, 6, 6]|

A가 주사위 #2를 가져갔을 때 승리할 확률이 가장 높습니다. A가 #2, B가 #1 주사위를 굴린 결과에 따른 승패는 아래 표와 같습니다.

|주사위 결과|1 (B)|2 (B)|3 (B)|4 (B)|5 (B)|6 (B)|
|-|-|-|-|-|-|-|
|2 (A)|승|무|패|패|패|패|
|2 (A)|승|무|패|패|패|패|
|4 (A)|승|승|승|무|패|패|
|4 (A)|승|승|승|무|패|패|
|6 (A)|승|승|승|승|승|무|
|6 (A)|승|승|승|승|승|무|

### 입출력 예 #3

|주사위|구성|
|-|-|
|#1|[40, 41, 42, 43, 44, 45]|
|#2|[43, 43, 42, 42, 41, 41]|
|#3|[1, 1, 80, 80, 80, 80]|
|#4|[70, 70, 1, 1, 70, 70]|

A가 가져가는 주사위 조합에 따라, 주사위를 굴린 1296가지 경우의 승패 결과를 세어보면 아래 표와 같습니다.

|A의 주사위|승|무|패|
|-|-|-|-|
|#1, #2|704|16|576|
|#1, #3|936|24|336|
|#1, #4|360|24|912|
|#2, #3|912|24|360|
|#2, #4|336|24|936|
|#3, #4|576|16|704|

따라서 A가 주사위 #1, #3을 가져갔을 때 승리할 확률이 가장 높습니다.

# 다른 사람의 풀이
```python
from itertools import combinations, product

def solution(dice):
    n = len(dice)
    A, result = [], []
    cases = list(combinations(range(n), n//2)) # 뽑을 수 있는 경우의 수 (인덱스)
    
    for case in cases:
        dices = [dice[c] for c in case] # 인덱스에 해당하는 실제 주사위
        nums = sorted([sum(i) for i in product(*dices)]) # 뽑은 주사위의 합의 경우의 수
        A.append(nums)
        
    a, p = 0, len(A)
    for i in range(p):
        B = A[p-i-1] # B와 A는대칭의 구조를 가지므로 B = A[p-i-1]
        
        #각 조합이 이기는 횟수 이분탐색
        temp = 0
        for t in A[i]:
            left, right = 0, len(B)-1
            while left <= right:
                mid = (left + right) // 2
                if B[mid] < t:
                    left = mid + 1
                else :
                    right = mid - 1
            temp += left
        
        # 가장 승리 확률이 높은 경우의 수 반환
        if a < temp:
            a = temp
            result = [x + 1 for x in cases[i]]
    
    return result
```
- `combinations`와 `product`를 사용하려 했지만, 결국 풀이가 떠오르지 않아 다른 사람의 풀이를 참고
  - 사실상 방법은 알았지만, 시간초과에 대한 걱정으로 풀지 못했다.
- 전략
  - `combination`으로 주사위의 인덱스를 뽑는 모든 경우의 수 count
  - `product`로 뽑은 주사위의 인덱스들의 모든 합(점수)을 정렬하여 저장
  - 뽑은 주사위 인덱스(`A`)와 반대되는 인덱스(`B`)의 모든 합을 비교하여 점수를 계산
    - 이 때, 점수들이 정렬되어 있으므로, **"binary search"**를 통해 점수를 계산하여 시간을 단축
  - 점수들을 모두 더하여, 점수의 총합이 최대일 때의 인덱스에다가 모두 +1하여 return
 
# 내 풀이
```python
from itertools import combinations, product
import heapq

def score_from_search(a, b_case):
    l, r = 0, len(b_case)
    while l<r:
        mid = (l+r)//2
        b = b_case[mid]
        if a<=b:
            r = mid
        elif a>b:
            l = mid+1

    return l

def solution(dice):
    l_dice = len(dice)
    case_ind = [com for com in combinations(range(l_dice), l_dice//2)]  ## 가능한 조합
    cases = []  # 가능한 조합에서 주사위들의 모든 합의 경우
    for ind in case_ind:
        dices = [dice[i] for i in ind]
        cases.append(sorted([sum(prod) for prod in product(*dices)]))
    
    max_score = 0
    for A, a_case in zip(case_ind, cases):
        score = 0
        B = set(range(l_dice))-set(A)
        b_case = cases[case_ind.index(tuple(B))]
        for a in a_case:
            score += score_from_search(a, b_case)
        if max_score<score:
            max_score = score
            max_A = A
    return [a+1 for a in max_A]
```
- product에서 중복되는 점수가 존재하므로, lower bound binary search를 통해, 나보다 점수가 낮은 것들만 count
- `from bisect import bisect_left`를 통해 구현하기도 함
