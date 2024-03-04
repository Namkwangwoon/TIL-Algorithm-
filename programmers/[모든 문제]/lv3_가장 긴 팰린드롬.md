# 가장 긴 팰린드롬
## 문제 설명
앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.

문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 return 하는 solution 함수를 완성해 주세요.

예를들면, 문자열 s가 "abcdcba"이면 7을 return하고 "abacde"이면 3을 return합니다.

## 제한사항
- 문자열 s의 길이 : 2,500 이하의 자연수
- 문자열 s는 알파벳 소문자로만 구성

## 입출력 예
|s|answer|
|-|-|
|"abcdcba"|7|
|"abacde"|3|

## 입출력 예 설명
### 입출력 예 #1
4번째자리 'd'를 기준으로 문자열 s 전체가 팰린드롬이 되므로 7을 return합니다.

### 입출력 예 #2
2번째자리 'b'를 기준으로 "aba"가 팰린드롬이 되므로 3을 return합니다.

# 내 풀이
```python
from collections import deque

def solution(s):
    if len(s)==1: return 1

    l = len(s)
    stack = [s[0]]
    s = deque(s[1:])
    answer = 1
    
    while s:
        word = s.popleft()
        
        if stack[-1]==word:
            answer = max(2, answer)
            ranges = min(len(stack)-1, len(s))
            for i in range(ranges):
                if stack[-1*i-2]!=s[i]:
                    answer = max(answer, 2*(i+1))
                    break
            else:
                answer = max(answer, (ranges+1)*2)
            
        if len(s)!=0 and stack[-1]==s[0]:
            answer = max(2, answer)
            ranges = min(len(stack), len(s))
            for i in range(ranges):
                if stack[-1*i-1]!=s[i]:
                    answer = max(answer, 2*(i+1)-1)
                    break
            else:
                answer = max(answer, ranges*2+1)
        
        
        stack.append(word)
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.3MB)
테스트 2 〉	통과 (0.02ms, 10.2MB)
테스트 3 〉	통과 (0.13ms, 10.1MB)
테스트 4 〉	통과 (0.16ms, 10.2MB)
테스트 5 〉	통과 (0.13ms, 10.1MB)
테스트 6 〉	통과 (0.10ms, 10.2MB)
테스트 7 〉	통과 (0.09ms, 10.2MB)
테스트 8 〉	통과 (0.09ms, 10.4MB)
테스트 9 〉	통과 (0.22ms, 10.3MB)
테스트 10 〉	통과 (0.11ms, 10.4MB)
테스트 11 〉	통과 (0.33ms, 10.3MB)
테스트 12 〉	통과 (0.76ms, 10.3MB)
테스트 13 〉	통과 (0.07ms, 10.1MB)
테스트 14 〉	통과 (0.12ms, 10.3MB)
테스트 15 〉	통과 (0.11ms, 10.3MB)
테스트 16 〉	통과 (0.23ms, 10.1MB)
테스트 17 〉	통과 (0.00ms, 10.3MB)
테스트 18 〉	통과 (0.00ms, 10.3MB)
테스트 19 〉	통과 (0.06ms, 10.3MB)
테스트 20 〉	통과 (0.20ms, 10.2MB)
테스트 21 〉	통과 (0.30ms, 10.4MB)
테스트 22 〉	통과 (0.01ms, 10.1MB)
테스트 23 〉	통과 (0.01ms, 10.2MB)
테스트 24 〉	통과 (0.01ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (4.48ms, 10.3MB)
테스트 2 〉	통과 (168.78ms, 10.2MB)
```
- 전략
  - 팰린드롬이 생성되는 경우의 수는 두 가지가 있다
    1. 짝수 길이의 팰린드롬이 생성되는 경우, ex) "abccba"
    2. 홀수 길이의 팰린드롬이 생성되는 경우, ex) "abcdcba"
  - 큐로 만든 `s`의 왼쪽부터 하나의 원소씩 빼면서, `stack`에 하나의 원소씩 넣어줌
  - 매번, `stack[-1]`부터 왼쪽 방향으로 & `s[0]`부터 오른쪽 방향으로 연속해서 같은 원소의 갯수를 센다. (팰린드롬의 조건)
    - 이 때, 위의 1번과 2번의 경우를 모두 고려해야 함
  - 가장 큰 팰린드롬의 길이를 정답으로 리턴

# 다른 사람의 풀이
```python
def solution(s):
    for i in range(len(s),0,-1):
        for j in range(len(s)-i+1):
            if s[j:j+i] == s[j:j+i][::-1]:
                return i
```
- 전략
  - 가장 긴 팰린드롬을 찾는 문제이므로, `s`의 길이만큼의 팰린드롬부터 계속해서 작은 길이를 찾음
  - `[::-1]`로 순서를 뒤집어도 같은 리스트이면 팰린드롬이라는 매우 쉬운 전략
  - 내 풀이에 비해 효율성이 좀 떨어지긴 하지만 그래도 코드가 간결해짐
