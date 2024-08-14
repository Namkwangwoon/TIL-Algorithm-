# 표 병합
## 문제 설명
당신은 표 편집 프로그램을 작성하고 있습니다.
표의 크기는 50 × 50으로 고정되어있고 초기에 모든 셀은 비어 있습니다.
각 셀은 문자열 값을 가질 수 있고, 다른 셀과 병합될 수 있습니다.


위에서 `r`번째, 왼쪽에서 `c`번째 위치를 (`r`, `c`)라고 표현할 때, 당신은 다음 명령어들에 대한 기능을 구현하려고 합니다.

1. `"UPDATE r c value"`
    - (`r`, `c`) 위치의 셀을 선택합니다.
    - 선택한 셀의 값을 `value`로 바꿉니다.
2. `"UPDATE value1 value2"`
    - `value1`을 값으로 가지고 있는 모든 셀을 선택합니다.
    - 선택한 셀의 값을 `value2`로 바꿉니다.
3. `"MERGE r1 c1 r2 c2"`
    - (`r1`, `c1`) 위치의 셀과 (`r2`, `c2`) 위치의 셀을 선택하여 병합합니다.
    - 선택한 두 위치의 셀이 같은 셀일 경우 무시합니다.
    - 선택한 두 셀은 서로 인접하지 않을 수도 있습니다. 이 경우 (`r1`, `c1`) 위치의 셀과 (`r2`, `c2`) 위치의 셀만 영향을 받으며, 그 사이에 위치한 셀들은 영향을 받지 않습니다.
    - 두 셀 중 한 셀이 값을 가지고 있을 경우 병합된 셀은 그 값을 가지게 됩니다.
    - 두 셀 모두 값을 가지고 있을 경우 병합된 셀은 (`r1`, `c1`) 위치의 셀 값을 가지게 됩니다.
    - 이후 (`r1`, `c1`) 와 (`r2`, `c2`) 중 어느 위치를 선택하여도 병합된 셀로 접근합니다.
4. `"UNMERGE r c"`
    - (`r`, `c`) 위치의 셀을 선택하여 해당 셀의 모든 병합을 해제합니다.
    - 선택한 셀이 포함하고 있던 모든 셀은 프로그램 실행 초기의 상태로 돌아갑니다.
    - 병합을 해제하기 전 셀이 값을 가지고 있었을 경우 (`r`, `c`) 위치의 셀이 그 값을 가지게 됩니다.
5. `"PRINT r c"`
    - (`r`, `c`) 위치의 셀을 선택하여 셀의 값을 출력합니다.
    - 선택한 셀이 비어있을 경우 `"EMPTY"`를 출력합니다.

아래는 `UPDATE` 명령어를 실행하여 빈 셀에 값을 입력하는 예시입니다.

|commands|효과|
|-|-|
|UPDATE 1 1 menu|(1,1)에 "menu" 입력|
|UPDATE 1 2 category|(1,2)에 "category" 입력|
|UPDATE 2 1 bibimbap|(2,1)에 "bibimbap" 입력|
|UPDATE 2 2 korean|(2,2)에 "korean" 입력|
|UPDATE 2 3 rice|(2,3)에 "rice" 입력|
|UPDATE 3 1 ramyeon|(3,1)에 "ramyeon" 입력|
|UPDATE 3 2 korean|(3,2)에 "korean" 입력|
|UPDATE 3 3 noodle|(3,3)에 "noodle" 입력|
|UPDATE 3 4 instant|(3,4)에 "instant" 입력|
|UPDATE 4 1 pasta|(4,1)에 "pasta" 입력|
|UPDATE 4 2 italian|(4,2)에 "italian" 입력|
|UPDATE 4 3 noodle|(4,3)에 "noodle" 입력|

위 명령어를 실행하면 아래 그림과 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/c1fbf2df-45ac-4f34-8af6-b54447bcf4fa)


아래는 `MERGE` 명령어를 실행하여 셀을 병합하는 예시입니다.

|commands|효과|
|-|-|
|MERGE 1 2 1 3|(1,2)와 (1,3) 병합|
|MERGE 1 3 1 4|(1,3)과 (1,4) 병합|

위 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/e4cbce7a-21fe-4964-aa1e-04d3a9a8fb1c)

병합한 셀은 `"category"` 값을 가지게 되며 (1,2), (1,3), (1,4) 중 어느 위치를 선택하더라도 접근할 수 있습니다.

아래는 `UPDATE` 명령어를 실행하여 셀의 값을 변경하는 예시입니다.

|commands|효과|
|-|-|
|UPDATE korean hansik|"korean"을 "hansik"으로 변경|
|UPDATE 1 3 group|(1,3) 위치의 셀 값을 "group"으로 변경|

위 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/4427011d-d9e7-4697-bf80-7e8b9e8be7db)

아래는 `UNMERGE` 명령어를 실행하여 셀의 병합을 해제하는 예시입니다.

|commands|효과|
|-|-|
|UNMERGE 1 4|셀 병합 해제 후 원래 값은 (1,4)가 가짐|

위 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/40f5f665-ffb3-426a-8eb0-031fe178ae8d)


실행할 명령어들이 담긴 1차원 문자열 배열 `commands`가 매개변수로 주어집니다. `commands`의 명령어들을 순서대로 실행하였을 때, `"PRINT r c"` 명령어에 대한 실행결과를 순서대로 1차원 문자열 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 1 ≤ `commands`의 길이 ≤ 1,000
- `commands`의 각 원소는 아래 5가지 형태 중 하나입니다.
  1. `"UPDATE r c value"`
      - `r`, `c`는 선택할 셀의 위치를 나타내며, 1~50 사이의 정수입니다.
      - `value`는 셀에 입력할 내용을 나타내며, 알파벳 소문자와 숫자로 구성된 길이 1~10 사이인 문자열입니다.
  2. `"UPDATE value1 value2"`
      - `value1`은 선택할 셀의 값, `value2`는 셀에 입력할 내용을 나타내며, 알파벳 소문자와 숫자로 구성된 길이 1~10 사이인 문자열입니다.
  3. `"MERGE r1 c1 r2 c2"`
      - `r1`, `c1`, `r2`, `c2`는 선택할 셀의 위치를 나타내며, 1~50 사이의 정수입니다.
  4. `"UNMERGE r c"`
      - `r`, `c`는 선택할 셀의 위치를 나타내며, 1~50 사이의 정수입니다.
  5. `"PRINT r c"`
      - `r`, `c`는 선택할 셀의 위치를 나타내며, 1~50 사이의 정수입니다.
- `commands`는 1개 이상의 `"PRINT r c"` 명령어를 포함하고 있습니다.

## 입출력 예
|commands|result|
|-|-|
|["UPDATE 1 1 menu", "UPDATE 1 2 category", "UPDATE 2 1 bibimbap", "UPDATE 2 2 korean", "UPDATE 2 3 rice", "UPDATE 3 1 ramyeon", "UPDATE 3 2 korean", "UPDATE 3 3 noodle", "UPDATE 3 4 instant", "UPDATE 4 1 pasta", "UPDATE 4 2 italian", "UPDATE 4 3 noodle", "MERGE 1 2 1 3", "MERGE 1 3 1 4", "UPDATE korean hansik", "UPDATE 1 3 group", "UNMERGE 1 4", "PRINT 1 3", "PRINT 1 4"]|["EMPTY", "group"]
|["UPDATE 1 1 a", "UPDATE 1 2 b", "UPDATE 2 1 c", "UPDATE 2 2 d", "MERGE 1 1 1 2", "MERGE 2 2 2 1", "MERGE 2 1 1 1", "PRINT 1 1", "UNMERGE 2 2", "PRINT 1 1"]|["d", "EMPTY"]

## 입출력 예 설명
### 입출력 예 #1

- 문제 예시와 같습니다. (1,3) 위치의 셀은 비어있고 (1,4) 위치의 셀 값은 `"group"`입니다. 따라서 `["EMPTY", "group"]`을 return 해야 합니다.

### 입출력 예 #2

- 모든 `UPDATE` 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/ac686a26-0d6e-4025-83d1-db36f96ed578)

- `"MERGE 1 1 1 2"` 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/e7a5b77e-c09b-42e9-89a9-7df82706d108)

- `"MERGE 2 2 2 1"` 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/dcd8321d-f9d3-4391-bf0a-10e51426f1f3)

- `"MERGE 2 1 1 1"` 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/44cf7b0a-11fe-4c8a-9bbf-52f82b127e01)

- `"UNMERGE 2 2"` 명령어를 실행하면 아래와 같은 상태가 됩니다.

![image](https://github.com/user-attachments/assets/c1cb2840-7a73-4e61-9e74-2e9b4e506645)

# 내 풀이
```python
from collections import defaultdict

def update(args, merged, table):
    # print('UPDATE!')
    if len(args)==3:
        r, c, v = args
        r, c = int(r), int(c)
        table[(r, c)] = v
        for (merged_r, merged_c) in merged[(r, c)]:
            table[(merged_r,merged_c)] = v
    else:
        visited = set()
        v1, v2 = args
        for (r, c) in table:
            if table[(r,c)]==v1 and not (r, c) in visited:
                table[(r,c)]=v2
                visited.add((r, c))
                for (merged_r, merged_c) in merged[(r, c)]:
                    if not (merged_r, merged_c) in visited:
                        table[(merged_r,merged_c)] = v2
                        visited.add((merged_r, merged_c))
    
    
def merge(args, merged, table):
    # print('MERGE!')
    r1, c1, r2, c2 = map(int, args)
    if (r2, c2) in merged[(r1, c1)]:
        return
    merging = merged[(r1, c1)] | merged[(r2, c2)] | set([(r1, c1), (r2, c2)])
    # print(merging)
    if table[(r1,c1)] and table[(r2,c2)]:
        v1 = table[(r1,c1)]
        table[(r2, c2)] = v1
    elif table[(r1,c1)] or table[(r2,c2)]:
        index = (r1,c1) if table[(r1,c1)] else (r2,c2)
        not_index = (r2,c2) if table[(r1,c1)] else (r1,c1)
        v1 = table[index]
        table[not_index] = v1
    else:
        v1 = ''
    
    for (merged_r, merged_c) in merged[(r2, c2)]:
        table[(merged_r, merged_c)]=v1
        merged[(merged_r, merged_c)] = set(list(merging))
        merged[(merged_r, merged_c)].remove((merged_r, merged_c))
    merged[(r2, c2)] = set(list(merging))
    merged[(r2, c2)].remove((r2, c2))
    for (merged_r, merged_c) in merged[(r1, c1)]:
        merged[(merged_r, merged_c)] = set(list(merging))
        merged[(merged_r, merged_c)].remove((merged_r, merged_c))
    merged[(r1, c1)] = set(list(merging))
    merged[(r1, c1)].remove((r1, c1))
    
    
def unmerge(args, merged, table):
    # print('UNMERGE!')
    r, c = map(int, args)
    
    if not merged[(r, c)]:
        return
    
    for (merged_x, merged_y) in merged[(r, c)]:
        del merged[(merged_x, merged_y)]
        del table[(merged_x, merged_y)]
    del merged[(r, c)]
    
    
def print_(args, merged, table):
    # print('PRINT!')
    r, c = map(int, args)
    if not table[(r, c)]:
        to_print = ['EMPTY']
    else:
        to_print = [table[(r, c)]]
    return to_print


def solution(commands):
    table = defaultdict(str)
    merged = defaultdict(set)
    answer = []
    
    
    for command in commands:
        command = command.split()
        comm, args = command[0], command[1:]
        
        if comm=='UPDATE':
            update(args, merged, table)
        elif comm=='MERGE':
            merge(args, merged, table)
        elif comm=='UNMERGE':
            unmerge(args, merged, table)
        elif comm=='PRINT':
            answer += print_(args, merged, table)
    
        # print('merged', merged)
        # print('table', table)
        # print()
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.17ms, 10.4MB)
테스트 2 〉	통과 (0.10ms, 10.4MB)
테스트 3 〉	통과 (0.05ms, 10.5MB)
테스트 4 〉	통과 (0.04ms, 10.6MB)
테스트 5 〉	통과 (0.05ms, 10.5MB)
테스트 6 〉	통과 (0.12ms, 10.4MB)
테스트 7 〉	통과 (0.07ms, 10.4MB)
테스트 8 〉	통과 (0.08ms, 10.4MB)
테스트 9 〉	통과 (0.20ms, 10.4MB)
테스트 10 〉	통과 (0.19ms, 10.4MB)
테스트 11 〉	실패 (0.28ms, 10.4MB)
테스트 12 〉	통과 (0.29ms, 10.5MB)
테스트 13 〉	통과 (2.60ms, 10.5MB)
테스트 14 〉	통과 (3.21ms, 10.5MB)
테스트 15 〉	통과 (31.09ms, 11.6MB)
테스트 16 〉	통과 (25.72ms, 11.4MB)
테스트 17 〉	통과 (53.54ms, 11.9MB)
테스트 18 〉	통과 (60.58ms, 11.9MB)
테스트 19 〉	통과 (182.68ms, 12.4MB)
테스트 20 〉	통과 (125.97ms, 22MB)
테스트 21 〉	통과 (9276.00ms, 39.1MB)
테스트 22 〉	통과 (3.37ms, 10.5MB)
```
- 전형적인 구현 문제였다.
- 전략
    - 셀의 값들 정보가 담겨있는 `table`과 병합된 셀 정보가 담겨있는 `merged` 모두 딕셔너리로 구성하여, 해시 및 공간적인 이점을 활용
        - `table`은 `(r,c)`에 값을 넣음
        - `merged`는 `(r,c)`에 병합된 셀의 좌표들을 모두 넣음
- 실수했던 부분을 결국 찾았지만, 하나의 테스트 케이스는 결국 실패 원인을 찾지 못했다,,
    - MERGE 부분에서 두 셀 중 하나만 값이 존재하는 경우를 빠뜨렸다 (문제를 꼼꼼히 읽자!)

# 다른 사람의 풀이
```python
def solution(commands):
    answer = []
    merged = [[(i, j) for j in range(50)] for i in range(50)]
    board = [["EMPTY"] * 50 for _ in range(50)]
    for command in commands:
        command = command.split(' ')
        if command[0] == 'UPDATE':
            if len(command) == 4:
                r,c,value = int(command[1])-1,int(command[2])-1,command[3]
                x,y = merged[r][c]
                board[x][y] = value
            elif len(command) == 3:
                value1, value2 = command[1], command[2]
                for i in range(50):
                    for j in range(50):
                        if board[i][j] == value1:
                            board[i][j] = value2
        elif command[0] == 'MERGE':
            r1,c1,r2,c2 = int(command[1])-1, int(command[2])-1, int(command[3])-1, int(command[4])-1
            x1,y1 = merged[r1][c1]
            x2,y2 = merged[r2][c2]
            if board[x1][y1] == "EMPTY":
                board[x1][y1] = board[x2][y2]
            for i in range(50):
                for j in range(50):
                    if merged[i][j] == (x2,y2):
                        merged[i][j] = (x1,y1)
        elif command[0] == 'UNMERGE':
            r, c = int(command[1])-1,int(command[2])-1
            x, y = merged[r][c]
            tmp = board[x][y]
            for i in range(50):
                for j in range(50):
                    if merged[i][j] == (x,y):
                        merged[i][j] = (i,j)
                        board[i][j] = "EMPTY"
            board[r][c] = tmp
        elif command[0] == 'PRINT':
            r, c = int(command[1])-1, int(command[2])-1
            x, y = merged[r][c]
            answer.append(board[x][y])
    return answer
```
- 전략
    - 내 풀이와 비슷하게 병합과 실제값을 관리하는 `merge`와 `board`를 두 가지 사용했지만, 이중 리스트로 사용
    - 가장 핵심인 `merge`의 값은, 하나의 좌표를 고정하고 이후 병합되는 모든 셀들에 이 좌표를 사용
