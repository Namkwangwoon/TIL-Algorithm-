# 기지국 설치
## 문제 설명
N개의 아파트가 일렬로 쭉 늘어서 있습니다. 이 중에서 일부 아파트 옥상에는 4g 기지국이 설치되어 있습니다. 기술이 발전해 5g 수요가 높아져 4g 기지국을 5g 기지국으로 바꾸려 합니다. 그런데 5g 기지국은 4g 기지국보다 전달 범위가 좁아, 4g 기지국을 5g 기지국으로 바꾸면 어떤 아파트에는 전파가 도달하지 않습니다.

예를 들어 11개의 아파트가 쭉 늘어서 있고, [4, 11] 번째 아파트 옥상에는 4g 기지국이 설치되어 있습니다. 만약 이 4g 기지국이 전파 도달 거리가 1인 5g 기지국으로 바뀔 경우 모든 아파트에 전파를 전달할 수 없습니다. (전파의 도달 거리가 W일 땐, 기지국이 설치된 아파트를 기준으로 전파를 양쪽으로 W만큼 전달할 수 있습니다.)

- 초기에, 1, 2, 6, 7, 8, 9번째 아파트에는 전파가 전달되지 않습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/030a9705-5fb9-4900-a776-4ea2b076e203)

- 1, 7, 9번째 아파트 옥상에 기지국을 설치할 경우, 모든 아파트에 전파를 전달할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/75320c12-a7da-4a05-b440-cd3c56540f1c)

- 더 많은 아파트 옥상에 기지국을 설치하면 모든 아파트에 전파를 전달할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/b3d9ff99-ba91-42f1-ba1d-767945ff597e)

이때, 우리는 5g 기지국을 최소로 설치하면서 모든 아파트에 전파를 전달하려고 합니다. 위의 예시에선 최소 3개의 아파트 옥상에 기지국을 설치해야 모든 아파트에 전파를 전달할 수 있습니다.

아파트의 개수 N, 현재 기지국이 설치된 아파트의 번호가 담긴 1차원 배열 stations, 전파의 도달 거리 W가 매개변수로 주어질 때, 모든 아파트에 전파를 전달하기 위해 증설해야 할 기지국 개수의 최솟값을 리턴하는 solution 함수를 완성해주세요

## 제한사항
- N: 200,000,000 이하의 자연수
- stations의 크기: 10,000 이하의 자연수
- stations는 오름차순으로 정렬되어 있고, 배열에 담긴 수는 N보다 같거나 작은 자연수입니다.
- W: 10,000 이하의 자연수

## 입출력 예
|N|stations|W|answer|
|-|-|-|-|
|11|[4, 11]|1|3|
|16|[9]|2|3|

## 입출력 예 설명
### 입출력 예 #1
문제의 예시와 같습니다

### 입출력 예 #2

- 초기에, 1~6, 12~16번째 아파트에는 전파가 전달되지 않습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/1cfd5e81-84c1-42f5-bb41-cae230708075)

- 3, 6, 14번째 아파트 옥상에 기지국을 설치할 경우 모든 아파트에 전파를 전달할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/75065e06-266e-4ad7-8a3b-0edb295f5937)

# 내 풀이
```python
def solution(n, stations, w):
    st_range = []
    last = 0
    for station in stations:
        m, M = max(1, station-w), min(n, station+w)
        if m<last:
            last = M
        else:
            st_range.append(m-last-1)
            last = M
    
    last_st = n-last
    if last_st != 0:
        st_range.append(last_st)
        
    answer=0
    
    frag = 2*w+1
    for r in st_range:
        answer += (r-1)//frag+1
        
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.2MB)
테스트 2 〉	통과 (0.00ms, 10.2MB)
테스트 3 〉	통과 (0.01ms, 10.1MB)
테스트 4 〉	통과 (0.01ms, 10.2MB)
테스트 5 〉	통과 (0.01ms, 10.3MB)
테스트 6 〉	통과 (0.01ms, 10.2MB)
테스트 7 〉	통과 (0.00ms, 10.2MB)
테스트 8 〉	통과 (0.01ms, 10.2MB)
테스트 9 〉	통과 (0.01ms, 10.1MB)
테스트 10 〉	통과 (0.01ms, 10.1MB)
테스트 11 〉	통과 (0.01ms, 10.3MB)
테스트 12 〉	통과 (0.01ms, 10.1MB)
테스트 13 〉	통과 (0.01ms, 10.2MB)
테스트 14 〉	통과 (0.01ms, 10.3MB)
테스트 15 〉	통과 (0.01ms, 10.1MB)
테스트 16 〉	통과 (0.01ms, 10.2MB)
테스트 17 〉	통과 (0.01ms, 10.2MB)
테스트 18 〉	통과 (0.01ms, 10MB)
테스트 19 〉	통과 (0.01ms, 10.3MB)
테스트 20 〉	통과 (0.01ms, 10.1MB)
테스트 21 〉	통과 (0.03ms, 10.4MB)
```
효율성  테스트
```
테스트 1 〉	통과 (5.45ms, 10.5MB)
테스트 2 〉	통과 (6.33ms, 10.5MB)
테스트 3 〉	통과 (9.54ms, 10.5MB)
테스트 4 〉	통과 (4.26ms, 10.6MB)
```
- 전략
  - 결국 우리가 필요한 것은, 전파가 도달하지 않는 영역의 길이들
    - 각 길이들에 최소한의 기지국을 건설하면 됨!
  - stations를 돌면서, 전파가 도달하지 않는 길이를 st_range에 추가
  - `(st_range의 원소 - 1) // (2*w+1) + 1` 를 해서, 필요한 최소한의 기지국 갯수를 구할 수 있음

# 다른 사람의 풀이
```python
def solution(n, stations, w):
    ans = 0
    idx = 0
    location = 1

    while(location <= n) :
        if(idx < len(stations) and location >= stations[idx]-w) :
            location = stations[idx]+w+1
            idx += 1
        else :
            location += 2*w+1
            ans += 1
    return ans
```
- 전략
  - ind 1 부터 시작
  - 만약 기지국 범위 밖이라면, 기지국 설치 & 그 범위만큼 앞으로 감
  - 만약 기지국 범위 안이라면, 범위를 벗어나도록 location 이동
    - `stations[idx]+w+1` 만큼 이동해서, idx번째 기지국의 범위 바로 밖으로 이동
