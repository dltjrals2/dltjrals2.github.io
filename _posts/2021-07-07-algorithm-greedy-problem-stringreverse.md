---
title: "[Greedy] 문자열 뒤집기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Greedy]

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

## 문자열 뒤집기  

난이도 : ⭐  
푼횟수 : 🟢⚪⚪  

다솜이는 0과 1로만 이루어진 문자열 S를 가지고 있습니다. 다솜이는 이 문자열 S에 있는 모든 숫자를 전부 같게 만들려고 합니다. 다솜이가 할 수 있는 행동은 S에서 연속된 하나 이상의 숫자를 잡고 모두 뒤집는 것입니다. 뒤집는 것은 1을 0으로, 0을 1로 바꾸는 것을 의미합니다.  
예를 들어 S = 0001100일 때는 다음과 같습니다.  
1. 전체를 뒤집으면 1110011이 됩니다.  
2. 4번째 문자부터 5번째 문자까지 뒤집으면 1111111이 되어서 두 번 만에 모두 같은 숫자로 만들 수 있습니다.  

하지만, 처음부터 4번째 문자부터 5번째 문자까지 문자를 뒤집으면 한 번에 0000000이 되어서 1번 만에 모두 같은 숫자를 만들 수 있습니다.  
문자열 S가 주어졌을 때, 다솜이가 해야 하는 행동의 최소 횟수를 출력하세요.  

**<u>입력 조건</u>**  
- 첫째 줄에 0과 1로만 이루어진 문자열 S가 주어집니다. S의 길이는 100만보다 작습니다.  

**<u>출력 조건</u>**  
- 첫째 줄에 다솜이가 해야 하는 행동의 최소 횟수를 출력합니다.  

**<u>입력 예시1</u>**  
0001100  

**<u>출력 예시1</u>**  
1  

> 나의 풀이  

```python
s = input()
array_s = []
zero_count = 0
one_count = 0

for i in range(len(s)):
    array_s.append(int(s[i]))

if array_s[0] == 1:
    zero_count += 1
else:
    one_count += 1

for i in range(len(array_s) - 1):
    if array_s[i] != array_s[i + 1]:
        if array_s[i + 1] == 1:
            zero_count += 1
        else:
            one_count += 1

print(min(zero_count, one_count))
```

> 문제 해설  

`전부 0으로 바꾸는 경우와 전부 1로 바꾸는 경우 중에서 더 작은 횟수를 가지는 경우를 계산`하면 된다.

예를 들어 문자열이 "0001100"이라고 가정해보면, 이때 '모두 0으로 만드는 경우'와 '모두 1로 만드는 경우'를 고려했을 때 각각 뒤집기 횟수를 계산하면 다음과 같다.  

1. 모두 0으로 만드는 경우  
![image](https://user-images.githubusercontent.com/37467408/124691608-0a996900-df17-11eb-8520-6d2afd3f86e8.PNG)  

2. 모두 1로 만드는 경우  
![image](https://user-images.githubusercontent.com/37467408/124691647-1a18b200-df17-11eb-9be8-8147e3c120f4.PNG)  

> 문제 답안  

```python
data = input()
count0 = 0 # 전부 0으로 바꾸는 경우
count1 = 0 # 전부 1로 바꾸는 경우

# 첫 번째 원소에 대해서 처리
if data[0] == '1':
  count0 += 1
else:
  count1 += 1

# 두 번째 원소부터 모든 원소를 확인하며
for i in range(len(data) - 1):
  if data[i] != data[i + 1]:
    # 다음 수에서 1로 바뀌는 경우
    if data[i + 1] == '1':
      count0 += 1
    # 다음 수에서 0으로 바뀌는 경우
    else:
      count1 += 1

print(min(count0, count1))
```  

<br>
기출 : 핵심유형  
링크 : <https://www.acmicpc.net/problem/1439>{: target="_blank"}  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
