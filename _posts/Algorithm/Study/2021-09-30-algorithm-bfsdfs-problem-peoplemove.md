---
title: "[BFS/DFS] 인구 이동"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS, 풀이실패]

toc:  true
toc_sticky: true

date: 2021-09-30
last_modified_at: 2021-09-30
---

## 인구 이동  

난이도 : 🌕🌕  
푼횟수 : 🔴⚪⚪  

N×N크기의 땅이 있고, 땅은 1×1개의 칸으로 나누어져 있다. 각각의 땅에는 나라가 하나씩 존재하며, r행 c열에 있는 나라에는 A[r][c]명이 살고 있다. 인접한 나라 사이에는 국경선이 존재한다. 모든 나라는 1×1 크기이기 때문에, 모든 국경선은 정사각형 형태이다.  

오늘부터 인구 이동이 시작되는 날이다.  

인구 이동은 하루 동안 다음과 같이 진행되고, 더 이상 아래 방법에 의해 인구 이동이 없을 때까지 지속된다.  

- 국경선을 공유하는 두 나라의 인구 차이가 L명 이상, R명 이하라면, 두 나라가 공유하는 국경선을 오늘 하루 동안 연다.  
- 위의 조건에 의해 열어야하는 국경선이 모두 열렸다면, 인구 이동을 시작한다.  
- 국경선이 열려있어 인접한 칸만을 이용해 이동할 수 있으면, 그 나라를 오늘 하루 동안은 연합이라고 한다.  
- 연합을 이루고 있는 각 칸의 인구수는 (연합의 인구수) / (연합을 이루고 있는 칸의 개수)가 된다. 편의상 소수점은 버린다.  
- 연합을 해체하고, 모든 국경선을 닫는다.  

각 나라의 인구수가 주어졌을 때, 인구 이동이 며칠 동안 발생하는지 구하는 프로그램을 작성하시오.  

**<u>입력</u>**  
첫째 줄에 N, L, R이 주어진다. (1 ≤ N ≤ 50, 1 ≤ L ≤ R ≤ 100)  

둘째 줄부터 N개의 줄에 각 나라의 인구수가 주어진다. r행 c열에 주어지는 정수는 A[r][c]의 값이다. (0 ≤ A[r][c] ≤ 100)  

인구 이동이 발생하는 일수가 2,000번 보다 작거나 같은 입력만 주어진다.  

**<u>출력</u>**  
인구 이동이 며칠 동안 발생하는지 첫째 줄에 출력한다.  

**<u>예제 입력1</u>**  
2 20 50  
50 30  
20 40  

**<u>예제 출력1</u>**  
1  

초기 상태는 아래와 같다.  

![image](https://user-images.githubusercontent.com/37467408/135371553-8833591e-2cbf-477d-a2d6-ccc934c76ef1.PNG)  

L = 20, R = 50 이기 때문에, 모든 나라 사이의 국경선이 열린다. (열린 국경선은 점선으로 표시)  

![image](https://user-images.githubusercontent.com/37467408/135371585-423a0ccd-e9ae-4aff-96b1-0bd4a0eb2a93.PNG)  

연합은 하나 존재하고, 연합의 인구는 (50 + 30 + 20 + 40) 이다. 연합의 크기가 4이기 때문에, 각 칸의 인구수는 140/4 = 35명이 되어야 한다.  

![image](https://user-images.githubusercontent.com/37467408/135371616-72a3025c-2dd6-481a-b96f-d69b3d56fb62.PNG)  

**<u>예제 입력2</u>**  
2 40 50  
50 30  
20 40  

**<u>예제 출력2</u>**  
0  

경계를 공유하는 나라의 인구 차이가 모두 L보다 작아서 인구 이동이 발생하지 않는다.  

**<u>예제 입력3</u>**  
2 20 50  
50 30  
30 40  

**<u>예제 출력3</u>**  
1  

초기 상태는 아래와 같다.  

![image](https://user-images.githubusercontent.com/37467408/135371694-7c450632-42a4-4fbd-9f2f-24590d1e900c.PNG)  

L = 20, R = 50이기 때문에, 아래와 같이 국경선이 열린다.  

![image](https://user-images.githubusercontent.com/37467408/135371727-dda1bb19-f039-48af-8951-5332b7c8ecff.PNG)  

인구 수는 합쳐져있는 연합의 인구수는 (50+30+30) / 3 = 36 (소수점 버림)이 되어야 한다.  

![image](https://user-images.githubusercontent.com/37467408/135371820-cbc7da53-54c9-4142-8840-abb9e6e69685.PNG)  

> 나의 풀이  

풀이 실패  

> 문제 해설  

DFS/BFS의 전형적인 문제로, 모든 나라의 위치에서 상, 하, 좌, 우로 국경선을 열 수 있는지를 확인해야 한다. 따라서 모든 나라의 위치에서 DFS 혹은 BFS를 수행하여 인접한 나라의 인구수를 확인한 뒤에, 가능하다면 국경선을 열고 인구 이동 처리를 진행하면 된다.  

> 문제 답안  

```python
from collections import deque

n, l, r = map(int, input().split())
graph = list()
for _ in range(n):
    graph.append(list(map(int, input().split())))

dx = [0, 0, -1, 1]
dy = [1, -1, 0, 0]

result = 0

# 특정 위치에서 출발하여 모든 연합을 체크한 뒤에 데이터 갱신
def process(x, y, index):
    # (x, y)의 위치와 연결된 나라(연합) 정보를 담는 리스트
    united = []
    united.append((x, y))
    # 너비 우선 탐색(BFS)을 위한 큐 자료구조 정의
    q = deque()
    q.append((x, y))
    union[x][y] = index # 현재 연합의 번호 할당
    summary = graph[x][y] # 현재 연합의 전체 인구 수
    count = 1 # 현재 연합의 국가 수
    # 큐가 빌 때까지 반복(BFS)
    while q:
        x, y = q.popleft()
        # 현재 위치에서 4가지 방향을 확인하며
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            # 바로 옆에 있는 나라를 확인하여
            if 0 <= nx < n and 0 <= ny < n and union[nx][ny] == -1:
                # 옆에 있는 나라와 인구 차이가 L명 이상, R명 이하라면
                if l <= abs(graph[nx][ny] - graph[x][y]) <= r:
                    q.append((nx, ny))
                    # 연합에 추가
                    union[nx][ny] = index
                    summary += graph[nx][ny]
                    count += 1
                    united.append((nx, ny))
    # 연합 국가끼리 인구를 분배
    for i, j in united:
        graph[i][j] = summary // count
    return count

total_count = 0

while True:
    union = [[-1] * n for _ in range(n)]
    index = 0
    for i in range(n):
        for j in range(n):
            if union[i][j] == -1: # 해당 나라가 아직 처리되지 않았다면
                process(i, j, index)
                index += 1
    # 모든 인구 이동이 끝난 경우
    if index == n * n:
        break
    total_count += 1

# 인구 이동 횟수 출력
print(total_count)
```


<br>
기출 : 삼성전자 SW 역량테스트  
링크 : <https://www.acmicpc.net/problem/16234>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
