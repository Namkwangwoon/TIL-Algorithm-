# 다리를 지나는 트럭
## 문제 설명
트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

|경과 시간|다리를 지난 트럭|다리를 건너는 트럭|대기 트럭|
|-|-|-|-|
|0|[]|[]|[7,4,5,6]|
|1~2|[]|[7]|[4,5,6]|
|3|[7]|[4]|[5,6]|
|4|[7]|[4,5]|[6]|
|5|[7,4]|[5]|[6]|
|6~7|[7,4,5]|[6]|[]|
|8|[7,4,5,6]|[]|[]|

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

## 제한 조건
- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

## 입출력 예
|bridge_length|weight|truck_weights|return|
|-|-|-|-|
|2|10|[7,4,5,6]|8|
|100|100|[10]|101|
|100|100|[10,10,10,10,10,10,10,10,10,10]|110|

# 내 풀이
```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    q = deque(truck_weights)
    bridge = deque([])
    bridge_weight = 0
    
    count = 0
    
    while q or bridge:
        count+=1
        
        for i, b in enumerate(bridge):
            bridge[i][0]+=1
            
        if bridge and bridge[0][0]==bridge_length:
            _, b_w = bridge.popleft()
            bridge_weight -= b_w
        
        if q:
            w = q.popleft()
            if w+bridge_weight>weight:
                q.appendleft(w)
            else:
                bridge.append([0, w])
                bridge_weight+=w
            
    return count
```
정확성  테스트
```
테스트 1 〉	통과 (1.32ms, 10.2MB)
테스트 2 〉	통과 (21.23ms, 10.2MB)
테스트 3 〉	통과 (0.06ms, 10.2MB)
테스트 4 〉	통과 (76.49ms, 9.99MB)
테스트 5 〉	통과 (589.01ms, 10.1MB)
테스트 6 〉	통과 (222.57ms, 10.2MB)
테스트 7 〉	통과 (1.17ms, 10.2MB)
테스트 8 〉	통과 (0.36ms, 10.2MB)
테스트 9 〉	통과 (5.05ms, 10.3MB)
테스트 10 〉	통과 (0.22ms, 10.3MB)
테스트 11 〉	통과 (0.01ms, 10.2MB)
테스트 12 〉	통과 (0.76ms, 10.3MB)
테스트 13 〉	통과 (1.75ms, 10.2MB)
테스트 14 〉	통과 (0.04ms, 10.2MB)
```
- 다리에서 트럭들이 순서대로 건너므로, queue를 사용하는 문제
- 어려운 개념은 없지만, 트럭이 다리를 건널 때, 다리의 최대 무게와 다리 위 트럭의 무게, 다리의 길이를 모두 고려해야 하므로, 꼼꼼이 풀어야하는 문제인 것 같다

# 다른 사람의 풀이
```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    bridge = deque(0 for _ in range(bridge_length))
    total_weight = 0
    step = 0
    truck_weights.reverse()

    while truck_weights:
        total_weight -= bridge.popleft()
        if total_weight + truck_weights[-1] > weight:
            bridge.append(0)
        else:
            truck = truck_weights.pop()
            bridge.append(truck)
            total_weight += truck
        step += 1

    step += bridge_length

    return step
```
- 더 직관적으로 하기 위해, bridge_length의 크기를 갖는 bridge의 새로운 큐를 생성, 트럭이 없는 구간은 0
