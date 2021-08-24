---
title: "[Implementation] 뱀"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation, 풀이실패]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-07-20
last_modified_at: 2021-08-24
sitemap :
  changefreq : daily
  priority : 1.0
---

## 뱀  

난이도 : ⭐⭐  
푼횟수 : 🔴⚪⚪  

'Dummy'라는 도스 게임이 있습니다. 이 게임에는 뱀이 나와서 기어 다니는데, 사과를 먹으면 뱀 길이가 늘어납니다. 뱀이 이리저리 기어 다니다가 벽 또는 자기 자신의 몸과 부딪히면 게임이 끝납니다.  
게임은 N x N 정사각 보드 위에서 진행되고, 몇몇 칸에는 사과가 놓아져 있습니다. 보드의 상하좌우 끝에는 벽이 있습니다. 게임을 시작할 때 뱀은 맨 위 좌측에 위치하고 뱀의 길이는 1입니다.  
뱀은 처음에 오른쪽을 향합니다.  
뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따릅니다.  
- 먼저 뱀은 몸길이를 늘려 머리를 다음 칸에 위치시킵니다.  
- 만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않습니다.  
- 만약 이동한 칸에 사과가 없다면, 몸 길이를 줄여서 꼬리가 위치한 칸을 비워줍니다. 즉, 몸길이는 변하지 않습니다.  
사과의 위치와 뱀의 이동 경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하세요.  

**<u>입력 조건</u>**  
- 첫째 줄에 보드의 크기 N이 주어집니다. (2 <= N <= 100) 다음 줄에 사과의 개수 K가 주어집니다. (0 <= K <= 100)  
- 다음 K개의 줄에는 사과의 위치가 주어지는데, 첫 번째 정수는 행, 두 번째 정수는 열 위치를 의미합니다. 사과의 위치는 모두 다르며, 맨 위 맨 좌측(1행 1열)에는 사과가 없습니다.  
- 다음 줄에는 뱀의 방향 변환 횟수 L이 주어집니다. (1 <= L <= 100)  
- 다음 L개의 줄에는 뱀의 방향 변환 정보가 주어지는데, 정수 X와 문자 C로 이루어져 있으며, 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L')또는 오른쪽(C가 'D')으로 90도 방향을 회전시킨다는 뜻입니다. X는 10,000이하의 양의 정수이며, 방향 전화 정보는 X가 증가하는 순으로 주어집니다.  

**<u>출력 조건</u>**  
- 첫째 줄에 게임이 몇 초에 끝나는지 출력합니다.  

**<u>입력 예시1</u>**  
6  
3  
3 4  
2 5  
5 3  
3  
3 D  
15 L  
17 D  

**<u>출력 예시1</u>**  
9  

**<u>입력 예시2</u>**  
10  
4  
1 2  
1 3  
1 4  
1 5  
4  
8 D  
10 D  
11 D  
13 L  

**<u>출력 예시2</u>**  
21  

**<u>입력 예시3</u>**  
10  
5  
1 5  
1 3  
1 2  
1 6  
1 7  
4  
8 D  
10 D  
11 D  
13 L  

**<u>출력 예시3</u>**  
13  

> 나의 풀이  

> 문제 해설  

전형적인 시뮬레이션 유형이다. 2차원 배열상의 맵에서 뱀이 이동하도록 해야 하므로 2차원 배열상의 특정위치에서 동, 남, 서, 북 위치로 이동하는 기능을 구현해야 한다. 이 문제의 경우, 뱀이 처음에는 오른쪽(동쪽)을 바라보고 있다는 점을 고려하자. 더불어 뱀의 머리가 뱀의 몸에 닿는 경우에도 종료해야 하므로, `매 시점마다 뱀이 존재하는 위치를 항상 2차원 리스트에 기록`해야 한다.  
이러한 시뮬레이션 유형의 문제는 그림으로 그려보는 것이 좋다.  

![image](https://user-images.githubusercontent.com/37467408/130561447-645a9852-23ed-4d02-9f6c-e4165b78c94d.PNG)  

![image](https://user-images.githubusercontent.com/37467408/130561547-f848e9de-5150-4255-8e1e-367cc7c3f4b6.PNG)

> 문제 답안  

```python
n = int(input())
k = int(input())
data = [[0] * (n + 1) for _ in range(n + 1)] # 맵 정보
info = [] # 방향 회전 정보

# 맵 정보(사과 있는 곳은 1로 표시)
for _ in range(k):
    a, b = map(int, input().split())
    data[a][b] = 1

# 방향 회전 정보 입력
l = int(input())
for _ in range(l):
    x, c = input().split()
    info.append((int(x), c))

# 처음에는 오른쪽을 보고 있으므로(동, 남, 서, 북)
dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]

def turn(direction, c):
    if c == "L":
        direction = (direction - 1) % 4
    else:
        direction = (direction + 1) % 4
    return direction

def simulate():
    x, y = 1, 1 # 뱀의 머리 위치
    data[x][y] = 2 # 뱀이 존재하는 위치는 2로 표시
    direction = 0 # 처음에는 동쪽을 보고 있음
    time = 0 # 시작한 뒤에 지난 '초' 시간
    index = 0 # 다음에 회전할 정보
    q = [(x, y)] # 뱀이 차지하고 있는 위치 정보 (꼬리가 앞쪽)
    while True:
        nx = x + dx[direction]
        ny = y + dy[direction]
        # 맵 범위 안에 있고, 뱀의 몸통이 없는 위치라면
        if 1 <= nx and nx <= n and 1 <= ny and ny <= n and data[nx][ny] != 2:
            # 사과가 없다면 이동 후에 꼬리 제거
            if data[nx][ny] == 0:
                data[nx][ny] = 2
                q.append((nx, ny))
                px, py = q.pop(0)
                data[px][py] = 0
            # 사과가 있다면 이동 후에 꼬리 그대로 두기
            if data[nx][ny] == 1:
                data[nx][ny] = 2
                q.append((nx, ny))
        # 벽이나 뱀의 몸통과 부딪혔다면
        else:
            time += 1
            break
        x, y = nx, ny # 다음 위치로 머리를 이동
        time += 1
        if index < l and time == info[index][0]: # 회전할 시간인 경우 회전
            direction = turn(direction, info[index][1])
            index += 1
    return time
print(simulate())
```


<br>
기출 : 삼성전자 SW 역량테스트  
링크 : <https://www.acmicpc.net/problem/3190>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
