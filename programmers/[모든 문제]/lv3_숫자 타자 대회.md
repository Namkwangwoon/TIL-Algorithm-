# 숫자 타자 대회
## 문제 설명

![image](https://github.com/user-attachments/assets/795a6270-2b16-4ece-9aae-b4f3af37730d)

위와 같은 모양으로 배열된 숫자 자판이 있습니다. 숫자 타자 대회는 이 동일한 자판을 사용하여 숫자로만 이루어진 긴 문자열을 누가 가장 빠르게 타이핑하는지 겨루는 대회입니다.

대회에 참가하려는 민희는 두 엄지 손가락을 이용하여 타이핑을 합니다. 민희는 항상 왼손 엄지를 4 위에, 오른손 엄지를 6 위에 두고 타이핑을 시작합니다. 엄지 손가락을 움직여 다음 숫자를 누르는 데에는 일정 시간이 듭니다. 민희는 어떤 두 숫자를 연속으로 입력하는 시간 비용을 몇몇 가중치로 분류하였습니다.

- 이동하지 않고 제자리에서 다시 누르는 것은 가중치가 1입니다.
- 상하좌우로 인접한 숫자로 이동하여 누르는 것은 가중치가 2입니다.
- 대각선으로 인접한 숫자로 이동하여 누르는 것은 가중치가 3입니다.
- 같지 않고 인접하지 않은 숫자를 누를 때는 위 규칙에 따라 가중치 합이 최소가 되는 경로를 따릅니다.

예를 들어 1 위에 있던 손가락을 0 으로 이동하여 누르는 것은 2 + 2 + 3 = 7 만큼의 가중치를 갖습니다.

단, 숫자 자판은 버튼의 크기가 작기 때문에 같은 숫자 버튼 위에 동시에 두 엄지 손가락을 올려놓을 수 없습니다. 즉, 어떤 숫자를 눌러야 할 차례에 그 숫자 위에 올려져 있는 손가락이 있다면 반드시 그 손가락으로 눌러야 합니다.

숫자로 이루어진 문자열 `numbers`가 주어졌을 때 최소한의 시간으로 타이핑을 하는 경우의 가중치 합을 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 1 ≤ `numbers`의 길이 ≤ 100,000
  - `numbers`는 아라비아 숫자로만 이루어진 문자열입니다.

## 입출력 예
|numbers|result|
|-|-|
|"1756"|10|
|"5123"|8|

## 입출력 예 설명
### 입출력 예 #1
왼손 엄지로 17, 오른손 엄지로 56을 누르면 가중치 10으로 최소입니다.

### 입출력 예 #2
오른손 엄지로 5, 왼손 엄지로 123을 누르거나 오른손 엄지로 5, 왼손 엄지로 1, 오른손 엄지로 23을 누르면 가중치 8로 최소입니다.

# 내 풀이1
```python
import sys
sys.setrecursionlimit(100000)

num_coor = [(3, 1)] + [(i, j) for i in range(3) for j in range(3)]
    
def get_dist(number, left, right):
    left_x, left_y = num_coor[left]
    right_x, right_y = num_coor[right]
    new_x, new_y = num_coor[number]
    distance = []
    
    # Press with Left
    x_diff, y_diff = abs(left_x-new_x), abs(left_y-new_y)
    min_diff = min(x_diff, y_diff)
    diff = max(x_diff, y_diff) - min_diff
    l_dist = max(1, min_diff*3 + diff*2)
    
    # Press with Right
    x_diff, y_diff = abs(right_x-new_x), abs(right_y-new_y)
    min_diff = min(x_diff, y_diff)
    diff = max(x_diff, y_diff) - min_diff
    r_dist = max(1, min_diff*3 + diff*2)
    
    return l_dist, r_dist

def dfs(numbers, distance, ind, left, right):
    answer = set()
    left_x, left_y = num_coor[left]
    right_x, right_y = num_coor[right]
    num = int(numbers[ind])
    new_x, new_y = num_coor[num]
    
    # Press with Left
    x_diff, y_diff = abs(left_x-new_x), abs(left_y-new_y)
    min_diff = min(x_diff, y_diff)
    diff = max(x_diff, y_diff) - min_diff
    l_dist = max(1, min_diff*3 + diff*2)
    if ind!=len(numbers)-1:
        answer.update(dfs(numbers, distance+l_dist, ind+1, num, right))
    else:
        answer.add(distance+l_dist)
    
    # Press with Right
    x_diff, y_diff = abs(right_x-new_x), abs(right_y-new_y)
    min_diff = min(x_diff, y_diff)
    diff = max(x_diff, y_diff) - min_diff
    r_dist = max(1, min_diff*3 + diff*2)
    if ind!=len(numbers)-1:
        answer.update(dfs(numbers, distance+r_dist, ind+1, left, num))
    else:
        answer.add(distance+r_dist)
    
    return answer


def solution(numbers):
    l = len(numbers)
    
    # distance, ind, left, right
    return min(dfs(numbers, 0, 0, 4, 6))
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.3MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.03ms, 10.4MB)
테스트 4 〉	통과 (0.05ms, 10.6MB)
테스트 5 〉	통과 (0.05ms, 10.4MB)
테스트 6 〉	실패 (0.21ms, 10.4MB)
테스트 7 〉	실패 (0.19ms, 10.5MB)
테스트 8 〉	실패 (0.27ms, 10.6MB)
테스트 9 〉	실패 (0.18ms, 10.4MB)
테스트 10 〉	실패 (0.17ms, 10.5MB)
테스트 11 〉	통과 (2283.91ms, 10.4MB)
테스트 12 〉	통과 (2197.86ms, 10.4MB)
테스트 13 〉	통과 (2081.14ms, 10.4MB)
테스트 14 〉	통과 (2028.04ms, 10.5MB)
테스트 15 〉	통과 (1881.70ms, 10.3MB)
테스트 16 〉	실패 (시간 초과)
테스트 17 〉	실패 (시간 초과)
테스트 18 〉	실패 (시간 초과)
테스트 19 〉	실패 (시간 초과)
테스트 20 〉	실패 (런타임 에러)
```
- 최소의 경로를 찾아가는 것은 불가능하다고 판단하여, dfs로 모든 경우를 보려고 함
- 매 숫자마다, 왼손으로 누르는 경우와 오른손으로 누르는 경우를 카운트
- $2^{100,000}$의 경우의수가 발생하므로, 당연히 시간초과가 발생했다.
- 각 숫자를 누를때 마다, (1) 다른 숫자로부터 왔지만 누르고 난 이후의 (왼손, 오른손) 위치가 같은 경우의 수는 중복을 제거해 줘야 하고, (2) 누르고자 하는 수가 특정 손가락 위에 있으면 그 손가락으로 눌러서 왼손-오른손이 겹치는 경우를 제외해야 함

# 다른 사람의 풀이 1
```python
costs =     [[1, 7, 6, 7, 5, 4, 5, 3, 2, 3]
            ,[7, 1, 2, 4, 2, 3, 5, 4, 5, 6]
            ,[6, 2, 1, 2, 3, 2, 3, 5, 4, 5]
            ,[7, 4, 2, 1, 5, 3, 2, 6, 5, 4]
            ,[5, 2, 3, 5, 1, 2, 4, 2, 3, 5]
            ,[4, 3, 2, 3, 2, 1, 2, 3, 2, 3]
            ,[5, 5, 3, 2, 4, 2, 1, 5, 3, 2]
            ,[3, 4, 5, 6, 2, 3, 5, 1, 2, 4]
            ,[2, 5, 4, 5, 3, 2, 3, 2, 1, 2]
            ,[3, 6, 5, 4, 5, 3, 2, 4, 2, 1]]

def solution(numbers):
    now_weight = 0
    left_pos = 4
    right_pos = 6
    all_dict = {}
    finger_pos = (left_pos, right_pos)
    all_dict[finger_pos] = now_weight
    
    for str_num in numbers:
        num = int(str_num)
        curr_dict = {}
        for finger_pos, weight in all_dict.items():
            left_pos, right_pos = finger_pos
            if right_pos == num:
                if not (left_pos, num) in curr_dict.keys() or curr_dict[(left_pos, num)] > weight + 1:
                    curr_dict[(left_pos, num)] = weight + 1
            elif left_pos == num:
                if not (num, right_pos) in curr_dict.keys() or curr_dict[(num, right_pos)] > weight + 1:
                    curr_dict[(num, right_pos)] = weight + 1
            else:
                if not (left_pos, num) in curr_dict.keys() or curr_dict[(left_pos, num)] > weight + costs[right_pos][num]:
                    curr_dict[(left_pos, num)] = weight + costs[right_pos][num]
                if not (num, right_pos) in curr_dict.keys() or curr_dict[(num, right_pos)] > weight + costs[left_pos][num]:
                    curr_dict[(num, right_pos)] = weight + costs[left_pos][num]
        all_dict = curr_dict

    return min(list(all_dict.values()))
```
- 위에서 말했던, 내가 빠뜨린 두 가지 방법을 모두 적용
- 추가로 숫자가 0~9로 몇 개 없으니, `num1`으로부터 `num2`까지의 소요 weight를 배열로 저장
  - `costs[i][j]`: `i`부터 `j`까지 소요 weight
- 각 숫자를 누를 때마다, 손가락이 존재할 수 있는 모든 위치를 매번 업데이트

# 내 풀이 2
```python
## weight[i][j]는 번호 i에서 번호 j까지 가는데 드는 weight
weights = [[1, 7, 6, 7, 5, 4, 5, 3, 2, 3], # 0
           [7, 1, 2, 4, 2, 3, 5, 4, 5, 6], # 1
           [6, 2, 1, 2, 3, 2, 3, 5, 4, 5], # 2
           [7, 4, 2, 1, 5, 3, 2, 6, 5, 4], # 3
           [5, 2, 3, 5, 1, 2, 4, 2, 3, 5], # 4
           [4, 3, 2, 3, 2, 1, 2, 3, 2, 3], # 5
           [5, 5, 3, 2, 4, 2, 1, 5, 3, 2], # 6
           [3, 4, 5, 6, 2, 3, 5, 1, 2, 4], # 7
           [2, 5, 4, 5, 3, 2, 3, 2, 1, 2], # 8
           [3, 6, 5, 4, 5, 3, 2, 4, 2, 1]] # 9 

def solution(numbers):
    save = {}
    save[(4,6)]=0
    
    for number in numbers:
        num = int(number)
        new_save = {}
        for cur in save:
            left, right = cur
            cur_weight = save[cur]
            
            if num==left or num==right:  # 숫자 위에 손가락이 있으면 그 손가락으로 누름
                new_weight = cur_weight+1
                if (left, right) in new_save:
                    new_weight = min(new_weight, new_save[(left, right)])
                new_save[(left, right)] = new_weight
            else:  # 숫자가 왼손 오른손 모두 아니면, 두 손가락으로 누르는 경우 모두 고려
                # 왼손으로 누름
                new_weight = cur_weight+weights[left][num]
                if (num, right) in new_save:
                    new_weight = min(new_weight, new_save[(num, right)])
                new_save[(num, right)] = new_weight
                
                # 오른손으로 누름
                new_weight = cur_weight+weights[right][num]
                if (left, num) in new_save:
                    new_weight = min(new_weight, new_save[(left, num)])
                new_save[(left, num)] = new_weight
        save = new_save
        
    return min(save.values())
```
정확성  테스트
```
테스트 1 〉	통과 (0.02ms, 10.4MB)
테스트 2 〉	통과 (0.02ms, 10.4MB)
테스트 3 〉	통과 (0.02ms, 10.4MB)
테스트 4 〉	통과 (0.02ms, 10.4MB)
테스트 5 〉	통과 (0.03ms, 10.4MB)
테스트 6 〉	통과 (0.04ms, 10.5MB)
테스트 7 〉	통과 (0.05ms, 10.4MB)
테스트 8 〉	통과 (0.04ms, 10.5MB)
테스트 9 〉	통과 (0.05ms, 10.5MB)
테스트 10 〉	통과 (0.05ms, 10.4MB)
테스트 11 〉	통과 (0.28ms, 10.5MB)
테스트 12 〉	통과 (0.15ms, 10.4MB)
테스트 13 〉	통과 (0.14ms, 10.4MB)
테스트 14 〉	통과 (0.14ms, 10.3MB)
테스트 15 〉	통과 (0.10ms, 10.3MB)
테스트 16 〉	통과 (271.63ms, 10.5MB)
테스트 17 〉	통과 (451.56ms, 10.5MB)
테스트 18 〉	통과 (705.34ms, 10.5MB)
테스트 19 〉	통과 (853.31ms, 10.4MB)
테스트 20 〉	통과 (1206.48ms, 10.4MB)
```
- 누르려는 숫자위에 특정 손가락이 있으면, 손가락의 위치는 변하지 않는다!
  - 이 점을 이용해, 다른 사람의 풀이에서 숫자 위에 왼손이 있는 경우와 오른손이 있는 경우를 통합
