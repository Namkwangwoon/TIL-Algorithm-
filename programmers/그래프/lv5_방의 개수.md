# 방의 개수
## 문제 설명
원점(0,0)에서 시작해서 아래처럼 숫자가 적힌 방향으로 이동하며 선을 긋습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/54b766aa-7f64-4734-8c52-1bdc1c933280)

ex) 1일때는 `오른쪽 위`로 이동

그림을 그릴 때, 사방이 막히면 방하나로 샙니다.

이동하는 방향이 담긴 배열 arrows가 매개변수로 주어질 때, 방의 갯수를 return 하도록 solution 함수를 작성하세요.

## 제한사항
- 배열 arrows의 크기는 1 이상 100,000 이하 입니다.
- arrows의 원소는 0 이상 7 이하 입니다.
- 방은 다른 방으로 둘러 싸여질 수 있습니다.

## 입출력 예
|arrows|return|
|-|-|
|[6, 6, 6, 4, 4, 4, 2, 2, 2, 0, 0, 0, 1, 6, 5, 5, 3, 6, 0]|3|

## 입출력 예 설명
![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/91e08446-0086-4176-bb32-c20477cc25b0)

- (0,0) 부터 시작해서 6(왼쪽) 으로 3번 이동합니다. 그 이후 주어진 arrows 를 따라 그립니다.
- 삼각형 (1), 큰 사각형(1), 평행사변형(1) = 3

# 다른 사람의 풀이
```python
from collections import defaultdict
def solution(arrows):
    answer = 0
    visit = defaultdict(list)
    x,y = 0,0
    dx,dy = [0,1,1,1,0,-1,-1,-1],[1,1,0,-1,-1,-1,0,1]

    for arrow in arrows:
        for _ in range(2):              # 대각선 판별을 위해 2씩
            nx = x + dx[arrow]
            ny = y + dy[arrow]  
            if (nx,ny) in visit and (x,y) not in visit[(nx,ny)]:    # 방문했던 점이지만 경로가 겹치지 않는 경우, 방+1
                answer += 1
                visit[(x,y)].append((nx,ny))
                visit[(nx,ny)].append((x,y))
            elif (nx,ny) not in visit:                  # 방문하지 않았던 경우
                visit[(x,y)].append((nx,ny))            # 경로가 겹치는 경우는 따로 해줄 필요 없음
                visit[(nx,ny)].append((x,y))
            x,y = nx, ny        # 이동
    return answer
```
- 방의 개수를 세는 조건은 비슷하게 구현했었다.
  - 기존에 이동한 점을 다시 방문하되, 이전에 방문했던 경로와 달라야 함
- 하지만, 대각선으로 만나게되는 조건을 생각 못했다. (여기서 갈린듯 싶다..)
  - 대각선 조건을 충족시키기 위해서는, 한번에 이동 거리를 2씩 하여, `x.5`에서 만나는 좌표를 `y.0`에서 만나도록 소수점을 없애줌
- 이 풀이에서 방문했던 점을 체크하는 코드도 주목할만함
  - 내 풀이에서는, 방문한 점 dict + 그 점에서 다른 점으로 이동할 때의 방향을 체크했었음
  - 이 풀이에서는, "[방문한 점] : 다음 방문할 점", "[다음 발문할 점] : 방문한 점" 을 갖는 dict 형태의 그래프를 만듦
    - key들은 방문한 점들이고, value들은 key와 이어진 점들을 나타내어 방향을 간접적으로 나타냄
