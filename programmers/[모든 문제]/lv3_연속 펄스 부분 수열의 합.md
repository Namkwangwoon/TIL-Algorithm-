# 연속 펄스 부분 수열의 합
## 문제 설명
어떤 수열의 연속 부분 수열에 같은 길이의 펄스 수열을 각 원소끼리 곱하여 연속 펄스 부분 수열을 만들려 합니다. 펄스 수열이란 [1, -1, 1, -1 …] 또는 [-1, 1, -1, 1 …] 과 같이 1 또는 -1로 시작하면서 1과 -1이 번갈아 나오는 수열입니다.

예를 들어 수열 [2, 3, -6, 1, 3, -1, 2, 4]의 연속 부분 수열 [3, -6, 1]에 펄스 수열 [1, -1, 1]을 곱하면 연속 펄스 부분수열은 [3, 6, 1]이 됩니다. 또 다른 예시로 연속 부분 수열 [3, -1, 2, 4]에 펄스 수열 [-1, 1, -1, 1]을 곱하면 연속 펄스 부분수열은 [-3, -1, -2, 4]이 됩니다.

정수 수열 `sequence`가 매개변수로 주어질 때, 연속 펄스 부분 수열의 합 중 가장 큰 것을 return 하도록 solution 함수를 완성해주세요.

## 제한 사항
- 1 ≤ `sequence`의 길이 ≤ 500,000
- -100,000 ≤ `sequence`의 원소 ≤ 100,000
  - `sequence`의 원소는 정수입니다.

## 입출력 예
|sequence|result|
|-|-|
|[2, 3, -6, 1, 3, -1, 2, 4]|10|

## 입출력 예 설명
주어진 수열의 연속 부분 수열 [3, -6, 1]에 펄스 수열 [1, -1, 1]을 곱하여 연속 펄스 부분 수열 [3, 6, 1]을 얻을 수 있고 그 합은 10으로서 가장 큽니다.

# 내 풀이
```python
from collections import deque

def solution(sequence):
    min_val, max_val = min(sequence), max(sequence)
    l = len(sequence)
    answer = max(min_val*-1, max_val)
    
    for i in range(l-1):
        for j in range(i+1, l):
            odd, even = sum(sequence[i:j+1:2]), sum(sequence[i+1:j+1:2])
            answer = max(answer, odd-even, even-odd)
            
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.00ms, 10.1MB)
테스트 3 〉	통과 (0.04ms, 10MB)
테스트 4 〉	통과 (0.15ms, 10.1MB)
테스트 5 〉	통과 (0.32ms, 10.1MB)
테스트 6 〉	통과 (0.64ms, 10.1MB)
테스트 7 〉	통과 (1.29ms, 10.2MB)
테스트 8 〉	통과 (309.65ms, 10.1MB)
테스트 9 〉	통과 (2045.02ms, 10MB)
테스트 10 〉	실패 (시간 초과)
테스트 11 〉	실패 (시간 초과)
테스트 12 〉	실패 (시간 초과)
테스트 13 〉	실패 (시간 초과)
테스트 14 〉	실패 (시간 초과)
테스트 15 〉	실패 (시간 초과)
테스트 16 〉	실패 (시간 초과)
테스트 17 〉	실패 (시간 초과)
테스트 18 〉	실패 (시간 초과)
테스트 19 〉	실패 (시간 초과)
테스트 20 〉	실패 (시간 초과)
```
- 역시나 이중 for문으로 돌았더니 시간초과, sequence의 길이 범위를 보니 DP일 것 같다는 생각이 들었지만 풀이를 떠올리지 못해 결국 다른 사람의 아이디어를 참고..

# 내 풀이 2
```python
def solution(sequence):
    l = len(sequence)
    pulse = [1, -1]
    seq_1 = [sequence[i]*pulse[i%2] for i in range(l)]
    seq_2 = [sequence[i]*pulse[(i+1)%2] for i in range(l)]
    max_1, max_2 = 0, 0
    answer = 0
    
    for i in range(l):
        max_1 = max(max_1+seq_1[i], seq_1[i])
        max_2 = max(max_2+seq_2[i], seq_2[i])
        answer = max(answer, max_1, max_2)
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.2MB)
테스트 3 〉	통과 (0.02ms, 10.1MB)
테스트 4 〉	통과 (0.04ms, 10.3MB)
테스트 5 〉	통과 (0.03ms, 10MB)
테스트 6 〉	통과 (0.04ms, 10.2MB)
테스트 7 〉	통과 (0.05ms, 10.2MB)
테스트 8 〉	통과 (0.40ms, 10.1MB)
테스트 9 〉	통과 (0.85ms, 10.1MB)
테스트 10 〉	통과 (4.31ms, 10.5MB)
테스트 11 〉	통과 (8.26ms, 11.2MB)
테스트 12 〉	통과 (131.95ms, 21.8MB)
테스트 13 〉	통과 (113.56ms, 21.8MB)
테스트 14 〉	통과 (86.32ms, 21.8MB)
테스트 15 〉	통과 (87.83ms, 21.9MB)
테스트 16 〉	통과 (91.55ms, 22.1MB)
테스트 17 〉	통과 (477.59ms, 71.1MB)
테스트 18 〉	통과 (473.47ms, 71.2MB)
테스트 19 〉	통과 (473.00ms, 71.7MB)
테스트 20 〉	통과 (466.69ms, 71.5MB)
```
- 전략
  - 펄스 수열은 1부터 시작하거나 -1부터 시작할 수 있으므로, 각각의 경우에 대한 수열 `seq_1`, `seq_2`를 만듦
  - DP 조건은 sequence에서 `i`번째 원소를 포함하는 부분 수열 중 최대를 업데이트
    - (`i-1`번째를 포함하는 최대 + `i`번째 원소)가 더 크면 순열이 이어지며, (`i`번째 원소)가 더 크면 순열이 끊어짐
  - 두 순열 `seq_1`, `seq_2`의 최대값중 더 큰 값이 정답

# 다른 사람의 풀이
```python
def solution(sequence):
    answer = 0
    prefixS = [0]
    for i in range(len(sequence)):
        pulse = 1 if i%2 ==0  else -1
        prefixS.append(prefixS[-1]+pulse*sequence[i])
        
    return abs(max(prefixS) - min(prefixS))
```
- 전략
  - 연속 펄스 부분 수열은 같은 인덱스끼리 절대값은 같고 부호만 다르다.
  - `prefixS`의 원소들은 다음과 같음
    - `[0, seq[0], seq[0]-seq[1], seq[0]-seq[1]+seq[2], seq[0]-seq[1]+seq[2]-seq[3], ...]`
  - `prefixS[j] - prefixS[i]`는 `seq[i]~seq[j+1]` 까지의 연속 펄스 부분 수열이 생성
  - 따라서 가장 큰 펄스 부분 수열을 생성하려면, `prefixS`의 가장 큰 원소와 가장 작은 원소의 차를 구하면 됨
