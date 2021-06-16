---
title: "[DFS] 탐색 알고리즘 DFS"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, DFS]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-16
last_modified_at: 2021-06-16
sitemap :
  changefreq : daily
  priority : 1.0
---

## 탐색 알고리즘 DFS  

> DFS  

DFS는 Depth-First Search, `깊이 우선 탐색이라고도 부르며, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘이다.`  
그래프는 노드(Node)와 간선(Edge)으로 표현되며 이때 노드를 정점(Vertex)이라고도 말한다. 그래프 탐색이란 하나의 노드를 시작으로 다수의 노드를 방문하는 것을 말한다. 또한 두 노드가 간선으로 연결되어 있다면 '두 노드는 인접하다(Adjacent)'라고 표현한다.  
프로그래밍에서 그래프는 크게 2가지 방식으로 표현할 수 있다.  
- 인접 행렬(Adjacency Matrix) : 2차원 배열로 그래프의 연결 관계를 표현하는 방식  
- 인접 리스트(Adgacency List) : 리스트로 그래프의 연결 관계를 표현하는 방식  

![image](https://user-images.githubusercontent.com/37467408/122142480-8bba8e80-ce8a-11eb-9a67-3a0884fa461e.PNG)  

먼저 **인접 행렬(Adjacency Matrix) 방식**은 2차원 배열에 각 노드가 연결된 형태를 기록하는 방식이다. 위와 같이 연결된 그래피를 인접 행렬로 표현할 때 파이썬에서는 2차원 리스트로 구현할 수 있다.  
연결이 되어 있지 않은 노드끼리는 무한(Infinity)의 비용이라고 작성한다. 논리적으로 정답이 될 수 없는 큰 값 중에서 999999999 등의 값으로 초기화하는 경우가 많다.  

```python
INF = 999999999 # 무한의 비용 선언

# 2차원 리스트를 이용해 인접 행렬 표현
graph = [
  [0, 7, 5],
  [7, 0, INF],
  [5, INF, 0]
]

print(graph)
```  

**인접 리스트(Adjacency List)방식**에서는 아래와 같이 모든 노드에 연결된 노드에 대한 정보를 차례대로 연결하여 저장한다.  

![image](https://user-images.githubusercontent.com/37467408/122142874-4185dd00-ce8b-11eb-9b32-a1aa4c85aa16.PNG)  

인접 리스트는 '연결 리스트'라는 자료구조를 이용해 구현한다. 파이썬은 기본 자료형인 리스트 자료형이 append() 메소드를 제공하므로, C++ / JAVA에서의 배열과 연결 리스트의 기능을 모두 기본으로 제공한다. 파이썬으로 인접 리스트를 이용해 그래프를 표현하고자 할 때에도 단순히 2차원 리스트를 이용하면 된다는 점을 기억하자.  

```python
# 행(Row)이 3개인 2차원 리스트로 인접 리스트 표현
graph = [[] for _ in range(3)]

# 노드 0에 연결된 노드 정보 저장(노드, 거리)
graph[0].append((1, 7))
graph[0].append((2, 5))

# 노드 1에 연결된 노드 정보 저장(노드, 거리)
graph[1].append((0, 7))

# 노드 2에 연결된 노드 정보 저장(노드, 거리)
graph[2].append((0, 5))

print(graph)
```  

두 방식의 차이점은 아래와 같다.  
`인접 행렬 방식` : 노드 개수가 많을수록 메모리가 불필요하게 낭비된다. 속도가 빠르다.  
`인접 리스트 방식` : 연결된 정보만을 저장하기 때문에 메모리를 효율적으로 사용한다. 속도가 느리다.  

`DFS`는 깊이 우선 탐색 알고리즘이다. 이 알고리즘은 특정한 경로로 탐색하다가 특정한 상황에서 최대한 깊숲이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로를 탐색하는 알고리즘이다.  
DFS는 스택 자료구조를 이용하며 구체적인 동작 과정은 다음과 같다.  

- 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.  
- 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리를 한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다.  
- 위 두번째 과정을 더 이상 수행할 수 없을 때가지 반복한다.  

아래의 그래프일 경우, 노드1을 시작 노드로 설정하여 DFS를 이용해 탐색을 진행하면 어떻게 될까? 직관적으로 생각하면, 단순하게 가장 깊숙이 위치하는 노드에 닿을 때까지 확인(탐색)하면 된다.  

![image](https://user-images.githubusercontent.com/37467408/122144126-cb36aa00-ce8d-11eb-9b6a-0d15d01f9f0a.PNG)  

일반적으로 인접한 노드 중에서 방문하지 않은 노드가 여러 개 있으면 번호가 낮은 순서부터 처리한다.  

**<u>step 1</u>** : 시작 노드인 '1'을 스택에 삽입하고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122144274-1c469e00-ce8e-11eb-9ad2-bb01ccbdd3bb.PNG)  

**<u>step 2</u>** : 스택의 최상단 노드인 '1'에 방문하지 않은 인접 노드 '2', '3', '8'이 있다. 이 중에서 가장 작은 노드인 '2'를 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122144365-531cb400-ce8e-11eb-844d-264d7209a18f.PNG)  

**<u>step 3</u>** : 스택의 최상단 노드인 '2'에 방문하지 않은 인접 노드 '7'이 있다. 따라서 '7'번 노드를 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122144450-7e9f9e80-ce8e-11eb-89bc-3a14d8af2b12.PNG)  

**<u>step 4</u>** : 스택의 최상단 노드인 '7'에 방문하지 않은 인접 노드 '6'과 '8'이 있다. 이 중에서 가장 작은 노드인 '6'을 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122144565-b7d80e80-ce8e-11eb-95bf-42262b5bbbff.PNG)  

**<u>step 5</u>** : 스택의 최상단 노드인 '6'에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 '6번' 노드를 꺼낸다.  

![image](https://user-images.githubusercontent.com/37467408/122144670-e229cc00-ce8e-11eb-99c2-e46b016d37e6.PNG)  

**<u>step 6</u>** : 스택의 최상단 노드인 '7'에 방문하지 않은 인접 노드 '8'이 있다. 따라서 '8'번 노드를 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122144771-0be2f300-ce8f-11eb-867a-0e8d213bac82.PNG)  

**<u>step 7</u>** : 스택의 최상단 노드인 '8'에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 '8'번 노드를 꺼낸다.  

![image](https://user-images.githubusercontent.com/37467408/122144848-32a12980-ce8f-11eb-8e16-6f8373345af7.PNG)  

**<u>step 8</u>** : 스택의 최상단 노드인 '7'에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 '7번' 노드를 꺼낸다.  

![image](https://user-images.githubusercontent.com/37467408/122145003-6aa86c80-ce8f-11eb-8a4f-6802e8803055.PNG)  

**<u>step 9</u>** : 스택의 최상단 노드인 '2'에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 '2'번 노드를 꺼낸다.  

![image](https://user-images.githubusercontent.com/37467408/122145081-8b70c200-ce8f-11eb-9134-26e748d5a7c0.PNG)  

**<u>step 10</u>** : 스택의 최상단 노드인 '1'에 방문하지 않은 인접 노드 '3'을 스택에 넣고 방문 처리한다.  

![image](https://user-images.githubusercontent.com/37467408/122145180-b6f3ac80-ce8f-11eb-89fc-6c038409e0a4.PNG)  

**<u>step 11</u>** : 스택의 최상단 노드인 '3'에 방문하지 않은 인접 노드 '4'와 '5'가 있다. 이 중에서 가장 작은 노드인 '4'를 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122145273-e5718780-ce8f-11eb-875c-eeaa174bf882.PNG)  

**<u>step 12</u>** : 스택의 최상단 노드인 '4'에 방문하지 않은 인접 노드 '5'가 있다. 따라서 '5'번 노드를 스택에 넣고 방문 처리를 한다.  

![image](https://user-images.githubusercontent.com/37467408/122145363-0f2aae80-ce90-11eb-9f87-f7211f56f6b7.PNG)  

**<u>step 13</u>** : 남아 있는 노드에 방문하지 않은 인접 노드가 없다. 따라서 모든 노드를 차례대로 꺼내면 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/122145431-35504e80-ce90-11eb-9ac4-01c75ba9c6ff.PNG)  

결과적으로 노드의 탐색 순서(스택에 들어간 순서)는 다음과 같다.  

- 1 -> 2 -> 7 -> 6 -> 8 -> 3 -> 4 -> 5  

깊이 우선 탐색 알고리즘인 DFS는 스택 자료구조에 기초한다는 점에서 구현이 간단하다. 실제로는 스택을 사용하지 않아도 되며 탐색을 수행함에 있어서 데이터의 개수가 N개인 경우 O(N)의 시간이 소요된다는 특징이 있다.  

```python
# DFS 메서드 정리
def dfs(graph, v, visited):
  # 현재 노드를 방문 처리
  visited[v] = True
  print(v, end = ' ')
  # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
  for i in graph[v]:
    if not visited[i]:
      dfs(graph, i, visited)

# 각 노드가 연결된 정보를 리스트 자료형으로 표현(2차원 리스트)
graph = [
  [],
  [2, 3, 8],
  [1, 7],
  [1, 4, 5],
  [3, 5],
  [3, 4]
  [7],
  [2, 6, 8],
  [1, 7]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 9

# 정의된 DFS 함수 호출
dfs(graph, 1, visited)
```




---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
