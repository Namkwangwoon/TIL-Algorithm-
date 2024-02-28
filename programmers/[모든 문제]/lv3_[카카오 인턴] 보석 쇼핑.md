# [카카오 인턴] 보석 쇼핑

## 문제 설명
[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

개발자 출신으로 세계 최고의 갑부가 된 `어피치`는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.
어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.
어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.
`진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매`

예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.

|진열대 번호|1|2|3|4|5|6|7|8|
|-|-|-|-|-|-|-|-|-|
|보석 이름|DIA|RUBY|RUBY|DIA|DIA|EMERALD|SAPPHIRE|DIA|

진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.

진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.

진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.

가장 짧은 구간의 `시작 진열대 번호`와 `끝 진열대 번호`를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 `시작 진열대 번호`가 가장 작은 구간을 return 합니다.

## [제한사항]
- gems 배열의 크기는 1 이상 100,000 이하입니다.
  - gems 배열의 각 원소는 진열대에 나열된 보석을 나타냅니다.
  - gems 배열에는 1번 진열대부터 진열대 번호 순서대로 보석이름이 차례대로 저장되어 있습니다.
  - gems 배열의 각 원소는 길이가 1 이상 10 이하인 알파벳 대문자로만 구성된 문자열입니다.

## 입출력 예
|gems|result|
|-|-|
|["DIA", "RUBY", "RUBY", "DIA", "DIA", "EMERALD", "SAPPHIRE", "DIA"]|[3, 7]|
|["AA", "AB", "AC", "AA", "AC"]|[1, 3]|
|["XYZ", "XYZ", "XYZ"]|[1, 1]|
|["ZZZ", "YYY", "NNNN", "YYY", "BBB"]|[1, 5]|

## 입출력 예에 대한 설명
### 입출력 예 #1
문제 예시와 같습니다.

### 입출력 예 #2
3종류의 보석(AA, AB, AC)을 모두 포함하는 가장 짧은 구간은 [1, 3], [2, 4]가 있습니다.
시작 진열대 번호가 더 작은 [1, 3]을 return 해주어야 합니다.

### 입출력 예 #3
1종류의 보석(XYZ)을 포함하는 가장 짧은 구간은 [1, 1], [2, 2], [3, 3]이 있습니다.
시작 진열대 번호가 가장 작은 [1, 1]을 return 해주어야 합니다.

### 입출력 예 #4
4종류의 보석(ZZZ, YYY, NNNN, BBB)을 모두 포함하는 구간은 [1, 5]가 유일합니다.
그러므로 [1, 5]를 return 해주어야 합니다.

# 내 풀이
```python
from collections import defaultdict

def solution(gems):
    gems_set = set(gems)
    l_gs = len(gems_set)
    gems_dict = defaultdict(int)
    dict_l_range = dict()
    
    for i, gem in enumerate(gems):
        gems_dict[gem] = i+1
        if len(gems_dict)==l_gs:
            v = gems_dict.values()
            m, M = min(v), max(v)
            if M-m+1==l_gs: return [m, M]
            if M-m not in dict_l_range.keys():
                dict_l_range[M-m] = [m, M]
    
    return dict_l_range[min(dict_l_range.keys())]
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.2MB)
테스트 2 〉	통과 (0.09ms, 10.2MB)
테스트 3 〉	통과 (0.48ms, 10.2MB)
테스트 4 〉	통과 (0.17ms, 10.1MB)
테스트 5 〉	통과 (0.04ms, 10.1MB)
테스트 6 〉	통과 (0.01ms, 10.2MB)
테스트 7 〉	통과 (0.02ms, 10.2MB)
테스트 8 〉	통과 (3.74ms, 10.2MB)
테스트 9 〉	통과 (2.06ms, 10.1MB)
테스트 10 〉	통과 (0.54ms, 10.3MB)
테스트 11 〉	통과 (0.84ms, 10.3MB)
테스트 12 〉	통과 (3.36ms, 10.4MB)
테스트 13 〉	통과 (8.82ms, 10.3MB)
테스트 14 〉	통과 (5.36ms, 10.3MB)
테스트 15 〉	통과 (11.53ms, 10.5MB)
```
효율성  테스트
```
테스트 1 〉	통과 (20.13ms, 10.6MB)
테스트 2 〉	통과 (54.16ms, 10.8MB)
테스트 3 〉	통과 (58.22ms, 11.1MB)
테스트 4 〉	통과 (25.53ms, 12.2MB)
테스트 5 〉	통과 (347.24ms, 12.3MB)
테스트 6 〉	통과 (77.36ms, 12.1MB)
테스트 7 〉	통과 (534.77ms, 13MB)
테스트 8 〉	통과 (639.71ms, 13.5MB)
테스트 9 〉	통과 (196.58ms, 13.4MB)
테스트 10 〉	통과 (562.94ms, 14MB)
테스트 11 〉	실패 (시간 초과)
테스트 12 〉	실패 (시간 초과)
테스트 13 〉	실패 (시간 초과)
테스트 14 〉	실패 (시간 초과)
테스트 15 〉	실패 (시간 초과)
```
- 전략
  - 보석의 모든 종류가 들어있는 가장 짧은 구간을 찾는 문제
  - `gems`를 처음부터 보면서, 여태까지 보석의 가장 큰 인덱스를 업데이트
  - 모든 보석을 찾으면, `[보석 중 가장 작은 index, 보석 증 가장 큰 index]` 가 정답의 후보
    - 이 중에서 가장 짧은 구간을 찾으면 되므로, 구간의 길이가 더 짧은 경우를 찾으면 update
- 결국은 효율성 테스트 5개를 통과하지 못하고, 다른 사람의 풀이를 참고

# 다른 사람의 풀이
```python
from collections import defaultdict


def solution(gems):
    answer = [0, 0]
    candidates = []
    start, end = 0, 0
    gems_len, gem_kind = len(gems), len(set(gems))
    gems_dict = defaultdict(lambda: 0)
    
    while True:
        kind = len(gems_dict)
        if start == gems_len:
            break
        if kind == gem_kind:
            candidates.append((start, end))
            gems_dict[gems[start]] -= 1
            if gems_dict[gems[start]] == 0:
                del gems_dict[gems[start]]
            start += 1
            continue
        if end == gems_len:
            break
        if kind != gem_kind:
            gems_dict[gems[end]] += 1
            end += 1
            continue

    length = float('inf')
    for s, e in candidates:
        if length > e-s:
            length = e-s
            answer[0] = s + 1
            answer[1] = e
    return answer
```
- 전략
  - 길이가 100,000 인 배열에 대한 탐색 알고리즘
    - 그 중에서도, (시작점-끝점)을 자유롭게 조절할 수 있는 투 포인터 알고리즘 사용
  - 살펴볼 구간의 시작점 `start`, 끝점 `end`를 설정
  - 구간 안에 모든 보석의 종류가 존재한다면, 정답의 후보
  - 처음 정답 후보를 찾은 후, 길이가 더 짧은 구간을 만난다면 update
  
# 내 풀이 2
```python
from collections import defaultdict

def solution(gems):
    start, end = 0, 0
    l_gems = len(gems)
    l_gems_set = len(set(gems))
    answer = [1, l_gems]
    min_l = l_gems-1
    gems_dict = defaultdict(int)
    gems_dict[gems[0]]=1
    
    while True:
        if len(gems_dict)<l_gems_set:
            if end==l_gems-1: break
            end+=1
            gems_dict[gems[end]]+=1
        elif len(gems_dict)==l_gems_set:
            if start==l_gems-1: break
            if end-start+1==l_gems_set: return [start+1, end+1]
            if end-start<min_l:
                min_l = end-start
                answer = [start+1, end+1]
            gems_dict[gems[start]]-=1
            if gems_dict[gems[start]]==0: del gems_dict[gems[start]]
            start+=1
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.2MB)
테스트 2 〉	통과 (0.07ms, 10.2MB)
테스트 3 〉	통과 (0.23ms, 10.3MB)
테스트 4 〉	통과 (0.36ms, 10.4MB)
테스트 5 〉	통과 (0.02ms, 10.3MB)
테스트 6 〉	통과 (0.01ms, 10.2MB)
테스트 7 〉	통과 (0.03ms, 10.2MB)
테스트 8 〉	통과 (0.57ms, 10.1MB)
테스트 9 〉	통과 (0.60ms, 10.2MB)
테스트 10 〉	통과 (0.38ms, 10.2MB)
테스트 11 〉	통과 (0.50ms, 10.3MB)
테스트 12 〉	통과 (1.10ms, 10.3MB)
테스트 13 〉	통과 (1.45ms, 10.3MB)
테스트 14 〉	통과 (1.00ms, 10.5MB)
테스트 15 〉	통과 (2.77ms, 10.4MB)
```
효율성  테스트
```
테스트 1 〉	통과 (3.19ms, 10.6MB)
테스트 2 〉	통과 (5.03ms, 10.5MB)
테스트 3 〉	통과 (9.97ms, 11.1MB)
테스트 4 〉	통과 (6.49ms, 11.9MB)
테스트 5 〉	통과 (16.44ms, 11.9MB)
테스트 6 〉	통과 (20.44ms, 12.1MB)
테스트 7 〉	통과 (23.80ms, 12.6MB)
테스트 8 〉	통과 (26.24ms, 13MB)
테스트 9 〉	통과 (30.29ms, 13.4MB)
테스트 10 〉	통과 (37.31ms, 13.7MB)
테스트 11 〉	통과 (38.18ms, 14.5MB)
테스트 12 〉	통과 (25.04ms, 15.5MB)
테스트 13 〉	통과 (34.64ms, 16.2MB)
테스트 14 〉	통과 (69.21ms, 17.1MB)
테스트 15 〉	통과 (63.63ms, 17.8MB)
```
- 다른 사람의 풀이를 참고하여, 투 표인터로 작성한 코드
