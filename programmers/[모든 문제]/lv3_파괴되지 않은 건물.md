# 파괴되지 않은 건물
## 문제 설명
**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

N x M 크기의 행렬 모양의 게임 맵이 있습니다. 이 맵에는 내구도를 가진 건물이 각 칸마다 하나씩 있습니다. 적은 이 건물들을 공격하여 파괴하려고 합니다. 건물은 적의 공격을 받으면 내구도가 감소하고 내구도가 0이하가 되면 파괴됩니다. 반대로, 아군은 회복 스킬을 사용하여 건물들의 내구도를 높이려고 합니다.

적의 공격과 아군의 회복 스킬은 항상 직사각형 모양입니다.

예를 들어, 아래 사진은 크기가 4 x 5인 맵에 내구도가 5인 건물들이 있는 상태입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/47265fbe-1ba9-41cf-9c55-c5408dfbfa61)

첫 번째로 적이 맵의 (0,0)부터 (3,4)까지 공격하여 4만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/b033abee-606c-41f6-b17f-0ede6fbd79d1)

두 번째로 적이 맵의 (2,0)부터 (2,3)까지 공격하여 2만큼 건물의 내구도를 낮추면 아래와 같이 4개의 건물이 파괴되는 상태가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/03cb7f40-83e1-4f6e-a211-8780f5f7e413)

세 번째로 아군이 맵의 (1,0)부터 (3,1)까지 회복하여 2만큼 건물의 내구도를 높이면 아래와 같이 2개의 건물이 파괴되었다가 복구되고 2개의 건물만 파괴되어있는 상태가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/c07d685c-bb7c-41e5-8372-a7b49abe33d3)

마지막으로 적이 맵의 (0,1)부터 (3,3)까지 공격하여 1만큼 건물의 내구도를 낮추면 아래와 같이 8개의 건물이 더 파괴되어 총 10개의 건물이 파괴된 상태가 됩니다. (내구도가 0 이하가 된 이미 파괴된 건물도, 공격을 받으면 계속해서 내구도가 하락하는 것에 유의해주세요.)

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/46e1d5cb-401d-43db-b406-3ea2fe1e2a6f)

최종적으로 총 10개의 건물이 파괴되지 않았습니다.

건물의 내구도를 나타내는 2차원 정수 배열 `board`와 적의 공격 혹은 아군의 회복 스킬을 나타내는 2차원 정수 배열 `skill`이 매개변수로 주어집니다. 적의 공격 혹은 아군의 회복 스킬이 모두 끝난 뒤 파괴되지 않은 건물의 개수를 return하는 solution함수를 완성해 주세요.

## 제한사항
- 1 ≤ `board`의 행의 길이 (= `N`) ≤ 1,000
- 1 ≤ `board`의 열의 길이 (= `M`) ≤ 1,000
- 1 ≤ `board`의 원소 (각 건물의 내구도) ≤ 1,000
- 1 ≤ `skill`의 행의 길이 ≤ 250,000
- `skill`의 열의 길이 = 6
- `skill`의 각 행은 `[type, r1, c1, r2, c2, degree]`형태를 가지고 있습니다.
  - type은 1 혹은 2입니다.
    - type이 1일 경우는 적의 공격을 의미합니다. 건물의 내구도를 낮춥니다.
    - type이 2일 경우는 아군의 회복 스킬을 의미합니다. 건물의 내구도를 높입니다.
  - (r1, c1)부터 (r2, c2)까지 직사각형 모양의 범위 안에 있는 건물의 내구도를 degree 만큼 낮추거나 높인다는 뜻입니다.
    - 0 ≤ r1 ≤ r2 < `board`의 행의 길이
    - 0 ≤ c1 ≤ c2 < `board`의 열의 길이
    - 1 ≤ degree ≤ 500
    - type이 1이면 degree만큼 건물의 내구도를 낮춥니다.
    - type이 2이면 degree만큼 건물의 내구도를 높입니다.
- 건물은 파괴되었다가 회복 스킬을 받아 내구도가 1이상이 되면 파괴되지 않은 상태가 됩니다. 즉, 최종적으로 건물의 내구도가 1이상이면 파괴되지 않은 건물입니다.

## 정확성 테스트 케이스 제한 사항
- 1 ≤ `board`의 행의 길이 (= `N`) ≤ 100
- 1 ≤ `board`의 열의 길이 (= `M`) ≤ 100
- 1 ≤ `board`의 원소 (각 건물의 내구도) ≤ 100
- 1 ≤ `skill`의 행의 길이 ≤ 100
- 1 ≤ degree ≤ 100

## 효율성 테스트 케이스 제한 사항
- 주어진 조건 외 추가 제한사항 없습니다.

## 입출력 예
|board|skill|result|
|-|-|-|
|[[5,5,5,5,5],[5,5,5,5,5],[5,5,5,5,5],[5,5,5,5,5]]|[[1,0,0,3,4,4],[1,2,0,2,3,2],[2,1,0,3,1,2],[1,0,1,3,3,1]]|10|
|[[1,2,3],[4,5,6],[7,8,9]]|[[1,1,1,2,2,4],[1,0,0,1,1,2],[2,2,0,2,0,100]]|6|

## 입출력 예 설명
### 입출력 예 #1

문제 예시와 같습니다.

### 입출력 예 #2

<초기 맵 상태>

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/643a6a0a-f9f6-434a-b9b3-a8c1c709ba96)

첫 번째로 적이 맵의 (1,1)부터 (2,2)까지 공격하여 4만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/5f5c8b36-4e6b-42a2-88f7-0782f365ba99)

두 번째로 적이 맵의 (0,0)부터 (1,1)까지 공격하여 2만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/987d533c-79fe-428b-8934-a8ab01a6a8ac)

마지막으로 아군이 맵의 (2,0)부터 (2,0)까지 회복하여 100만큼 건물의 내구도를 높이면 아래와 같은 상황이 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/28ccf0a5-0de3-4bc9-899c-ac585d817783)

총, 6개의 건물이 파괴되지 않았습니다. 따라서 6을 return 해야 합니다.

## 제한시간 안내
- 정확성 테스트 : 10초
- 효율성 테스트 : 언어별로 작성된 정답 코드의 실행 시간의 적정 배수

# 내 풀이
```python
def solution(board, skill):
    answer = len(board) * len(board[0])
    
    for s in skill:
        if s[0]==1:
            for i in range(s[1],s[3]+1):
                for j in range(s[2], s[4]+1):
                    alive = False
                    if board[i][j]>0: alive=True
                    board[i][j]-=s[5]
                    if board[i][j]<=0 and alive: answer-=1
            
        elif s[0]==2:
            for i in range(s[1],s[3]+1):
                for j in range(s[2], s[4]+1):
                    dead = False
                    if board[i][j]<=0: dead=True
                    board[i][j]+=s[5]
                    if board[i][j]>0 and dead: answer+=1
    
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.10ms, 10.2MB)
테스트 3 〉	통과 (0.28ms, 10.2MB)
테스트 4 〉	통과 (1.10ms, 10.1MB)
테스트 5 〉	통과 (2.29ms, 10.2MB)
테스트 6 〉	통과 (7.49ms, 10.3MB)
테스트 7 〉	통과 (6.53ms, 10.2MB)
테스트 8 〉	통과 (19.04ms, 10.2MB)
테스트 9 〉	통과 (13.73ms, 10.4MB)
테스트 10 〉	통과 (25.85ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	실패 (시간 초과)
테스트 2 〉	실패 (시간 초과)
테스트 3 〉	실패 (시간 초과)
테스트 4 〉	실패 (시간 초과)
테스트 5 〉	실패 (시간 초과)
테스트 6 〉	실패 (시간 초과)
테스트 7 〉	실패 (시간 초과)
```
- 안될 줄 알았지만, 일단은 문제가 시키는 대로 다중 for문으로 해봤다.
  - 역시나 정확성만 통과하고 효율성에서 실패
  - numpy도 써봤지만, 역시 결과는 같다
- 결국 다른 사람의 코드를 참고

# 다른 사람의 풀이
```python
def solution(board, skill):
    new_board = [[0]*(len(board[0])+1) for _ in range(len(board)+1)]
    
    for s, r1, c1, r2, c2, val in skill:
        if s==1:
            new_board[r1][c1]-=val
            new_board[r2+1][c2+1]-=val
            new_board[r1][c2+1]+=val
            new_board[r2+1][c1]+=val
        elif s==2:
            new_board[r1][c1]+=val
            new_board[r2+1][c2+1]+=val
            new_board[r1][c2+1]-=val
            new_board[r2+1][c1]-=val
    
    for i in range(len(new_board)-1):
        for j in range(len(new_board[0])-1):
            new_board[i][j+1] += new_board[i][j]
    
    for j in range(len(new_board[0])-1):
        for i in range(len(new_board)-1):
            new_board[i+1][j] += new_board[i][j]
    
    answer = 0
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j]+new_board[i][j]>0: answer+=1
    
    return answer
```
- 모르는 개념인, 누적합 이라는 개념을 사용했다.
- 이렇게 하면, 미리 업데이트 하고자 하는 범위의 모서리 4개를 찍어놓고, 한 번에 업데이트 가능하다.

## 누적합
### 1차원
- `[-5, 0, 0, 0, 0, 5]` 가 있다고 가정하자.
  - 인덱스 0에는 -5, 5에는 5가 있다.
- 인덱스 0부터 끝까지 바로 자신의 값을 바로 다음 인덱스의 값에 차례로 더한다고 해보자
  - 결과는 `[-5, -5, -5, -5, -5, 0]`
- 그러면 인덱스 0부터 4까지 -5를 갖게 된다.
- 따라서 인덱스 i부터 j까지 특정 값 a를 갖게 하고 싶다면, 인덱스 i가 a, (j+1)이 -a를 갖게 한 후 누적합 하면 됨

### 2차원
- 2차원으로 확장해보자. `[[-5, 0, 5], [0, 0, 0], [5, 0, -5]]`
- 먼저 열 방향으로 (행 부터 해도 상관없다.) 누적합
  - `[[-5, -5, 0], [0, 0, 0], [5, 5, 0]]`
- 그 다음, 행 방향으로 누적합
  - `[[-5, -5, 0], [-5, -5, 0], [0, 0, 0]]`
- 2차원은 특이하게, (x1, y1)부터 (x2, y2)까지 특정 값 a를 갖게 하고 싶다면, [x1][y1]과 [x2+1][y2+1]가 a, [x1][y2+1]와 [x2+1][y1]이 -a를 갖도록 하면 됨
