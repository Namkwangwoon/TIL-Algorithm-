# N으로 표현
## 문제 설명
아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

## 제한사항
- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

## 입출력 예
|N|number|return|
|-|-|-|
|5|12|4|
|2|11|3|

## 입출력 예 설명
### 예제 #1
문제에 나온 예와 같습니다.

## 예제 #2
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.

# 내 풀이
```python
from collections import defaultdict
from itertools import product

cases = defaultdict(set)

def solution(N, number):
    if N==number: return 1

    cases[1].add(N)
    
    
    for count in range(2, 9):
        if int(str(N)*count)==number: return count
        cases[count].add(int(str(N)*count))
        
        for i in range(1, count//2+1):
            case1, case2 = cases[i], cases[count-i]
            
            for c1, c2 in product(case1, case2):
                max_, min_ = max(c1, c2), min(c1, c2)
                if min_!=0: case = [c1+c2, c1*c2, max_-min_, max_//min_, 0]
                else: case = [c1+c2, c1*c2, max_-min_, 0]
                
                if number in case:
                    return count
                cases[count].update(case)
                
    return -1
```
정확성  테스트
```
테스트 1 〉	통과 (0.22ms, 10.4MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.05ms, 10.2MB)
테스트 4 〉	통과 (16.82ms, 11.1MB)
테스트 5 〉	통과 (4.54ms, 10.6MB)
테스트 6 〉	통과 (0.15ms, 10.4MB)
테스트 7 〉	통과 (0.13ms, 10.5MB)
테스트 8 〉	통과 (6.90ms, 10.5MB)
테스트 9 〉	통과 (0.00ms, 10.2MB)
```
- 동적계획법 문제라는걸 몰랐다면, 풀 수 있었을지 잘 모르겠지만.. 일단 정리해 보겠다.
- 전략
  - count를 1부터 늘려가면서, count개의 N을 사용하여 number를 만들 수 있는지 확인
  - 특정 count에서 만들 수 있는 수의 경우의 수는 다음과 같음
    - "NN...N", ex) 5555
    - "{N i개로 만든 수} (사칙연산) {N (count-i)개로 만든 수}"
  - 중복이 많으므로, set를 통해 제거해 준다
  - 연산 결과가 음수이거나 0이면 의미가 없으므로, (-)와 (/) 연산은 "{큰 값} (연산) {작은 값}"으로 진행한다.
    - 혹시나 연산 중간에 음수가 쓰일 수 있지만, 양수에 (-) 연산을 하면 결과는 동일
    - "{작은 값} (/) {큰 값}"은 항상 0 이므로, 0은 따로 추가해줌 (나중에 확인해보니, 안해줘도 되긴 함)
  - 만약 number를 만들 수 있는 경우의 수를 발견하면, count를 바로 리턴, N을 8개보다 많이 사용해도 못 만들면 -1 리턴
