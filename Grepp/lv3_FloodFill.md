# FloodFill
## 문제 설명
n x m 크기 도화지에 그려진 그림의 색깔이 2차원 리스트로 주어집니다. 같은 색깔은 같은 숫자로 나타난다고 할 때, 그림에 있는 영역은 총 몇 개인지 알아내려 합니다. 영역이란 상하좌우로 연결된 같은 색상의 공간을 말합니다.

예를 들어, [[1,2,3], [3,2,1]] 같은 리스트는 다음과 같이 표현할 수 있습니다.

![image](https://user-images.githubusercontent.com/19163372/117694795-823e6680-b1fa-11eb-85bd-6347e6766c0c.png)


이때, 이 그림에는 총 5개 영역이 있습니다.

도화지의 크기 n과 m, 도화지에 칠한 색깔 image가 주어질 때, 그림에서 영역이 몇 개 있는지 리턴하는 solution 함수를 작성해주세요.

## 제한 사항
- n과 m은 1 이상 250 이하인 정수입니다.
- 그림의 색깔은 1 이상 30000 미만인 정수로만 주어집니다.
## 입출력 예
|n|m|images|정답|
|---|---|---|---|
|2|3|[[1, 2, 3], [3, 2, 1]]|5|
|3|2|[[1, 2], [1, 2], [4, 5]]|4|
### 입출력 예 #1

앞서 설명한 예와 같습니다.

### 입출력 예 #2

주어진 이미지는 다음과 같이 표현할 수 있습니다.

![image](https://user-images.githubusercontent.com/19163372/117694809-87031a80-b1fa-11eb-9a60-07350561d11a.png)


따라서 이 이미지에는 4개 영역이 있습니다.
# 풀이
처음 문제를 봤을 때, 제일 처음 떠오른 아이디어는

이미지의 모든 지점을 순회하면서 각 지점과 같은 숫자로 이루어진 인접한 영역들을 DFS로 방문을 체크하면서 돌면 되겠다는 생각이었다.

그러나, 그렇게 되면 여러 겹의 반복문이 사용될 것 같아 걱정되었지만, 일단 진행해보았다.
```python
def solution(n, m, image):
    count = 0
    for i in range(n):
        for j in range(m):
            if image[i][j]!=0:
                stack=[]
                stack.append((i,j,image[i][j]))
                image[i][j]=0
                while stack:
                    a,b,num = stack.pop()
                    image[a][b]=0
                    if 0<=a-1 and image[a-1][b]==num:
                        stack.append((a-1,b,num))
                    if 0<=b-1 and image[a][b-1]==num:
                        stack.append((a,b-1,num))
                    if a+1<n and image[a+1][b]==num:
                        stack.append((a+1,b,num))
                    if b+1<m and image[a][b+1]==num:
                        stack.append((a,b+1,num))
                count+=1
    return count
```
정확성 테스트
```
테스트 1 〉	통과 (54.00ms, 13.3MB)
테스트 2 〉	통과 (54.17ms, 13.3MB)
테스트 3 〉	통과 (43.53ms, 12.3MB)
테스트 4 〉	통과 (49.60ms, 14MB)
테스트 5 〉	통과 (49.58ms, 12.6MB)
테스트 6 〉	통과 (45.06ms, 12.8MB)
테스트 7 〉	통과 (0.02ms, 10.3MB)
```
3중 반복문을 사용하였지만, 효율성 검사가 없는 문제여서 다행히 통과하였다.

~혹시나 복잡도를 줄일 수 있는 코드를 찾아보았지만, 대부분의 코드들도 3번 이상의 반복문을 사용했다.~
