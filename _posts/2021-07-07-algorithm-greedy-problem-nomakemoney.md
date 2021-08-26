---
title: "[Greedy] 만들 수 없는 금액"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Greedy, 풀이실패]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-07-07
last_modified_at: 2021-07-07
sitemap :
  changefreq : daily
  priority : 1.0
---

## 만들 수 없는 금액  

난이도 : ⭐  
푼횟수 : 🔴⚪⚪  

동네 편의점의 주인인 동빈이는 N개의 동전을 가지고 있습니다. 이때 N개의 동전을 이용하여 만들 수 없는 양의 정수 금액 중 최솟값을 구하는 프로그램을 작성하세요.  
예를 들어, N = 5이고, 각 동전이 각각 3원, 2원, 1원, 1원, 9원짜리(화폐 단위) 동전이라고 가정합시다. 이때 동빈이가 만들 수 없는 양의 정수 금액 중 최솟값은 1원입니다.  
또 다른 예시로, N = 3이고, 각 동전이 각각 3원, 5원, 7원짜리(화폐 단위) 동전이라고 가정합시다. 이때 동빈이가 만들 수 없는 양의 정수 금액 중 최솟값은 1원입니다.  

**<u>입력 조건</u>**  
- 첫째 줄에는 동전의 개수를 나타내는 양의 정수 N이 주어집니다. (1 <= N <= 1,000)  
- 둘째 줄에는 각 동전의 화폐 단위를 나타내는 N개의 자연수가 주어지며, 각 자연수는 공백으로 구분합니다. 이때, 각 화폐 단위는 1,000,000 이하의 자연수입니다.  

**<u>출력 조건</u>**  
- 첫째 줄에 주어진 동전들로 만들 수 없는 양의 정수 금액 중 최솟값을 출력합니다.  

**<u>입력 예시1</u>**  
5  
3 2 1 1 9  

**<u>출력 예시1</u>**  
8  

> 나의 풀이  

풀기 실패  

> 문제 해설  

문제 해결 아이디어는 동전에 대한 정보가 주어졌을 때, 화폐 단위 기준으로 오름차순 정렬을 한다. 이후에 1부터 차례대로 특정한 금액을 만들 수 있는지 확인하면 된다. 1부터 target - 1까지의 모든 금액을 만들 수 있다고 가정해보자. 화폐 단위가 작은 순서대로 동전을 확인하며, 현재 확인하는 동전을 이용해 target 금액 또한 만들 수 있는지 확인하면 된다. 만약 target 금액을 만들 수 있다면, traget 값을 증가시키는 방식을 이용한다.  

예를 들어 3개의 동전이 있고, 각 화폐의 단위가 1, 2, 3이라고 하자.  
그러면 1부터 6까지의 모든 금액을 만들 수 있다.  
- 1원: 1  
- 2원: 2  
- 3원: 3  
- 4원: 1 + 3  
- 5원: 2 + 3  
- 6원: 1 + 2 + 3  

그다음 금액 7도 만들 수 있는지 확인하면 된다. 이때 화폐 단위가 5인 동전 하나가 새롭게 주어졌다고 가정하자. 이제 화폐 단위가 5인 동전이 주어졌기 때문에, 1부터 11까지의 모든 금액을 만들 수 있다.  
- 1원: 1  
- 2원: 2  
- 3원: 3  
- 4원: 1 + 3  
- 5원: 5  
- 6원: 1 + 5  
- 7원: 2 + 5  
- 8원: 3 + 5  
- 9원: 1 + 3 + 5  
- 10원: 2 + 3 + 5  
- 11원: 1 + 2 + 3 + 5  

이후에 우리는 금액 12도 만들 수 있는지 확인을 하면 되는 방식이다. 이때 화폐 단위가 13인 동전 하나가 새롭게 주어졌다고 가정하자. 이때 금액 12를 만든느 방법은 존재하지 않는다. 그래서 이 경우에는 정답이 12가 된다.  
다른 예시 1, 2, 3, 8을 들어보자.  
- step0 : 처음에는 금액 1을 만들 수 있는지 확인하기 위해, target = 1로 설정한다.  
- step1 : target = 1을 만족할 수 있는지 확인한다. 우리에게는 화폐 단위가 1인 동전이 있다. 이 동전을 이용해서 금액 1을 만들 수 있다. 이어서 target = 1 + 1 = 2로 업데이트 한다. (1까지의 모든 금액을 만들 수 있다는 말과 같다.)  
- step2 : target = 2를 만족할 수 있는지 확인한다. 화폐 단위가 2인 동전이 있다. 따라서 target = 2 + 2 = 4가 된다.(3까지의 모든 금액을 만들 수 있다는 말과 같다.)  
- step3 : target = 4를 만족할 수 있는지 확인한다. 화폐 단위가 3인 동전이 있다. 따라서 target = 4 + 3 = 7이 된다.(6까지의 모든 금액을 만들 수 있다는 말과 같다.)  
- step4 : target = 7을 만족할 수 있는지 확인한다. 우리에게는 이보다 큰, 화폐 단위가 8인 동전이 있다. 따라서 금액 7을 만드는 방법은 없다. 따라서 정답은 7이 된다.  

> 문제 답안  

```python
n = int(input())
data = list(map(int, input().split()))
data.sort()

target = 1
for x in data:
  # 만들 수 없는 금액을 찾았을 때 반복 종료
  if target < x:
    break
  target += x

# 만들 수 없는 금액 출력
print(target)
```

<br>
기출 : K 대회 기출  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊