# 소수 찾기
## 문제 설명
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

## 입출력 예
|numbers|return|
|-|-|
|"17"|3|
|"011"|2|

## 입출력 예 설명
### 예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

### 예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.
- 11과 011은 같은 숫자로 취급합니다.

# 내 풀이
```python
import itertools

def solution(numbers):
    nums = [n for n in numbers]
    
    perms = []
    for i in range(1, len(nums)+1):
        perms += list(itertools.permutations(nums, i))
    
    perm_nums = set()
    for p in perms:
        perm_nums.add(int("".join(p)))
    
    max_num = max(perm_nums)
    primes = []
    table = [False, False] + [True] * (max_num-1)
    for i in range(2, max_num+1):
        if table[i]:
            primes.append(i)
        for j in range(2*i, max_num+1, i):
            table[j] = False
            
    answer = 0
    for p_n in perm_nums:
        if p_n in primes:
            answer+=1
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (1.84ms, 10.3MB)
테스트 2 〉	통과 (1609.74ms, 20.5MB)
테스트 3 〉	통과 (0.04ms, 10.4MB)
테스트 4 〉	통과 (414.52ms, 14.9MB)
테스트 5 〉	통과 (3769.23ms, 62.1MB)
테스트 6 〉	통과 (0.03ms, 10.2MB)
테스트 7 〉	통과 (1.88ms, 10.4MB)
테스트 8 〉	통과 (6651.67ms, 96.1MB)
테스트 9 〉	통과 (0.17ms, 10.5MB)
테스트 10 〉	통과 (2223.05ms, 25.4MB)
테스트 11 〉	통과 (97.80ms, 11.8MB)
테스트 12 〉	통과 (40.16ms, 11.1MB)
```
- 에라토스테네스의 체 구현
- itertools.permutations() (+ itertools.combinations)
  - itertools.permutations(리스트, 순열의 길이)
  - tuple 리턴
- 위 라이브러리의 사용법 이외에도, 순열의 개수를 1부터 끝까지 고려해야된다는 생각을 하지 못했었다.
  - 1, 2, 3, 4, 5 에서 1, 3 만을 뽑아 수를 만들 수 있기 때문
