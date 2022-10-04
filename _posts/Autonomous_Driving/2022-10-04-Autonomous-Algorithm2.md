---
title: "A* (A-star) 최단 경로 찾기 Algorithm"

categories:
  - Autonomous_Algorithm
tags:
  - [A*, A-star, Autonomous Drive, Python]

use_math: true

toc:  true
toc_sticky: true

date: 2022-10-04
last_modified_at: 2022-10-04
---

## A*(A-star) 최단 경로 알고리즘  

### Node 클래스  

Node 클래스는 아래와 같다.  

```python
class Node:
  def __init__(self, parent=None, position=None):
    self.parent = parent # 이전 노드
    self.position = position # 현재 위치

    self.f = 0
    self.g = 0
    self.h = 0
```  

Node는 2차원 평면 상에서 이뤄지므로 xy좌표계를 사용한다.  

start = (0, 0), end = (9, 9)  

### F(x) = G(x) + H(x)  

편의상 F(x) = F, G(x) = G, H(x) = H 라고 하자.

A* 알고리즘의 핵심 값인 F, G, H에 대해 알아보자.  

위 F, G, H 3개의 변수는 노드를 추가할 때마다 값이 갱신된다.  

- F = 출발 지점에서 목적지까지의 총 cost 합  
- G = 현재 노드에서 출발 지점까지의 총 cost
- H = Heuristic(휴리스틱), 현재 노드에서 목적지까지의 추정 거리  

이해를 돕기 위해, 아래의 예제를 들어보며 확인해보자.(초록 점 = 시작, 빨간 점 = 종료)  

![image](https://user-images.githubusercontent.com/37467408/193744026-fb26689a-b2df-4c18-aa0d-462ef264f8ae.png)  

<center>시작 노드를 `node(0)`, 도착 노드를 `node(8)`</center>  

<center>현재 노드가 `node(2)`라고 해보자.</center>  
<br>

### G(x)  

- G = 현재 노드에서 출발 지점까지의 총 cost  

현재 노드 = node(2), 출발 지점에서 2 cost 만큼 떨어져 있다. (parent 노드는 `node(1)`)  

따라서, 현재 `node(2)`는 아래와 같이 표현이 된다.  

```python
node(2).g = 2
```

### H(x)  

- H = Heuristic(휴리스틱), 현재 노드에서 목적지까지의 추정 거리  

현재 노드 = `node(2)`에서 목적지까지의 거리를 추정해보자. (현재 노드부터 1이라고 가정)  

5 cost만큼 오른쪽으로 가서, 위로 3 cost만큼 가면 목적지 노드가 있다.  

![image](https://user-images.githubusercontent.com/37467408/193744488-9aea0539-ba97-4637-a506-ea4bcb4299f2.png)  

여기서 피타고라스의 정리($a^2$ + $b^2$ = $c^2$), 를 이용해서 H의 값을 계산해보면, 아래와 같다.  

node(2).h = $5^2$ + $3^2$ = 34  

<center>$\sqrt{34}$ 로 지정하는게 맞지만, 매번 계산 하지 않아도 결과는 똑같이 나온다.</center>  
<br>

하지만, 근접 거리나 값이 클 경우 다익스트라 처럼 작동할 수 있다.  

파이썬 코드로는 아래와 같다. (맨해튼 거리)  

```python
child.h = (abs(child.position[0] - end_node.position[0]) ** 2) + \
          (abs(child.position[1] - end_node.position[1]) ** 2)
```  

또는, 정확하게 하고 싶으면, octile distance + diagonal distance를 이용하면 된다.  

```python
def heuristic(node, goal, D=1, D2=2 ** 0.5):
  dx = abs(node.position[0] - goal.position[0])
  dy = abs(node.position[1] - goal.position[1])
  return D * (dx + dy) + (D2 - 2 * D) * min(dx, dy)
```

4방향으로 움직이면 보통, 맨해튼 거리를 이용하고, 8방향으로 움직이면 Diagonal Distance를 이용한다.  

### F(x)  

- F = 출발 지점에서 목적지까지의 총 cost 합  

이제 현재 노드(node(2))의 G값과 H값을 더해보면,  

`node(2).f` = node(2).g + node(2).h = 2 + 34 = 36  

### F값 사용하기  

F 값은 현재 노드가 목적지에 도달하는 데 있어서, 최선의 방법인지 확인할 수 있는 값이다.  

이 F 값이 없으면, Dijkstra 길 찾기 알고리즘처럼 모든 노드에서 목적지까지 가는 방법을 찾는다.  

![image](https://user-images.githubusercontent.com/37467408/193746639-a5aa34a6-6244-434c-9fef-7514fc7d6713.png)  

<center>모든 노드에서 길을 찾는 Dijkstra 알고리즘</center>  
<br>

![image](https://user-images.githubusercontent.com/37467408/193746306-05edc928-5d4b-4519-b0c9-1c1d80a2ddbc.png)  

<center>F 값을 이용해, 최선의 노드를 찾아 길을 찾는 A* 알고리즘</center>  
<br>  

<center>`Pseudo Code`</center>
<br>  

```python
/*
1. openList와 closeList라는 보조 데이터를 사용한다.
2. F = G + H를 매번 노드를 생성할 때마다 계산한다.
3. openist에는 현재 노드에서 갈 수 있는 노드를 전부 추가해서 F, G, H를 계산한다.
openList에 중복된 노드가 있다면, F값이 제일 작은 노드로 바꾼다.
4. clostList에는 openList에서 F값이 가장 작은 노드를 추가시킨다.
*/

openList.add(startNode) # openList는 시작 노드로 init

while openList is not empty:
  # 현재 노드 = openList에서 F 값 가장 작은 것
  currentNode <- node in openList having the lowset F value
  openList.remove(currentNode) # openList에서 현재 노드 제거
  closeList.add(currentNode) # closeList에서 현재 노드 추가

  if goalNode in currentNode:
    currentNode.parent.position 계속 추가 후
    경로 출력 후 종료

  children <- currentNode와 인접한 모든 노드 추가

  for each child in children:
    if child in closeList:
      continue

    child.g = currentNode.g + child와 currentNode 거리(1)
    child.h = child와 목적지까지의 거리
    child.f = child.g + child.h

    # child가 openList에 있고, child의 g 값이 openList에 중복된 노드 g값과 같으면
    # 다른 자식 불러오기
    if child in openList and child.g > openNode.g in openList:
      continue

    openList.add(child)
```  

<center>`Python Code`</center>  
<br>

<iframe src="https://tech.io/snippet-widget/lmvEz2S" width="100%" frameborder="0" scrolling="no" allowtransparency="true" style="visibility:hidden">
</iframe>
<script>if(void 0===window.techioScriptInjected){window.techioScriptInjected=!0;var script=document.createElement("script");script.src="https://files.codingame.com/codingame/iframe-v-1-4.js",(document.head||document.body).appendChild(script)}</script>  

---
**🐢 현재 공부하고 있는 자율주행/자율운항을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

[참고 블로그](https://choiseokwon.tistory.com/210)
[PathFinding 사이트](https://qiao.github.io/PathFinding.js/visual/)

감사합니다.😊
