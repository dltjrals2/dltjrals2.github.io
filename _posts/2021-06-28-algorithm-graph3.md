---
title: "[Graph] 그래프 이론3"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Graph, Topology Sort]

toc:  true
toc_sticky: true
show_date: true
read_time: false
use_math: true

date: 2021-06-28
last_modified_at: 2021-06-28
sitemap :
  changefreq : daily
  priority : 1.0
---

## 다양한 그래프 알고리즘  

## 위상 정렬  

`위상 정렬(Topology Sort)`은 정렬 알고리즘의 일종이다. 위상 정렬은 순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘이다. 즉, `방향 그래프의 모든 노드를 '방향성에 거스르지 않도록 순서대로 나열하는 것'이다.`  
위상 정렬 알고리즘을 이해하기에 앞서 `진입차수(Indegree)`를 알아야 한다. 진입차수란 특정한 노드로 '들어오는' 간선의 개수를 의미한다.  
위상 정렬 알고리즘은 다음과 같다.  

1. 진입차수가 0인 노드를 큐에 넣는다.  
2. 큐가 빌 때까지 다음의 과정을 반복한다.  
- 큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.  
- 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.  

큐가 빌 때까지 큐에서 원소를 계속 꺼내서 처리하는 과정을 반복한다. 이때 모든 원소를 방문하기 전에 큐가 빈다면 사이클이 존재한다고 판단할 수 있다. 다시 말해 큐에서 원소가 V번 추출되기 전에 큐가 비어버리면 사이클이 발생한 것이다. `잘 이해가 안되는군.. 무슨 말이지..?`  
다시 말해 큐에서 원소가 V번 추출되기 전에 큐가 비어버리면 사이클이 발생한 것이다. 사이클이 존재하는 경우 사이클에 포함되어 있는 원소 중에서 어떠한 원소도 큐에 들어가지 못하기 때문이다. 다만, 기본적으로 위상 정렬 문제에서는 사이클이 발생하지 않는다고 명시하는 경우가 더 많으므로, 그건 고려하지 않아도 된다. 예를 들어 이해를 해보자!  

![image](https://user-images.githubusercontent.com/37467408/123568617-aaf8da80-d7ff-11eb-98a9-be6c9c48e579.PNG)  

**<u>step 0</u>** 초기 단계에서는 진입차수가 0인 노드를 큐에 넣는다. 현재 노드 1의 진입차수만 0이기 때문에 큐에 노드 1만 삽입한다. 큐에 삽입된 노드는 그림처럼 색을 다르게 표현했다.  

![image](https://user-images.githubusercontent.com/37467408/123568793-032fdc80-d800-11eb-9fe5-b9f42744895b.PNG)  

**<u>step 1</u>** 먼저 큐에 들어 있는 노드 1을 꺼낸다. 이제 노드 1과 연결되어 있는 간선들을 제거한다. 그러면 새롭게 노드 2와 노드 5의 진입차수가 0이 된다. 따라서 노드 2와 노드 5를 큐에 삽입한다. 처리된 노드와 간선은 점선으로 표기한다.  

![image](https://user-images.githubusercontent.com/37467408/123568871-32464e00-d800-11eb-91a7-de93dba9f40e.PNG)  

**<u>step 2</u>** 그다음 큐에 들어 있는 노드 2를 꺼낸다. 이제 노드 2와 연결되어 있는 간선들을 제거한다. 그러면 새롭게 노드 3의 진입차수가 0이 된다. 따라서 노드 3을 큐에 삽입한다.  

![image](https://user-images.githubusercontent.com/37467408/123568960-6cafeb00-d800-11eb-8bd1-2f8bdf0a7ee0.PNG)  

**<u>step 3</u>** 그다음 큐에 들어 있는 노드 5를 꺼낸다. 이제 노드 5와 연결되어 있는 간선들을 제거한다. 그러면 새롭게 노드 6의 진입차수가 0이 된다. 따라서 노드 6을 큐에 삽입한다.  

![image](https://user-images.githubusercontent.com/37467408/123569094-a8e34b80-d800-11eb-84b4-c393f3ad934d.PNG)  

**<u>step 4</u>** 그다음 큐에 들어 있는 노드 3을 꺼낸다. 이제 노드 3과 연결되어 있는 간선들을 제거한다. 이번 단계에서는 새롭게 진입차수가 0이 되는 노드가 없으므로 그냥 넘어간다.  

![image](https://user-images.githubusercontent.com/37467408/123569205-d9c38080-d800-11eb-956e-5936a2c9519d.PNG)  

**<u>step 5</u>** 그다음 큐에 들어 있는 노드 6을 꺼낸다. 이제 노드 6과 연결되어 있는 간선을 제거한다. 그러면 새롭게 노드 4의 진입차수가 0이 된다. 따라서 노드 4를 큐에 삽입한다.  

![image](https://user-images.githubusercontent.com/37467408/123569286-05466b00-d801-11eb-804f-c0bba51734a3.PNG)  

**<u>step 6</u>** 그다음 큐에 들어 있는 노드 4를 꺼낸다. 이제 노드 4와 연결되어 있는 간선을 제거한다. 그러면 새롭게 노드 7의 진입차수가 0이 된다. 따라서 노드 7을 큐에 삽입한다.  

![image](https://user-images.githubusercontent.com/37467408/123569411-3888fa00-d801-11eb-93e4-b671e3a415df.PNG)  

**<u>step 7</u>** 그다음 큐에 들어 있는 노드 7을 꺼낸다. 이제 노드 7과 연결되어 있는 간선을 제거해야 한다. 이번 단계에서는 새롭게 진입차수가 0이 되는 노드가 없으므로 그냥 넘어간다.  

![image](https://user-images.githubusercontent.com/37467408/123569468-58202280-d801-11eb-9ca8-f5bb50123d59.PNG)  

위 과정을 수행하는 동안 큐에서 빠져나간 노드를 순서대로 출력하면, 그것이 바로 위상 정렬을 수행한 결과가 된다. 전체 소스코드는 다음과 같다.  

```python
from collections import deque

# 노드의 개수와 간선의 개수를 입력받기
v, e = map(int, input().split())
# 모든 노드에 대한 진입차수는 0으로 초기화
indegree = [0] * (v + 1)
# 각 노드에 연결된 간선 정보를 담기 위한 연결 리스트(그래프) 초기화
graph = [[] for i in range(v + 1)]

# 방향 그래프의 모든 간선 정보를 입력받기
for _ in range(e):
  a, b = map(int, input().split())
  graph[a].append(b) # 정점 A에서 B로 이동 가능
  # 진입차수를 1 증가
  indegree[b] += 1

# 위상 정렬 함수
def topology_sort():
  result = [] # 알고리즘 수행 결과를 담을 리스트
  q = deque() # 큐 기능을 위한 deque 라이브러리 사용

  # 처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
  for i in range(1, v + 1):
    if indegree[i] == 0:
      q.append(i)

  # 큐가 빌 때까지 반복
  while q:
    # 큐에서 원소 꺼내기
    now = q.popleft()
    result.append(now)
    # 해당 원소와 연결된 노드들의 진입차수에서 1 빼기
    for i in graph[now]:
      indegree[i] -= 1
      # 새롭게 진입차수가 0이 되는 노드를 큐에 삽입
      if indegree[i] == 0:
        q.append(i)

# 위상 정렬을 수행한 결과 출력
for i in result:
  print(i, end = ' ')

topology_sort()
```  

> 위상 정렬의 시간 복잡도  

위상 정렬의 시간 복잡도는 `O(V + E)`이다.  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
