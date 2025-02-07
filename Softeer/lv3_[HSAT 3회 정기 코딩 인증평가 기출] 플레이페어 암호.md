# 플레이페어 암호
## 문제
대학교 학부생활을 마치고 현대자동차에 프로그래머로 취직한 사회초년생 현빈이는 팀장님에게 보안에 관련한 지식이 하나도 없음을 들키고 말았다. 그래서 현빈이는 업무시간 틈틈이 보안과 관련된 주제들을 공부하고 있다.

오늘 공부할 주제는 암호화 방식중 하나인 Playfair cipher(플레이페어 암호)다. Playfair cipher는 알파벳으로 이루어진 어떤 문자열(평문; plaintext)을 암호화하는 방법으로, 이를 위해 알파벳으로 이루어진 문자열인 키(key)가 필요하다. Playfair cipher는 빈도분석을 어렵게 하기 위해 한번에 두 글자 단위로 암호화를 진행하며, 5×5크기의 표를 사용하기 때문에 알파벳 26개를 모두 담기에는 칸이 한 개 부족해 I와 J를 동일한 것으로 생각한다. 이 문제에서는 편의상 J가 아예 주어지지 않는다.



먼저, 주어진 키를 5×5크기의 표로 변환한다. 키를 한 글자씩 보면서 왼쪽 위 칸부터 한줄씩 표를 채운다. 만약 이전에 봤던 알파벳이 한번 더 등장하면 무시하고 다음 글자를 보면 된다. 키를 다 보고도 칸이 남는다면, 아직 등장하지 않은 알파벳을 순서대로 채워넣으면 된다. 예를 들어 키가 PLAYFAIRCIPHERKEY라면 다음과 같이 표가 만들어진다. 굵게 표시된 알파벳이 키를 통해 채워진 알파벳이다.

![image](https://github.com/user-attachments/assets/e91b20ef-1970-4173-a7d0-6f21a227328e)


다음 일은 암호화하려는 메세지를 두 글자씩 나누는 일이다. 예를 들어, HELLOWORLD라는 메세지를 두 글자씩 나눈다면 HE LL OW OR LD가 된다. LL같이 두 글자로 이루어진 쌍이 생기면 중간에 다른 글자를 넣어 쌍을 파괴해줘야 한다. 이렇게 같은 두 글자로 이루어진 쌍이 생기면 그 중 가장 앞에 있는 쌍 사이에 X를 넣고 뒤쪽은 새롭게 쌍을 구성하면 된다. 만약, 쌍이 XX였다면 X를 넣어서는 해결이 안되기 때문에 Q를 넣는 것으로 해결 한다. 이렇게 쌍을 모두 맞추고 마지막에 한 글자가 남는다면 이것도 암호화가 불가능하기 때문에 여기도 X를 덧붙여 강제로 쌍을 맞춰준다. 마지막 남은 한 글자가 X인 경우에는 예외적으로 XX로 쌍을 맞춘다.

그러므로, HELLOWORLD를 두 글자씩 나누는 올바른 방법은 HE LX LO WO RL DX이고, XXYYY를 두 글자씩 나누는 올바른 방법은 XQ XY YX YX가 된다. 마지막으로, 쌍을 만든 두 글자를 암호화하는 일이 남았다. 다음과 같은 세 가지 경우가 있는데, 위에서 만든 5×5표를 통해 설명해본다.

1. 만약, 두 글자가 표에서 같은 행에 존재하면, 오른쪽으로 한 칸 이동한 칸에 적힌 글자로 암호화된다. 예를 들어 HE를 암호화하면 EI가 되고, XX를 암호화하면 ZZ가 된다. 위치가 다섯 번째 열이라면 첫 번째 열로 이동하게 된다.


![image](https://github.com/user-attachments/assets/53d4547d-33b6-4737-a71b-951b1184b8a5)


2. 1.의 경우를 만족하지 않으면서 두 글자가 표에서 같은 열에 존재하면, 아래쪽으로 한 칸 이동한 칸에 적힌 글자로 암호화된다. 예를 들어 LO를 암호화하면 RV가 된다. 위치가 다섯 번째 행이라면 첫 번째 행으로 이동하게 된다.

![image](https://github.com/user-attachments/assets/14599500-a143-4665-b2ec-ca1a2a582931)


3. 1, 2의 경우를 만족하지 않으면서, 두 글자가 표에서 서로 다른 행, 열에 존재하면, 두 글자가 위치하는 칸의 열이 서로 교환된 위치에 적힌 글자로 암호화된다. 예를 들어 LX를 암호화하면 YV가 된다.

![image](https://github.com/user-attachments/assets/d6aaee4d-fbe6-494f-a6a0-4f316ed7890e)


이 과정에 따르면, HELLOWORLD를 Playfair cipher로 암호화한 결과는 EIYVRVVQBRGW가 된다.

현빈이는 어떤 메세지와 키가 주어졌을 때 주어진 메세지를 Playfair cipher로 암호화하려고 한다.

## 제약조건
메세지의 길이는 1 이상 1,000 이하이다.
키의 길이는 1 이상 100 이하이다.

## 입력형식
첫 번째 줄에 J를 제외한 알파벳 대문자로 이루어진 메세지가 주어진다.
두 번째 줄에 J를 제외한 알파벳 대문자로 이루어진 키가 주어진다.

## 출력형식
첫 번째 줄에 Playfair cipher로 암호화된 결과를 출력한다.

### 입력예제1
```
HELLOWORLD
PLAYFAIRCIPHERKEY
```
### 출력예제1
```
EIYVRVVQBRGW
```
### 입력예제2
```
LEMONSTRAWBERRYAPPLEIUICE
WATERMELON
```
### 출력예제2
```
NALNBQEWTANRTZEZTKKOWQWUGW
```

# 내 풀이
```python
from sys import stdin
from collections import deque

message = stdin.readline().rstrip()
key = stdin.readline().rstrip()

board = [["-"]*5 for _ in range(5)]
alp_used = set('J')

def new_char(m1, m2, board):
    r1, c1 = m1
    r2, c2 = m2
    if r1==r2:
        c1 = (c1+1)%5
        c2 = (c2+1)%5
    elif c1==c2:
        r1 = (r1+1)%5
        r2 = (r2+1)%5
    else:
        tmp = c1
        c1 = c2
        c2 = tmp

    return board[r1][c1]+board[r2][c2]


# Step 1: 5x5 board 채우기
key_i, key_l = 0, len(key)
fill_from_key = True
last_r, last_c = -1, -1
for r in range(5):
    for c in range(5):
        if fill_from_key:
            while key_i<key_l and key[key_i] in alp_used:
                key_i+=1
            if key_i==key_l:
                fill_from_key = False
                last_r, last_c = r, c
                break
            board[r][c] = key[key_i]
            alp_used.add(key[key_i])
            key_i+=1

not_used_alp = [chr(ord('A')+a) for a in range(26) if chr(ord('A')+a) not in alp_used]

not_used_i = 0
c = last_c
if fill_from_key==False:
    for r in range(last_r, 5):
        for c in range(c, 5):
            board[r][c] = not_used_alp[not_used_i]
            not_used_i+=1
        c=0


# Step 2: message 두 글자씩 끊기
mes_deque = deque(message)
new_deque = deque()
while mes_deque:
    m_1 = mes_deque.popleft()
    if not mes_deque:
       m_2 = 'X'
    elif mes_deque[0]==m_1:
        if m_1=='X': m_2 = 'Q'
        else: m_2 = 'X'
    else:
        m_2 = mes_deque.popleft()
    new_deque.append((m_1,m_2))


# Step 3: board에서 message 찾기
answer = ''
while new_deque:
    m1_r, m1_c, m2_r, m2_c = -1, -1, -1, -1
    m1, m2 = new_deque.popleft()
    for r in range(5):
        if m1 in board[r]:
            m1_r, m1_c = r, board[r].index(m1)
        if m2 in board[r]:
            m2_r, m2_c = r, board[r].index(m2)
    answer += new_char((m1_r, m1_c), (m2_r, m2_c), board)
print(answer)
```
![image](https://github.com/user-attachments/assets/03ba9b09-69ff-455b-b0b5-adbb38d9eddb)

- 구현문제였다.
