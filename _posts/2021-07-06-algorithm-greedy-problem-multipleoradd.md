---
title: "[Greedy] 곱하기 혹은 더하기"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Greedy]

toc:  true
toc_sticky: true
show_date: true
read_time: false

date: 2021-07-06
last_modified_at: 2021-07-07
sitemap :
  changefreq : daily
  priority : 1.0
---

## 곱하기 혹은 더하기  

난이도 : ⭐  
푼횟수 : 🟢⚪⚪  

각 자리가 숫자(0부터 9)로만 이어진 문자열 S가 주어졌을 때, 왼쪽부터 오른쪽으로 하나씩 모든 숫자를 확인하며 숫자 사이에 'X' 혹은 '+'연산자를 넣어 결과적으로 만들어질 수 있는 가장 큰 수를 구하는 프로그램을 작성하세요. 단, +보다 X를 먼저 계산하는 일반적인 방식과 달리, 모든 연산은 왼쪽에서부터 순서대로 이루어진다고 가정합니다.  
예를 들어 02984라는 문자열이 주어지면, 만들어질 수 있는 가장 큰 수는 ((((0 + 2) X 9) X 8) X 4) = 576입니다. 또한 만들어질 수 있는 가장 큰 수는 항상 20억 이하의 정수가 되도록 입력이 주어집니다.  

**<u>입력 조건</u>**  
- 첫째 줄에 여러 개의 숫자로 구성된 하나의 문자열 S가 주어집니다. (1 <= S의 길이 <= 20)  

**<u>출력 조건</u>**  
- 첫째 줄에 만들어질 수 있는 가장 큰 수를 출력합니다.  

**<u>입력 예시1</u>**  
02894  

**<u>출력 예시1</u>**  
576  

**<u>입력 예시2</u>**  
567  

**<u>출력 예시2</u>**  
210  

> 나의 풀이  

```python
s = input()
array_s = []

for i in range(len(s)):
    array_s.append(int(s[i]))

result = array_s[0]

for i in range(1, len(array_s)):
    if array_s[i] <= 1 or result <= 1:
        result += array_s[i]
    else:
        result *= array_s[i]

print(result)
```

> 문제 해설  

일반적으로 특정한 두 수에 대하여 연산을 수행할 때, 대부분은 '+'보다는 'X'가 더 값을 크게 만든다. 하지만, 두 수 중에서 하나라도 '0' 혹은 '1'인 경우에는 곱하기보다는 더하기를 수행하는 것이 효율적이다. 즉, `두 수에 대하여 연산을 수행할 때, 두 수 중에서 하나라도 1 이하인 경우에는 더하며, 두 수가 모두 2 이상인 경우에는 곱하면 된다.`  

> 문제 답안  

```python
data = input()

# 첫 번째 문자를 숫자로 변경하여 대입
result = int(data[0])

for i in range(1, len(data)):
  # 두 수 중에서 하나라도 '0' 혹은 '1'인 경우, 곱하기보다는 더하기 수행
  num = int(data[i])
  if num <= 1 or result <= 1:
    result += num
  else:
    result *= num

print(result)
```  

<br>
기출 : Facebook 인터뷰  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
