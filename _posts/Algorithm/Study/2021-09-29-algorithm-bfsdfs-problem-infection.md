---
title: "[BFS/DFS] 경쟁적 전염"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS, 풀이참고]

toc:  true
toc_sticky: true

date: 2021-09-29
last_modified_at: 2021-09-29
---

## 경쟁적 전염  

난이도 : ⭐⭐  
푼횟수 : 🟡⚪⚪  

NxN 크기의 시험관이 있다. 시험관은 1x1 크기의 칸으로 나누어지며, 특정한 위치에는 바이러스가 존재할 수 있다. 모든 바이러스는 1번부터 K번까지의 바이러스 종류 중 하나에 속한다.  

시험관에 존재하는 모든 바이러스는 1초마다 상, 하, 좌, 우의 방향으로 증식해 나간다. 단, 매 초마다 번호가 낮은 종류의 바이러스부터 먼저 증식한다. 또한 증식 과정에서 특정한 칸에 이미 어떠한 바이러스가 존재한다면, 그 곳에는 다른 바이러스가 들어갈 수 없다.  

시험관의 크기와 바이러스의 위치 정보가 주어졌을 때, S초가 지난 후에 (X,Y)에 존재하는 바이러스의 종류를 출력하는 프로그램을 작성하시오. 만약 S초가 지난 후에 해당 위치에 바이러스가 존재하지 않는다면, 0을 출력한다. 이 때 X와 Y는 각각 행과 열의 위치를 의미하며, 시험관의 가장 왼쪽 위에 해당하는 곳은 (1,1)에 해당한다.  

예를 들어 다음과 같이 3x3 크기의 시험관이 있다고 하자. 서로 다른 1번, 2번, 3번 바이러스가 각각 (1,1), (1,3), (3,1)에 위치해 있다. 이 때 2초가 지난 뒤에 (3,2)에 존재하는 바이러스의 종류를 계산해보자.  

![image](https://user-images.githubusercontent.com/37467408/135186505-6721fa2e-81f7-43db-a5b3-88d3f783a1e0.PNG)  

1초가 지난 후에 시험관의 상태는 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/135186558-b52f2e7b-11f1-4b35-8561-94b1d5d06026.PNG)  

2초가 지난 후에 시험관의 상태는 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/135186611-5c84a002-5859-45bd-b83e-0d79bd444bc2.PNG)  

결과적으로 2초가 지난 뒤에 (3, 2)에 존재하는 바이러스의 종류는 3번 바이러스다. 따라서 3을 출력하면 정답이다.  

**<u>입력</u>**  
첫째 줄에 자연수 N, K가 공백을 기준으로 구분되어 주어진다. (1 ≤ N ≤ 200, 1 ≤ K ≤ 1,000) 둘째 줄부터 N개의 줄에 걸쳐서 시험관의 정보가 주어진다. 각 행은 N개의 원소로 구성되며, 해당 위치에 존재하는 바이러스의 번호가 공백을 기준으로 구분되어 주어진다. 단, 해당 위치에 바이러스가 존재하지 않는 경우 0이 주어진다. 또한 모든 바이러스의 번호는 K이하의 자연수로만 주어진다. N+2번째 줄에는 S, X, Y가 공백을 기준으로 구분되어 주어진다. (0 ≤ S ≤ 10,000, 1 ≤ X, Y ≤ N)  

**<u>출력</u>**  
S초 뒤에 (X,Y)에 존재하는 바이러스의 종류를 출력한다. 만약 S초 뒤에 해당 위치에 바이러스가 존재하지 않는다면, 0을 출력한다.  

**<u>예제 입력1</u>**  
3 3  
1 0 2  
0 0 0  
3 0 0  
2 3 2  

**<u>예제 출력1</u>**  
3  

**<u>예제 입력2</u>**  
3 3  
1 0 2  
0 0 0  
3 0 0  
1 2 2  

**<u>예제 출력2</u>**  
0  

> 나의 풀이  

```python
from collections import deque

# n, k : 크기, 바이러스 종류
n, k = map(int, input().split())

graph = []
data = []

for i in range(n):
    graph.append(list(map(int, input().split())))
    for j in range(n):
        # 바이러스가 존재하는 경우
        if graph[i][j] != 0:
            data.append((graph[i][j], 0, i, j))

# 오름차순 정렬
data.sort()
q = deque(data)

target_s, target_x, target_y = map(int, input().split())

# 상, 하, 좌, 우
dx = [0 ,0, -1, 1]
dy = [1, -1, 0, 0]

while q:
    virus_kind, time, x, y = q.popleft()
    if time == target_s:
        break

    for i in range(4):
        nx = x + dx[i]
        ny = y + dy[i]
        if nx < n and nx >= 0 and ny < n and ny >= 0:
            if graph[nx][ny] == 0:
                graph[nx][ny] = virus_kind
                q.append((virus_kind, time + 1, nx, ny))

print(graph[target_x - 1][target_y - 1])
```  

> 문제 해설  

너비 우선 탐색으로 해결할 수 있다. 다만, 문제에 나와 있는 대로 각 바이러스가 낮은 번호부터 증식한다는 점을 기억하자. 낮은 번호부터 증식하므로, 초기에 Queue에 원소를 삽입할 때 낮은 바이러스의 번호부터 삽입해야 한다. 이후에 너비 우선 탐색을 수행하며 방문하지 않은 위치를 차례대로 방문하도록 하면 된다.  

> 문제 답안  


<br>
기출 : 핵심 유형  
링크 : <https://www.acmicpc.net/problem/18405>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
