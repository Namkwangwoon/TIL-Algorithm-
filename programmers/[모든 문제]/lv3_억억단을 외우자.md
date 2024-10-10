# 억억단을 외우자
## 문제 설명
영우는 천하제일 암산대회를 앞두고 있습니다. 암산보다는 암기에 일가견이 있는 영우는 구구단을 확장하여 억억단을 만들고 외워버리기로 하였습니다.

![image](https://github.com/user-attachments/assets/28edbc40-b91d-4035-b80f-0271cb0da367)


억억단은 1억 x 1억 크기의 행렬입니다. 억억단을 외우던 영우는 친구 수연에게 퀴즈를 내달라고 부탁하였습니다.

수연은 평범하게 문제를 내봐야 영우가 너무 쉽게 맞히기 때문에 좀 어렵게 퀴즈를 내보려고 합니다. 적당한 수 `e`를 먼저 정하여 알려주고 `e` 이하의 임의의 수 `s`를 여러 개 얘기합니다. 영우는 각 `s`에 대해서 `s`보다 크거나 같고 `e` 보다 작거나 같은 수 중에서 억억단에서 가장 많이 등장한 수를 답해야 합니다. 만약 가장 많이 등장한 수가 여러 개라면 그 중 가장 작은 수를 답해야 합니다.

수연은 영우가 정답을 말하는지 확인하기 위해 당신에게 프로그램 제작을 의뢰하였습니다. `e`와 `s`의 목록 `starts`가 매개변수로 주어질 때 각 퀴즈의 답 목록을 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 1 ≤ `e` ≤ 5,000,000
- 1 ≤ `starts`의 길이 ≤ min {`e`,100,000}
- 1 ≤ `starts`의 원소 ≤ `e`
- `starts`에는 중복되는 원소가 존재하지 않습니다.

## 입출력 예
|e|starts|result|
|-|-|-|
|8|[1,3,7]|[6,6,8]|

## 입출력 예 설명
억억단에서 1 ~ 8이 등장하는 횟수는 다음과 같습니다.

> 1번 등장 : 1
>
> 2번 등장 : 2, 3, 5, 7
>
> 3번 등장 : 4
>
> 4번 등장 : 6, 8

[1, 8] 범위에서는 6과 8이 각각 4번씩 등장하여 가장 많은데 6이 더 작은 수이므로 6이 정답입니다.

[3, 8] 범위에서도 위와 같으므로 6이 정답입니다.

[7, 8] 범위에서는 7은 2번, 8은 4번 등장하므로 8이 정답입니다.

# 다른 사람의 풀이
```python
def solution(e, starts):
    divisor_list = [0]*(e+1)
    min_s = min(starts)
    for i in range(1, e + 1):
        if i * i <= e:
            divisor_list[i*i] += 1
        for j in range(i+1, e + 1):
            n = i * j
            if n > e:
                break
            divisor_list[n] += 2
            
    max_div_list = [0]*(e+1)
    max_div_list[-1] = e
    for i in range(e-1, min_s-1, -1) :
        if divisor_list[max_div_list[i+1]] <= divisor_list[i] :
            max_div_list[i] = i
        else :
            max_div_list[i] = max_div_list[i+1]
    answer = [ max_div_list[s] for s in starts ]
    
    return answer
```
- 각 수의 등장 횟수는 약수의 갯수인 것은 알았지만, 풀이 방법이 떠오르지 않아 다른 사람의 풀이를 참고
- 전략
  - `e` 이하의 수들을 각각 약수의 갯수를 세서 저장
  - 범위가 `e ~ e`에서 가장 많이 등장한 수는 확정적으로 `e` 이므로, 여기서부터 출발
  - `e`부터 `1`까지 보면서, 그 전까지의 최대 등장 횟수보다 크거나 같은 수를 만나면 그 인덱스를 기억
    - 등장 횟수가 같으면 더 적은 수를 써야 하므로, '크거나 같을 때' 인데스 업데이트
   
# 내 풀이
```python
def solution(e, starts):
    table = [0] + [1 for _ in range(e)]
    for i in range(2, e+1):
        for j in range(i, e+1, i):
            table[j]+=1
    
    max_ind = [0 for _ in range(e+1)]
    max_ind[e] = e
    cur_max_ind = e
    for i in range(e-1, 0, -1):
        if table[i]>=table[cur_max_ind]:
            cur_max_ind = i
        max_ind[i] = cur_max_ind
        
    return [max_ind[i] for i in starts]
```
정확성  테스트
```
테스트 1 〉	통과 (0.01ms, 10.3MB)
테스트 2 〉	통과 (0.01ms, 10MB)
테스트 3 〉	통과 (0.07ms, 10.1MB)
테스트 4 〉	통과 (0.36ms, 10.2MB)
테스트 5 〉	통과 (1.35ms, 10.2MB)
테스트 6 〉	통과 (4.79ms, 10.3MB)
테스트 7 〉	통과 (13.11ms, 10.3MB)
테스트 8 〉	통과 (58.52ms, 11.1MB)
테스트 9 〉	통과 (1986.38ms, 28MB)
테스트 10 〉	통과 (9993.63ms, 91.7MB)
```
