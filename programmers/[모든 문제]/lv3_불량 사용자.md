# 불량 사용자
## 문제 설명
개발팀 내에서 이벤트 개발을 담당하고 있는 "무지"는 최근 진행된 카카오이모티콘 이벤트에 비정상적인 방법으로 당첨을 시도한 응모자들을 발견하였습니다. 이런 응모자들을 따로 모아 `불량 사용자`라는 이름으로 목록을 만들어서 당첨 처리 시 제외하도록 이벤트 당첨자 담당자인 "프로도" 에게 전달하려고 합니다. 이 때 개인정보 보호을 위해 사용자 아이디 중 일부 문자를 '*' 문자로 가려서 전달했습니다. 가리고자 하는 문자 하나에 '*' 문자 하나를 사용하였고 아이디 당 최소 하나 이상의 '*' 문자를 사용하였습니다.
"무지"와 "프로도"는 불량 사용자 목록에 매핑된 응모자 아이디를 `제재 아이디` 라고 부르기로 하였습니다.

예를 들어, 이벤트에 응모한 전체 사용자 아이디 목록이 다음과 같다면

|응모자 아이디|
|-|
|frodo|
|fradi|
|crodo|
|abc123|
|frodoc|

다음과 같이 불량 사용자 아이디 목록이 전달된 경우,

|불량 사용자|
|-|
|fr*d*|
|abc1**|

불량 사용자에 매핑되어 당첨에서 제외되어야 야 할 제재 아이디 목록은 다음과 같이 두 가지 경우가 있을 수 있습니다.

|제재 아이디|
|-|
|frodo|
|abc123|

|제재 아이디|
|-|
|fradi|
|abc123|

이벤트 응모자 아이디 목록이 담긴 배열 user_id와 불량 사용자 아이디 목록이 담긴 배열 banned_id가 매개변수로 주어질 때, 당첨에서 제외되어야 할 제재 아이디 목록은 몇가지 경우의 수가 가능한 지 return 하도록 solution 함수를 완성해주세요.

## [제한사항]
- user_id 배열의 크기는 1 이상 8 이하입니다.
- user_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
  - 응모한 사용자 아이디들은 서로 중복되지 않습니다.
  - 응모한 사용자 아이디는 알파벳 소문자와 숫자로만으로 구성되어 있습니다.
- banned_id 배열의 크기는 1 이상 user_id 배열의 크기 이하입니다.
- banned_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
  - 불량 사용자 아이디는 알파벳 소문자와 숫자, 가리기 위한 문자 '*' 로만 이루어져 있습니다.
  - 불량 사용자 아이디는 '*' 문자를 하나 이상 포함하고 있습니다.
  - 불량 사용자 아이디 하나는 응모자 아이디 중 하나에 해당하고 같은 응모자 아이디가 중복해서 제재 아이디 목록에 들어가는 경우는 없습니다.
- 제재 아이디 목록들을 구했을 때 아이디들이 나열된 순서와 관계없이 아이디 목록의 내용이 동일하다면 같은 것으로 처리하여 하나로 세면 됩니다.

## [입출력 예]
|user_id|banned_id|result|
|-|-|-|
|["frodo", "fradi", "crodo", "abc123", "frodoc"]|["fr*d*", "abc1**"]|2|
|["frodo", "fradi", "crodo", "abc123", "frodoc"]|["*rodo", "*rodo", "******"]|2|
|["frodo", "fradi", "crodo", "abc123", "frodoc"]|["fr*d*", "*rodo", "******", "******"]|3|

## 입출력 예에 대한 설명
### 입출력 예 #1
문제 설명과 같습니다.

### 입출력 예 #2
다음과 같이 두 가지 경우가 있습니다.

|제재 아이디|
|-|
|frodo|
|crodo|
|abc123|

|제재 아이디|
|-|
|frodo|
|crodo|
|frodoc|

### 입출력 예 #3
다음과 같이 세 가지 경우가 있습니다.

|제재 아이디|
|-|
|frodo|
|crodo|
|abc123|
|frodoc|

|제재 아이디|
|-|
|fradi|
|crodo|
|abc123|
|frodoc|

|제재 아이디|
|-|
|fradi|
|frodo|
|abc123|
|frodoc|

# 내 풀이
```python
from collections import deque

def check(user, banned):
    if len(user)!=len(banned):
        return False
    for u, b in zip(user, banned):
        if b!='*' and u!=b:
            return False
    return True

cases = []

def check_case(banned_num, visited):
    global cases
    
    if len(banned_num)==1:
        case = set([])
        for i, v in enumerate(visited):
            if v: case.add(i)
        for banned in banned_num[0]:
            if visited[banned]==0:
                new_case = case|{banned}
                if new_case not in cases:
                    cases.append(case | {banned})
    else:
        for b in banned_num[0]:
            if visited[b]==0:
                visited[b]=1
                check_case(banned_num[1:], visited)
                visited[b]=0

def solution(user_id, banned_id):
    banned_num = []
    
    for banned in banned_id:
        tmp = []
        for i, user in enumerate(user_id):
            if check(user, banned):
                tmp.append(i)
        banned_num.append(tmp)
        
    visited = [False for i in range(len(user_id))]
    
    if len(banned_num)==1: return len(banned_num[0])
    
    check_case(banned_num, visited)
    
    global cases
    return len(cases)
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.04ms, 10.2MB)
테스트 3 〉	통과 (0.04ms, 10.1MB)
테스트 4 〉	통과 (0.59ms, 10.2MB)
테스트 5 〉	통과 (96.78ms, 10.4MB)
테스트 6 〉	통과 (0.84ms, 10.3MB)
테스트 7 〉	통과 (0.06ms, 10.1MB)
테스트 8 〉	통과 (0.07ms, 10.1MB)
테스트 9 〉	통과 (0.08ms, 10.1MB)
테스트 10 〉	통과 (0.04ms, 10.2MB)
테스트 11 〉	통과 (0.08ms, 10.2MB)
```
- 카카오 문제답게, (리스트 처리 + 복잡한 과정)의 문제였다
- 전략
  - 우선은 `check()`를 사용해, `banned_id` 각각의 원소가 어떤 `user_id`에 매핑되는지 `banned_num`에 저장 (`user_id`의 인덱스로 저장)
  - 그 다음 `check_case()`의 재귀호출을 통해, DFS 방식으로 가능한 모든 케이스를 찾고, global 변수 `cases`에 중복이 없도록 저장
    - `set()`을 사용하려고 했지만, 리스트 형태의 원소로는 hashing이 안되어 set 기능을 사용할 수 없는 것 같았다.
  - 가능한 중복 없는 모든 경우의 `cases`의 갯수가 정답!
- 다른 사람의 풀이를 보니,
  - `banned_id`에 매핑되는 `user_id`를 인덱스가 아닌 id 자체를 저장하여, `set()` 처리하기도 함
