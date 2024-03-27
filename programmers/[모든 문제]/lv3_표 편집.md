# 표 편집
## 문제 설명
**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

업무용 소프트웨어를 개발하는 니니즈웍스의 인턴인 앙몬드는 명령어 기반으로 표의 행을 선택, 삭제, 복구하는 프로그램을 작성하는 과제를 맡았습니다. 세부 요구 사항은 다음과 같습니다

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/60dd882b-92f5-4ab7-b671-381fbb1dc791)


위 그림에서 파란색으로 칠해진 칸은 현재 선택된 행을 나타냅니다. 단, 한 번에 한 행만 선택할 수 있으며, 표의 범위(0행 ~ 마지막 행)를 벗어날 수 없습니다. 이때, 다음과 같은 명령어를 이용하여 표를 편집합니다.

- `"U X"`: 현재 선택된 행에서 X칸 위에 있는 행을 선택합니다.
- `"D X"`: 현재 선택된 행에서 X칸 아래에 있는 행을 선택합니다.
- `"C"` : 현재 선택된 행을 삭제한 후, 바로 아래 행을 선택합니다. 단, 삭제된 행이 가장 마지막 행인 경우 바로 윗 행을 선택합니다.
- `"Z"` : 가장 최근에 삭제된 행을 원래대로 복구합니다. **단, 현재 선택된 행은 바뀌지 않습니다.**

예를 들어 위 표에서 `"D 2"`를 수행할 경우 아래 그림의 왼쪽처럼 4행이 선택되며, `"C"`를 수행하면 선택된 행을 삭제하고, 바로 아래 행이었던 "네오"가 적힌 행을 선택합니다(4행이 삭제되면서 아래 있던 행들이 하나씩 밀려 올라오고, 수정된 표에서 다시 4행을 선택하는 것과 동일합니다).

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/eb2664c7-cd43-4ec0-88eb-0380e1743264)

다음으로 `"U 3"`을 수행한 다음 `"C"`를 수행한 후의 표 상태는 아래 그림과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/4fd441f7-7ccd-4489-8592-d4997835b9e2)

다음으로 `"D 4"`를 수행한 다음 `"C"`를 수행한 후의 표 상태는 아래 그림과 같습니다. 5행이 표의 마지막 행 이므로, 이 경우 바로 윗 행을 선택하는 점에 주의합니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/4da87ce4-65c4-41fe-b1bd-ac5f4e9043f7)

다음으로 `"U 2"`를 수행하면 현재 선택된 행은 2행이 됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/22306006-2615-4e94-b00a-82b95cc36377)

위 상태에서 `"Z"`를 수행할 경우 가장 최근에 제거된 `"라이언"`이 적힌 행이 원래대로 복구됩니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/56851d90-3c3f-45e0-a8bc-fa098c8c176f)

다시한번 `"Z"`를 수행하면 그 다음으로 최근에 제거된 `"콘"`이 적힌 행이 원래대로 복구됩니다. 이때, 현재 선택된 행은 바뀌지 않는 점에 주의하세요.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/05b31d80-a9c0-4f37-88ac-dece8a4c40d0)

이때, 최종 표의 상태와 처음 주어진 표의 상태를 비교하여 삭제되지 않은 행은 `"O"`, 삭제된 행은 `"X"`로 표시하면 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/2bee2a1e-c4fa-4983-bb82-050d89b53b9a)

처음 표의 행 개수를 나타내는 정수 n, 처음에 선택된 행의 위치를 나타내는 정수 k, 수행한 명령어들이 담긴 문자열 배열 cmd가 매개변수로 주어질 때, 모든 명령어를 수행한 후 표의 상태와 처음 주어진 표의 상태를 비교하여 삭제되지 않은 행은 O, 삭제된 행은 X로 표시하여 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

## 제한사항
- 5 ≤ `n` ≤ 1,000,000
- 0 ≤ `k` < `n`
- 1 ≤ `cmd`의 원소 개수 ≤ 200,000
  - `cmd`의 각 원소는 `"U X"`, `"D X"`, `"C"`, `"Z"` 중 하나입니다.
  - X는 1 이상 300,000 이하인 자연수이며 0으로 시작하지 않습니다.
  - X가 나타내는 자연수에 ',' 는 주어지지 않습니다. 예를 들어 123,456의 경우 123456으로 주어집니다.
  - `cmd`에 등장하는 모든 X들의 값을 합친 결과가 1,000,000 이하인 경우만 입력으로 주어집니다.
  - 표의 모든 행을 제거하여, 행이 하나도 남지 않는 경우는 입력으로 주어지지 않습니다.
  - 본문에서 각 행이 제거되고 복구되는 과정을 보다 자연스럽게 보이기 위해 `"이름"` 열을 사용하였으나, `"이름"`열의 내용이 실제 문제를 푸는 과정에 필요하지는 않습니다. `"이름"`열에는 서로 다른 이름들이 중복없이 채워져 있다고 가정하고 문제를 해결해 주세요.
- 표의 범위를 벗어나는 이동은 입력으로 주어지지 않습니다.
- 원래대로 복구할 행이 없을 때(즉, 삭제된 행이 없을 때) "Z"가 명령어로 주어지는 경우는 없습니다.
- 정답은 표의 0행부터 n - 1행까지에 해당되는 O, X를 순서대로 이어붙인 문자열 형태로 return 해주세요.

## 정확성 테스트 케이스 제한 사항
- 5 ≤ `n` ≤ 1,000
- 1 ≤ `cmd`의 원소 개수 ≤ 1,000

## 효율성 테스트 케이스 제한 사항
- 주어진 조건 외 추가 제한사항 없습니다.

## 입출력 예
|n|k|cmd|result|
|-|-|-|-|
|8|2|["D 2","C","U 3","C","D 4","C","U 2","Z","Z"]|"OOOOXOOO"|
|8|2|["D 2","C","U 3","C","D 4","C","U 2","Z","Z","U 1","C"]|"OOXOXOOO"|

## 입출력 예 설명
### 입출력 예 #1

문제의 예시와 같습니다.

### 입출력 예 #2

다음은 9번째 명령어까지 수행한 후의 표 상태이며, 이는 입출력 예 #1과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/b676d8ad-e175-476c-bba0-c6fc59a5cf72)

10번째 명령어 `"U 1"`을 수행하면 `"어피치"`가 적힌 2행이 선택되며, 마지막 명령어 `"C"`를 수행하면 선택된 행을 삭제하고, 바로 아래 행이었던 `"제이지"`가 적힌 행을 선택합니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/97c38fcd-d604-44a0-aa47-cbcef4e55d42)

따라서 처음 주어진 표의 상태와 최종 표의 상태를 비교하면 다음과 같습니다.

![image](https://github.com/Namkwangwoon/TIL-Algorithm-/assets/19163372/72c07e33-cff4-4522-a5ea-aceb62d86398)

## 제한시간 안내
- 정확성 테스트 : 10초
- 효율성 테스트 : 언어별로 작성된 정답 코드의 실행 시간의 적정 배수

# 내 풀이
- 처음에 아무런 자료구조도 쓰지 않고, 배열로 풀려고 시도했다.
- 시간초과로 실패
  - 'U'과 'D'에서 `'X'`를 건너뛰는 과정에서 for문을 사용하여 시간초과가 난 것 같다.
- 다른 사람의 풀이를 참고하여, 딕셔너리 자료형을 사용한 간단한 구조의 linked list를 구현
```python
def solution(n, k, cmd):
    data = {i:[i-1, i+1] for i in range(n)}
    data[0][0], data[n-1][1] = None, None
    stack = []
    answer = ['O'] * n
    
    for cm in cmd:
        cm = cm.split()
        if len(cm)==2:
            com, opt = cm
            opt = int(opt)
        else:
            com = cm[0]
            
        if com=='U':
            for _ in range(opt):
                k = data[k][0]
        
        elif com=='D':
            for _ in range(opt):
                k = data[k][1]
        
        elif com=='C':
            pre, nex = data[k]
            answer[k]='X'
            stack.append((data[k][0], k, data[k][1]))
            if pre==None:
                data[nex][0] = pre
                k=nex
            elif nex==None:
                data[pre][1] = nex
                k=pre
            else:
                data[pre][1] = nex
                data[nex][0] = pre
                k=nex
        elif com=='Z':
            pre, cur, nex = stack.pop()
            answer[cur]='O'
            # data[cur]=[pre, nex]
            if pre==None:
                data[nex][0]=cur
            elif nex==None:
                data[pre][1]=cur
            else:
                data[nex][0]=cur
                data[pre][1]=cur
        
    return "".join(answer)
```
- 전략
  - `n`길이 만큼의 `'O'`로 이루어진 배열을 만들고, 상황마다 `'X'`로 업데이트
  - 숫자로 이루어진 linked list인 `data`
    - 1: [0, 2] -> 1번 노드의 앞 노드는 0번 노드, 다음 노드는 2번 노드
    - 앞이나 뒤에 노드가 없으면 None으로 표시
    - `U`와 `D`는 횟수만큼 앞 혹은 뒤로 움직이면 된다.
    - `C`에서는 (앞 노드, 삭제하려는 노드, 다음 노드)를 stack에 append, 현재 가리키는 노드의 인덱스 `k`는 다음 노드가 됨
      - 삭제하려는 노드의 앞 노드와 다음 노드를 연결해줌
      - 맨 앞 노드를 삭제시에는 다음 노드가 가리키는 앞 노드가 `None`
      - 맨 뒤 노드를 삭제시에는 앞 노드가 가리키는 다음 노드가 `None`
        - 가리키는 노드의 인덱스가 앞 노드가 되어야 함!!
    - `Z`에서는 stack에서 꺼낸 값인 (앞 노드, 삭제했던 노드, 다음 노드)를 연결해 줌
- 딕셔너리를 사용한 간단한 linked list의 구현법을 배웠다.
