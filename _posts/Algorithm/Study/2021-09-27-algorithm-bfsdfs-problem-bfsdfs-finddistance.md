---
title: "[BFS/DFS] 특정 거리의 도시 찾기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS, BFS, 풀이참고]

toc:  true
toc_sticky: true

date: 2021-09-27
last_modified_at: 2021-09-27
---

## 특정 거리의 도시 찾기  

난이도 : ⭐⭐⭐  
푼횟수 : 🟡⚪⚪  

어떤 나라에는 1 ~ N번까지의 도시와 M개의 단방향 도로가 존재합니다. 모든 도로의 거리는 1입니다. 이때 특정한 도시 X로부터 출발하여 도달할 수 있는 모든 도시 중에서, 최단 거리가 정확히 K인 모든 도시의 번호를 출력하는 프로그램을 작성하세요. 또한 출발 도시 X에서 출발 도시 X로 가는 최단 거리는 항상 0이라고 가정합니다.  
예를 들어 N=4, K=2, X=1일 때 다음과 같이 그래프가 구성되어 있다고 가정하자.  

![image](https://user-images.githubusercontent.com/37467408/134837822-d695474a-d779-46fd-8c5f-3c46bace79fd.PNG)  

이 때 1번 도시에서 출발하여 도달할 수 있는 도시 중에서, 최단 거리가 2인 도시는 4번 도시 뿐이다.  2번과 3번 도시의 경우, 최단 거리가 1이기 때문에 출력하지 않는다.  

**<u>입력 조건</u>**  
- 첫째 줄에 도시의 개수 N, 도로의 개수 M, 거리 정보 K, 출발 도시의 번호 X가 주어진다. (2 ≤ N ≤ 300,000, 1 ≤ M ≤ 1,000,000, 1 ≤ K ≤ 300,000, 1 ≤ X ≤ N)  
- 둘째 줄부터 M개의 줄에 걸쳐서 두 개의 자연수 A, B가 공백을 기준으로 구분되어 주어진다. 이는 A번 도시에서 B번 도시로 이동하는 단방향 도로가 존재한다는 의미다. (1 ≤ A, B ≤ N) 단, A와 B는 서로 다른 자연수이다.  

**<u>출력 조건</u>**  
- X로부터 출발하여 도달할 수 있는 도시 중에서, 최단 거리가 K인 모든 도시의 번호를 한 줄에 하나씩 오름차순으로 출력한다.  
- 이 때 도달할 수 있는 도시 중에서, 최단 거리가 K인 도시가 하나도 존재하지 않으면 -1을 출력한다.  

**<u>입력 예시1</u>**  
4 4 2 1  
1 2  
1 3  
2 3  
2 4  

**<u>출력 예시1</u>**  
4  

**<u>입력 예시2</u>**  
4 3 2 1  
1 2  
1 3  
1 4  

**<u>출력 예시2</u>**  
-1  

> 나의 풀이  

```python
import sys
from collections import deque

input = sys.stdin.readline

# n : 도시의 개수
# m : 도로의 개수
# k : 거리 정보
# x : 출발 도시의 번호
n, m, k, x = map(int, input().split())
graph = [[] for _ in range(n + 1)]

for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)

distance = [-1] * (n + 1)
distance[x] = 0

# BFS
queue = deque([x])
while queue:
    now = queue.popleft()
    for next_node in graph[now]:
        if distance[next_node] == -1:
            distance[next_node] = distance[now] + 1
            queue.append(next_node)

check = False
for i in range(1, n + 1):
    if distance[i] == k:
        print(i)
        check = True

if check == False:
    print(-1)
```

> 문제 해설  

문제에서 모든 도로의 거리는 1이다. 이는 다시 말해 모든 간선의 비용이 1이라는 의미인데, 그래프에서 모든 간선의 비용이 동일할 때는 너비 우선 탐색을 이용하여 최단 거리를 찾을 수 있다. 다시 말해 `모든 도로의 거리는 1이라는 조건 덕분에 너비 우선 탐색을 이용하여 간단히 해결`할 수 있는 것이다.  
문제의 조건을 살펴보면 노드의 개수 N은 최대 300,000개이며 간선의 개수 M은 최대 1,000,000개이다. 따라서 이 문제는 너비 우선 탐색을 이용하여 시간 복잡도 O(N + M)으로 동작하는 소스 코드를 작성하여 시간 초과 없이 해결할 수 있다.  

> 문제 답안  


<br>
기출 : 핵심 유형  
링크 : <https://programmers.co.kr/learn/courses/30/lessons/60062>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
