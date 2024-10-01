# 매칭 점수
## 문제 설명
프렌즈 대학교 조교였던 제이지는 허드렛일만 시키는 네오 학과장님의 마수에서 벗어나, 카카오에 입사하게 되었다.

평소에 관심있어하던 검색에 마침 결원이 발생하여, 검색개발팀에 편입될 수 있었고, 대망의 첫 프로젝트를 맡게 되었다.

그 프로젝트는 검색어에 가장 잘 맞는 웹페이지를 보여주기 위해 아래와 같은 규칙으로 검색어에 대한 웹페이지의 매칭점수를 계산 하는 것이었다.

- 한 웹페이지에 대해서 기본점수, 외부 링크 수, 링크점수, 그리고 매칭점수를 구할 수 있다.
- 한 웹페이지의 기본점수는 해당 웹페이지의 텍스트 중, 검색어가 등장하는 횟수이다. (대소문자 무시)
- 한 웹페이지의 외부 링크 수는 해당 웹페이지에서 다른 외부 페이지로 연결된 링크의 개수이다.
- 한 웹페이지의 링크점수는 해당 웹페이지로 링크가 걸린 다른 웹페이지의 기본점수 ÷ 외부 링크 수의 총합이다.
- 한 웹페이지의 매칭점수는 기본점수와 링크점수의 합으로 계산한다.

예를 들어, 다음과 같이 A, B, C 세 개의 웹페이지가 있고, 검색어가 hi라고 하자.

![image](https://github.com/user-attachments/assets/b5d4652c-c061-4bf5-8047-5a51d1fc2ff7)

이때 A 웹페이지의 매칭점수는 다음과 같이 계산할 수 있다.

- 기본 점수는 각 웹페이지에서 hi가 등장한 횟수이다.
  - A,B,C 웹페이지의 기본점수는 각각 1점, 4점, 9점이다.
- 외부 링크수는 다른 웹페이지로 링크가 걸린 개수이다.
  - A,B,C 웹페이지의 외부 링크 수는 각각 1점, 2점, 3점이다.
- A 웹페이지로 링크가 걸린 페이지는 B와 C가 있다.
  - A 웹페이지의 링크점수는 B의 링크점수 2점(4 ÷ 2)과 C의 링크점수 3점(9 ÷ 3)을 더한 5점이 된다.
- 그러므로, A 웹페이지의 매칭점수는 기본점수 1점 + 링크점수 5점 = 6점이 된다.

검색어 word와 웹페이지의 HTML 목록인 pages가 주어졌을 때, 매칭점수가 가장 높은 웹페이지의 index를 구하라. 만약 그런 웹페이지가 여러 개라면 그중 번호가 가장 작은 것을 구하라.

## 제한사항
- pages는 HTML 형식의 웹페이지가 문자열 형태로 들어있는 배열이고, 길이는 `1` 이상 `20` 이하이다.
- 한 웹페이지 문자열의 길이는 `1` 이상 `1,500` 이하이다.
- 웹페이지의 index는 pages 배열의 index와 같으며 0부터 시작한다.
- 한 웹페이지의 url은 HTML의 태그 내에 태그의 값으로 주어진다.
  - 예를들어, 아래와 같은 meta tag가 있으면 이 웹페이지의 url은 `https://careers.kakao.com/index` 이다.
  - `<meta property="og:url" content="https://careers.kakao.com/index" />`
- 한 웹페이지에서 모든 외부 링크는 `<a href="https://careers.kakao.com/index">`의 형태를 가진다.
  - `<a>` 내에 다른 attribute가 주어지는 경우는 없으며 항상 href로 연결할 사이트의 url만 포함된다.
  - 위의 경우에서 해당 웹페이지는 `https://careers.kakao.com/index` 로 외부링크를 가지고 있다고 볼 수 있다.
- 모든 url은 `https://` 로만 시작한다.
- 검색어 word는 하나의 영어 단어로만 주어지며 알파벳 소문자와 대문자로만 이루어져 있다.
- word의 길이는 `1` 이상 `12` 이하이다.
- 검색어를 찾을 때, 대소문자 구분은 무시하고 찾는다.
  - 예를들어 검색어가 blind일 때, HTML 내에 Blind라는 단어가 있거나, BLIND라는 단어가 있으면 두 경우 모두 해당된다.
- 검색어는 단어 단위로 비교하며, 단어와 완전히 일치하는 경우에만 기본 점수에 반영한다.
  - 단어는 알파벳을 제외한 다른 모든 문자로 구분한다.
  - 예를들어 검색어가 "aba" 일 때, "abab abababa"는 단어 단위로 일치하는게 없으니, 기본 점수는 0점이 된다.
  - 만약 검색어가 "aba" 라면, "aba@aba aba"는 단어 단위로 세개가 일치하므로, 기본 점수는 3점이다.
- 결과를 돌려줄때, 동일한 매칭점수를 가진 웹페이지가 여러 개라면 그중 index 번호가 가장 작은 것를 리턴한다
  - 즉, 웹페이지가 세개이고, 각각 매칭점수가 3,1,3 이라면 제일 적은 index 번호인 0을 리턴하면 된다.
  
## 입출력 예 #1
- word : blind
- pages :
```
["<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://a.com\"/>\n</head>  \n<body>\nBlind Lorem Blind ipsum dolor Blind test sit amet, consectetur adipiscing elit. \n<a href=\"https://b.com\"> Link to b </a>\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://b.com\"/>\n</head>  \n<body>\nSuspendisse potenti. Vivamus venenatis tellus non turpis bibendum, \n<a href=\"https://a.com\"> Link to a </a>\nblind sed congue urna varius. Suspendisse feugiat nisl ligula, quis malesuada felis hendrerit ut.\n<a href=\"https://c.com\"> Link to c </a>\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://c.com\"/>\n</head>  \n<body>\nUt condimentum urna at felis sodales rutrum. Sed dapibus cursus diam, non interdum nulla tempor nec. Phasellus rutrum enim at orci consectetu blind\n<a href=\"https://a.com\"> Link to a </a>\n</body>\n</html>"]
```
- pages는 다음과 같이 3개의 웹페이지에 해당하는 HTML 문자열이 순서대로 들어있다.
```
<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="utf-8">
  <meta property="og:url" content="https://a.com"/>
</head>
<body>
Blind Lorem Blind ipsum dolor Blind test sit amet, consectetur adipiscing elit. 
<a href="https://b.com"> Link to b </a>
</body>
</html>
```
```
<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="utf-8">
  <meta property="og:url" content="https://b.com"/>
</head>
<body>
Suspendisse potenti. Vivamus venenatis tellus non turpis bibendum, 
<a href="https://a.com"> Link to a </a>
blind sed congue urna varius. Suspendisse feugiat nisl ligula, quis malesuada felis hendrerit ut.
<a href="https://c.com"> Link to c </a>
</body>
</html>
```
```
<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="utf-8">
  <meta property="og:url" content="https://c.com"/>
</head>
<body>
Ut condimentum urna at felis sodales rutrum. Sed dapibus cursus diam, non interdum nulla tempor nec. Phasellus rutrum enim at orci consectetu blind
<a href="https://a.com"> Link to a </a>
</body>
</html>
```
위의 예를 가지고 각각의 점수를 계산해보자.

- 기본점수 및 외부 링크수는 아래와 같다.
  - a.com의 기본점수는 3, 외부 링크 수는 1개
  - b.com의 기본점수는 1, 외부 링크 수는 2개
  - c.com의 기본점수는 1, 외부 링크 수는 1개
- 링크점수는 아래와 같다.
  - a.com의 링크점수는 b.com으로부터 0.5점, c.com으로부터 1점
  - b.com의 링크점수는 a.com으로부터 3점
  - c.com의 링크점수는 b.com으로부터 0.5점
- 각 웹 페이지의 매칭 점수는 다음과 같다.
  - a.com : 4.5 점
  - b.com : 4 점
  - c.com : 1.5 점

따라서 매칭점수가 제일 높은 첫번째 웹 페이지의 index인 0을 리턴 하면 된다.

## 입출력 예 #2
- word : Muzi
- pages :
```
["<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://careers.kakao.com/interview/list\"/>\n</head>  \n<body>\n<a href=\"https://programmers.co.kr/learn/courses/4673\"></a>#!MuziMuzi!)jayg07con&&\n\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://www.kakaocorp.com\"/>\n</head>  \n<body>\ncon%\tmuzI92apeach&2<a href=\"https://hashcode.co.kr/tos\"></a>\n\n\t^\n</body>\n</html>"]
```
- pages는 다음과 같이 2개의 웹페이지에 해당하는 HTML 문자열이 순서대로 들어있다.
```
<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="utf-8">
  <meta property="og:url" content="https://careers.kakao.com/interview/list"/>
</head>
<body>
<a href="https://programmers.co.kr/learn/courses/4673"></a>#!MuziMuzi!)jayg07con&&

</body>
</html>
```
```
<html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="utf-8">
  <meta property="og:url" content="https://www.kakaocorp.com"/>
</head>
<body>
con%    muzI92apeach&2<a href="https://hashcode.co.kr/tos"></a>

    ^
</body>
</html>
```
- 기본점수 및 외부 링크수는 아래와 같다.
  - `careers.kakao.com/interview/list` 의 기본점수는 0, 외부 링크 수는 1개
  - `www.kakaocorp.com` 의 기본점수는 1, 외부 링크 수는 1개
- 링크점수는 아래와 같다.
  - `careers.kakao.com/interview/list` 의 링크점수는 0점
  - `www.kakaocorp.com` 의 링크점수는 0점
- 각 웹 페이지의 매칭 점수는 다음과 같다.
  - `careers.kakao.com/interview/list` : 0점
  - `www.kakaocorp.com` : 1 점

따라서 매칭점수가 제일 높은 두번째 웹 페이지의 index인 1을 리턴 하면 된다.

# 내 풀이 1
```python
from collections import defaultdict


def check_score(word, splited):
    count = 0
    from_, to_ = ord('a'), ord('z')
    while word in splited:
        print(word, splited)
        ind = splited.index(word)
        l_word = len(word)
        # 단어 단위로 비교
        if (ind==0 or (ind>0 and not from_<=ord(splited[ind-1])<=to_)) and (ind+l_word==len(splited) or (ind+l_word<len(splited) and not from_<=ord(splited[ind+l_word])<=to_)):
            print("count!")
            count+=1
        splited = splited[ind+l_word-1:]
    return count
    
    
def check_link(page_name, splited, linked_from):
    linked_from[splited.split("\"")[-2]].append(page_name)


def solution(word, pages):
    word = word.lower()
    # linked_from = {링크 : [나를 링크하는 페이지1, 페이지2, ...], ...}
    # page_infos = {링크 : [기본 점수, 외부 링크 수], ...}
    linked_from, page_infos = defaultdict(list), defaultdict(list)
    for page in pages:
        score, link = 0, 0
        for splited in page.split():
            if splited.startswith("content="):
                page_name = splited.split("\"")[-2]
                
            splited = splited.lower()
            if word in splited:
                score += check_score(word, splited[:])
            
            if splited.startswith("href="):
                link += 1
                check_link(page_name, splited, linked_from)  # "href="로 시작
        page_infos[page_name] = [score, link]
    
    print(linked_from)
    print(page_infos)
    
    final_scores = []
    for i, page in enumerate(page_infos):  # 전체 페이지들에 대해
        matching_score = page_infos[page][0]
        score_by_links = 0
        for linking_page in linked_from[page]:
            if linking_page in page_infos:
                info = page_infos[linking_page]
                score_by_links += info[0] / info[1]
        matching_score += score_by_links
        final_scores.append((-1*matching_score, i))
    
    print(final_scores)
    
    return sorted(final_scores)[0][1]
```
정확성  테스트
```
테스트 1 〉	통과 (1.31ms, 10.2MB)
테스트 2 〉	통과 (0.76ms, 10.2MB)
테스트 3 〉	통과 (0.73ms, 10.2MB)
테스트 4 〉	통과 (0.91ms, 10.1MB)
테스트 5 〉	통과 (1.11ms, 10.2MB)
테스트 6 〉	통과 (1.34ms, 10.2MB)
테스트 7 〉	통과 (1.65ms, 10.2MB)
테스트 8 〉	통과 (1.33ms, 10.5MB)
테스트 9 〉	실패 (1.43ms, 10.3MB)
테스트 10 〉	실패 (1.57ms, 10.2MB)
테스트 11 〉	통과 (1.21ms, 10.2MB)
테스트 12 〉	통과 (1.17ms, 10.2MB)
테스트 13 〉	통과 (1.00ms, 10.2MB)
테스트 14 〉	통과 (0.92ms, 10.2MB)
테스트 15 〉	통과 (0.67ms, 10.2MB)
테스트 16 〉	통과 (0.21ms, 10.2MB)
테스트 17 〉	통과 (0.33ms, 10.2MB)
테스트 18 〉	통과 (0.09ms, 10.3MB)
테스트 19 〉	통과 (0.21ms, 10.3MB)
테스트 20 〉	통과 (0.50ms, 10.2MB)
```
- 9, 10번을 제외하고 전부 통과했다.
  - 아마, 단어 등장 횟수를 찾는 부분에서 미세한 부분을 놓쳤던 것 같다.
- 결국 다른 사람의 풀이를 참고

# 다른 사람의 풀이
```python
import re
 
def solution(word, pages):
    total_score = []
    basic_score = {}
    exlink_cnt = {}
    to_me_link = {}
    
    for page in pages:
        title = re.search('<meta property="og:url" content="(https://\S+)"', page).group(1)
        basic_score[title] = 0
        exlink_cnt[title] = 0
        
        for find in re.findall('[a-zA-Z]+', page):
            if find.upper() == word.upper():
                basic_score[title] += 1
                
        for link in re.findall('<a href="(https://\S+)"', page):
            exlink_cnt[title] += 1
            if link in to_me_link:
                to_me_link[link].append(title)
            else:
                to_me_link[link] = [title]
                
    for curr in basic_score:
        link_score = 0
        if curr in to_me_link:
            for ex in to_me_link[curr]:
                link_score += basic_score[ex] / exlink_cnt[ex]
        total_score.append(basic_score[curr] + link_score)
        
    return total_score.index(max(total_score))
Colored
```
- 가장 핵심은 "**정규식**" 이였다. 정규식을 쓰면 긴 페이지에서 단어, 페이지 이름, 링크되 페이지 이름을 쉽게 찾을 수 있다.
- 나머지는 구현하기만 하면 됨
- 정규식을 급하게 공부하고 내 풀이에 적용해 보았다.

# 내 풀이 2
```python
from collections import defaultdict
import re


def check_score(word, page):
    count=0
    for find in re.findall('[a-zA-Z]+', page):
        if find.lower() == word.lower():
            count+=1
    return count


def solution(word, pages):
    word = word.lower()
    # linked_from = {링크 : [나를 링크하는 페이지1, 페이지2, ...], ...}
    # page_infos = {링크 : [기본 점수, 외부 링크 수], ...}
    linked_from, page_infos = defaultdict(list), defaultdict(list)
    for page in pages:
        page_name = re.search('<meta property="og:url" content="https://(\S+)"', page).group(1)
        
        score = check_score(word, page)
        
        links = re.findall('<a href="https://(\S+)">', page)
        num_link = len(links)
        for link in links:
            linked_from[link].append(page_name)
            
        page_infos[page_name] = [score, num_link]
    
    final_scores = []
    for i, page in enumerate(page_infos):  # 전체 페이지들에 대해
        matching_score = page_infos[page][0]
        score_by_links = 0
        for linking_page in linked_from[page]:
            if linking_page in page_infos:
                info = page_infos[linking_page]
                score_by_links += info[0] / info[1]
        matching_score += score_by_links
        final_scores.append((-1*matching_score, i))
    
    return sorted(final_scores)[0][1]
```
- 전략
  - 큰 틀은 비슷하다.
    - 각 페이지마다 연결된 링크, 단어 등장 횟수 를 체크해서 점수 계산 후 재분배하면 됨
  - 세 가지 정규식 등장
    1. 페이지의 이름: `'<meta property="og:url" content="https://(\S+)"'`
        - (\S+) : 공백이 아닌 문자들(\S)로 연속됨(+)
        - 이렇게 한 이유는, 링크 안에는 문자, 숫자, 특수문자 모두 등장하며 공백은 존재하지 않기 때문
    2. 검색어 찾기: `'[a-zA-Z]+'`
        - [a-zA-Z]+ : 문자로만[a-zA-Z] 연속됨(+)
        - 이렇게 한 이유는, 그저 word를 찾아버리면, 단어 중간에 끼여있는 문자까지도 찾아버릴 수 있음
        - 그래서, 일단 문자들로만 이루어진 단위로 분리한 후, 그것들이 단어와 일치하는지 확인함
    3. 외부 링크: `'<a href="https://(\S+)">'`
        - 1번과 같음
  - 이후, 각 페이지의 점수를 계산하면 됨
