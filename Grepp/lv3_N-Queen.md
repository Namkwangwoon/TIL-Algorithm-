# N-Queen
## 문제 설명
가로, 세로 길이가 n인 정사각형으로된 체스판이 있습니다. 체스판 위의 n개의 퀸이 서로를 공격할 수 없도록 배치하고 싶습니다.

예를 들어서 n이 4인경우 다음과 같이 퀸을 배치하면 n개의 퀸은 서로를 한번에 공격 할 수 없습니다.

![image](https://user-images.githubusercontent.com/19163372/118607717-d8f80180-b7f3-11eb-9c4e-7fa18302ba30.png)

![image](https://user-images.githubusercontent.com/19163372/118607727-dc8b8880-b7f3-11eb-9034-374ce36b7962.png)

체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.

## 제한사항
- 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
- n은 12이하의 자연수 입니다.
## 입출력 예
|n|result|
|---|---|
|4|2|
## 입출력 예 설명
### 입출력 예 #1
문제의 예시와 같습니다.
# 풀이
예전에 강의에서 멘토분께서 문제에 대한 간략한 설명과 풀이를 해줬었다.

문제를 dfs로 풀어야겠다는 생각은 했었지만, 도저히 방법이 떠오르지 않아서 멘토분의 풀이를 주석과 함께 이해하면서 참고했다.
```python
# row를 순차적으로 돌면서 가능한 조합의 갯수 check!
def nqueen(lst, row, n):
    count = 0
    if row==n:  # 가능한 경우 (행의 끝에 도달한 경우)
        return 1
    for col in range(n):
        lst[row] = col
        for i in range(row):    # 이전 row들에서
            # 같은 열에 다른 퀸이 존재하는지 check
            if col==lst[i]:
                break
            # 대각선에 다른 퀸이 존재하는지 check
            if i+lst[i]==row+lst[row] or i-lst[i]==row-lst[row]:
                break
        else:
            count += nqueen(lst, row+1, n)
    return count
            
def solution(n):
    
    return nqueen([-1]*n, 0, n)
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.1MB)
테스트 3 〉	통과 (0.02ms, 10.2MB)
테스트 4 〉	통과 (0.19ms, 10.1MB)
테스트 5 〉	통과 (0.85ms, 10.2MB)
테스트 6 〉	통과 (2.66ms, 10.3MB)
테스트 7 〉	통과 (10.92ms, 10.3MB)
테스트 8 〉	통과 (54.35ms, 10.2MB)
테스트 9 〉	통과 (284.43ms, 10.3MB)
테스트 10 〉	통과 (1579.56ms, 10.2MB)
테스트 11 〉	통과 (8955.58ms, 10.2MB)
```
- dfs를 함수에 대한 재귀호출 방식으로 구현했다. (n=12로 제한되었기 때문에 가능)
- 재귀호출의 과정중 if문을 통해 퀸을 놓을 수 없는 위치에 대해서는 back tracking 방법을 사용해서, dfs시 이후에 방문을 하지 않아도 되는 노드는 방문하지 않도록 했다.
- lst 리스트를 각 행에 놓은 퀸의 열을 넣음으로, 같은 행에 두 개의 퀸이 들어가는 경우를 배제했다.
