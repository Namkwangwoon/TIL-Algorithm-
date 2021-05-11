# 방문 길이
## 문제 설명
게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

- U: 위쪽으로 한 칸 가기

- D: 아래쪽으로 한 칸 가기

- R: 오른쪽으로 한 칸 가기

- L: 왼쪽으로 한 칸 가기

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.

![image](https://user-images.githubusercontent.com/19163372/117781295-71820500-b27b-11eb-960a-bf6b3ac0eb61.png)

예를 들어, "ULURRDLLU"로 명령했다면

![image](https://user-images.githubusercontent.com/19163372/117781310-75ae2280-b27b-11eb-96e7-063701895f14.png)

- 1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

![image](https://user-images.githubusercontent.com/19163372/117781318-79da4000-b27b-11eb-8b64-238b8ea3ac71.png)

- 8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

![image](https://user-images.githubusercontent.com/19163372/117781329-7cd53080-b27b-11eb-8515-ec0142444018.png)

이때, 우리는 게임 캐릭터가 지나간 길 중 **캐릭터가 처음 걸어본 길의 길이**를 구하려고 합니다. 예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

예를 들어, "LULLLLLLU"로 명령했다면

![image](https://user-images.githubusercontent.com/19163372/117781355-852d6b80-b27b-11eb-8c8b-6210a61cc26c.png)

- 1번 명령어부터 6번 명령어대로 움직인 후, 7, 8번 명령어는 무시합니다. 다시 9번 명령어대로 움직입니다.

![image](https://user-images.githubusercontent.com/19163372/117781361-88285c00-b27b-11eb-922c-e5cdd4cb6797.png)

이때 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다.

명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.

## 제한사항
- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.
## 입출력 예
|dirs|answer|
|---|---|
|"ULURRDLLU"|7|
|"LULLLLLLU"|7|
## 입출력 예 설명
### 입출력 예 #1
문제의 예시와 같습니다.

### 입출력 예 #2
문제의 예시와 같습니다.
# 풀이
1. 좌표들을 돌면서 방문을 체크한 결과로 해보고자 하였지만, 이미 방문한 좌표를 다시 지나더라도 지나간 길에는 겹치지 않는 경우가 있어 실패했다.
> ex. UULDRR 실행시 방문했던 좌표를 지나지만, 방문한 경로는 겹치지 않는다.
2. (좌표 + 방향) 으로 이루어진 노드들의 집합으로 중복을 제거하면서 저장하려고 하였지만, 방향성이 생겨버리므로 값이 다르다.
> ex. ( (0,1),D )와 ( (0,0),U )은 같다.

그래서 생각한 것은 두 좌표를 포함하는 set 원소로 같는 노드들로 중복을 체크하면서 저장하는 방법이다. (두 점을 잇는 것이 그 사이의 길이라는 생각)
```python
def solution(dirs):
    ways = []
    pos = [0, 0]
    for d in dirs:
        new_pos = pos.copy()
        if d=='U' and pos[1]+1<=5:
            new_pos[1]+=1
        elif d=='D' and pos[1]-1>=-5:
            new_pos[1]-=1
        elif d=='L' and pos[0]-1>=-5:
            new_pos[0]-=1
        elif d=='R' and pos[0]+1<=5:
            new_pos[0]+=1
        way = set([tuple(pos),tuple(new_pos)])
        if pos!=new_pos and way not in ways:
            ways.append(way)
        pos = new_pos.copy()
    
    return len(ways)
```
정확성 테스트
```
테스트 1 〉	통과 (0.10ms, 10.2MB)
테스트 2 〉	통과 (0.01ms, 10.3MB)
테스트 3 〉	통과 (0.02ms, 10.3MB)
테스트 4 〉	통과 (0.43ms, 10.3MB)
테스트 5 〉	통과 (0.40ms, 10.4MB)
테스트 6 〉	통과 (0.15ms, 10.2MB)
테스트 7 〉	통과 (0.05ms, 10.2MB)
테스트 8 〉	통과 (0.10ms, 10.3MB)
테스트 9 〉	통과 (0.08ms, 10.2MB)
테스트 10 〉	통과 (0.09ms, 10.3MB)
테스트 11 〉	통과 (0.10ms, 10.2MB)
테스트 12 〉	통과 (0.28ms, 10.2MB)
테스트 13 〉	통과 (0.24ms, 10.3MB)
테스트 14 〉	통과 (0.25ms, 10.2MB)
테스트 15 〉	통과 (0.21ms, 10.2MB)
테스트 16 〉	통과 (1.52ms, 10.3MB)
테스트 17 〉	통과 (1.60ms, 10.2MB)
테스트 18 〉	통과 (1.57ms, 10.3MB)
테스트 19 〉	통과 (1.53ms, 10.2MB)
테스트 20 〉	통과 (1.65ms, 10.2MB)
```
# 해답
```python
def solution(dirs):
    x, y = 0, 0 # 시작 좌표를 0, 0으로 지정
    map = dict() # 좌표를 키로 사용하는 해시 생성
    for command in dirs: # O(dirs)
        if command == 'U' and y < 5: # 위로
            map[(x, y, x, y+1)] = True # 현재 좌표, 이동할 좌표
            # x, y 좌표 작은게 왼쪽으로~
            y += 1
        elif command == 'D' and y > -5: # 아래로
            map[(x, y-1, x, y)] = True # 이동할 좌표, 현재 좌표
            y -= 1
        elif command == 'R' and x < 5: # 오른쪽으로
            map[(x, y, x+1, y)] = True # 현재 좌표, 이동할 좌표
            x += 1
        elif command == 'L' and x > -5: # 왼쪽으로
            map[(x-1, y, x, y)] = True # 이동할 좌표, 현재 좌표.
            x -= 1
    
    return len(map) # 추가된 값들이 곧 방문 길이
```
- 딕셔너리를 이용해 key가 방문한 길이고 value가 True를 갖도록 했다.
- 두 좌표로 길을 표현한 것은 같지만, (x,y)를 값의 크기가 작은 것이 앞에 위치하도록 하는 간단한 방법으로, 풀이의 2번에서 생겼던 문제점인 방향성을 제거했다.
