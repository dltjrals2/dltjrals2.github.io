---
title: "[BFS/DFS] 감시 피하기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS, 풀이실패]

toc:  true
toc_sticky: true
show_date: true
read_time: false
use_math: false

date: 2021-09-30
last_modified_at: 2021-09-30
sitemap :
  changefreq : daily
  priority : 1.0
---

## 감시 피하기  

난이도 : 🌕🌕🌗  
푼횟수 : 🔴⚪⚪  

NxN 크기의 복도가 있다. 복도는 1x1 크기의 칸으로 나누어지며, 특정한 위치에는 선생님, 학생, 혹은 장애물이 위치할 수 있다. 현재 몇 명의 학생들은 수업시간에 몰래 복도로 빠져나왔는데, 복도로 빠져나온 학생들은 선생님의 감시에 들키지 않는 것이 목표이다.  

각 선생님들은 자신의 위치에서 상, 하, 좌, 우 4가지 방향으로 감시를 진행한다. 단, 복도에 장애물이 위치한 경우, 선생님은 장애물 뒤편에 숨어 있는 학생들은 볼 수 없다. 또한 선생님은 상, 하, 좌, 우 4가지 방향에 대하여, 아무리 멀리 있더라도 장애물로 막히기 전까지의 학생들은 모두 볼 수 있다고 가정하자.  

다음과 같이 3x3 크기의 복도의 정보가 주어진 상황을 확인해보자. 본 문제에서 위치 값을 나타낼 때는 (행,열)의 형태로 표현한다. 선생님이 존재하는 칸은 T, 학생이 존재하는 칸은 S, 장애물이 존재하는 칸은 O로 표시하였다. 아래 그림과 같이 (3,1)의 위치에는 선생님이 존재하며 (1,1), (2,1), (3,3)의 위치에는 학생이 존재한다. 그리고 (1,2), (2,2), (3,2)의 위치에는 장애물이 존재한다.  

![image](https://user-images.githubusercontent.com/37467408/135367168-7c3af480-5de0-463e-8920-cddb2846e91e.PNG)  

이 때 (3,3)의 위치에 존재하는 학생은 장애물 뒤편에 숨어 있기 때문에 감시를 피할 수 있다. 하지만 (1,1)과 (2,1)의 위치에 존재하는 학생은 선생님에게 들키게 된다.  

학생들은 복도의 빈 칸 중에서 장애물을 설치할 위치를 골라, 정확히 3개의 장애물을 설치해야 한다. 결과적으로 3개의 장애물을 설치하여 모든 학생들을 감시로부터 피하도록 할 수 있는지 계산하고자 한다. NxN 크기의 복도에서 학생 및 선생님의 위치 정보가 주어졌을 때, 장애물을 정확히 3개 설치하여 모든 학생들이 선생님들의 감시를 피하도록 할 수 있는지 출력하는 프로그램을 작성하시오.  

예를 들어 N=5일 때, 다음과 같이 선생님 및 학생의 위치 정보가 주어졌다고 가정하자.  

![image](https://user-images.githubusercontent.com/37467408/135367192-eb143b2b-1b6c-46b2-8936-38e8bcca6502.PNG)  

이 때 다음과 같이 3개의 장애물을 설치하면, 모든 학생들을 선생님의 감시로부터 피하도록 만들 수 있다.  

**<u>입력</u>**  
첫째 줄에 자연수 N이 주어진다. (3 ≤ N ≤ 6) 둘째 줄에 N개의 줄에 걸쳐서 복도의 정보가 주어진다. 각 행에서는 N개의 원소가 공백을 기준으로 구분되어 주어진다. 해당 위치에 학생이 있다면 S, 선생님이 있다면 T, 아무것도 존재하지 않는다면 X가 주어진다.  

단, 전체 선생님의 수는 5이하의 자연수, 전체 학생의 수는 30이하의 자연수이며 항상 빈 칸의 개수는 3개 이상으로 주어진다.  

**<u>출력</u>**  
첫째 줄에 정확히 3개의 장애물을 설치하여 모든 학생들을 감시로부터 피하도록 할 수 있는지의 여부를 출력한다. 모든 학생들을 감시로부터 피하도록 할 수 있다면 "YES", 그렇지 않다면 "NO"를 출력한다.  

**<u>예제 입력1</u>**  
5  
X S X X T  
T X S X X  
X X X X X  
X T X X X  
X X T X X  

**<u>예제 출력1</u>**  
YES  

**<u>예제 입력2</u>**  
4  
S S S T  
X X X X  
X X X X  
T T T X  

**<u>예제 출력2</u>**  
NO  

> 나의 풀이  

풀이 실패  

> 문제 해설  

장애물을 정확히 3개 설치하는 모든 경우를 확인하여, 매 경우마다 모든 학생을 감시로부터 피하도록 할 수 있는지의 여부를 출력해야 한다. 그렇다면 장애물을 정확히 3개 설치하는 모든 경우의 수는 얼마나 될지 생각해보자. 모든 조합의 수는 최악의 경우 64C3이 될 것이다. 따라서 모든 조합을 찾기 위해서 DFS 혹은 BFS를 이용해 모든 조합을 반환하는 함수를 작성하거나, 파이썬의 조합 라이브러리를 이용할 수 있다.  
또한, 정확히 3개의 장애물이 설치된 모든 조합마다, 선생님들의 위치 좌표를 하나씩 확인하고 각각 선생님의 위치에서 상, 하, 좌, 우를 확인하며 학생이 한 명이라도 감지되는지를 확인해야 한다. 이는 별도의 함수를 구현하면 편하다.  


> 문제 답안  

```python
from itertools import combinations

n = int(input())
board = []
teachers = []
spaces = []

for i in range(n):
    board.append(list(input().split()))
    for j in range(n):
        if board[i][j] == "T":
            teachers.append((i, j))
        elif board[i][j] == "X":
            spaces.append((i, j))

# True : 학생 발견, False : 학생 미발견
def watch(x, y, direction):
    # 왼쪽 방향 감시
    if direction == 0:
        while y >= 0:
            if board[x][y] == "S":
                return True
            elif board[x][y] == "O":
                return False
            y -= 1
    # 오른쪽 방향으로 감시
    elif direction == 1:
        while y < n:
            if board[x][y] == "S":
                return True
            elif board[x][y] == "O":
                return False
            y += 1
    # 위쪽 방향으로 감시
    elif direction == 2:
        while x >= 0:
            if board[x][y] == "S":
                return True
            elif board[x][y] == "O":
                return False
            x -= 1
    elif direction == 3:
        while x < n:
            if board[x][y] == "S":
                return True
            elif board[x][y] == "O":
                return False
            x += 1
    return False

def process():
    for x, y in teachers:
        for i in range(4):
            if watch(x, y, i):
                return True
    return False

find = False

for data in combinations(spaces, 3):
    for x, y in data:
        board[x][y] = "O"
    if not process():
        find = True
        break
    for x, y in data:
        board[x][y] = "X"

if find:
    print("YES")
else:
    print("NO")

```

<br>
기출 : 핵심유형  
링크 : <https://www.acmicpc.net/problem/18428>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
