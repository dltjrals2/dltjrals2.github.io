---
title: "[Greedy] 모험가 길드"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Greedy]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-07-06
last_modified_at: 2021-07-06
sitemap :
  changefreq : daily
  priority : 1.0
---

## 모험가 길드  

난이도 : ⭐⭐  
푼횟수 : 🟢⚪⚪  

한 마을에 모함가가 N명 있습니다. 모험가 길드에서는 N며의 모험가를 대상으로 '공포도'를 측정했는데, '공포도'가 높은 모험가는 쉽게 공포를 느껴 위험 상황에서 제대로 대처할 능력이 떨어집니다.  
모험가 길드장인 동빈이는 모험가 그룹을 안전하게 구성하고자 공포다가 X인 모험가는 반드시 X명 이상으로 구성한 모험가 그룹에 참여해야 여행을 떠날 수 있도록 규정했습니다. 동빈이는 최대 몇개의 모험가 그룹을 만들 수 있는지 궁금합니다.  
동빈이를 위해 N명의 모험가에 대한 정보가 주어졌을 때, 여행을 떠날 수 있는 그룹 수의 최댓값을 구하는 프로그램을 작성하세요.  
예를 들어 N = 5이고, 각 모험가의 공포도가 다음과 같다고 가정합시다.  

> 2 3 1 2 2  

이때, 그룹 1에 공포도가 1, 2, 3인 모험가를 한 명씩 넣고, 그룹 2에 공포도가 2인 남은 두명을 넣게 되면, 총 2개의 그룹을 만들 수 있습니다. 또한 몇 명의 모험가는 마을에 그대로 남아 있어도 되기 때문에, 모든 모험가를 특정한 그룹에 넣을 필요는 없습니다.  

**<u>입력 조건</u>**  
- 첫째 줄에 모험가의 수 N이 주어집니다. (1 <= N <= 100,000)  
- 둘째 줄에 각 모험가의 공포도의 값을 N 이하의 자연수로 주어지며, 각 자연수는 공백으로 구분합니다.  

**<u>출력 조건</u>**  
- 여행을 떠날 수 있는 그룹 수의 최댓값을 출력합니다.  

**<u>입력 예시</u>**  
5  
2 3 1 2 2  

**<u>출력 예시</u>**  
2  

> 나의 풀이  

```python
n = int(input())
people = list(map(int, input().split()))
people.sort()
num = people.pop()

if num == 1:
    result = 1
else:
    result = 0

while people:
    if num == 1:
        num = people.pop()

    else:
        for _ in range(num - 1):
            pop_num = people.pop()
            if pop_num > num:
                num = pop_num
    result += 1

print(result)
```

> 문제 해설  

공포도를 기준으로 오름차순으로 정렬을 수행하고, 이후에 공포도가 가장 낮은 모험가부터 하나씩 확인하며, 그룹에 포함될 모험가의 수를 계산할 수 있다. 만약에 현재 그룹에 포함된 모험가의 수가 현재 확인되고 있는 공포도보다 크거나 같다면, 그룹을 결성할 수 있을 것이다.  
예를 들어 문제에서의 예시 입력을 그림으로 표현하면 다음과 같다.  

![image](https://user-images.githubusercontent.com/37467408/124556051-c3f23300-de72-11eb-9d47-c73565af09c5.PNG)  

가장 먼저 5명의 공포도를 오름차순으로 정렬하면 다음과 같이 구성된다.  

![image](https://user-images.githubusercontent.com/37467408/124556099-d7050300-de72-11eb-87f9-4ce0a30df0a5.PNG)  

이제 앞에서부터 공포도를 하나씩 확인하며, '현재 그룹에 포함된 모험가의 수'가 '현재 확인하고 있는 공포도'보다 크거나 같다면, 이를 그룹으로 설정하면 된다.  

![image](https://user-images.githubusercontent.com/37467408/124556152-e84e0f80-de72-11eb-9065-5f0ea509e03a.PNG)  

> 문제 답안  

```python
n = int(input())
data = list(map(int, input().split()))
data.sort()

result = 0 # 총 그룹의 수
count = 0 # 현재 그룹에 포함된 모험가의 수

for i in data: # 공포도를 낮은 것부터 하나씩 확인하며
  count += 1 # 현재 그룹에 해당 모험가를 포함시키기
  if count >= i: # 현재 그룹에 포함된 모험가의 수가 현재의 공포도 이상이라면, 그룹 결성
    result += 1 # 총 그룹의 수 증가시키기
    count = 0 # 현재 그룹에 포함된 모험가의 수 초기화

print(result) # 총 그룹의 수 출력
```  

<br>
기출 : 핵심 유형  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
