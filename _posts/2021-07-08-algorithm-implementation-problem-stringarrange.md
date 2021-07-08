---
title: "[Implementation] 문자열 재정렬"

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

## 문자열 재정렬  

난이도 : ⭐  
푼횟수 : 🟢⚪⚪  

알파벳 대문자와 숫자(0 ~ 9)로만 구성된 문자열이 입력으로 주어집니다. 이때 모든 알파벳을 오름차순으로 정렬하여 이어서 출력한 뒤에, 그 뒤에 모든 숫자를 더한 값을 이어서 출력합니다.  
예를 들어 K1KA5CB7이라는 값이 들어오면 ABCKK13을 출력합니다.  

**<u>입력 조건</u>**  
- 첫째 줄에 하나의 문자열 S가 주어집니다. (1 <= S의 길이 <= 10,000)  

**<u>출력 조건</u>**  
- 첫째 줄에 문제에서 요구하는 정답을 출력합니다.  

**<u>입력 예시1</u>**  
K1KA5CB7  

**<u>출력 예시1</u>**  
ABCKK13  

**<u>입력 예시2</u>**  
AJKDLSI412K4JSJ9D  

**<u>출력 예시2</u>**  
ADDIJJJKKLSS20  

> 나의 풀이  

```python
s = input()
array_string = []
array_sum = 0
result = ""

for i in range(len(s)):
    if ord(s[i]) >= 65:
        array_string.append(s[i])
    else:
        array_sum += int(s[i])

array_string.sort()
for i in range(len(array_string)):
    result += array_string[i]

result += str(array_sum)

print(result)
```

> 문제 해설  

이 문제는 `요구하는 내용을 그대로 구현`하면 되는 문제이다. 문자열이 입력되었을 때 문자를 하나씩 확인한 뒤에, 숫자인 경우 따로 합계를 계산하고, 알파벳인 경우 별도의 리스트에 저장한다. 그래서 결과적으로 리스트에 저장된 알파벳들을 정렬해 출력하고, 합계를 뒤에 붙여서 출력하면 된다.  

> 문제 답안  

```python
data = input()
result = []
value = 0

# 문자를 하나씩 확인하며
for x in data:
  # 알파벳인 경우 결과 리스트에 삽입
  if x.isalpha():
    result.append(x)
  # 숫자는 따로 더하기
  else:
    value += int(x)

# 알파벳을 오름차순으로 정렬
result.sort()

# 숫자가 하나라도 존재하는 경우 가장 뒤에 삽입
if value != 0:
  result.append(str(value))

# 최종 결과 출력(리스트를 문자열로 변환하여 출력)
print(''.join(result))
```  

<br>
기출 : Facebook 인터뷰

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
