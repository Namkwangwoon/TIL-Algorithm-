# 추석 트래픽
## 문제 설명
이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다. 장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인 9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다. 초당 최대 처리량은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.

## 입력 형식
- `solution` 함수에 전달되는 `lines` 배열은 **N**(1 ≦ **N** ≦ 2,000)개의 로그 문자열로 되어 있으며, 각 로그 문자열마다 요청에 대한 응답완료시간 **S**와 처리시간 **T**가 공백으로 구분되어 있다.
- 응답완료시간 **S**는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 `2016-09-15 hh:mm:ss.sss` 형식으로 되어 있다.
- 처리시간 **T**는 `0.1s`, `0.312s`, `2s` 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 `s`로 끝난다.
- 예를 들어, 로그 문자열 `2016-09-15 03:10:33.020 0.011s`은 "2016년 9월 15일 오전 3시 10분 **33.010초**"부터 "2016년 9월 15일 오전 3시 10분 **33.020초**"까지 "**0.011초**" 동안 처리된 요청을 의미한다. **(처리시간은 시작시간과 끝시간을 포함)**
- 서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 **0.001 ≦ T ≦ 3.000**이다.
- `lines` 배열은 응답완료시간 **S**를 기준으로 오름차순 정렬되어 있다.

## 출력 형식
- `solution` 함수에서는 로그 데이터 `lines` 배열에 대해 **초당 최대 처리량**을 리턴한다.

## 입출력 예제
### 예제1
- 입력: [

  "2016-09-15 01:00:04.001 2.0s",

  "2016-09-15 01:00:07.000 2s"
  
  ]

- 출력: 1

### 예제2
- 입력: [

  "2016-09-15 01:00:04.002 2.0s",
  
  "2016-09-15 01:00:07.000 2s"
  
  ]

- 출력: 2

- 설명: 처리시간은 시작시간과 끝시간을 포함하므로

  첫 번째 로그는 `01:00:02.003 ~ 01:00:04.002`에서 2초 동안 처리되었으며,

  두 번째 로그는 `01:00:05.001 ~ 01:00:07.000`에서 2초 동안 처리된다.

  따라서, 첫 번째 로그가 끝나는 시점과 두 번째 로그가 시작하는 시점의 구간인 `01:00:04.002 ~ 01:00:05.001` 1초 동안 최대 2개가 된다.

### 예제3
- 입력: [
  
  "2016-09-15 20:59:57.421 0.351s",

  "2016-09-15 20:59:58.233 1.181s",
  
  "2016-09-15 20:59:58.299 0.8s",

  "2016-09-15 20:59:58.688 1.041s",

  "2016-09-15 20:59:59.591 1.412s",

  "2016-09-15 21:00:00.464 1.466s",

  "2016-09-15 21:00:00.741 1.581s",

  "2016-09-15 21:00:00.748 2.31s",

  "2016-09-15 21:00:00.966 0.381s",

  "2016-09-15 21:00:02.066 2.62s"

  ]

- 출력: 7

- 설명: 아래 타임라인 그림에서 빨간색으로 표시된 1초 각 구간의 처리량을 구해보면 `(1)`은 4개, `(2)`는 7개, `(3)`는 2개임을 알 수 있다. 따라서 초당 최대 처리량은 7이 되며, 동일한 최대 처리량을 갖는 1초 구간은 여러 개 존재할 수 있으므로 이 문제에서는 구간이 아닌 개수만 출력한다.

![image](https://github.com/user-attachments/assets/103b23f2-eb82-4e5f-bbe9-499fda4a4c99)

# 내 풀이
```python
def to_int(S, T):
    S = S.split(':')
    T = T[:-1]
    if '.' not in T:
        T+='.'
    T += '0'*(5-len(T))
    S[-1], T = ''.join(S[-1].split('.')), int(''.join(T.split('.')))
    S = list(map(int, S))
    
    return S, T

def align_time(time):
    # Second
    if time[2]<0:
        time[2]+=60000
        time[1]-=1
    elif time[2]>=60000:
        time[2]-=60000
        time[1]+=1
    # Minute
    if time[1]<0:
        time[1]+=60
        time[0]-=1
    elif time[1]>=60:
        time[1]-=60
        time[0]+=1
        
    if time[0]<0:
        time = [0, 0, 0]
        
    return time

def start_end(S, T):
    start, end = S[:2] + [S[2]-T+1], S
    start = align_time(start)
        
    return start, end
    

def solution(lines):
    times = []
    answer = 0
    sec = 1000
    max_process = 3000
    
    if len(lines)==1:
        return 1
    
    # (h, m, s) <= (24, 60, 60000)
    for line in lines:
        date, S, T = line.split()
        S, T = to_int(S, T)
        start, end = start_end(S, T)
        times.append((start, end))
    
    for i, (start, end) in enumerate(times[:-1]):
        from_, to_ = end[:], end[:]
        to_[2] += sec-1
        to_ = align_time(to_)
        max_ = to_[:]
        max_[2] += max_process-1
        max_ = align_time(max_)
        count = 1
        for new_start, new_end in times[i+1:]:
            # 확인 종료 (3sec을 넘지 못하는 요청의 길이)
            if new_end>max_:
                break
            if new_start<=to_:
                count+=1
        answer = max(answer, count)
        
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.07ms, 10.4MB)
테스트 2 〉	통과 (5.96ms, 10.6MB)
테스트 3 〉	통과 (39.12ms, 10.6MB)
테스트 4 〉	통과 (0.04ms, 10.4MB)
테스트 5 〉	통과 (1.18ms, 10.5MB)
테스트 6 〉	통과 (1.15ms, 10.4MB)
테스트 7 〉	통과 (4.91ms, 10.7MB)
테스트 8 〉	통과 (4.70ms, 10.6MB)
테스트 9 〉	통과 (0.67ms, 10.5MB)
테스트 10 〉	통과 (0.09ms, 10.5MB)
테스트 11 〉	통과 (0.11ms, 10.5MB)
테스트 12 〉	통과 (5.75ms, 10.6MB)
테스트 13 〉	통과 (0.93ms, 10.4MB)
테스트 14 〉	통과 (0.04ms, 10.4MB)
테스트 15 〉	통과 (0.04ms, 10.5MB)
테스트 16 〉	통과 (0.03ms, 10.5MB)
테스트 17 〉	통과 (0.03ms, 10.5MB)
테스트 18 〉	통과 (242.09ms, 10.9MB)
테스트 19 〉	통과 (16.86ms, 10.9MB)
테스트 20 〉	통과 (14.29ms, 10.9MB)
테스트 21 〉	통과 (0.00ms, 10.3MB)
테스트 22 〉	통과 (0.00ms, 10.3MB)
```
- 예전에 했던 구간별 단속카메라? 문제랑 비숫한 것 같다.
    - (시간 문자열을 숫자로 바꿔야 하고, 0.0001s 단위에서 꼬아내서 더 더럽게 낸 문제 같다)
- 전략
  - 우선은 hh, mm, ss를 숫자로, 특히 ss는 ms단위로 변경
  - 첫 번째 요청부터, 해당 요청이 끝나는 시간+1초동안 몇 개의 요청들과 겹치는지를 count
    - 요청의 최대 길이는 3s 이므로, (해당 요청이 끝나는 시간 + 0.999초 + 2.999초)보다 더 이후에 끝나는 요청이 등장하면 더 이상 겹칠 수 없음
    - 최대로 겹치는 수가 정답

# 다른 사람의 풀이1
```python
def solution(lines):

    #get input
    S , E= [], [] 
    totalLines = 0 
    for line in lines:
        totalLines += 1
        type(line)
        (d,s,t) = line.split(" ")

       ##time to float
        t = float(t[0:-1])
        (hh, mm, ss) = s.split(":")
        seconds = float(hh) * 3600 + float(mm) * 60 + float(ss)

        E.append(seconds + 1)
        S.append(seconds - t + 0.001)

    #count the maxTraffic
    S.sort()

    curTraffic = 0
    maxTraffic = 0
    countE = 0
    countS = 0
    while((countE < totalLines) & (countS < totalLines)):
        if(S[countS] < E[countE]):
            curTraffic += 1
            maxTraffic = max(maxTraffic, curTraffic)
            countS += 1
        else: ## it means that a line is over.
            curTraffic -= 1
            countE += 1

    return maxTraffic
```
- 전체적으로 풀이는 비슷
- 전략
  - 시간을 전부 초 단위로 변경
    - 분 *= 60, 시 *= 3600
  - 시작 시간과 종료 시간을 따로 리스트에 저장하여 sort
    - 종료 시간은 이미 sort되어있으므로, 시작 시간만 sort
    - 종료 시간에는 각자 1을 더해서 미리 동시처리를 위한 시간을 더해줌
  - 시작 시간과 종료 시간 각각의 인덱스를 봐가면서, 겹치는 갯수를 카운트
