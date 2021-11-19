---
title: "[Shortest Path] 플로이드"

categories:
  - Algorithm
tags:
  - [Shortest Path, Python]

toc:  true
toc_sticky: true

date: 2021-11-19
last_modified_at: 2021-11-19
---

## 플로이드  

난이도 : 🌕🌗   
푼횟수 : 🟢⚪⚪  

n(1 <= n <= 100)개의 도시가 있고, 한 도시에서 출발하여 다른 도시에 도착하는 m(1 <= m <= 100,000)개의 버스가 있습니다. 각 버스는 한 번 사용할 때 필요한 비용이 있습니다. 모든 도시의 쌍(A, B)에 대해서 도시 A에서 B로 가는 데 필요한 비용의 최솟값을 구하는 프로그램을 작성하세요.  

**<u>입력 조건</u>**  
- 첫째 줄에 도시의 개수 n(1 <= n <= 100)이 주어집니다.  
- 둘째 줄에는 버스의 개수 m(1 <= m <= 100,000)이 주어집니다.  
- 셋째 줄부터 m + 2줄까지 다음과 같은 버스의 정보가 주어집니다. 먼저 처음에서 그 버스의 출발 도시의 번호가 주어집니다. 버스의 정보는 버스의 시작 도시 a, 도착 도시 b, 한 번 타는데 필요한 비용 c로 이루어져 있습니다. 시작 도시와 도착 도시가 같은 경우는 없습니다. 비용 100,000보다 작거나 같은 자연수입니다.  
- 시작 도시와 도착 도시를 연결하는 노선은 하나가 아닐 수 있습니다.  

**<u>출력 조건</u>**  
- n개의 줄을 출력해야 한다. i번째 줄에 출력하는 j번째 숫자는 도시 i에서 j로 가는데 필요한 최소 비용이다. 만약, i에서 j로 갈 수 없는 경우에는 그 자리에 0을 출력한다.  

**<u>입력 예시 1</u>**  
5  
14  
1 2 2  
1 3 3  
1 4 1  
1 5 10  
2 4 2  
3 4 1  
3 5 1  
4 5 3  
3 5 10  
3 1 8  
1 4 2  
5 1 7  
3 4 2  
5 2 4  

**<u>출력 예시 1</u>**  
0 2 3 1 4  
12 0 15 2 5  
8 5 0 1 1  
10 7 13 0 3  
7 4 10 6 0  

> 나의 풀이  

```python
INF = int(1e9)

n = int(input()) # 노드
m = int(input()) # 간선

graph = [[INF] * (n + 1) for _ in range(n + 1)]

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

for _ in range(m):
    a, b, c = map(int, input().split())
    if graph[a][b] > c:
        graph[a][b] = c

for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if graph[a][b] == INF:
            print(0, end = " ")
        else:
            print(graph[a][b], end = " ")
    print()
```

> 문제 해설  

이 문제는 전형적인 최단 경로 문제이다. 다만 문제의 입력 조건에 따르면, 시작 도시 A와 도착 도시 B를 연결하는 간선이 여러 개일 수 있다는 점을 알 수 있다. 예를 들어 문제에서 주어진 입력 예시를 확인해보면 도시 3에서 도시 4로 연결된 간선이 2개이다. 각각의 비용은 1과 2인데, 이 경우에는 비용이 짧은 간선(비용이 1인 간선)만 고려하면 된다.  
또한 도시의 개수 n이 100 이하의 정수이므로, 플로이드 워셜 알고리즘을 이용하는 것이 효과적이다. 그러므로 초기에 간선 정보를 입력받을 때 '가장 짧은 간선' 장보만 저장한 뒤에, 플로이드 워셜 알고리즘을 수행하여 결과를 출력하면 된다.  

> 문제 답안  

```python
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수와 간선의 개수를 입력받기
n = int(input())
m = int(input())
# 2차원 리스트(그래프 표현)을 만들고, 모든 값을 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(n + 1):
    for b in range(n + 1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대한 정보를 입력받아, 그 값으로 초기화
for _ in range(m):
    # A에서 B로 가는 비용은 C라고 설정
    a, b, c = map(int, input().split())
    # 가장 짧은 간선 정보만 저장
    if c < graph[a][b]:
        graph[a][b] = c

# 점화식에 따라 플로이드 워셜 알고리즘을 수행
for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

# 수행된 결과를 출력
for a in range(1, n + 1):
    for b in range(1, n + 1):
        # 도달할 수 없는 경우, 0을 출력
        if graph[a][b] == INF:
            print(0, end = " ")
        # 도달할 수 있는 경우, 거리를 출력
        else:
            print(graph[a][b], end = " ")

    print()
```

<br>
기출 : 핵심 유형  
링크 : <https://www.acmicpc.net/problem/11404>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊