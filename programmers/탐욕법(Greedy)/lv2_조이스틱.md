# 조이스틱
## 문제 설명
조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동 (마지막 위치에서 오른쪽으로 이동하면 첫 번째 문자에 커서)
```
예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
```
만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

## 제한 사항
- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

## 입출력 예
|name|return|
|-|-|
|"JEROEN"|56|
|"JAN"|23|

# 다른 사람의 풀이
- 중간에 'AAAA..'와 같이 A가 연속되는 경우가 핵심이라고 생각은 했지만 어떻게 풀어야 할지 감이 안와서, 다른 사람의 풀이를 참고
```python
def solution(name):
    A, end = ord('A'), ord('Z')+1
    answer = 0
    moves = len(name)-1
    
    for i, n in enumerate(name):
        answer += min(ord(n)-A, end-ord(n))
        
        As = i
        while As+1<len(name) and name[As+1]=='A':
            As+=1
            
        moves = min(moves, 2*i+len(name)-As-1, i+2*(len(name)-As-1))
        
        
    return answer+moves
```
- (▲, ▼)와 (◀, ▶)는 사실상 별개이다.
  - 최소의 (◀, ▶)로 잘 이동만 해준 후, (▲, ▼)를 해주면 됨
- 최소의 (◀, ▶) 이동은 연속된 'A'에 따라 달라지므로, 핵심은 연속된 가장 긴'A'를 찾고, 그것을 고려한 최소 이동을 찾으면 됨
