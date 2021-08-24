---
title: "[Implementation] 문자열 압축"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Implementation, 풀이실패]

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

## 문자열 압축  

난이도 : ⭐⭐  
푼횟수 : 🔴⚪⚪  

데이터 처리 전문가가 되고 싶은 "어피치"는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.  
간단한 예로 "aabbaccc"의 경우 "2a2ba3c"(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, "abcabcdede"와 같은 문자열은 전혀 압축되지 않습니다. "어피치"는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.  
예를 들어, "ababcdcdababcdcd"의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 "2ab2cd2ab2cd"로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 "2ababcdcd"로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.  
다른 예로, "abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.  
압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.  

**<u>제한 사항</u>**  
- s의 길이는 1 이상 1,000 이하입니다.  
- s는 알파벳 소문자로만 이루어져 있습니다.  

**<u>입출력 예</u>**  
![image](https://user-images.githubusercontent.com/37467408/124846544-03846080-dfd4-11eb-9cb2-7b2aacd584e9.PNG)  

**<u>입출력 예에 대한 설명</u>**  
- 입출력 예1  
문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.  
- 입출력 예2  
문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.  
- 입출력 예3  
문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.  
- 입출력 예4  
문자열을 2개 단위로 자르면 "abcabcabcabc6de" 가 됩니다.  
문자열을 3개 단위로 자르면 "4abcdededededede" 가 됩니다.  
문자열을 4개 단위로 자르면 "abcabcabcabc3dede" 가 됩니다.  
문자열을 6개 단위로 자를 경우 "2abcabc2dedede"가 되며, 이때의 길이가 14로 가장 짧습니다.  
- 입출력 예5  
문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다.  
따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다.  
이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.  

> 나의 풀이  

풀이 실패

> 문제 해설  

입력으로 주어지는 문자열의 길이가 1,000 이하이기 때문에 가능한 모든 경우의 수를 탐색하는 완전 탐색을 수행할 수 있다. 예를 들어 길이가 N인 문자열이 입력되었다면 1부터 N / 2까지의 모든 수를 단위로 하여 문자열을 압축하는 방법을 모두 확인하고, 가장 짧게 압축되는 길이를 출력하면 된다.  
예를 들어 "aaaabbabbabb"라는 문자열이 입력으로 들어왔다고 해보자.  
![image](https://user-images.githubusercontent.com/37467408/124860755-3b4bd200-dfed-11eb-9c89-dae38b1e511a.PNG)  
이때 문자열의 길이가 12이므로 1부터 6까지의 수를 '단위(step)'로 하여 문자열을 압축하는 모든 방법을 확인하면 된다.  

- step1 : 문자열을 1개 단위로 자르는 경우  
![image](https://user-images.githubusercontent.com/37467408/124860823-5e768180-dfed-11eb-9b51-b02567ba6181.PNG)  
출력 결과: "4a2ba2ba2b"  

- step2 : 문자열을 2개 단위로 자르는 경우  
![image](https://user-images.githubusercontent.com/37467408/124860897-7e0daa00-dfed-11eb-8fde-74d31945c225.PNG)  
출력 결과: "2aabbabbabb"  

- step3 : 문자열을 3개 단위로 자르는 경우  
![image](https://user-images.githubusercontent.com/37467408/124860960-9da4d280-dfed-11eb-95ad-31102d3e55fd.PNG)  
출력 결과: "aaa3abb"  

이후에 step4 ~ step6에 대해서도 마찬가지로 진행하면 된다. 단 모든 경우 중에서 '문자열을 3개 단위로 잘랐을 때' 문자열의 길이가 7이 되며, 이는 가장 문자열이 많이 압축되는 경우이다.

> 문제 답안  

```python
def solution(s):
  answer = len(s)
  # 1개 단위(step)부터 압축 단위를 늘려가며 확인
  for step in range(1, len(s) // 2 + 1):
    compressed = ""
    prev = s[0:step] # 앞에서부터 step 만큼의 문자열 추출
    count = 1
    # 단위(step) 크기만큼 증가시키며 이전 문자열과 비교
    for j in range(step, len(s), step):
      # 이전 상태와 동일하다면 압축 횟수(count) 증가
      if prev == s[j:j + step]:
        count += 1
      # 다른 문자열이 나왔다면(더 이상 압축하지 못하는 경우라면)
      else:
        compressed += str(count) + prev if count >= 2 else prev
        prev = s[j:j + step] # 다시 상태 초기화
        count = 1
    # 남아 있는 문자열에 대해서 처리
    compressed += str(count) + prev if count >= 2 else prev
    # 만들어지는 압축 문자열이 가장 짧은 것이 정답
    answer = min(answer, len(compressed))
  return answer
```  

<br>
기출 : 2020 카카오 신입 공채  
링크 : <https://programmers.co.kr/learn/courses/30/lessons/60057>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊
