# 정수 삼각형
## 문제 설명

![image](https://github.com/user-attachments/assets/c88cb94d-e925-42a2-8346-292a453522ea)

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

# 제한사항
- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

# 입출력 예
|triangle|result|
|-|-|
|[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]]|30|

# 내 풀이 #1
```python
def solution(triangle):
    l = len(triangle)
    if l==1:
        return triangle[0][0]
    new_tri = [triangle[0]]
    
    for i in range(1, l):
        sums = [triangle[i][0]+new_tri[i-1][0]]
        for j in range(1, i):
            sums.append(max(new_tri[i-1][j-1], new_tri[i-1][j])+triangle[i][j])
        sums.append(triangle[i][-1]+new_tri[i-1][-1])
        new_tri.append(sums)
    
    return max(new_tri[-1])
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	통과 (0.03ms, 10.2MB)
테스트 3 〉	통과 (0.05ms, 10.3MB)
테스트 4 〉	통과 (0.31ms, 10.2MB)
테스트 5 〉	통과 (1.17ms, 10.5MB)
테스트 6 〉	통과 (0.35ms, 10.2MB)
테스트 7 〉	통과 (1.17ms, 10.3MB)
테스트 8 〉	통과 (0.26ms, 10.2MB)
테스트 9 〉	통과 (0.02ms, 10.2MB)
테스트 10 〉	통과 (0.15ms, 10.2MB)
```
효율성  테스트
```
테스트 1 〉	통과 (37.21ms, 17.8MB)
테스트 2 〉	통과 (28.24ms, 16.2MB)
테스트 3 〉	통과 (41.03ms, 18.9MB)
테스트 4 〉	통과 (37.54ms, 17.9MB)
테스트 5 〉	통과 (35.13ms, 17.3MB)
테스트 6 〉	통과 (43.30ms, 19MB)
테스트 7 〉	통과 (40.66ms, 18.6MB)
테스트 8 〉	통과 (32.93ms, 16.9MB)
테스트 9 〉	통과 (34.93ms, 17.5MB)
테스트 10 〉	통과 (41.19ms, 18.5MB)
```
# 내 풀이 #2
```python
def solution(triangle):
    pre_line = triangle[0]
    max_val = 0
    for l, line in enumerate(triangle[1:]):
        l+=2
        new_line = []
        
        new_line.append(pre_line[0]+line[0])
        for i in range(1, l-1):
            sums = line[i]+max(pre_line[i-1], pre_line[i])
            if max_val<sums:
                max_val = sums
            new_line.append(sums)
        new_line.append(pre_line[-1]+line[-1])
        pre_line = new_line
    
    return max_val
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 9.36MB)
테스트 2 〉	통과 (0.02ms, 9.28MB)
테스트 3 〉	통과 (0.04ms, 9.22MB)
테스트 4 〉	통과 (0.24ms, 9.29MB)
테스트 5 〉	통과 (1.73ms, 9.33MB)
테스트 6 〉	통과 (0.28ms, 9.29MB)
테스트 7 〉	통과 (1.91ms, 9.33MB)
테스트 8 〉	통과 (0.40ms, 9.23MB)
테스트 9 〉	통과 (0.02ms, 9.22MB)
테스트 10 〉	통과 (0.24ms, 9.28MB)
```
효율성  테스트
```
테스트 1 〉	통과 (27.11ms, 13.2MB)
테스트 2 〉	통과 (20.98ms, 12.2MB)
테스트 3 〉	통과 (30.39ms, 13.7MB)
테스트 4 〉	통과 (30.44ms, 13.3MB)
테스트 5 〉	통과 (25.33ms, 12.9MB)
테스트 6 〉	통과 (31.20ms, 13.9MB)
테스트 7 〉	통과 (28.97ms, 13.5MB)
테스트 8 〉	통과 (27.05ms, 12.7MB)
테스트 9 〉	통과 (25.62ms, 12.9MB)
테스트 10 〉	통과 (30.61ms, 13.8MB)
```
