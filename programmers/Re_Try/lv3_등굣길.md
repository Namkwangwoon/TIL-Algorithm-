# 등굣길
## 문제 설명
계속되는 폭우로 일부 지역이 물에 잠겼습니다. 물에 잠기지 않은 지역을 통해 학교를 가려고 합니다. 집에서 학교까지 가는 길은 m x n 크기의 격자모양으로 나타낼 수 있습니다.

아래 그림은 m = 4, n = 3 인 경우입니다.

![image](https://github.com/user-attachments/assets/ecd58ee3-7b4a-48cd-9262-e3ab5e2edfa8)

가장 왼쪽 위, 즉 집이 있는 곳의 좌표는 (1, 1)로 나타내고 가장 오른쪽 아래, 즉 학교가 있는 곳의 좌표는 (m, n)으로 나타냅니다.

격자의 크기 m, n과 물이 잠긴 지역의 좌표를 담은 2차원 배열 puddles이 매개변수로 주어집니다. **오른쪽과 아래쪽으로만 움직여** 집에서 학교까지 갈 수 있는 최단경로의 개수를 1,000,000,007로 나눈 나머지를 return 하도록 solution 함수를 작성해주세요.

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
![image](https://github.com/user-attachments/assets/b1f5c398-4464-4b27-b138-58e77f7506c3)

# 내 풀이
```python
def solution(m, n, puddles):
    maps = [[1]*(m+1) for _ in range(n+1)]
    puddle_x_min, puddle_y_min = 101, 101
    
    for puddle_x, puddle_y in puddles:
        if puddle_x==1:
            puddle_y_min = min(puddle_y_min, puddle_y)
        if puddle_y==1:
            puddle_x_min = min(puddle_x_min, puddle_x)
        maps[puddle_y][puddle_x] = 0
    
    for x in range(puddle_x_min, m+1):
        maps[1][x] = 0
    for y in range(puddle_y_min, n+1):
        maps[y][1] = 0
        
    for y in range(2, n+1):
        for x in range(2, m+1):
            if maps[y][x]!=0:
                maps[y][x] = (maps[y-1][x] + maps[y][x-1]) % 1000000007
    
    return maps[-1][-1]
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.2MB)
테스트 3 〉	통과 (0.02ms, 10.1MB)
테스트 4 〉	통과 (0.03ms, 10.3MB)
테스트 5 〉	통과 (0.04ms, 10.2MB)
테스트 6 〉	통과 (0.03ms, 10.2MB)
테스트 7 〉	통과 (0.03ms, 10.3MB)
테스트 8 〉	통과 (0.06ms, 10.2MB)
테스트 9 〉	통과 (0.03ms, 10.3MB)
테스트 10 〉	통과 (0.02ms, 10.3MB)
```
효율성  테스트
```
테스트 1 〉	통과 (2.22ms, 10.4MB)
테스트 2 〉	통과 (0.96ms, 10.3MB)
테스트 3 〉	통과 (1.25ms, 10.2MB)
테스트 4 〉	통과 (1.39ms, 10.3MB)
테스트 5 〉	통과 (1.11ms, 10.3MB)
테스트 6 〉	통과 (1.94ms, 10.2MB)
테스트 7 〉	통과 (1.22ms, 10.2MB)
테스트 8 〉	통과 (1.61ms, 10.2MB)
테스트 9 〉	통과 (1.68ms, 10.3MB)
테스트 10 〉	통과 (1.69ms, 10.2MB)
```
