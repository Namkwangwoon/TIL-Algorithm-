# 자물쇠와 열쇠
## 문제 설명
고고학자인 "튜브"는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 `1 x 1`인 `N x N` 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 `M x M` 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
  - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

## 입출력 예
|key|lock|result|
|-|-|-|
|[[0, 0, 0], [1, 0, 0], [0, 1, 1]]|[[1, 1, 1], [1, 1, 0], [1, 0, 1]]|true|

## 입출력 예에 대한 설명

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/c7a8098c-ae0e-4085-a983-46ce1d970cd5)

key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.

# 내 풀이
```python
def rotate(a):
    rot_a = [a]
    I, J = len(a), len(a[0])
    K = I+J
    
    for _ in range(3):
        I, J = K-I, K-J
        new_mat = [[0]*J for _ in range(I)]
        ori_mat = rot_a[-1]
        for i in range(I):
            for j in range(J):
                new_mat[i][j] = ori_mat[j][I-i-1]
        rot_a.append(new_mat)
        
    return rot_a


def check(i, j, M, key, new_lock):
    I, J = len(new_lock), len(new_lock[0])
    for a in range(I):
        for b in range(J):
            if new_lock[a][b]!=key[i+a][j+b]:
                return False
    return True
    

def solution(key, lock):
    M, N = len(key), len(lock)
    lock_i, lock_j, lock_I, lock_J = N, N, 0, 0
    is_zero=False
    for i in range(N):
        for j in range(N):
            if lock[i][j]==0:
                lock_i = min(lock_i, i)
                lock_j = min(lock_j, j)
                lock_I = max(lock_I, i)
                lock_J = max(lock_J, j)
                is_zero=True
                
    if not is_zero: return True
    
    if lock_I-lock_i+1>M or lock_J-lock_j+1>M: return False

    new_lock = [[0] * (lock_J-lock_j+1) for _ in range(lock_I-lock_i+1)]
    for i in range(lock_I-lock_i+1):
        for j in range(lock_J-lock_j+1):
            new_lock[i][j]=1-lock[lock_i+i][lock_j+j]
    
    for l in rotate(new_lock):
        l_i, l_j = len(l), len(l[0])
        for i in range(M-l_i+1):
            for j in range(M-l_j+1):
                if check(i, j, M, key, l):
                    return True
    return False
```
정확성  테스트
```
테스트 1 〉	통과 (0.04ms, 10.4MB)
테스트 2 〉	통과 (0.01ms, 10.4MB)
테스트 3 〉	통과 (0.11ms, 10.3MB)
테스트 4 〉	통과 (0.01ms, 10.4MB)
테스트 5 〉	통과 (0.19ms, 10.3MB)
테스트 6 〉	통과 (0.04ms, 10.2MB)
테스트 7 〉	통과 (0.08ms, 10.3MB)
테스트 8 〉	통과 (0.12ms, 10.4MB)
테스트 9 〉	통과 (0.27ms, 10.4MB)
테스트 10 〉	통과 (0.38ms, 10.3MB)
테스트 11 〉	통과 (0.29ms, 10.4MB)
테스트 12 〉	통과 (0.01ms, 10.2MB)
테스트 13 〉	통과 (0.10ms, 10.3MB)
테스트 14 〉	통과 (0.06ms, 10.2MB)
테스트 15 〉	통과 (0.04ms, 10.3MB)
테스트 16 〉	통과 (0.14ms, 10.4MB)
테스트 17 〉	통과 (0.09ms, 10.4MB)
테스트 18 〉	통과 (0.08ms, 10.4MB)
테스트 19 〉	통과 (0.05ms, 10.3MB)
테스트 20 〉	통과 (0.13ms, 10.3MB)
테스트 21 〉	통과 (0.33ms, 10.4MB)
테스트 22 〉	통과 (0.13ms, 10.2MB)
테스트 23 〉	통과 (0.05ms, 10.4MB)
테스트 24 〉	통과 (0.08ms, 10.3MB)
테스트 25 〉	통과 (0.11ms, 10.3MB)
테스트 26 〉	통과 (0.16ms, 10.3MB)
테스트 27 〉	통과 (0.25ms, 10.3MB)
테스트 28 〉	통과 (0.14ms, 10.2MB)
테스트 29 〉	통과 (0.01ms, 10.4MB)
테스트 30 〉	통과 (0.11ms, 10.4MB)
테스트 31 〉	통과 (0.04ms, 10.4MB)
테스트 32 〉	통과 (0.07ms, 10.3MB)
테스트 33 〉	통과 (0.13ms, 10.3MB)
테스트 34 〉	통과 (0.04ms, 10.4MB)
테스트 35 〉	통과 (0.08ms, 10.3MB)
테스트 36 〉	통과 (0.07ms, 10.2MB)
테스트 37 〉	통과 (0.09ms, 10.5MB)
테스트 38 〉	통과 (0.04ms, 10.3MB)
```
- 키와 자물쇠의 크기가 20보다 작기 때문에, 완전 탐색이 가능할 것이라 생각했다.
- 그렇게 되면 결국은 구현 문제
- 전략
  - `key`의 전체를 쓰든 일부를 쓰든 `lock`의 전체가 1로 채워지면 된다.
  - 그러므로 `lock`에서 0이 있는 범위의 직사각형만 잘라 비교하기로 했다.
    - 이 때 주의할 점은, `lock`에 0이 없으면 이미 자물쇠가 다 채워진 것이므로 그대로 `True`를 리턴하면 된다. (여기에서 시간을 좀 잡아먹음..)
  - `lock`을 잘랐을 때, `key`의 가로 세로 중 하나라도 자른 `lock`보다 작으면 결국 빈공간을 채우지 못하므로 `False` 리턴
  - `lock`을 자르게 되면, 그 부분들을 `key`가 모두 채워야 하므로, 자른 `lock`의 전체 부분이 `key` 안쪽에 포함되는 경우만 생각하면 된다.
  - 90도로 세 번 회전한 결과를 리스트로 리턴해주는 `rotate()`와 시작점 `(i, j)`로 부터 `key`와 잘린 `lock`이 맞는지를 리턴해주는 `check()`으로 확인
