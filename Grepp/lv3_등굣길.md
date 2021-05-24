# 등굣길
## 문제 설명
계속되는 폭우로 일부 지역이 물에 잠겼습니다. 물에 잠기지 않은 지역을 통해 학교를 가려고 합니다. 집에서 학교까지 가는 길은 m x n 크기의 격자모양으로 나타낼 수 있습니다.

아래 그림은 m = 4, n = 3 인 경우입니다.

![image](https://user-images.githubusercontent.com/19163372/119344525-bb82d600-bcd2-11eb-88e1-7a1fb9cb99b6.png)

가장 왼쪽 위, 즉 집이 있는 곳의 좌표는 (1, 1)로 나타내고 가장 오른쪽 아래, 즉 학교가 있는 곳의 좌표는 (m, n)으로 나타냅니다.

격자의 크기 m, n과 물이 잠긴 지역의 좌표를 담은 2차원 배열 puddles이 매개변수로 주어집니다. 집에서 학교까지 갈 수 있는 최단경로의 개수를 1,000,000,007로 나눈 나머지를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 격자의 크기 m, n은 1 이상 100 이하인 자연수입니다.
- m과 n이 모두 1인 경우는 입력으로 주어지지 않습니다.
- 물에 잠긴 지역은 0개 이상 10개 이하입니다.
- 집과 학교가 물에 잠긴 경우는 입력으로 주어지지 않습니다.
## 입출력 예
|m|n|puddles|return|
|---|---|---|---|
|4|3|[[2, 2]]|4|
## 입출력 예 설명
![image](https://user-images.githubusercontent.com/19163372/119344641-dce3c200-bcd2-11eb-8e25-8215f3d4b954.png)
# 풀이
- 처음에 문제를 봤을때 다익스트라 알고리즘이나 bfs를 사용하고자 하였다.
- 그러나 최단 경로를 찾는 문제가 아닌, 최단 경로로 학교까지 가는 경로의 갯수를 구하는 문제이기 때문에, bfs를 사용하기로 했다.
```python
from collections import deque

def solution(m, n, puddles):
    min_d = float('inf')
    q = deque([(1, 1, 0, 'S')])
    count=0
    
    while q:
        x, y, distance, direction = q.popleft()
        if distance>min_d:
            return count%1000000007
        elif x==n and y==m and distance<=min_d:
            count+=1
            min_d = distance
            continue
        if x-1>=1 and direction!='D' and [y, x-1] not in puddles:
            q.append((x-1, y, distance+1, 'U'))
        if y-1>=1 and direction!='R' and [y-1, x] not in puddles:
            q.append((x, y-1, distance+1, 'L'))
        if x+1<=n and direction!='U' and [y, x+1] not in puddles:
            q.append((x+1, y, distance+1, 'D'))
        if y+1<=m and direction!='L' and [y+1, x] not in puddles:
            q.append((x, y+1, distance+1, 'R'))
    else:
        return 0
```
- 위의 코드의 결과가 나왔는데, 일부분의 점수만 획득했고, 결국 해답을 참조했다..
# 해답
```python
def solution(m, n, puddles):
    map = [[0] * (m + 1) for _ in range(n + 1)]
    map[1][1] = 1 # 1, 1 좌표의 경우의 수는 1입니다
    
    for x, y in puddles:
        map[y][x] = -1 # 웅덩이 좌표 값을 -1로 바꿉니다.
        
    for y in range(1, n + 1):
        for x in range(1, m + 1):
            if map[y][x] == -1: # 웅덩이는 생략합니다.
                map[y][x] = 0 # 이후 합에 영향이 안가도록 0으로 바꿉니다
                continue

            # 왼쪽과 위의 경우의 수를 합칩니다.
            map[y][x] += (map[y][x - 1] + map[y - 1][x]) % 1000000007

    return map[n][m] # n, m 좌표의 경우의 수를 리턴합니다.
```
그러나 위 해답은 내가 이해한 문제와는 다른 해답이였다. 예를 들면,
|S|X||||
|---|---|---|---|---|
||X||||
||||X|E|

다음과 같은 경로가 있을 때, 나는 이 상태에서 학교에 갈 수 있는 최단 경로의 갯수 즉,

|S|X||||
|---|---|---|---|---|
|O|X|O|O|O|
|O|O|O|X|E|

이렇게 갈 수 있는 1가지의 경로가 존재한다고 생각했지만,

해답은 현재 어떤 경로로 가도, 물 웅덩이가 없을 때의 최단경로 가지 못하기 때문에 0이라는 답이 나오도록 했다.

그래서 지극히 개인적인 생각이지만, ~~문제 이해에 대한 오해를 불러 일으킬 수 있으므로, 별로 좋지 않은 문제인 것 같다~~
