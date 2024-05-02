# 표현 가능한 이진트리
## 문제 설명
당신은 이진트리를 수로 표현하는 것을 좋아합니다.

이진트리를 수로 표현하는 방법은 다음과 같습니다.

1. 이진수를 저장할 빈 문자열을 생성합니다.
2. 주어진 이진트리에 더미 노드를 추가하여 포화 이진트리로 만듭니다. **루트 노드는 그대로 유지합니다.**
3. 만들어진 포화 이진트리의 노드들을 가장 왼쪽 노드부터 가장 오른쪽 노드까지, 왼쪽에 있는 순서대로 살펴봅니다. 노드의 높이는 살펴보는 순서에 영향을 끼치지 않습니다.
4. 살펴본 노드가 더미 노드라면, 문자열 뒤에 0을 추가합니다. 살펴본 노드가 더미 노드가 아니라면, 문자열 뒤에 1을 추가합니다.
5. 문자열에 저장된 이진수를 십진수로 변환합니다.

**이진트리에서 리프 노드가 아닌 노드는 자신의 왼쪽 자식이 루트인 서브트리의 노드들보다 오른쪽에 있으며, 자신의 오른쪽 자식이 루트인 서브트리의 노드들보다 왼쪽에 있다고 가정합니다.**

다음은 이진트리를 수로 표현하는 예시입니다.

주어진 이진트리는 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/0b79c3c9-182e-48e9-83ba-e3bb8d75c62e)

주어진 이진트리에 더미노드를 추가하여 포화 이진트리로 만들면 다음과 같습니다. **더미 노드는 점선으로 표시하였고, 노드 안의 수는 살펴보는 순서를 의미합니다.**

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/4f69ab3e-8144-4980-af72-4527549e83d2)

노드들을 왼쪽에 있는 순서대로 살펴보며 0과 1을 생성한 문자열에 추가하면 `"0111010"`이 됩니다. 이 이진수를 십진수로 변환하면 58입니다.

당신은 수가 주어졌을때, 하나의 이진트리로 해당 수를 표현할 수 있는지 알고 싶습니다.

이진트리로 만들고 싶은 수를 담은 1차원 정수 배열 `numbers`가 주어집니다. `numbers`에 주어진 순서대로 하나의 이진트리로 해당 수를 표현할 수 있다면 1을, 표현할 수 없다면 0을 1차원 정수 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 1 ≤ `numbers`의 길이 ≤ 10,000
  - 1 ≤ `numbers`의 원소 ≤ $10^{15}$

## 입출력 예
|numbers|result|
|-|-|
|[7, 42, 5]|[1, 1, 0]|
|[63, 111, 95]|[1, 1, 0]|

## 입출력 예 설명
### 입출력 예 #1

7은 다음과 같은 이진트리로 표현할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/17b79e0d-7916-4633-bb62-570dcb308434)

42는 다음과 같은 이진트리로 표현할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/f2da43c1-faaf-49f4-aa0b-94dbd09dec67)

5는 이진트리로 표현할 수 없습니다.

따라서, [1, 0]을 return 하면 됩니다.

## 입출력 예 #2

63은 다음과 같은 이진트리로 표현할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/69492e07-5b1f-4020-8947-65367be04a46)

111은 다음과 같은 이진트리로 표현할 수 있습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/c5290cd2-2d52-4024-80ce-79e5a104447e)

95는 이진트리로 표현할 수 없습니다.

따라서, [1, 1, 0]을 return 하면 됩니다.

# 내 풀이
```python
def solution(numbers):
    answer = []
    for n in numbers:
        possible=True
        num = ""
        while n>0:
            num = str(n%2)+num
            n//=2
        
        for i in range(1, 7):
            if 2**i-1>=len(num):
                num = num.zfill(2**i-1)
                break
        l = len(num)
        
        for i in range(1, 6):
            start = 2**i-1
            stride = 2**(i+1)
            look = 2**(i-1)

            for j in range((l-start-1)//stride+1):
                node = start + stride*j
                if not (num[node]=='1' or (num[node]=='0' and num[node-look]=='0' and num[node+look]=='0')):
                    answer.append(0)
                    possible=False
                    break
            if not possible:
                break
        else:
            answer.append(1)
    return answer
```
정확성  테스트
```
테스트 1 〉	통과 (0.03ms, 10.2MB)
테스트 2 〉	통과 (0.06ms, 10.3MB)
테스트 3 〉	통과 (0.09ms, 10MB)
테스트 4 〉	통과 (0.35ms, 10.3MB)
테스트 5 〉	통과 (1.47ms, 10.3MB)
테스트 6 〉	통과 (1.43ms, 10.2MB)
테스트 7 〉	통과 (2.46ms, 10.2MB)
테스트 8 〉	통과 (1.90ms, 10.4MB)
테스트 9 〉	통과 (11.73ms, 10.3MB)
테스트 10 〉	통과 (102.42ms, 10.9MB)
테스트 11 〉	통과 (119.95ms, 11.4MB)
테스트 12 〉	통과 (113.28ms, 11.2MB)
테스트 13 〉	통과 (109.06ms, 11MB)
테스트 14 〉	통과 (101.67ms, 11MB)
테스트 15 〉	통과 (70.49ms, 10.7MB)
테스트 16 〉	통과 (213.97ms, 11.3MB)
테스트 17 〉	통과 (177.49ms, 11.4MB)
테스트 18 〉	통과 (229.03ms, 11MB)
테스트 19 〉	통과 (152.31ms, 11.2MB)
테스트 20 〉	통과 (99.32ms, 10.6MB)
```
- 전략
  - 숫자를 먼저 이진수로 만들어준다.
    - `bin()`을 이용하면 쉽게 만들 수 있었다,,
  - 완전 이진트리 형태를 위해, 길이가 $2^n-1$이 되도록 맞춰준다.
  - 완전 이진트리 가능 여부를 체크한다.
    - 완전 이진트리가 가능한 경우
      - ?-1-? or 0-0-0
    - 완전 이진트리가 불가능한 경우
      - 1-0-1 or 0-0-1 or 1-0-0
    - 체크해야 하는 인덱스는, 규칙성을 따른다.
      - 1 $\pm$ 1, 5 $\pm$ 1, ..., 61 $\pm$ 1
      - 3 $\pm$ 2, 11 $\pm$ 2, ... 59 $\pm$ 2
      - ...
      - 31 $\pm$ 16

# 다른 사람의 풀이
```python
def search(number) :
    length = len(number)
    if length == 1 or '1' not in number or '0' not in number:
        return True
    
    mid = length // 2
    if number[mid] == '0':
        return False
    
    return search(number[:mid]) and search(number[mid+1:])

def solution(numbers):
    bin_numbers = [ bin(x)[2:] for x in numbers]
    bin_list = [ 2**x - 1 for x in range(50)]
    answer = list()
    for number in bin_numbers :
        length = len(number)
        for num in bin_list :
            if num >= length :
                number = '0'*(num-length) + number
                break
        answer.append(1 if search(number) else 0)
    
    return answer
```
- 전략
  - 트리의 깊이 별로 체크하는 재귀를 사용
  - 각 서브트리의 최상위 노드는 길이의 절반 인덱스에 위치
  - 트리가 확실하게 가능한 경우의 수를 세 가지로 분류했다.
    - 노드의 갯수가 1인 트리 (최하단 노드로, 1인지 0인지 상관 없음)
    - 모든 노드가 0인 노드
    - 모든 노드가 1인 노드
  - 위 경우가 아니면서(노드의 갯수가 3개 이상 & 1과 0이 섞여있음), 최상위 노드가 0이면 불가능한 트리라고 판단.
  - 재귀를 통해, 왼쪽 및 오른쪽 절반 서브트리를 탐색
  - 모든 서브트리가 가능한 경우에만 가능
