# 최대 용량이 정해진 FIFO 큐 클래스
## 문제 설명
이번 문제에서는 다음 두 가지 일을 해야 합니다.

1. 최대 용량이 정해진 FIFO 큐 클래스 구현
2. 표준 입력으로 들어온 명령어로 큐 조작
### 1. 최대 용량이 정해진 FIFO 큐 클래스 구현
스택 두 개를 통해 최대 용량이 max_size가 정해진 FIFO 큐 클래스, MyQueue를 구현하세요. 구현할 메소드는 다음과 같습니다.

1. qsize()
  - 큐가 가진 원소 수를 리턴합니다.
2. push(x)
  - 입력받은 인자, x를 큐에 넣습니다.
  - 단, 현재 큐가 꽉 찬 경우 인자를 넣지 말고 False를 리턴하세요.
3. pop():
  - 큐가 가진 원소 중, 가장 처음에 들어온 원소를 큐에서 제거하고 리턴합니다.
  - 큐에 원소가 없다면 Empty Exception을 raise 하세요. Empty Exception은 본인이 직접 만드셔야 합니다.

**※ 주의사항**
1. 위 메소드를 구현할때에는 주어진 MyStack 클래스를 변경하지 말아주세요.
2. MyQueue 클래스에서 MyStack 클래스의 변수인 `lst`를 직접 접근하지 않아야합니다. ex) `self.stack1.lst` 처럼 접근 금지
### 2. 표준 입력으로 들어온 명령어로 큐 조작
1. 첫 줄에는 앞으로 들어올 명령어 수 n과 큐의 최대 크기인 max_size가 공백으로 구분되어 주어집니다.
2. 그 뒤로는 N 줄에 걸쳐, 큐를 조작할 명령어가 주어집니다.

|명령어 종류|result|
|---|---|
|SIZE|현재 큐에 들은 원소 수를 출력합니다.|
|PUSH X|정수 X를 큐에 넣습니다. 성공했다면 True를, 아니라면 False를 출력합니다.|
|POP|큐에서 원소를 빼냅니다. 성공했다면 빼낸 원소를, 아니라면 False를 출력합니다.|

## 제한 조건
- n은 1 이상 100 이하입니다.
- max_size는 1 이상 100 이하입니다.
- PUSH 명령어에서 들어오는 X는 -100 이상 100 이하인 정수입니다.
## 입출력 예
### 입력
```
5 1
PUSH 3
PUSH -5
POP
SIZE
POP
```
### 출력
```
True
False
3
0
False
```
### 설명

명령어는 5개, 큐의 최대 허용량은 1입니다.

|명령어|결과|
|---|---|
|PUSH 3|큐에 3이 들어갑니다. True를 출력합니다.|
|PUSH -5|큐가 꽉 찼습니다. 더 이상 원소를 넣을 수 없어 False를 출력합니다.|
|POP|큐의 원소 중, 가장 먼저 들어간 3을 꺼냅니다. 따라서 3을 출력합니다.|
|SIZE|큐에 남은 원소는 0개 이므로, 0을 출력합니다.|
|POP|큐에 원소가 없으므로 False를 출력합니다.|

# 풀이
주어진 MyStack 클래스를 이용해서 두개의 스택으로 큐인 MyQueue를 구현하고, 그 MyQueue를 갖고 입력된 명령어를 처리하는 평소에 잘 보지 못했던 유형의 문제였다.

빈 stack에서 pop을 할 때 생기는 에러를 처리해주기 위해서 try-except 문으로 IndexError를 처리해줬다.
```python
class MyStack(object):
    def __init__(self):
        self.lst = list()

    def push(self, x):
        self.lst.append(x)

    def pop(self):
        return self.lst.pop()

    def size(self):
        return len(self.lst)

class MyQueue(object):
    def __init__(self, max_size):
        self.stack1 = MyStack()
        self.stack2 = MyStack()
        self.max_size = max_size

    def qsize(self):
        # 구현하세요
        return self.stack1.size()

    def push(self, item):
        # 구현하세요
        if self.qsize()==self.max_size:
            return False
        self.stack1.push(item)
        return True

    def pop(self):
        # 구현하세요
        try:
            for i in range(self.qsize()-1):
                self.stack2.push(self.stack1.pop())
            poped = self.stack1.pop()
            for i in range(self.stack2.size()):
                self.push(self.stack2.pop())
            return poped
        except IndexError:
            return False  
    
n, max_size = map(int, input().strip().split(' '))

q = MyQueue(max_size)

for i in range(n):
    command = input().strip().split()
    if "SIZE" in command:
        print(q.qsize())
    elif "PUSH" in command:
        print(q.push(command[1]))
    elif "POP" in command:
        print(q.pop())
```
정확성  테스트
```
테스트 1 〉	통과 (12.81ms, 7.71MB)
테스트 2 〉	통과 (16.67ms, 7.69MB)
테스트 3 〉	통과 (13.50ms, 7.68MB)
테스트 4 〉	통과 (13.34ms, 7.63MB)
테스트 5 〉	통과 (14.87ms, 7.66MB)
테스트 6 〉	통과 (16.80ms, 7.7MB)
```
