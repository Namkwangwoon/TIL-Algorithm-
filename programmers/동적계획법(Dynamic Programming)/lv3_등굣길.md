# 등굣길
## 문제 설명
계속되는 폭우로 일부 지역이 물에 잠겼습니다. 물에 잠기지 않은 지역을 통해 학교를 가려고 합니다. 집에서 학교까지 가는 길은 m x n 크기의 격자모양으로 나타낼 수 있습니다.

아래 그림은 m = 4, n = 3 인 경우입니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/8155b22e-1403-4646-99c2-b0b8c26b7fbe)

가장 왼쪽 위, 즉 집이 있는 곳의 좌표는 (1, 1)로 나타내고 가장 오른쪽 아래, 즉 학교가 있는 곳의 좌표는 (m, n)으로 나타냅니다.

격자의 크기 m, n과 물이 잠긴 지역의 좌표를 담은 2차원 배열 puddles이 매개변수로 주어집니다. 오른쪽과 아래쪽으로만 움직여 집에서 학교까지 갈 수 있는 최단경로의 개수를 1,000,000,007로 나눈 나머지를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 격자의 크기 m, n은 1 이상 100 이하인 자연수입니다.
  - m과 n이 모두 1인 경우는 입력으로 주어지지 않습니다.
- 물에 잠긴 지역은 0개 이상 10개 이하입니다.
- 집과 학교가 물에 잠긴 경우는 입력으로 주어지지 않습니다.

## 입출력 예
|m|n|puddles|return|
|-|-|-|-|
|4|3|[[2, 2]]|4|

## 입출력 예 설명

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/81c8ba44-ab33-406e-8043-6d707730365f)

# 내 풀이 1
```python
def solution(m, n, puddles):
    answer = 0
    
    maps = [[1 if i==0 or j==0 else -1 for i in range(m)] for j in range(n)]
    
    for puddle in puddles:
        if puddle[0]==1:
            for i in range(puddle[1]-1, n):
                maps[i][0]=0
        if puddle[1]==1:
            for i in range(puddle[0]-1, m):
                maps[0][i]=0
        else:
            maps[puddle[1]-1][puddle[0]-1] = 0
                
    
    for i in range(n):
        for j in range(m):
            if maps[i][j]==-1:
                if i!=0 and j!=0:
                    maps[i][j] = (maps[i-1][j] + maps[i][j-1]) % 1000000007
    
    return maps[-1][-1]
```
- 전략
  - 좌표들을 돌면서 최단 경로의 갯수를 칸마다 채워주면 된다. (방향이 왼쪽 & 아래쪽이 되도록 돌아야 함)
  - 모든 칸에 대해서, 좌표 중 하나라도 1값을 포함하고 있으면 1(오직 하나의 경로) 아니면 -1(미방문)을 대입
  - 물이 잠긴 지역은 0을 대입
- **주의할 점!**
  - 좌표 중 하나라도 1값을 포함하는 모서리의 칸들에 대해서, 물이 잠긴 칸이 있으면 그 이후 칸들은 1이 아닌 0이다!!
  - 물론, 0 패딩을 통해서 이 처리를 하지 않을 수도 있다. (내 풀이2)
  - 몇 가지 케이스에서만 계속 오류가 나길래, 자료형 때문인줄 알고 구글링하다가 봐버린 개념,,

# 내 풀이 2
- 0 패딩을 통해서, 크기가 (m+1, n+1)인 지도를 만든 풀이
```python
def solution(m, n, puddles):
    answer = 0
    
    maps = [[0 for i in range(n+1)] for j in range(m+1)]
    
    for puddle in puddles:
        maps[puddle[0]][puddle[1]] = -1
    
    for i in range(1, m+1):
        for j in range(1, n+1):
            if i==1 and j==1: maps[i][j]=1
            elif maps[i][j]==-1: maps[i][j]=0
            else:
                maps[i][j] = (maps[i-1][j] + maps[i][j-1]) % 1000000007
    
    return maps[-1][-1]
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.04ms, 10.1MB)
테스트 4 〉	통과 (0.04ms, 10.3MB)
테스트 5 〉	통과 (0.06ms, 10.2MB)
테스트 6 〉	통과 (0.04ms, 10.3MB)
테스트 7 〉	통과 (0.08ms, 10.2MB)
테스트 8 〉	통과 (0.08ms, 10.4MB)
테스트 9 〉	통과 (0.05ms, 10.2MB)
테스트 10 〉	통과 (0.02ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (2.46ms, 10MB)
테스트 2 〉	통과 (1.11ms, 10.1MB)
테스트 3 〉	통과 (1.09ms, 10.2MB)
테스트 4 〉	통과 (1.99ms, 10.3MB)
테스트 5 〉	통과 (1.72ms, 10.3MB)
테스트 6 〉	통과 (2.48ms, 10.3MB)
테스트 7 〉	통과 (1.15ms, 10.2MB)
테스트 8 〉	통과 (2.05ms, 10.3MB)
테스트 9 〉	통과 (2.14ms, 10.4MB)
테스트 10 〉	통과 (1.70ms, 10.2MB)
```
