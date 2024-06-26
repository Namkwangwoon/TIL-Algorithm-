# 외벽 점검
## 문제 설명
레스토랑을 운영하고 있는 "스카피"는 레스토랑 내부가 너무 낡아 친구들과 함께 직접 리모델링 하기로 했습니다. 레스토랑이 있는 곳은 스노우타운으로 매우 추운 지역이어서 내부 공사를 하는 도중에 주기적으로 외벽의 상태를 점검해야 할 필요가 있습니다.

레스토랑의 구조는 **완전히 동그란 모양**이고 **외벽의 총 둘레는 n미터**이며, 외벽의 몇몇 지점은 추위가 심할 경우 손상될 수도 있는 **취약한 지점들**이 있습니다. 따라서 내부 공사 도중에도 외벽의 취약 지점들이 손상되지 않았는 지, 주기적으로 친구들을 보내서 점검을 하기로 했습니다. 다만, 빠른 공사 진행을 위해 점검 시간을 1시간으로 제한했습니다. 친구들이 1시간 동안 이동할 수 있는 거리는 제각각이기 때문에, 최소한의 친구들을 투입해 취약 지점을 점검하고 나머지 친구들은 내부 공사를 돕도록 하려고 합니다. 편의 상 레스토랑의 정북 방향 지점을 0으로 나타내며, 취약 지점의 위치는 정북 방향 지점으로부터 시계 방향으로 떨어진 거리로 나타냅니다. 또, 친구들은 출발 지점부터 시계, 혹은 반시계 방향으로 외벽을 따라서만 이동합니다.

외벽의 길이 n, 취약 지점의 위치가 담긴 배열 weak, 각 친구가 1시간 동안 이동할 수 있는 거리가 담긴 배열 dist가 매개변수로 주어질 때, 취약 지점을 점검하기 위해 보내야 하는 친구 수의 최소값을 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- n은 1 이상 200 이하인 자연수입니다.
- weak의 길이는 1 이상 15 이하입니다.
  - 서로 다른 두 취약점의 위치가 같은 경우는 주어지지 않습니다.
  - 취약 지점의 위치는 오름차순으로 정렬되어 주어집니다.
  - weak의 원소는 0 이상 n - 1 이하인 정수입니다.
- dist의 길이는 1 이상 8 이하입니다.
  - dist의 원소는 1 이상 100 이하인 자연수입니다.
- 친구들을 모두 투입해도 취약 지점을 전부 점검할 수 없는 경우에는 -1을 return 해주세요.

## 입출력 예
|n|weak|dist|result|
|-|-|-|-|
|12|[1, 5, 6, 10]|[1, 2, 3, 4]|2|
|12|[1, 3, 4, 9, 10]|[3, 5, 7]|1|

## 입출력 예에 대한 설명
### 입출력 예 #1

원형 레스토랑에서 외벽의 취약 지점의 위치는 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/059fbc8d-de1e-45bf-a9f7-85c7de74684f)

친구들을 투입하는 예시 중 하나는 다음과 같습니다.

- 4m를 이동할 수 있는 친구는 10m 지점에서 출발해 시계방향으로 돌아 1m 위치에 있는 취약 지점에서 외벽 점검을 마칩니다.
- 2m를 이동할 수 있는 친구는 4.5m 지점에서 출발해 6.5m 지점에서 외벽 점검을 마칩니다.

그 외에 여러 방법들이 있지만, 두 명보다 적은 친구를 투입하는 방법은 없습니다. 따라서 친구를 최소 두 명 투입해야 합니다.

### 입출력 예 #2

원형 레스토랑에서 외벽의 취약 지점의 위치는 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/fd8261dc-8eb3-498a-aa15-073953a237b5)

7m를 이동할 수 있는 친구가 4m 지점에서 출발해 반시계 방향으로 점검을 돌면 모든 취약 지점을 점검할 수 있습니다. 따라서 친구를 최소 한 명 투입하면 됩니다.

# 내 풀이
```python
from collections import deque
from itertools import permutations

def solution(n, weak, dist):
    dist.sort(reverse=True)
    if len(weak)==1:
        return 1
    weak.append(n+weak[0])
    weak_bet = deque([weak[i+1]-weak[i] for i in range(len(weak)-1)])
    answer = float('inf')
    
    for _ in range(len(weak_bet)):
        weak_bet.rotate(1)
        for dis in permutations(dist):
            new_weak = list(weak_bet)[:-1]
            for i, d in enumerate(dis):
                sums = 0
                for j, w in enumerate(new_weak):
                    if sums+w>d:
                        new_weak = new_weak[j:]
                        new_weak.pop(0)
                        break
                    sums+=w
                else:
                    answer = min(answer, i+1)
                    break
                if len(new_weak)==0 and i!=len(dist)-1:
                    answer = min(answer, i+2)
                    break
                
    if answer>len(dist):
        return -1
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.11ms, 10.3MB)
테스트 2 〉	통과 (0.12ms, 10.3MB)
테스트 3 〉	통과 (1756.39ms, 10.1MB)
테스트 4 〉	통과 (1484.50ms, 10.4MB)
테스트 5 〉	통과 (5.01ms, 10.3MB)
테스트 6 〉	통과 (318.60ms, 10.3MB)
테스트 7 〉	통과 (0.06ms, 10.2MB)
테스트 8 〉	통과 (0.75ms, 10.4MB)
테스트 9 〉	통과 (0.87ms, 10.2MB)
테스트 10 〉	통과 (2589.08ms, 10.2MB)
테스트 11 〉	통과 (2518.61ms, 10.3MB)
테스트 12 〉	통과 (2619.49ms, 10.3MB)
테스트 13 〉	통과 (2510.42ms, 10.2MB)
테스트 14 〉	통과 (2504.63ms, 10.1MB)
테스트 15 〉	통과 (2482.49ms, 10.3MB)
테스트 16 〉	통과 (1820.91ms, 10.1MB)
테스트 17 〉	통과 (2177.86ms, 10.1MB)
테스트 18 〉	통과 (2029.25ms, 10.4MB)
테스트 19 〉	통과 (1509.31ms, 10.3MB)
테스트 20 〉	통과 (1722.49ms, 10.3MB)
테스트 21 〉	통과 (1675.89ms, 10.4MB)
테스트 22 〉	통과 (0.57ms, 10.4MB)
테스트 23 〉	통과 (0.87ms, 10.4MB)
테스트 24 〉	통과 (0.54ms, 10.3MB)
테스트 25 〉	통과 (612.93ms, 10.3MB)
```
- 전략
  - `weak`와 `dist`의 길이가 작은 것을 봤을 떄, 완전탐색이 가능하다.
  - 먼저, `weak`를 각 취약점들 사이의 거리인 `weak_bet`으로 표현
  - `weak_bet`을 deque로 만들어 `.rotate()`를 통해 모든 시작 취약점을 조사한다.
  - `dist`에 대해 `permutations()`를 하여, 모든 친구들의 순서를 조사한다.
  - 각 시작 취약점으로부터, 친구의 순서대로 취약점들 사이의 거리를 보면서, 점검한 취약점들을 제거해 나감
    - 특정 취약점을 점검했을때, 그 취약점들과 연관된 사이 거리들을 잘 제거해줘야함
  - 취약점들을 모두 점검했을때 몇 명의 친구가 필요했는지 체크하며, 최소로 필요했던 친구의 수를 구함

# 다른 사람의 풀이
```python
from itertools import permutations
def solution(n, weak, dist):
    length = len(weak)
    
    # 원형을 선형으로 
    for i in range(length) :
        weak.append(weak[i] + n)
    # 최소값을 구하는 거니까 나올 수 없는 최대값으로 초기화 
    # 모든 친구들 동원 + 1 
    answer = len(dist) +1 
    
    #  각각의 weak지점에서 시작 
    for start in range(length) :
        for friends in list(permutations(dist,len(dist))) :
            count = 1 
            # 첫번째 친구만 나옴 
            position = weak[start] + friends[0]
            # weak들을 돈다. 
            for index in range(start,start+length) :
                # 보수한 위치가 weak보다 작으면 
                if position < weak[index] :
                    # 한명 더 나온다.
                    count+=1
                    # 친구보다 나온 사람이 많으면 그만둬 (불가능)
                    if count>len(dist) :
                        break
                    # weak[index]에서 친구가 나옴 
                    position = weak[index] + friends[count-1]
            answer = min(answer, count)
    if answer > len(dist) :
        return -1 
    return answer
```
- 내 풀이와는 다르게, 취약점 사이의 거리가 아닌 취약점의 좌표를 통해 점검 가능 지역을 체크함
- 원형을 선형으로 바꿔주는 과정에서 `weak`를 `[w1, w2, w3]`를 `[w1, w2, w3, w1+n, w2+n, w3+n]`으로 바꿔줌
  - `+n`을 하지 않은 모든 원소로(`w1, w2, w3`)부터 `+n`을 갈 수 있도록 하기 위해
