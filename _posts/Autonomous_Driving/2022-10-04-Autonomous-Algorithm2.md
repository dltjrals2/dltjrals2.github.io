---
title: "A* (A-star) 최단 경로 찾기 Algorithm"

categories:
  - Autonomous_Algorithm
tags:
  - [A*, A-star, Autonomous Drive, Python, Robotics]

use_math: true

toc:  true
toc_sticky: true

date: 2022-10-04
last_modified_at: 2022-10-04
---

## A*(A-star) 최단 경로 알고리즘  

- 출발 꼭짓점부터 목표 꼭짓점까지 가는 최단 경로 찾기 알고리즘  
- Dijkstra 알고리즘과 유사하나 차이가 분명히 있다.  
-- Heuristic 추정값 h가 존재  
- 평가함수인 f를 모든 꼭짓점마다 계산  

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

![image](https://user-images.githubusercontent.com/37467408/193836940-525e32f6-9d72-4255-9c9c-b3ba7ceb2794.png)  

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

`Heuristic은 사용하는 거리에 따라 연산 속도, 정확도 등이 다르다.`  

`여러 번의 테스트 후 최적의 Heuristic 값을 대입해야 한다.`  

현재 노드 = `node(2)`에서 목적지까지의 거리를 추정해보자. (현재 노드부터 1이라고 가정)  

5 cost만큼 오른쪽으로 가서, 위로 3 cost만큼 가면 목적지 노드가 있다.  

![image](https://user-images.githubusercontent.com/37467408/193744488-9aea0539-ba97-4637-a506-ea4bcb4299f2.png)  

여기서 피타고라스의 정리($a^2$ + $b^2$ = $c^2$), 를 이용해서 H의 값을 계산해보면, 아래와 같다.  

node(2).h = $5^2$ + $3^2$ = 34  

<center>$\sqrt{34}$ 로 지정하는게 맞지만, 매번 계산 하지 않아도 결과는 똑같이 나온다.</center>  
<br>

하지만, 근접 거리나 값이 클 경우 다익스트라 처럼 작동할 수 있다.  

진행 방향이 4방향인 맨해튼 거리의 파이썬 코드로는 아래와 같다.  

```python
child.h = (abs(child.position[0] - end_node.position[0]) ** 2) + \
          (abs(child.position[1] - end_node.position[1]) ** 2)
```  

또는, 진행 방향을 8방향으로 하고 싶으면 Diagonal distance를 이용하면 된다.  

![image](https://user-images.githubusercontent.com/37467408/193836177-3aa2d038-5808-44ca-b7d8-1bfb72d4d33a.png)  

현 지점에서 8개 방향의 각 지점의 f값을 계산하여, 가장 작은 f를 가진 지점을 다음 지점으로 선택  

```python
def heuristic(node, goal, D=1, D2=2 ** 0.5):
  dx = abs(node.position[0] - goal.position[0])
  dy = abs(node.position[1] - goal.position[1])
  return D * (dx + dy) + (D2 - 2 * D) * min(dx, dy)
```  

### F(x)  

- F = 해당 지점 n에 대한 평가함수  
- 출발점 ~ 목표점까지 총 cost의 합  
- f값이 없으면, Dijkstra 알고리즘처럼 '모든 지점에서 목표점까지 가는 방법'을 찾게 되어 시간적, 공간적 낭비이다.  

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
"""
1. Open List : 아직 조사하지 않은(f 평가함수가 계산이 안된) 노드
2. Closed List : 조사를 마친 노드
3. 장애물을 제외하고 갈 수 있는 노드를 전부 Open List에 넣음(가운데 Node가 Parent가 되는 것)
4. 시작점을 Close List에 저장
5. Open List의 노드 중 가장 작은 f 가지는 사각형 택 & Close List에 저장
- 해당 노드 기준으로 OpenList 넣고 계산하는 과정 반복
- 이 때 Open List에는 Close List에 있는 노드 / 장애물을 제외하고 추가, 중복된 노드가 Open List에 있다면, f 값이 가장 작은 노드로 바꾸기
"""  

open_list.add(start_node) # 시작되는 노드를 먼저 삽입
while open_list is not empty:
    current_node <- (open_list에서 가장 작은 f값을 갖는 노드)
    open_list.remove(current_node) # open_list에서 current_node(가장 작은 f값을 갖는 노드) 제거
    close_list.add(current_node) # close_list에서 current_node(가장 작은 f값을 갖는 노드) 추가

    if goal_node is current_node: # 만약 current_node가 목표지점이라면
        current_node.parent.position 추가 후 경로 출력, 종료

    children <- (current_node와 인접한 모든 노드 추가)

    for each child in children: # 인접한 노드 하나하나를 child로 대입
        if child in close_list: # 민약 child가 close_list에 있다면(이미 최소 f값)
            continue            # 다른 child를 검사할 수 있도록 continue

        child.g = current_node.g + child와 current_node 간 거리
        child.h = child와 목표점 간 거리
        child.f = child.g + child.h
    
        if (child in open_list) and (child.g > open_node.g in open_list):
            continue # 만약 child가 open_list에 이미 있고, g가 open_list의 g보다 크면(목표점서 더 멀다) open_list에 추가 안함
    
        open_list.add(child) # open_list에 child를 추가함(f, g, h 연산 다 된 노드)
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
