---
title: "[BFS/DFS] 연구소"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS, 풀이실패]

toc:  true
toc_sticky: true

date: 2021-09-28
last_modified_at: 2021-09-28
---

## 연구소  

난이도 : ⭐⭐  
푼횟수 : 🔴⚪⚪  

인체에 치명적인 바이러스를 연구하던 연구소에서 바이러스가 유출되었다. 다행히 바이러스는 아직 퍼지지 않았고, 바이러스의 확산을 막기 위해서 연구소에 벽을 세우려고 한다.  

연구소는 크기가 N×M인 직사각형으로 나타낼 수 있으며, 직사각형은 1×1 크기의 정사각형으로 나누어져 있다. 연구소는 빈 칸, 벽으로 이루어져 있으며, 벽은 칸 하나를 가득 차지한다.   

일부 칸은 바이러스가 존재하며, 이 바이러스는 상하좌우로 인접한 빈 칸으로 모두 퍼져나갈 수 있다. 새로 세울 수 있는 벽의 개수는 3개이며, 꼭 3개를 세워야 한다.  

예를 들어, 아래와 같이 연구소가 생긴 경우를 살펴보자.  

2 0 0 0 1 1 0  
0 0 1 0 1 2 0  
0 1 1 0 1 0 0  
0 1 0 0 0 0 0  
0 0 0 0 0 1 1  
0 1 0 0 0 0 0  
0 1 0 0 0 0 0  

이때, 0은 빈 칸, 1은 벽, 2는 바이러스가 있는 곳이다. 아무런 벽을 세우지 않는다면, 바이러스는 모든 빈 칸으로 퍼져나갈 수 있다.  

2행 1열, 1행 2열, 4행 6열에 벽을 세운다면 지도의 모양은 아래와 같아지게 된다.  

2 1 0 0 1 1 0  
1 0 1 0 1 2 0  
0 1 1 0 1 0 0  
0 1 0 0 0 1 0  
0 0 0 0 0 1 1  
0 1 0 0 0 0 0  
0 1 0 0 0 0 0  

바이러스가 퍼진 뒤의 모습은 아래와 같아진다.  

2 1 0 0 1 1 2  
1 0 1 0 1 2 2  
0 1 1 0 1 2 2  
0 1 0 0 0 1 2  
0 0 0 0 0 1 1  
0 1 0 0 0 0 0  
0 1 0 0 0 0 0  

벽을 3개 세운 뒤, 바이러스가 퍼질 수 없는 곳을 안전 영역이라고 한다. 위의 지도에서 안전 영역의 크기는 27이다.  

연구소의 지도가 주어졌을 때 얻을 수 있는 안전 영역 크기의 최댓값을 구하는 프로그램을 작성하시오.  

**<u>입력</u>**  
- 첫째 줄에 지도의 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 8)  

- 둘째 줄부터 N개의 줄에 지도의 모양이 주어진다. 0은 빈 칸, 1은 벽, 2는 바이러스가 있는 위치이다. 2의 개수는 2보다 크거나 같고, 10보다 작거나 같은 자연수이다.  

- 빈 칸의 개수는 3개 이상이다.  

**<u>출력</u>**  
- 첫째 줄에 얻을 수 있는 안전 영역의 최대 크기를 출력한다.  

**<u>예제 입력1</u>**  
7 7  
2 0 0 0 1 1 0  
0 0 1 0 1 2 0  
0 1 1 0 1 0 0  
0 1 0 0 0 0 0  
0 0 0 0 0 1 1  
0 1 0 0 0 0 0  
0 1 0 0 0 0 0  

**<u>예제 출력1</u>**  
27  

**<u>예제 입력2</u>**  
4 6  
0 0 0 0 0 0  
1 0 0 0 0 2  
1 1 1 0 0 2  
0 0 0 0 0 2  

**<u>예제 출력2</u>**  
9  

> 나의 풀이  

풀이 실패  

> 문제 해설  

이 문제는 벽을 3개 설치하는 모든 경우의 수를 다 계산해야 한다. 전체 맵의 크기가 8 x 8이므로, 벽을 설치할 수 있는 모든 조합의 수는 최악의 경우(바이러스가 하나도 존재하지 않는 경우) 64{C}3 이다. 이는 100,000보다도 작은 수이므로, 모든 경우의 수를 고려해도 제한 시간 안에 문제를 해결할 수 있다.  
모든 조합을 계산할 때는 파이썬의 조합 라이브러리를 이용하거나, DFS 혹은 BFS를 이용하여 해결할 수 있다. 따라서 벽의 개수가 3개가 되는 모든 조합을 찾은 뒤에 그러한 조합에서 안전 영역의 크기를 계산하면 된다. 안전 영역의 크기를 구하는 것 또한 DFS나 BFS를 이용하여 계산할 수 있다.  
결과적으로 여기서는 가능한 모든 경우의 수를 계산하되, 안전 영역을 계산할 때 DFS나 BFS를 적절히 이용해야 한다는 점이 특징이다.

> 문제 답안

```python
# n : 세로의 크기, m : 가로의 크기
# 0 : 빈칸, 1 : 빈 벽, 2 : 바이러스
n, m = map(int, input().split())
data = []  # 초기 맵 리스트
temp = [[0] * m for _ in range(n)]  # 벽을 설치한 뒤의 맵 리스트

for _ in range(n):
    data.append(list(map(int, input().split())))

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

result = 0


def virus(x, y):
    for i in range(4):
        nx = x + dx[i]
        ny = y + dy[i]
        if nx >= 0 and nx < n and ny >= 0 and ny < m:
            if temp[nx][ny] == 0:
                temp[nx][ny] = 2
                virus(nx, ny)


def get_score():
    score = 0
    for i in range(n):
        for j in range(m):
            if temp[i][j] == 0:
                score += 1
    return score


def dfs(count):
    global result
    if count == 3:
        for i in range(n):
            for j in range(m):
                temp[i][j] = data[i][j]
        for i in range(n):
            for j in range(m):
                if temp[i][j] == 2:
                    virus(i, j)

        result = max(result, get_score())
        return

    for i in range(n):
        for j in range(m):
            if data[i][j] == 0:
                data[i][j] = 1
                count += 1
                dfs(count)
                data[i][j] = 0
                count -= 1


dfs(0)
print(result)
```


<br>
기출 : 핵심 유형  
링크 : <https://www.acmicpc.net/problem/14502>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊