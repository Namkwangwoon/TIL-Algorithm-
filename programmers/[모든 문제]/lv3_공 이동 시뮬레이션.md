# 공 이동 시뮬레이션
## 문제 설명
`n`행 `m`열의 격자가 있습니다. 격자의 각 행은 0, 1, ..., `n-1`번의 번호, 그리고 각 열은 0, 1, ..., `m-1`번의 번호가 순서대로 매겨져 있습니다. 당신은 이 격자에 공을 하나 두고, 그 공에 다음과 같은 쿼리들을 날리고자 합니다.

- 열 번호가 감소하는 방향으로 `dx`칸 이동하는 쿼리 (`query(0, dx)`)
- 열 번호가 증가하는 방향으로 `dx`칸 이동하는 쿼리 (`query(1, dx)`)
- 행 번호가 감소하는 방향으로 `dx`칸 이동하는 쿼리 (`query(2, dx)`)
- 행 번호가 증가하는 방향으로 `dx`칸 이동하는 쿼리 (`query(3, dx)`)

단, 공은 격자 바깥으로 이동할 수 없으며, 목적지가 격자 바깥인 경우 공은 이동하다가 더 이상 이동할 수 없을 때 멈추게 됩니다. 예를 들어, 5행 × 4열 크기의 격자 내의 공이 3행 2열에 있을 때 `query(3, 10)` 쿼리를 받은 경우 공은 4행 2열에서 멈추게 됩니다. (격자의 크기가 5행 × 4열이므로, 0~4번 행과 0~3번 열로 격자가 구성되기 때문입니다.)

격자의 행의 개수 `n`, 열의 개수 `m`, 정수 `x`와 `y`, 그리고 쿼리들의 목록을 나타내는 2차원 정수 배열 `queries`가 매개변수로 주어집니다. `n × m`개의 가능한 시작점에 대해서 해당 시작점에 공을 두고 `queries` 내의 쿼리들을 순서대로 시뮬레이션했을 때, `x`행 `y`열에 도착하는 시작점의 개수를 return 하도록 solution 함수를 완성해주세요.

# 제한사항
- 1 ≤ `n` ≤ 10 $^9$
- 1 ≤ `m` ≤ 10 $^9$
- 0 ≤ `x` < n
- 0 ≤ `y` < m
- 1 ≤ `queries`의 행의 개수 ≤ 200,000
  - `queries`의 각 행은 `[command,dx]` 두 정수로 이루어져 있습니다.
  - 0 ≤ `command` ≤ 3
  - 1 ≤ `dx` ≤ 10 $^9$
  - 이는 `query(command, dx)`를 의미합니다.
# 입출력 예
|n|m|x|y|queries|result|
|-|-|-|-|-|-|
|2|2|0|0|[[2,1],[0,1],[1,1],[0,1],[2,1]]|4|
|2|5|0|1|[[3,1],[2,2],[1,1],[2,3],[0,1],[2,1]]|2|
# 입출력 예 설명
## 입출력 예 #1

- 다음 애니메이션은 4개의 가능한 시작점에 대한 모든 시뮬레이션을 나타낸 것입니다.

<img src="https://grepp-programmers.s3.amazonaws.com/production/file_resource/101/Ball_ex1.gif">

- 어떤 곳에서 출발하더라도 항상 0행 0열에 도착하기 때문에, 4를 return 해야 합니다.

## 입출력 예 #2

다음 애니메이션은 10개의 가능한 시작점에 대한 모든 시뮬레이션을 나타낸 것입니다.

<img src="https://grepp-programmers.s3.amazonaws.com/production/file_resource/107/Ball_ex2.gif">

- 0행 1열, 1행 1열에서 출발했을 때만 0행 1열에 도착하므로, 2를 return 해야 합니다.

# 내 풀이
```python
# 순방향
dr = [0, 0, -1, 1]
dc = [-1, 1, 0, 0]

# 역방향
rdr = [0, 0, 1, -1]
rdc = [1, -1, 0, 0]


def impossible(r_min, r_max, c_min, c_max, n, m):
    if (r_min<0 and r_max<0) or (r_min>=n and r_max>=n) or (c_min<0 and c_max<0) or (c_min>=m and c_max>=m):
        return True


# 역방향으로 찾아가는 함수
def change_coor(direc, dist, r_min, r_max, c_min, c_max, n, m):
    R, C = rdr[direc]*dist, rdc[direc]*dist
    checking_impossible = False
    if R:
        if R>0:  # 아래 방향으로 역추적
            r_max+=R
            if r_min!=0:
                checking_impossible = True
                r_min+=R
        else:  # 위 방향으로 역추적
            r_min+=R
            if r_max!=n-1: 
                checking_impossible = True
                r_max+=R
    elif C:
        if C>0:  # 오른쪽 방향으로 역추적
            c_max+=C
            if c_min!=0: 
                checking_impossible = True
                c_min+=C
        else:  # 왼쪽 방향으로 역추적
            c_min+=C
            if c_max!=m-1: 
                checking_impossible = True
                c_max+=C
    
    return r_min, r_max, c_min, c_max, checking_impossible

def solution(n, m, x, y, queries):
    # 직사각형 형태
    r_min, r_max, c_min, c_max = x, x, y, y
    for q in queries[::-1]:
        direc, dist = q
        r_min, r_max, c_min, c_max, checking_impossible = change_coor(direc, dist, r_min, r_max, c_min, c_max, n ,m)
        if checking_impossible and impossible(r_min, r_max, c_min, c_max, n, m):
            return 0
        r_min, r_max, c_min, c_max = max(r_min, 0), min(r_max, n-1), max(c_min, 0), min(c_max, m-1)
        
    return (r_max-r_min+1)*(c_max-c_min+1)
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (1.17ms, 10.2MB)
테스트 3 〉	통과 (1.14ms, 10.3MB)
테스트 4 〉	통과 (1.11ms, 10.3MB)
테스트 5 〉	통과 (113.82ms, 21.8MB)
테스트 6 〉	통과 (205.76ms, 34.4MB)
테스트 7 〉	통과 (163.21ms, 28.4MB)
테스트 8 〉	통과 (165.10ms, 28.4MB)
테스트 9 〉	통과 (167.57ms, 29.1MB)
테스트 10 〉	통과 (146.15ms, 26.2MB)
테스트 11 〉	통과 (123.97ms, 23.6MB)
테스트 12 〉	통과 (169.35ms, 28.4MB)
테스트 13 〉	통과 (176.35ms, 28.4MB)
테스트 14 〉	통과 (180.32ms, 29.2MB)
테스트 15 〉	통과 (364.05ms, 34.4MB)
테스트 16 〉	통과 (257.72ms, 34.4MB)
테스트 17 〉	통과 (285.50ms, 34.5MB)
테스트 18 〉	통과 (246.85ms, 34.6MB)
테스트 19 〉	통과 (259.35ms, 34.4MB)
테스트 20 〉	통과 (236.00ms, 34.6MB)
테스트 21 〉	통과 (258.21ms, 34.3MB)
테스트 22 〉	통과 (304.99ms, 34.3MB)
테스트 23 〉	통과 (218.74ms, 34.5MB)
테스트 24 〉	통과 (246.26ms, 34.5MB)
테스트 25 〉	통과 (0.98ms, 10.2MB)
테스트 26 〉	통과 (5.60ms, 10.4MB)
테스트 27 〉	통과 (2.69ms, 10.3MB)
테스트 28 〉	통과 (0.16ms, 10.2MB)
테스트 29 〉	통과 (0.93ms, 10.2MB)
테스트 30 〉	통과 (0.45ms, 10.1MB)
테스트 31 〉	통과 (0.44ms, 10.1MB)
테스트 32 〉	통과 (6.25ms, 10.4MB)
테스트 33 〉	통과 (13.96ms, 33.3MB)
테스트 34 〉	통과 (2.66ms, 22.7MB)
```
- 구현문제
- 전략
  - 모든 칸을 이동시켜서 가능성을 확인하는 것이 베스트이지만, 범위가 $10^9$ 이므로, 격자의 칸은 $log$를 해야만 이용 가능하다.
  - 따라서, 도착점을 기반으로 역추적을 통해 가능한 시작점을 찾아가기로 함
    - 순방향 `dr`와 `dc`외에도, 역방향 `rdr`, `rdc`도 고려
    - 후보 점들은 격자 안에서 직사각형의 영역을 이루기 때문에, 좌상단 좌표`(x_min, y_min)` 및 좌하단 좌표`(x_max, y_max)`를 고려
    - 격자의 edge에 있다가 안쪽 방향으로 이동할 때, 후보 점들의 갯수가 늘어남.
      - 예) 역방향 추적중에, `(X, 0)`에서 왼쪽으로 `k`만큼 이동하면 이동한 만큼 후보가 늘어남 => `(X,0) ~ (X, k)`
    - 위 조건처럼 edge에서 이동하는 것이 아닌데 격자의 범위를 벗어나게 되면, 후보 점들의 갯수가 고정된 채로 격자를 벗어나므로, 그 즉시 가능한 칸이 없어지게됨
  - 마지막으로 후보 영역에서 칸의 갯수를 세면 됨
