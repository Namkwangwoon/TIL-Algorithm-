# 선입 선출 스케줄링
## 문제 설명
처리해야 할 동일한 작업이 n 개가 있고, 이를 처리하기 위한 CPU가 있습니다.

이 CPU는 다음과 같은 특징이 있습니다.

- CPU에는 여러 개의 코어가 있고, 코어별로 한 작업을 처리하는 시간이 다릅니다.
- 한 코어에서 작업이 끝나면 작업이 없는 코어가 바로 다음 작업을 수행합니다.
- 2개 이상의 코어가 남을 경우 앞의 코어부터 작업을 처리 합니다.

처리해야 될 작업의 개수 n과, 각 코어의 처리시간이 담긴 배열 cores 가 매개변수로 주어질 때, 마지막 작업을 처리하는 코어의 번호를 return 하는 solution 함수를 완성해주세요.

## 제한 사항
- 코어의 수는 10,000 이하 2이상 입니다.
- 코어당 작업을 처리하는 시간은 10,000이하 입니다.
- 처리해야 하는 일의 개수는 50,000개를 넘기지 않습니다.

## 입출력 예
|n|cores|result|
|-|-|-|
|6|[1,2,3]|2|

## 입출력 예 설명
### 입출력 예 #1
처음 3개의 작업은 각각 1,2,3번에 들어가고, 1시간 뒤 1번 코어에 4번째 작업,다시 1시간 뒤 1,2번 코어에 5,6번째 작업이 들어가므로 2를 반환해주면 됩니다.

# 다른 사람의 풀이
- Binary Search로 접근했지만, 도저히 풀리지가 않아서 다른 사람의 코드를 참고했다.
```python
def solution(n, cores):
    l = len(cores)
    if n<=l:
        return n
    n-=l
    l, r = 1, max(cores)*n
    while l<r:
        mid = (l+r)//2
        thread = 0
        for c in cores:
            thread += mid//c
            
        if thread>=n:
            r = mid
        else:
            l = mid+1
            
    mid = (l+r)//2
    
    for i, c in enumerate(cores):
        n -= (mid-1)//c
        if n==0: return i+1
    
    for i, c in enumerate(cores):
        if (mid)%c==0:
            n-=1
        if n==0: return i+1
```
- Lower bound 및 Upper bound binary search라는 개념을 배웠다!
  - Lower bound binary search : target 값과 같은 값이 처음 나오는 위치
  - Upper bound binary search : target 값보다 큰 값이 처음 나오는 위치
- 그냥 binary search
```python
target, left, right

while left<=right:
  mid = (left+right)//2

  if mid==target:
    return target
  elif mid<target:
    left = mid+1
  elif mid>target:
    right = mid-1
```
- Lower bound binary search
```python
target, left, right

while left<right:
  mid = (left+right)//2

  if mid>=target:
    right = mid
  elif mid<target:
    left = mid+1
    
return left
```
- Upper bound binary seatch
```python
target, left, right

while left<right:
  mid = (left+right)//2

  if mid>target:
    right = mid
  elif mid<=target:
    left = mid+1

return left
```
- 주의할 점
  - `while l<r`
  - right를 변경할 때, `right = mid`
  - `mid==target`일때, 탐색 범위를 더 낮추면(`right=mid`) lower bound, 탐색 범위를 더 높이면(`left=mid+1`) upper bound
