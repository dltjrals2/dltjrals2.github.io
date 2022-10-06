---
title: "Dijkstra Algorithm"

categories:
  - Autonomous_Algorithm
tags:
  - [Dijkstra Algorithm, Autonomous Drive, Python, Robotics]

toc:  true
toc_sticky: true

date: 2022-09-28
last_modified_at: 2022-09-30
---

## 가장 빠른 길 찾기  

### 다익스트라 최단 경로 알고리즘  

다익스트라 최단 경로 알고리즘은 그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다.  

알고리즘의 원리는 다음과 같다.  

1. 출발 노드를 설정한다.  
2. 최단 거리 테이블을 초기화한다.  
3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.  
4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.  
5. 위 과정에서 3번과 4번을 반복한다.  

![image](https://user-images.githubusercontent.com/37467408/193202671-b31bb91a-bb8b-48cf-8083-f9afecfc5756.png)  

위 문제를 기준으로 다익스트라 최단 경로 알고리즘을 구현해보자.  

1번 노드가 출발 노드인 경우로 설정하고, 출발 노드를 제외한 모든 노드의 최단 거리를 무한으로 설정하자.  

우선순위 큐에 1번 노드 정보(거리: 0, 노드: 1)를 넣는다.  

![image](https://user-images.githubusercontent.com/37467408/193203080-b0c178b4-349d-4c63-b07f-4e17390e152b.png)  

우선순위 큐에서 기본적으로 거리가 가장 짧은 원소가 우선순위 큐의 최상위 원소로 위치해 있기 때문에, 우선순위 큐에서 노드를 꺼낸 뒤에 `해당 노드를 이미 처리한 적이 있다면 무시`하면 되고, 아직 처리하지 않은 노드에 대해서만 처리하면 된다.  

1번 노드를 거쳐서 2번, 3번, 4번 노드로 가는 최소 비용을 계산하면 된다.  

![image](https://user-images.githubusercontent.com/37467408/193203617-98430e10-68c8-48ad-9dfc-7ada0303d92b.png)  

다시, 우선순위 큐에서 가장 짧은 원소 (1, 4)의 값을 갖는 원소가 추출된다.  

이때, 1에서 4번 노드 까지의 최단 거리는 1이므로 4번 노드를 거쳐서 3번과 5번 노드로 가는 최소 비용은 차례대로 4(1 + 3)과 2(1 + 1)이다.  

![image](https://user-images.githubusercontent.com/37467408/193204923-ea8a99ae-1735-4703-aea3-c9b303b09d37.png)  

다시, 우선순위 큐에서 가장 짧은 원소 (2, 2)의 값을 갖는 원소가 추출된다.  

2번 노드를 거쳐서 가는 경우 중 현재의 최단 거리를 더 짧게 갱신할 수 있는 방법은 없다.  

![image](https://user-images.githubusercontent.com/37467408/193206274-641b2082-a029-45bd-9fcc-0281d56cb336.png)  

다시, 우선순위 큐에서 가장 짧은 원소 (2, 5)의 값을 갖는 원소가 추출된다.  

5번 노드를 거쳐서 3번과 6번 노드로 갈 수 있다. 5번 노드에서 3번 노드로 가는 거리인 1을 더한 3이 기존의 값인 4보다 작다. 따라서 새로운 값인 3으로 갱신한다.  

![image](https://user-images.githubusercontent.com/37467408/193207373-41c9eec5-31a2-49cf-90c7-db3f70e7498f.png)  
![image](https://user-images.githubusercontent.com/37467408/193207413-07ccad85-299f-4aa7-baba-6a99853a4f32.png)  

다시, (3, 3)을 꺼내서 알고리즘을 수행하고, 마찬가지로 (4, 3), (4, 6), (5, 3) 원소에 대해 알고리즘을 수행한다.  

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수, 간선의 개수를 입력받기
n, m = map(int, input().split())
# 시작 노드 번호를 입력받기
start = int(input())
# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만들기
graph = [[] for i in range(n + 1)]
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF] * (n + 1)

# 모든 간선 정보를 입력받기
for _ in range(m):
  a, b, c = map(int, input().split())
  # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
  graph[a].append((b, c))

def dijkstra(start):
  q = []
  # 시작 노드로 가기 위한 최단 경로는 0으로 설정하여, 큐에 삽입
  heapq.heappush(q, (0, start))
  distance[start] = 0
  while q: # 큐가 비어있지 않다면
    # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
    dist, now = heapq.heappop(q)
    # 현재 노드가 이미 처리된 적이 있는 노드라면 무시
    if distance[now] < dist:
      continue
    # 현재 노드와 연결된 다른 인접한 노드들을 확인
    for i in graph[now]:
      cost = dist + i[1]
      # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
      if cost < distance[i[0]]:
        distance[i[0]] = cost
        heapq.heappush(q, (cost, i[0]))

# 다익스트라 알고리즘을 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리를 출력
for i in range(1, n + 1):
  # 도달할 수 없는 경우, 무한이라고 출력
  if distance[i] == INF:
    print("INF")
  else:
    print(distance[i])
```  

---
**🐢 현재 공부하고 있는 자율주행/자율운항을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
