# 가장 긴 팰린드롬
## 문제 설명
### ※ 주의

본 문제는 일부러 시간복잡도가 오래 걸려도 정답이 나오도록 제한 시간이 넉넉하게 설정되어 있습니다.

본 문제를 정말 빠른 알고리즘으로 풀려면 구글에서 longest palindrom subsequence로 검색을 해보세요.

- - -

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.

문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 return 하는 solution 함수를 완성해 주세요.

예를들면, 문자열 s가 "abcdcba"이면 7을 return하고 "abacde"이면 3을 return합니다.

## 제한사항
- 문자열 s의 길이 : 2500 이하의 자연수
- 문자열 s는 알파벳 소문자로만 구성
## 입출력 예
|s|answer|
|---|---|
|"abcdcba"|7|
|"abacde"|3|
## 입출력 예 설명
### 입출력 예 #1
4번째자리 'd'를 기준으로 문자열 s 전체가 팰린드롬이 되므로 7을 return합니다.

### 입출력 예 #2
2번째자리 'b'를 기준으로 "aba"가 팰린드롬이 되므로 3을 return합니다.
# 풀이
제한 시간이 넉넉하다고 주어져서, 안심하고 3중 반복문을 사용했다.

그러나 2번 효율성 테스트에서 통과를 하지 못해서 반복문의 차원은 그대로 유지하되, 한 차원 안에서의 횟수를 줄여보고자 했더니 통과했다.

(현재까지 찾은 최대 팰린드롬보다 작은 길이의 부분문자열은 살펴보지 않는 조건문을 추가했다.)
```python
# 인자 s가 팰린드롬을 만족하는지 검사하는 함수
def pal(s):
    for i in range(len(s)//2):
        if s[i]!=s[-i-1]:
            return False
    else:
        return True

def solution(s):
    l = 1
    if len(s)==1:
        return l
    for i in range(len(s)-1):
        for j in range(len(s), i+1, -1):
            if j-i<l:   # 실행시간을 줄이기 위해, 현재까지의 최대 팰린드롬의 길이보다 작은 영역은 과감히 PASS
                break
            if pal(s[i:j]) and l<len(s[i:j]):   # 최대 팰린드롬을 찾았을 때
                l = len(s[i:j])
                break
    return l
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.3MB)
테스트 2 〉	통과 (0.03ms, 10.2MB)
테스트 3 〉	통과 (2.49ms, 10.1MB)
테스트 4 〉	통과 (2.32ms, 10.2MB)
테스트 5 〉	통과 (2.44ms, 10.2MB)
테스트 6 〉	통과 (2.82ms, 10.2MB)
테스트 7 〉	통과 (2.55ms, 10.2MB)
테스트 8 〉	통과 (2.34ms, 10.2MB)
테스트 9 〉	통과 (0.07ms, 10MB)
테스트 10 〉	통과 (0.06ms, 10.2MB)
테스트 11 〉	통과 (0.06ms, 10.2MB)
테스트 12 〉	통과 (0.05ms, 10.2MB)
테스트 13 〉	통과 (1.81ms, 10.1MB)
테스트 14 〉	통과 (6.61ms, 10.3MB)
테스트 15 〉	통과 (5.62ms, 10.2MB)
테스트 16 〉	통과 (7.14ms, 10.2MB)
테스트 17 〉	통과 (0.00ms, 10.2MB)
테스트 18 〉	통과 (0.01ms, 10.2MB)
테스트 19 〉	통과 (2.38ms, 10.2MB)
테스트 20 〉	통과 (1.52ms, 10.2MB)
테스트 21 〉	통과 (2.97ms, 10.1MB)
```
효율성  테스트
```
테스트 1 〉	통과 (1896.92ms, 10.2MB)
테스트 2 〉	통과 (1.52ms, 10.1MB)
```
# 해답
```python
def is_palindrome(s, start, end):
    for i in range((end - start) // 2 + 1):
        if s[start + i] != s[end - i]:
            return False

    return True


def solution(s):
    for answer in range(len(s), 0, -1): # 문자열 최대 길이에서 하나씩 줄여나갑니다.
        start = 0 # 0에서
        end = answer - 1 # answer 길이까지
        
        while end < len(s): 
            if is_palindrome(s, start, end): # 팰린드롬인지 확인합니다
                return answer; # 팰린드롬이면 그대로 리턴
            start += 1
            end += 1 # 한 칸씩 순회합니다.
    
    return 1 # 한 글자일 경우 1을 리턴합니다.
```
- 부분문자열의 길이를 1씩 줄어나가면서 그 길이를 갖는 모든 경우의 문자열을 검사했다.
- 길이가 가장 긴 부분문자열부터 검사하기 때문에, 팰린드롬 문자열을 찾자마자 그 길이가 답이 된다.
# 추가
- 더 쉽게 팰린드롬을 검사하는 방법 sub_s[:]==sub_s[::-1] 이면 팰린드롬이다!!
