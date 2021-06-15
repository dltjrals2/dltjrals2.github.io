---
title: "[Inplementation] 왕실의 나이트"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-06-15
last_modified_at: 2021-06-15
sitemap :
  changefreq : daily
  priority : 1.0
---
## 왕실의 나이트  

난이도 : ⭐  

행복 왕국의 왕실 정원은 체스판과 같은 8 X 8 좌표 평면이다. 왕실 정원의 특정한 한 칸에 나이트가 서 있다. 나이트는 매우 충성스러운 신하로서 매일 무술을 연마한다.  
나이트는 말을 타고 있기 때문에 이동을 할 때는 L자 형태로만 이동할 수 있으며 정원 밖으로는 나갈 수 없다. 나이트는 특정한 위치에서 다음과 같은 2가지 경우로 이동할 수 있다.  

- 수평으로 두칸 이동한 뒤에 수직으로 한 칸 이동하기  
- 수직으로 두칸 이동한 뒤에 수평으로 한 칸 이동하기  

![image](https://user-images.githubusercontent.com/37467408/121976695-4d0bd200-cdbf-11eb-9172-9e2e4e49587d.PNG)  

이처럼 8 X 8 좌표 평면상에서 나이트의 우치가 주어졌을 때 나이트가 이동할 수 있는 경우의 수를 출력하는 프로그램을 작성하시오. 이때 왕실의 정원에서 행 위치를 표현할 때는 1부터 8로 표현하며, 열의 위치를 표현할 때는 a부터 h로 표현한다.  
예를 들어 만약 나이트가 a1에 있을 때 이동할 수 있는 경우의 수는 다음 2가지이다. a1의 위치는 좌표 평면에서 구석의 위치에 해당하며 나이트는 정원의 밖으로는 나갈 수 없기 때문이다.  

- 오른쪽으로 두 칸 이동 후 아래로 한 칸 이동하기(c2)  
- 아래로 두 칸 이동 후 오른쪽으로 한 칸 이동하기(b3)  

또 다른 예로 나이트가 c2에 위치해 있다면 나이트가 이동할 수 있는 경우의 수는 6가지이다. 이건 직접 계산해보시오.  

**<u>입력 조건</u>**  
- 첫째 줄에 8 X 8 좌표 평면상에서 현재 나이트가 위치한 곳의 좌표를 나타내는 두 문자로 구성된 문자열이 입력된다. 입력 문자는 a1처럼 열과 행으로 이뤄진다.  

**<u>출력 조건</u>**  
- 첫째 줄에 나이트가 이동할 수 있는 경우의 수를 출력하시오.  

**<u>입력 예시</u>**  
a1  

**<u>출력 예시</u>**  
2  

> 나의 풀이

```python
position = input()
row = int(position[1])
column = int(ord(position[0])) - int(ord('a')) + 1

move = [(-2, -1), (-2, 1), (2, -1), (2, 1), (-1, 2), (1, 2), (-1, -2), (1, -2)]
count = 0

for moving in move:
    next_row = row + moving[0]
    next_column = column + moving[1]
    if next_row < 1 or next_row > 8 or next_column < 1 or next_column > 8:
        continue
    count += 1

print(count)
```

> 문제 해설  

앞선 문제인 `상하좌우`문제와 유사하다.  
나이트의 이동 경로를 steps 변수에 넣는다면, 위 이동 규칙에 따라서 steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, -2), (-2, 1)]로 값을 대입할 수 있다. 현재 위치를 기준으로 아래쪽과 오른쪽은 양수의 값을, 위쪽과 왼쪽은 음수의 값을 대입한 결과이다.  

> 답안 예시  

```python
# 현재 나이트의 위치 입력받기
input_data = input()
row = int(input_data)
column = int(ord(input_data[0])) - int(ord('a')) + 1

# 나이트가 이동할 수 있는 8가지 방향 정의
steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, -2), (-2, 1)]

# 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
result = 0
for step in steps:
    # 이동하고자 하는 위치 확인
    next_row = row + step[0]
    next_column = column + step[1]
    # 해당 위치로 이동이 가능하다면 카운트 증가
    if next_row >= 1 and next_row <= 8 and next_column >= 1 and next_column <= 9:
        result += 1

print(result)
```

앞선 문제인 `상하좌우` 문제에서는 이동할 방향을 dx, dy에 기록했지만, 이번 문제에서는 steps 변수에 이동할 방향을 저장해뒀다. 2가지 형태 모두 자주 사용되므로 기억하도록 하자.

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

감사합니다.😊
