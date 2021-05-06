# 문자열 압축 사본
## 문제 설명
데이터 처리 전문가가 되고 싶은 "어피치"는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
간단한 예로 "aabbaccc"의 경우 "2a2ba3c"(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, "abcabcdede"와 같은 문자열은 전혀 압축되지 않습니다. "어피치"는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

예를 들어, "ababcdcdababcdcd"의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 "2ab2cd2ab2cd"로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 "2ababcdcd"로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

다른 예로, "abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
s의 길이는 1 이상 1,000 이하입니다.
s는 알파벳 소문자로만 이루어져 있습니다.
## 입출력 예
|s|result|
|---|---|
|"aabbaccc"|7|
|"ababcdcdababcdcd"|9|
|"abcabcdede"|8|
|"abcabcabcabcdededededede"|14|
|"xababcdcdababcdcd"|17|
## 입출력 예에 대한 설명
### 입출력 예 #1

문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.

### 입출력 예 #2

문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.

### 입출력 예 #3

문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.

### 입출력 예 #4

문자열을 2개 단위로 자르면 "abcabcabcabc6de" 가 됩니다.
문자열을 3개 단위로 자르면 "4abcdededededede" 가 됩니다.
문자열을 4개 단위로 자르면 "abcabcabcabc3dede" 가 됩니다.
문자열을 6개 단위로 자를 경우 "2abcabc2dedede"가 되며, 이때의 길이가 14로 가장 짧습니다.

### 입출력 예 #5

문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다.
따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다.
이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.
# 풀이
푸는데 시간이 좀 걸리긴 했지만, 결국 모든 문자열의 경우의 수를 구하니 답이 명확하게 나왔다.
```python
def solution(s):
    answer = len(s)
    # token_size: token의 길이
    for token_size in range(1,len(s)//2+1):
        result = ''
        pre_token = s[:token_size]    # 바로 직전의 token
        count=1
        # i: s를 잘라서 만들 token의 시작 index
        for i in range(token_size, len(s)+token_size, token_size):
            # end: token의 끝 index
            end = i+token_size
            if end>len(s):
                end = len(s)
            token = s[i:end]
            if pre_token==token:
                count+=1
            else:
                if count!=1:
                    result+=str(count)
                result+=pre_token
                pre_token = token
                count=1
        if len(result)<answer:
            answer = len(result)
    return answer
```
정확성 테스트
```
테스트 1 〉	통과 (0.04ms, 10.2MB)
테스트 2 〉	통과 (0.50ms, 10.3MB)
테스트 3 〉	통과 (0.27ms, 10.2MB)
테스트 4 〉	통과 (0.04ms, 10.2MB)
테스트 5 〉	통과 (0.00ms, 10.2MB)
테스트 6 〉	통과 (0.05ms, 10.3MB)
테스트 7 〉	통과 (0.60ms, 10.2MB)
테스트 8 〉	통과 (0.71ms, 10.3MB)
테스트 9 〉	통과 (0.98ms, 10.2MB)
테스트 10 〉	통과 (2.55ms, 10.2MB)
테스트 11 〉	통과 (0.11ms, 10.3MB)
테스트 12 〉	통과 (0.13ms, 10.2MB)
테스트 13 〉	통과 (0.14ms, 10.2MB)
테스트 14 〉	통과 (1.38ms, 10.2MB)
테스트 15 〉	통과 (0.15ms, 10.2MB)
테스트 16 〉	통과 (0.02ms, 10.3MB)
테스트 17 〉	통과 (1.47ms, 10.2MB)
테스트 18 〉	통과 (1.48ms, 10.2MB)
테스트 19 〉	통과 (1.43ms, 10.2MB)
테스트 20 〉	통과 (2.65ms, 10.1MB)
테스트 21 〉	통과 (2.98ms, 10.2MB)
테스트 22 〉	통과 (2.99ms, 10.2MB)
테스트 23 〉	통과 (2.90ms, 10.2MB)
테스트 24 〉	통과 (2.76ms, 10.1MB)
테스트 25 〉	통과 (2.91ms, 10.2MB)
테스트 26 〉	통과 (2.89ms, 10.2MB)
테스트 27 〉	통과 (2.65ms, 10.2MB)
테스트 28 〉	통과 (0.02ms, 10.2MB)
```
카카오 기출문제라 그런지 긴 시간동안의 집중이 필요했다.
# 해답
```python
def solution(s):
    answer = len(s)

    for size in range(1, len(s) // 2 + 1):
        count = 1
        compress = 0
        
        prev = s[:size]
        for i in range(size, len(s) + size, size):
            curr = s[i:i + size]
            if prev == curr:
                count += 1
            else:
                compress += size + len(str(count)) if 1 < count else len(prev)
                prev = curr
                count = 1
        answer = min(answer, compress)

    return answer
```
1. 나의 풀이처럼 결과 문자열을 만드는 대신, compress를 통해 바로 문자열의 길이(필요한 결과만)를 늘려나갔다. -> 공간 & 시간 절약
2. min(a, b)를 통한 간단한 값 비교
