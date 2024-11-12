# 아방가르드 타일링
## 문제 설명
정우는 예술적 감각이 뛰어난 타일공입니다. 그는 단순한 타일을 활용하여 불규칙하면서도 화려하게 타일링을 하곤 합니다.

어느 날 정우는 가로 길이 `n`, 세로 길이 3 인 판을 타일링하는 의뢰를 맡았습니다. 아방가르드한 디자인 영감이 떠오른 정우는 다음과 같은 두 가지 종류의 타일로 타일링을 하기로 결정했습니다.
![image](https://github.com/user-attachments/assets/5ff8509b-dc54-4433-8230-e881e2da3c7b)
각 타일은 90도씩 회전할 수 있으며 타일의 개수는 제한이 없습니다.

`n`이 주어졌을 때, 이 두 가지 종류의 타일로 `n` x 3 크기의 판을 타일링하는 방법의 수를 return 하도록 solution 함수를 완성해주세요.

## 제한 사항
- 1 ≤ `n` ≤ 100,000
- 결과는 매우 클 수 있으므로 1,000,000,007 로 나눈 나머지를 return합니다.

## 입출력 예
|n|result|
|-|-|
|2|3|
|3|10|

## 입출력 예 설명
### 입출력 예 #1
![image](https://github.com/user-attachments/assets/36ea1a01-3c2a-4c76-a600-7b648a0e420a)
위 그림과 같이 3 가지 방법으로 타일링할 수 있습니다.

### 입출력 예 #2
![image](https://github.com/user-attachments/assets/d3aa1cf7-c9a5-4bcd-8019-5597cca4ff6c)
위 그림과 같이 10 가지 방법으로 타일링할 수 있습니다.

# 다른 사람의 풀이
```python
MOD = 1_000_000_007

def solution(n):
    dp = [0, 1, 3, 10, 23, 62, 170]
    
    if n < 7:
        return dp[n]
    
    for i in range(7, n+1):
        x = (dp[-1] + 2 * dp[-2] + 6 * dp[-3] + dp[-4] - dp[-6]) % MOD
        dp = dp[1:] + [x]

    return dp[-1]
```
- 나도 DP로 풀어야겠다 생각했지만, 풀이를 떠올리지 못했다.
- 이 문제의 관건은 **"꼼꼼하고"** **"진득하게"** 모든 경우의 수를 그려가며, 규칙을 찾아야 했다.
- 전략
  - 먼저, `n`이 `1~3`인 케이스는 문제에서 주어졌다.
  - `n=2`부터 보면, 이전 `n` 케이스에서 등장하지 않는 새로운 경우들이 존재하며, 이 케이스는 더 이상 쪼개지지 않음
    - `n=2`
      ![image](https://github.com/user-attachments/assets/8c94d787-75a3-46c5-acef-c587efa59f2d)
    - `n=3`
      ![image](https://github.com/user-attachments/assets/7036d806-c37b-4d73-b3e9-cffccd09b303)
    - `n=4`
      ![image](https://github.com/user-attachments/assets/c376a299-216c-4833-9201-17b5fdd694eb)
    - `n=5`
      ![image](https://github.com/user-attachments/assets/a15ac9f3-ee65-4b73-97c5-03ef209f3e30)
    - `n=6`
      ![image](https://github.com/user-attachments/assets/0a1156a3-4a87-4f4c-9905-38b1473c4528)
    - `n=7`
      ![image](https://github.com/user-attachments/assets/765bb01e-4a04-4344-a37f-7e91024da8dc)
    - `n=7`부터는 `n-3` 케이스의 중간에 `3 x 1` 블록들을 끼워 넣은 것과 같으므로, `n=4` 부터 (2, 2, 4) 씩 반복됨
  - 앞의 케이스를 이용하면, 중복이 없는 블록의 경우의 수를 구할 수 있음 (앞의 더 이상 쪼개지지 않는 케이스를 [x]라 하자)
    - `DP(x) = [x] + [x-1]*DP(1) + [x-2]*DP(2) + ... + [1]*DP(x-1)`
  - `x>=7`에서는 `[x]`가 세 번의 텀마다 반복이 일어나므로, 아래와 같이 식을 표현할 수 있음
    - `DP(x) = [1]*DP(x-1) + [2]*DP(x-2) + [3]*DP(x-3) + [4]*DP(x-4) + [5] + [6] + [7] + [8] + [9] + `
      `= 1*DP(x-1) + 2*DP(x-2) + 5*DP(x-3) + 2*DP(x-4) + 2*DP(x-5) + 4*DP(x-6) + ...` : `(2, 2, 4)` 반복
    - `DP(x+3) = 1*DP(x+2) + 2*DP(x+1) + 5*DP(x) + 2*DP(x-1) + 2*DP(x-2) + 4*DP(x-3) + ...` : `(2, 2, 4)` 반복
    - `DP(x+3) - DP(x) = DP(x+2) + 2*DP(x+1) + 5*DP(x) + DP(x-1) + -DP(x-3)` : `DP(x-4)` 부터는 앞의 계수가 같아서 상쇄됨
    - `DP(x+3) = DP(x+2) + 2*DP(x+1) + 6*DP(x) + DP(x-1) - DP(x-3)`
    - `x>=7`인 `x`에 대해, `DP(x) = DP(x-1) + 2*DP(x-2) + 6*DP(x-3) + DP(x-4) - DP(x-6)`
  - 따라서 `x<7`까지 카운트 한 후, `n=7` 부터는 위 식에 따라 계산해 나가면 됨
  - 결과는 매 dp마다 `1,000,000,007`로 나눈 나머지를 주면 됨

# 내 풀이 1
```python
def solution(n):
    div = 1000000007
    dp = [0, 1, 3, 10, 23, 62, 170] + [0]*(n-6)
    
    if n<7: return dp[n]
    
    for i in range(7, n+1):
        dp[i] = (dp[i-1] + 2*dp[i-2] + 6*dp[i-3] + dp[i-4] - dp[i-6])%div
    
    return dp[n]
```
정확성  테스트
```
테스트 1 〉	통과 (0.00ms, 10.3MB)
테스트 2 〉	통과 (0.07ms, 10.2MB)
테스트 3 〉	통과 (0.40ms, 10.2MB)
테스트 4 〉	통과 (0.15ms, 10MB)
테스트 5 〉	통과 (0.44ms, 10.2MB)
테스트 6 〉	통과 (0.34ms, 10.2MB)
테스트 7 〉	통과 (0.04ms, 10.3MB)
테스트 8 〉	통과 (0.24ms, 10.3MB)
테스트 9 〉	통과 (0.11ms, 10.1MB)
테스트 10 〉	통과 (0.36ms, 10.1MB)
테스트 11 〉	통과 (16.76ms, 11.2MB)
테스트 12 〉	통과 (50.51ms, 13.5MB)
테스트 13 〉	통과 (6.38ms, 10.4MB)
테스트 14 〉	통과 (40.89ms, 13MB)
테스트 15 〉	통과 (53.16ms, 13.7MB)
```
