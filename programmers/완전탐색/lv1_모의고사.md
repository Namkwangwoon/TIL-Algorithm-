# 모의고사

## 문제 설명
수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...

2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...

3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

## 제한 조건
- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

## 입출력 예

|answers|return|
|-|-|
|[1,2,3,4,5]|[1]|
|[1,3,2,4,2]|[1,2,3]|

## 입출력 예 설명
### 입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

### 입출력 예 #2

- 모든 사람이 2문제씩을 맞췄습니다.

# 내 풀이
```python
import numpy as np

def solution(answers):
    l = len(answers)
    first = np.array(([1, 2, 3, 4, 5] * (len(answers)//5 + 1))[:l])
    second = np.array(([2, 1, 2, 3, 2, 4, 2, 5] * (len(answers)//8 + 1))[:l])
    third = np.array(([3, 3, 1, 1, 2, 2, 4, 4, 5, 5] * (len(answers)//10 + 1))[:l])
    np_answers = np.array(answers)
    
    scores = np.array([(first==np_answers).sum(), (second==np_answers).sum(), (third==np_answers).sum()])
    max_score = max(scores)
    print(np.array([1, 2, 3])[scores == max_score])
    return np.array([1, 2, 3])[scores == max_score].tolist()
```
정확성  테스트
```
테스트 1 〉	통과 (0.25ms, 28.3MB)
테스트 2 〉	통과 (0.22ms, 28.5MB)
테스트 3 〉	통과 (0.23ms, 28.8MB)
테스트 4 〉	통과 (0.23ms, 28.4MB)
테스트 5 〉	통과 (0.43ms, 28.3MB)
테스트 6 〉	통과 (0.28ms, 28.5MB)
테스트 7 〉	통과 (1.50ms, 28.6MB)
테스트 8 〉	통과 (0.64ms, 28.9MB)
테스트 9 〉	통과 (5.62ms, 28.9MB)
테스트 10 〉	통과 (1.22ms, 28.9MB)
테스트 11 〉	통과 (2.49ms, 28.8MB)
테스트 12 〉	통과 (2.23ms, 29.1MB)
테스트 13 〉	통과 (0.41ms, 28.6MB)
테스트 14 〉	통과 (2.64ms, 28.5MB)
```
- numpy로 풀어도 되나..?

# 다른 사람의 풀이1
```python
def solution(answers):
    pattern1 = [1,2,3,4,5]
    pattern2 = [2,1,2,3,2,4,2,5]
    pattern3 = [3,3,1,1,2,2,4,4,5,5]
    score = [0, 0, 0]
    result = []

    for idx, answer in enumerate(answers):
        if answer == pattern1[idx%len(pattern1)]:
            score[0] += 1
        if answer == pattern2[idx%len(pattern2)]:
            score[1] += 1
        if answer == pattern3[idx%len(pattern3)]:
            score[2] += 1

    for idx, s in enumerate(score):
        if s == max(score):
            result.append(idx+1)

    return result
```
- answers를 하나씩 차례대로 보면서 pattern과 비교하며 점수 계산
- 최대 점수와 같은 점수면 (index+1)번째 사람이 가장 높은 점수를 받은 사람
- 깔끔하다

# 다른 사람의 풀이2
```python
from itertools import cycle

def solution(answers):
    giveups = [
        cycle([1,2,3,4,5]),
        cycle([2,1,2,3,2,4,2,5]),
        cycle([3,3,1,1,2,2,4,4,5,5]),
    ]
    scores = [0, 0, 0]
    for num in answers:
        for i in range(3):
            if next(giveups[i]) == num:
                scores[i] += 1
    highest = max(scores)

    return [i + 1 for i, v in enumerate(scores) if v == highest]
```
### itertools.cycle()
- 요소를 반환하고 각각의 복사본을 저장하는 반복자를 만듦
- next() 혹은 for문으로 사용 가능
