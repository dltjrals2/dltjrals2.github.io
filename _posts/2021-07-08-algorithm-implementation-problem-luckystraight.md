---
title: "[Implementation] 럭키 스트레이트"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-07-08
last_modified_at: 2021-07-08
sitemap :
  changefreq : daily
  priority : 1.0
---

## 럭키 스트레이트  

난이도 : ⭐  
푼횟수 : 🟢⚪⚪  

게임의 아웃복서 캐릭터는 필살기인 '럭키 스트레이트' 기술이 있습니다. 이 기술은 매우 강력한 대신에 게임 내에서 점수가 특정 조건을 만족할 때만 사용할 수 있습니다.  
특정 조건이란 현재 캐릭터의 점수를 N이라고 할 때 자릿수를 기준으로 점수 N을 반으로 나누어 왼쪽 부분의 각 자릿수의 합과 오른쪽 부분의 각 자릿수의 합을 더한 값이 동일한 상황을 의미합니다.  
예를 들어 현재 점수가 123,402라면 왼쪽 부분의 각 자릿수의 합은 1 + 2 + 3, 오른쪽 부분의 각 자릿수의 합은 4 + 0 + 2이므로 두 합이 6으로 동일하여 럭키 스트레이트를 사용할 수 있습니다.  
현재 점수 N이 주어지면 럭키 스트레이트를 사용할 수 있는 상태인지 아닌지를 알려주는 프로그램을 작성하세요.  

**<u>입력 조건</u>**  
- 첫째 줄에 점수 N이 정수로 주어집니다. (10 <= N <= 99,999,999) 단, 점수 N의 자릿수는 항상 짝수 형태로만 주어집니다. 예를 들어 자릿수가 5인 12,345와 같은 수는 입력으로 들어오지 않습니다.  

**<u>출력 조건</u>**  
- 첫째 줄에 럭키 스트레이트를 사용할 수 있다면 "LUCKY"를, 사용할 수 없다면 "READY"를 출력합니다.  

**<u>입력 예시1</u>**  
123402  

**<u>출력 예시1</u>**  
LUCKY  

**<u>입력 예시2</u>**  
7755  

**<u>출력 예시2</u>**  
READY  

> 나의 풀이  

```python
n = input()
len_n = len(n)
left_sum = 0
right_sum = 0

for i in range(0, int(len_n / 2)):
    left_sum += int(n[i])

for j in range(int(len_n / 2), int(len_n)):
    right_sum += int(n[j])

if left_sum == right_sum:
    print("LUCKY")
else:
    print("READY")
```

> 문제 해설  

정수형 데이터가 입력으로 들어왔을 때, 이를 각 자릿수로 구분하여 합을 계산해야 한다. 파이썬의 경우 입력받은 데이터가 문자열 형태이므로, 문자열에서 각 문자를 하나씩 확인하며 정수형으로 변환한 뒤에 그 값을 합 변수에 더할 수 있다.  

> 문제 답안  

```python
n = input()
length = len(n) # 정수값의 총 자릿수
summary = 0

# 왼쪽 부분의 자릿수 합 더하기  
for i in range(length // 2):
  summary += int(n[i])

# 오른쪽 부분의 자릿수 합 빼기
for i in range(length // 2, length):
  summary -= int(n[i])

# 왼쪽 부분과 오른쪽 부분의 자릿수 합이 동일한지 검사
if summary == 0:
  print("LUCKY")
else:
  print("READY")
```

<br>
기출 : 핵심유형  
링크 : <https://www.acmicpc.net/problem/18406>{: target="_blank"}  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
