---
title: "[Inplementation] 게임 개발"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation]

toc:  true
toc_sticky: true

date: 2021-06-15
last_modified_at: 2021-06-15
---
## 게임 개발

난이도 : ⭐⭐

현민이는 게임 캐릭터가 맵 안에서 움직이는 시스템을 개발 중이다. 캐릭터가 있는 장소는 1 X 1 크기의 정사각형으로 이뤄진 N X M 크기의 직사각형으로, 각각의 칸은 육지 또는 바다이다. 캐릭터는 동서남북 중 한 곳을 바라본다.  
맵의 각 칸은 (A, B)로 나타낼 수 있고, A는 북쪽으로부터 떨어진 칸의 개수, B는 서쪽으로부터 떨어진 칸의 개수이다. 캐릭터는 상하좌우로 움직일 수 있고, 바다로 되어 있는 공간에는 갈 수 없다. 캐릭터의 움직임을 설명하기 위해 정해 놓은 메뉴얼은 이러하다.  

<ol>1. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향(반시계 방향으로 90도 회전)부터 차례대로 갈 곳을 정한다.</ol>  
<ol>2. 캐릭터의 바로 왼쪽 방향에 아직 가보지 않은 칸이 존재한다면, 왼쪽 방향으로 회전한 다음 왼쪽으로 한 칸을 전진한다. 왼쪽 방향에 가보지 않은 칸이 없다면, 왼쪽 방향으로 회전만 수행하고 1단계로 돌아간다.</ol>
<ol>3. 만약 네 방향 모두 이미 가본 칸이거나 바다로 되어 있는 칸인 경우에는, 바라보는 방향을 유지한 채로 한 칸 뒤로 가고 1단계로 돌아간다. 단, 이때 뒤쪽 방향이 바다인 칸이라 뒤로 갈 수 없는 경우에는 움직임을 멈춘다.</ol>  

현민이는 위 과정을 반복적으로 수행하면서 캐릭터의 움직임에 이상이 있는지 테스트하려고 한다. 메뉴얼에 따라 캐릭터를 이동시킨 뒤에, 캐릭터가 방문한 칸의 수를 출력하는 프로그램을 만드시오.  


**<u>입력 조건</u>**  
- 첫째 줄에 맵의 세로 크기 N과 가로 크기 M을 공백으로 구분하여 입력한다. (3 <= N, M <= 50)  
- 둘째 줄에 게임 캐릭터가 있는 칸의 좌표 (A, B)와 바라보는 방향 d가 각각 서로 공백으로 구분하여 주어진다. 방향 d의 값으로는 다음과 같이 4가지가 존재한다.  
<ul>- 0: 북쪽</ul>  
<ul>- 1: 동쪽</ul>  
<ul>- 2: 남쪽</ul>  
<ul>- 3: 서쪽</ul>  
- 셋째 줄부터 맵이 육지인지 바다인지에 대한 정보가 주어진다. N개의 줄에 맵의 상태가 북쪽부터 남쪽 순서대로, 각 줄의 데이터는 서쪽부터 동쪽 순서대로 주어진다. 맵의 외곽은 항상 바다로 되어 있다.  
<ul>- 0: 육지</ul>  
<ul>- 1: 바다</ul>  
- 처음에 게임 캐릭터가 위치한 칸의 상태는 항상 육지이다.  

**<u>출력 조건</u>**  
- 첫째 줄에 이동을 마친 후 캐릭터가 방문한 칸의 수를 출력한다.  

**<u>입력 예시</u>**  
4 4       # 4 x 4 맵 생성  
1 1 0     # (1, 1)에 북쪽(0)을 바라보고 서 있는 캐릭터  
1 1 1 1   # 첫 줄은 모두 바다  
1 0 0 1   # 둘째 줄은 바다/육지/육지/바다  
1 1 0 1   # 셋째 줄은 바다/바다/육지/바다  
1 1 1 1   # 넷째 줄은 모두 바다  

**<u>출력 예시</u>**  
3  

> 나의 풀이

푸는 것에 실패해서 추후에 다시 풀기.

> 문제 해설  

`전형적인 시뮬레이션 문제`이다.  
일반적으로 방향(direction)을 설정해서 이동하는 문제 유형에서는 `dx, dy`라는 별도의 리스트를 만들어 방향을 정하는 것이 효과적이다. 이처럼 코드를 작성하면, 반복문을 이용하여 모든 방향을 차례대로 확인할 수 있다는 점에서 유용하다.  
그리고, 답안 예시 코드에서는 리스트 컴프리헨션 문법을 사용해 2차원 리스트를 초기화했다. 파이썬에서 2차원 리스트를 선언할 때는 컴프리헨션을 이용하는 것이 효율적이다.  
왼쪽으로 회전하는 함수 turn_left()에서 global 키워드를 사용했는데, 이는 정수형 변수인 direction 변수가 함수 바깥에서 선언된 전역변수이기 때문이다.  

**📌 리스트 컴프리 헨션이란?**
{: .notice--primary}  
리스트 컴프리헨션은 리스트를 초기화하는 방법이다. 대괄호([])안에 조건문과 반복문을 넣는 방식으로 리스트를 초기화할 수 있다.  

```python
# 0부터 19까지의 수 중에서 홀수만 포함되는 리스트
array = [i for i in range(20) if i % 2 == 1]

# 1부터 9까지의 수의 제곱 값을 포함하는 리스트
array = [i * i for i in range(1, 10)]

# N x M 크기의 2차원 리스트 초기화
n = 3
m = 4
array = [[0] * m for _ in range(n)]
```

언더바(_)는 반복을 수행하되 반복을 위한 변수의 값을 무시하고자 할 때 언더바를 자주 사용한다.  

```python
# 이럴 경우는 사용
summary = 0
for i in range(1, 10):
  summary += i

# 이럴 경우는 언더바(_) 사용
for _ in range(5):
  print("Hello World")
```

> 답안 예시

```python
# N, M을 공백으로 구분하여 입력받기
n, m = map(int, input().split())

# 방문한 위치를 저장하기 위한 맵을 생성하여 0으로 초기화
d = [[0] * m for _ in range(n)]
# 현재 캐릭터의 X 좌표, Y 좌표, 방향을 입력받기
x, y, direction = map(int, input().split())
d[x][y] = 1 # 현재 좌표 방문 처리

# 전체 맵 정보를 입력받기
array = []
for i in range(n):
    array.append(list(map(int, input().split())))

# 북, 동, 남, 서 방향 정의
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

# 왼쪽으로 회전
def turn_left():
    global direction
    direction -= 1
    if direction == -1:
        direction = 3

# 시뮬레이션 시작
count = 1
turn_time = 0
while True:
    # 왼쪽으로 회전
    turn_left()
    nx = x + dx[direction]
    nx = y + dy[direction]
    # 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
    if d[nx][ny] == 0 and array[nx][ny] == 0:
        d[nx][ny] = 1
        x = nx
        y = ny
        count += 1
        turn_time = 0
        continue
    # 회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
    else:
        turn_time += 1
    # 네 방향 모두 갈 수 없는 경우
    if turn_time == 4:
        nx = x - dx[direction]
        ny = y - dy[direction]
        # 뒤로 갈 수 있다면 이동하기
        if array[nx][ny] == 0:
            x = nx
            y = ny
        # 뒤가 바다로 막혀있는 경우
        else:
            break
        turn_time = 0

# 정답 출력
print(count)
```

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
