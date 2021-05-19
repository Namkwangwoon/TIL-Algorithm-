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
|---|---|---|
|5|12|4|
|2|11|3|
## 입출력 예 설명
### 예제 #1
문제에 나온 예와 같습니다.

### 예제 #2
11 = 22 / 2와 같이 2를 3번만 사용하여 표현할 수 있습니다.

## 출처
https://www.oi.edu.pl/old/php/show.php?ac=e181413&module=show&file=zadania/oi6/monocyfr
# 풀이
- N이 1이상 9이하이고, 답이 8보다 크면 -1이라는 점을 보고 dp 문제일 것 같다고 생각했다.
- N을 1번만 사용한 경우부터 차근차근 하나씩 늘려가며 결과를 봤는데, N을 사용한 갯수의 집합끼리의 연산임을 확인했다.
```python
def solution(N, number):
    if N==number:
        return 1
    
    list_numset = []
    for i in range(1,9):
        numset = set()
        numset.add(int(str(N)*i))
        
        for j in range(0,i-1):
            set1 = list_numset[j]
            set2 = list_numset[-j-1]
            
            for s1 in set1:
                for s2 in set2:
                    numset.add(s1+s2)
                    numset.add(s1*s2)
                    if s1>=s2:
                        numset.add(s1-s2)
                    else:
                        numset.add(s2-s2)
                    if s1!=0 and s2/s1==s2//s1:
                        numset.add(int(s2/s1))
                    if s2!=0 and s1/s2==s1//s2:
                        numset.add(int(s1/s2))
        if number in numset:
            return i
        
        list_numset.append(numset)
        
    else:
        return -1
```
정확성  테스트
```
테스트 1 〉	통과 (1.03ms, 10.4MB)
테스트 2 〉	통과 (0.03ms, 10.3MB)
테스트 3 〉	통과 (0.04ms, 10.4MB)
테스트 4 〉	통과 (11.05ms, 11.1MB)
테스트 5 〉	통과 (8.92ms, 10.5MB)
테스트 6 〉	통과 (0.29ms, 10.4MB)
테스트 7 〉	통과 (0.30ms, 10.4MB)
테스트 8 〉	통과 (0.04ms, 10.3MB)
테스트 9 〉	통과 (12.13ms, 11.2MB)
테스트 10 〉	통과 (0.00ms, 10.2MB)
테스트 11 〉	통과 (0.00ms, 10.2MB)
```
