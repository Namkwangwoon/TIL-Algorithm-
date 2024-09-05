# 최적의 행렬 곱셈
## 문제 설명
크기가 a by b인 행렬과 크기가 b by c 인 행렬이 있을 때, 두 행렬을 곱하기 위해서는 총 a x b x c 번 곱셈해야합니다.
예를 들어서 크기가 5 by 3인 행렬과 크기가 3 by 2인 행렬을 곱할때는 총 5 x 3 x 2 = 30번의 곱하기 연산을 해야 합니다.

행렬이 2개일 때는 연산 횟수가 일정 하지만, 행렬의 개수가 3개 이상일 때는 연산의 순서에 따라서 곱하기 연산의 횟수가 바뀔 수 있습니다. 예를 들어서 크기가 5 by 3인 행렬 A, 크기가 3 by 10인 행렬 B, 크기가 10 by 6인 행렬 C가 있을 때, 순서대로 A와 B를 먼저 곱하고, 그 결과에 C를 곱하면 A와 B행렬을 곱할 때 150번을, AB 에 C를 곱할 때 300번을 연산을 해서 총 450번의 곱하기 연산을 합니다. 하지만, B와 C를 먼저 곱한 다음 A 와 BC를 곱하면 180 + 90 = 270번 만에 연산이 끝납니다.

각 행렬의 크기 matrix_sizes 가 매개변수로 주어 질 때, 모든 행렬을 곱하기 위한 최소 곱셈 연산의 수를 return하는 solution 함수를 완성해 주세요.

## 제한 사항
- 행렬의 개수는 3이상 200이하의 자연수입니다.
- 각 행렬의 행과 열의 크기는 200이하의 자연수 입니다.
- 계산을 할 수 없는 행렬은 입력으로 주어지지 않습니다.

## 입출력 예
|matrix_sizes|result|
|-|-|
|[[5,3],[3,10],[10,6]]|270|

## 입출력 예 설명
### 입출력 예#1
문제의 예시와 같습니다.

# 내 풀이
```python
def solution(matrix_sizes):
    linked_list = [[None, matrix_sizes[0][0], matrix_sizes[0][1]]]
    
    # Linked list 생성
    for i in range(len(matrix_sizes)-1):
        linked_list.append([matrix_sizes[i][0], matrix_sizes[i][1], matrix_sizes[i+1][1]])
    linked_list.append([matrix_sizes[-1][0], matrix_sizes[-1][1], None])
    
    answer = 0
    # 링크드 리스트 돌며 최대의 key를 제거
    while len(linked_list)>3:
        max_key = 0
        min_oper = float('inf')
        for i in range(1, len(linked_list)-1):
            left, key, right = linked_list[i]
            if max_key<key:
                if max_key==key:
                    if min_oper<=key*left*right: continue
                max_key = key
                min_oper = key*left*right
                target_idx = i
        linked_list[target_idx-1][-1] = right
        linked_list[target_idx+1][0] = left
        linked_list = linked_list[:target_idx] + linked_list[target_idx+1:]
        answer += min_oper
    
    last_l, last_k, last_r = linked_list[1]
    
    return answer + (last_l * last_k * last_r)
```
- 전략
  - 처음에는 잘못된 방법으로 접근했다.
    - 행렬 연산을 하고 나면 가운데 차원은 없어진다는 점을 생각하여, 가운데 차원이 가장 큰 행렬부터 연산하고자 함
    - 링크드 리스트를 통해, 행렬 연산을 하고 나면 두 행렬을 하나의 노드로 만들어주는 방식으로 통합
    - 하지만, 위 방법이 항상 최소가 아니였음
  - 결국 다른 사람의 풀이 참고

# 다른 사람의 풀이 1
```python
import sys
  
def solution(matrix_sizes):
    answer = 0
    L = len(matrix_sizes)
    dp = [[0 for _ in range(L)] for _ in range(L)]
    
    for dist in range(1, L):  # dist는 곱할 두 행렬간의 간격
        for start in range(L - dist):  # start는 행렬곱의 시작 행렬
            a = start
            b = start + dist
            dp[a][b] = sys.maxsize
            for k in range(a, b):
                middle_product = matrix_sizes[a][0] * matrix_sizes[k][1] * matrix_sizes[b][1]
                dp[a][b] = min(dp[a][b], dp[a][k] + middle_product + dp[k + 1][b])
    return dp[0][-1]
```
- 전략
  - 결국 최선의 방법이랑 없고 모든 경우의 수를 봐야함
  - 모든 경우의 수를 곱해보는건 199 * 198 * ... * 1 이기 때문에 당연히 불가능
  - dp를 이용해야 함
    - 거리가 1인 행렬끼리의 최솟값은 유일
    - 거리가 2인 행렬끼리의 최솟값은 2번의 경우의 수에서 최소 연산을 구함
    - 거리가 3인 행렬끼리의 최솟값은 min({ (1 2 3) 4 }, { 1 (2 3 4) })
    - 이런식으로 거리를 조금씩 늘려가면서 최소 연산을 저장하다 보면, 거리가 먼 행렬끼리의 최솟값도 처리 가능
  - dp[i][j] : i번째 행렬부터 j번째 행렬까지의 최소 연산
    - `i==j`이면, 최소 연산은 0
    - `i+1==j`이면, 최소 연산은 i번째 행렬과, j번째 행렬의 연산 횟수
    - dp를 0으로 초기화 한 다음, i와 j사이의 거리가 1부터 최대가 될 때까지 채우면 됨
    - `k`는 중간을 끊어주는 역할
      - 행렬 i~j의 최소 연산 횟수의 후보들 : {i~k까지의 최소연산} + {k+1~j까지의 최소연산} + 행렬(i~k)과 행렬(k+1~j)의 연산 횟수
    - 정답은 전체에 대한 최소 연산이므로 dp[0][-1]

# 내 풀이
```python
def solution(matrix_sizes):
    size = len(matrix_sizes)
    # dp[i][j] : i부터 j까지의 행렬곱 연산의 최소
    # i==j 이면 연산이 없으므로 0
    # i+1==j 이면 최소가 i와 j번째의 연산으로 유일
    dp = [[0] * size for _ in range(size)]
    
    for l in range(1, size):
        for i in range(size-l):
            left, right = i, i+l
            min_count = float('inf')
            for mid in range(left, right):
                count = dp[left][mid] + dp[mid+1][right]
                count += matrix_sizes[left][0] * matrix_sizes[mid][1] * matrix_sizes[right][1]
                min_count = min(min_count, count)
            dp[left][right] = min_count
    
    return dp[0][-1]
```
