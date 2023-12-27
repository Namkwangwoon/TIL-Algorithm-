# 전화번호 목록
## 문제 설명
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

구조대 : 119
박준영 : 97 674 223
지영석 : 11 9552 4421
전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

## 제한 사항
phone_book의 길이는 1 이상 1,000,000 이하입니다.
각 전화번호의 길이는 1 이상 20 이하입니다.
같은 전화번호가 중복해서 들어있지 않습니다.
## 입출력 예제
|phone_book|return|
|---|---|
|["119", "97674223", "1195524421"]|false|
|["123","456","789"]|true|
|["12","123","1235","567","88"]|false|

## 입출력 예 설명
### 입출력 예 #1
앞에서 설명한 예와 같습니다.

### 입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

### 입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

-----

# 풀이 1

```python
def solution(phone_book):
    new_pb = sorted(phone_book)
    
    for i in range(len(new_pb)-1):
        if new_pb[i] in new_pb[i+1]:
            return False

    return True
```
정확성 테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	통과 (0.01ms, 10.3MB)
테스트 3 〉	통과 (0.01ms, 10.2MB)
테스트 4 〉	통과 (0.01ms, 10.1MB)
테스트 5 〉	통과 (0.01ms, 10.3MB)
테스트 6 〉	통과 (0.01ms, 10.3MB)
테스트 7 〉	통과 (0.01ms, 10.3MB)
테스트 8 〉	통과 (0.00ms, 10.1MB)
테스트 9 〉	통과 (0.00ms, 10.2MB)
테스트 10 〉	통과 (0.01ms, 10.3MB)
테스트 11 〉	통과 (0.00ms, 10.3MB)
테스트 12 〉	통과 (0.01ms, 10.3MB)
테스트 13 〉	실패 (0.01ms, 10.3MB)
테스트 14 〉	통과 (0.47ms, 10.4MB)
테스트 15 〉	통과 (0.59ms, 10.3MB)
테스트 16 〉	통과 (0.75ms, 10.3MB)
테스트 17 〉	통과 (0.53ms, 10.3MB)
테스트 18 〉	통과 (0.77ms, 10.4MB)
테스트 19 〉	통과 (0.68ms, 10.3MB)
테스트 20 〉	통과 (0.98ms, 10.4MB)
```
효율성  테스트
```
테스트 1 〉	통과 (2.96ms, 10.8MB)
테스트 2 〉	통과 (2.97ms, 10.8MB)
테스트 3 〉	통과 (106.70ms, 30.7MB)
테스트 4 〉	통과 (79.42ms, 28MB)
```
채점 결과
```
정확성: 79.2
효율성: 16.7
합계: 95.8 / 100.0
```

# 다른 사람의 풀이
```python
def solution(phoneBook):
    phoneBook = sorted(phoneBook)

    for p1, p2 in zip(phoneBook, phoneBook[1:]):
        if p2.startswith(p1):
            return False
    return True
```

해시 풀이
```python
def solution(phone_book):
    answer = True
    hash_map = {}
    for phone_number in phone_book:
        hash_map[phone_number] = 1
    for phone_number in phone_book:
        temp = ""
        for number in phone_number:
            temp += number
            if temp in hash_map and temp != phone_number:
                answer = False
    return answer
```

# 풀이 2
```python
def solution(phone_book):
    new_pb = sorted(phone_book)
    
    for i in range(len(new_pb)-1):
        if new_pb[i+1].startswith(new_pb[i]):
            return False

    return True
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.1MB)
테스트 2 〉	통과 (0.01ms, 10.2MB)
테스트 3 〉	통과 (0.01ms, 10.1MB)
테스트 4 〉	통과 (0.01ms, 10.3MB)
테스트 5 〉	통과 (0.01ms, 10.3MB)
테스트 6 〉	통과 (0.01ms, 10.2MB)
테스트 7 〉	통과 (0.01ms, 10.3MB)
테스트 8 〉	통과 (0.01ms, 10.1MB)
테스트 9 〉	통과 (0.01ms, 10.3MB)
테스트 10 〉	통과 (0.01ms, 10.1MB)
테스트 11 〉	통과 (0.01ms, 10.1MB)
테스트 12 〉	통과 (0.01ms, 10.2MB)
테스트 13 〉	통과 (0.01ms, 10.3MB)
테스트 14 〉	통과 (0.46ms, 10.2MB)
테스트 15 〉	통과 (0.64ms, 10.3MB)
테스트 16 〉	통과 (0.54ms, 10.4MB)
테스트 17 〉	통과 (1.13ms, 10.4MB)
테스트 18 〉	통과 (0.95ms, 10.3MB)
테스트 19 〉	통과 (0.92ms, 10.3MB)
테스트 20 〉	통과 (1.74ms, 10.4MB)
```
효율성  테스트
```
테스트 1 〉	통과 (2.77ms, 10.7MB)
테스트 2 〉	통과 (3.02ms, 10.8MB)
테스트 3 〉	통과 (98.26ms, 30.6MB)
테스트 4 〉	통과 (130.26ms, 28.1MB)
```
채점 결과
```
정확성: 83.3
효율성: 16.7
합계: 100.0 / 100.0
```

in -> startswith 으로만 바꿨는데 하나의 케이스에서 통과했다.

예를 들어, ['119', '23119']인 경우 '119'가 앞에 나오지 않음에도 in은 False로 리턴할 수 있다.
