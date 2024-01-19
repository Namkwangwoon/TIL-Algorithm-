# 아이템 줍기
## 문제 설명
다음과 같은 다각형 모양 지형에서 캐릭터가 아이템을 줍기 위해 이동하려 합니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/b005b026-0a22-4601-b489-22d83f1c1fac)

지형은 각 변이 x축, y축과 평행한 직사각형이 겹쳐진 형태로 표현하며, 캐릭터는 이 다각형의 둘레(굵은 선)를 따라서 이동합니다.

만약 직사각형을 겹친 후 다음과 같이 중앙에 빈 공간이 생기는 경우, 다각형의 가장 바깥쪽 테두리가 캐릭터의 이동 경로가 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/ad469b17-7e40-4e9e-bfa4-c74a8c786f38)

단, 서로 다른 두 직사각형의 x축 좌표 또는 y축 좌표가 같은 경우는 없습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/77efc742-1c09-4066-b39a-2c44095b92bc)

즉, 위 그림처럼 서로 다른 두 직사각형이 꼭짓점에서 만나거나, 변이 겹치는 경우 등은 없습니다.

다음 그림과 같이 지형이 2개 이상으로 분리된 경우도 없습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/f055ccd0-7953-4594-be36-2913059c81f3)

한 직사각형이 다른 직사각형 안에 완전히 포함되는 경우 또한 없습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/e78d4e1b-7119-451a-a5d2-8697bfab4f5f)

지형을 나타내는 직사각형이 담긴 2차원 배열 rectangle, 초기 캐릭터의 위치 characterX, characterY, 아이템의 위치 itemX, itemY가 solution 함수의 매개변수로 주어질 때, 캐릭터가 아이템을 줍기 위해 이동해야 하는 가장 짧은 거리를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- rectangle의 세로(행) 길이는 1 이상 4 이하입니다.
- rectangle의 원소는 각 직사각형의 [좌측 하단 x, 좌측 하단 y, 우측 상단 x, 우측 상단 y] 좌표 형태입니다.
  - 직사각형을 나타내는 모든 좌표값은 1 이상 50 이하인 자연수입니다.
  - 서로 다른 두 직사각형의 x축 좌표, 혹은 y축 좌표가 같은 경우는 없습니다.
  - 문제에 주어진 조건에 맞는 직사각형만 입력으로 주어집니다.
- charcterX, charcterY는 1 이상 50 이하인 자연수입니다.
  - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- itemX, itemY는 1 이상 50 이하인 자연수입니다.
  - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- 캐릭터와 아이템의 처음 위치가 같은 경우는 없습니다.

---

- 전체 배점의 50%는 직사각형이 1개인 경우입니다.
- 전체 배점의 25%는 직사각형이 2개인 경우입니다.
- 전체 배점의 25%는 직사각형이 3개 또는 4개인 경우입니다.

## 입출력 예
|rectangle|characterX|characterY|itemX|itemY|result|
|-|-|-|-|-|-|
|[[1,1,7,4],[3,2,5,5],[4,3,6,9],[2,6,8,8]]|1|3|7|8|17|
|[[1,1,8,4],[2,2,4,9],[3,6,9,8],[6,3,7,7]]|9|7|6|1|11|
|[[1,1,5,7]]|1|1|4|7|9|
|[[2,1,7,5],[6,4,10,10]]|3|1|7|10|15|
|[[2,2,5,5],[1,3,6,4],[3,1,4,6]]|1|4|6|3|10|

## 입출력 예 설명
### 입출력 예 #1

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/bb1da33e-6143-4e70-b8f1-8fa691086825)

캐릭터 위치는 (1, 3)이며, 아이템 위치는 (7, 8)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

### 입출력 예 #2

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/75d2648c-07e5-4b10-aa14-1a4a6e8b3ef5)

캐릭터 위치는 (9, 7)이며, 아이템 위치는 (6, 1)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

### 입출력 예 #3

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/1280f753-3d27-4775-bcc9-a5db3dd63702)

캐릭터 위치는 (1, 1)이며, 아이템 위치는 (4, 7)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

### 입출력 예 #4, #5

설명 생략

# 내 풀이 1
```python
from collections import deque

def solution(rectangle, characterX, characterY, itemX, itemY):
    
    def check_rec(x, y):
        check = []
        
        for i, rec in enumerate(rectangle):
            if x>rec[0] and x<rec[2] and y>rec[1] and y<rec[3]:
                return False
            if (x==rec[0] and (y>=rec[1] and y<=rec[3])) or (y==rec[3] and (x>=rec[0] and x<=rec[2])) or (x==rec[2] and (y>=rec[1] and y<=rec[3])) or (y==rec[1] and (x>=rec[0] and x<=rec[2])):
                check.append(i)
        
        return check
            
    
    q = deque([(0, characterX, characterY, 's')])
    maxX, maxY = 0, 0
    for rec in rectangle:
        if rec[2]+1>maxX: maxX = rec[2]+1
        if rec[3]+1>maxY: maxY = rec[3]+1
    visited = [[0 for i in range(maxY)] for i in range(maxX)]
    
    while q:
        dist, x, y, direc = q.popleft()
        
        if direc!='u' and check_rec(x, y-1) and (set(check_rec(x, y)) & set(check_rec(x, y-1))) and visited[x][y-1] != 1:
            if x==itemX and y-1==itemY: return dist+1
            visited[x][y-1] = 1
            q.append((dist+1, x, y-1, 'd'))
        if direc!='d' and check_rec(x, y+1) and (set(check_rec(x, y)) & set(check_rec(x, y+1))) and visited[x][y+1] != 1:
            if x==itemX and y+1==itemY: return dist+1
            visited[x][y+1] = 1
            q.append((dist+1, x, y+1, 'u'))
        if direc!='l' and check_rec(x+1, y) and (set(check_rec(x, y)) & set(check_rec(x+1, y))) and visited[x+1][y] != 1:
            if x+1==itemX and y==itemY: return dist+1
            visited[x+1][y] = 1
            q.append((dist+1, x+1, y, 'r'))
        if direc!='r' and check_rec(x-1, y) and (set(check_rec(x, y)) & set(check_rec(x-1, y))) and visited[x-1][y] != 1:
            if x-1==itemX and y==itemY: return dist+1
            visited[x-1][y] = 1
            q.append((dist+1, x-1, y, 'l'))
    
    return False
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.3MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.02ms, 10.3MB)
테스트 4 〉	통과 (0.01ms, 10.4MB)
테스트 5 〉	통과 (0.02ms, 10.2MB)
테스트 6 〉	통과 (0.04ms, 10.3MB)
테스트 7 〉	통과 (0.10ms, 10.3MB)
테스트 8 〉	통과 (0.11ms, 10.3MB)
테스트 9 〉	통과 (0.50ms, 10.3MB)
테스트 10 〉	통과 (0.37ms, 10.4MB)
테스트 11 〉	통과 (0.53ms, 10.3MB)
테스트 12 〉	통과 (0.50ms, 10.4MB)
테스트 13 〉	통과 (0.64ms, 10.3MB)
테스트 14 〉	통과 (0.46ms, 10.5MB)
테스트 15 〉	통과 (0.33ms, 10.3MB)
테스트 16 〉	통과 (0.46ms, 10.3MB)
테스트 17 〉	통과 (0.88ms, 10.4MB)
테스트 18 〉	실패 (1.36ms, 10.3MB)
테스트 19 〉	실패 (0.90ms, 10.3MB)
테스트 20 〉	통과 (0.96ms, 10.2MB)
테스트 21 〉	통과 (1.49ms, 10.3MB)
테스트 22 〉	실패 (0.39ms, 10.4MB)
테스트 23 〉	통과 (1.40ms, 10.2MB)
테스트 24 〉	통과 (0.94ms, 10.4MB)
테스트 25 〉	통과 (0.34ms, 10.3MB)
테스트 26 〉	통과 (0.62ms, 10.3MB)
테스트 27 〉	실패 (0.42ms, 10.3MB)
테스트 28 〉	실패 (0.49ms, 10.3MB)
테스트 29 〉	실패 (0.30ms, 10.2MB)
테스트 30 〉	실패 (0.30ms, 10.4MB)
```
- 가장 짧은 거리만 찾으면 되므로, bfs 사용
- 테두리를 타고 가는 방법은 맞았다
- 하지만, 아래 그림과 같이, 좁은 부분에서 경로를 잘못 찾는 문제가 발생했다
  ![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/a187e0c2-e17e-4812-9de7-9229ead56d23)
- 다른 사람의 풀이를 참고하여, 모든 좌표들을 두 배씩 해준 후, 이동 거리는 2로 나누어줬다. (내 풀이2)

# 내 풀이 2
```python
from collections import deque

def solution(rectangle, characterX, characterY, itemX, itemY):
    maxX, maxY = 0, 0
    for rec in rectangle:
        if maxX < rec[2]: maxX = rec[2]
        if maxY < rec[3]: maxY = rec[3]
    maps = [[-1 for i in range(2*(maxY+2))] for j in range(2*(maxX+2))]
    
    for rec in rectangle:
        x, y, X, Y = rec
        x, y, X, Y = 2*x, 2*y, 2*X, 2*Y
        for xs in range(x, X+1):
            if maps[xs][y]==-1: maps[xs][y]=1
            if maps[xs][Y]==-1: maps[xs][Y]=1
        for ys in range(y, Y+1):
            if maps[x][ys]==-1: maps[x][ys]=1
            if maps[X][ys]==-1: maps[X][ys]=1
        for xs in range(x+1, X):
            for ys in range(y+1, Y):
                maps[xs][ys] = 0
                
    q = deque([(0, characterX*2, characterY*2, 's')])
    
    while q:
        dist, x, y, direc = q.popleft()
        
        if direc!='d' and maps[x][y+1]==1:
            if x==2*itemX and y+1==2*itemY: return (dist+1)/2
            else: q.append((dist+1, x, y+1, 'u'))
        if direc!='u' and maps[x][y-1]==1:
            if x==2*itemX and y-1==2*itemY: return (dist+1)/2
            else: q.append((dist+1, x, y-1, 'd'))
        if direc!='r' and maps[x-1][y]==1:
            if x-1==2*itemX and y==2*itemY: return (dist+1)/2
            else: q.append((dist+1, x-1, y, 'l'))
        if direc!='l' and maps[x+1][y]==1:
            if x+1==2*itemX and y==2*itemY: return (dist+1)/2
            else: q.append((dist+1, x+1, y, 'r'))
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.2MB)
테스트 2 〉	통과 (0.02ms, 10.3MB)
테스트 3 〉	통과 (0.02ms, 10.3MB)
테스트 4 〉	통과 (0.02ms, 10.3MB)
테스트 5 〉	통과 (0.02ms, 10.3MB)
테스트 6 〉	통과 (0.02ms, 10.3MB)
테스트 7 〉	통과 (0.06ms, 10.2MB)
테스트 8 〉	통과 (0.11ms, 10.3MB)
테스트 9 〉	통과 (0.63ms, 10.3MB)
테스트 10 〉	통과 (0.56ms, 10.2MB)
테스트 11 〉	통과 (0.65ms, 10.3MB)
테스트 12 〉	통과 (0.88ms, 10.3MB)
테스트 13 〉	통과 (0.60ms, 10.5MB)
테스트 14 〉	통과 (0.34ms, 10.3MB)
테스트 15 〉	통과 (0.33ms, 10.3MB)
테스트 16 〉	통과 (0.40ms, 10.3MB)
테스트 17 〉	통과 (0.70ms, 10.4MB)
테스트 18 〉	통과 (0.50ms, 10.2MB)
테스트 19 〉	통과 (0.88ms, 10.2MB)
테스트 20 〉	통과 (0.83ms, 10.3MB)
테스트 21 〉	통과 (0.44ms, 10.3MB)
테스트 22 〉	통과 (0.24ms, 10.2MB)
테스트 23 〉	통과 (0.50ms, 10.3MB)
테스트 24 〉	통과 (0.38ms, 10.3MB)
테스트 25 〉	통과 (0.08ms, 10.3MB)
테스트 26 〉	통과 (0.09ms, 10.3MB)
테스트 27 〉	통과 (0.11ms, 10.3MB)
테스트 28 〉	통과 (0.11ms, 10.3MB)
테스트 29 〉	통과 (0.12ms, 10.3MB)
테스트 30 〉	통과 (0.11ms, 10.3MB)
```
- 이외에도 바깥은 -1, 테두리는 1, 안쪽은 0 으로 이루어진 map을 만드는 개념을 적용
