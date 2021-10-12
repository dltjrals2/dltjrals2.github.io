---
title: "[Sorting] 실패율"

categories:
  - Algorithm
tags:
  - [Algorithm, Python, Sotring, Programmers]

toc:  true
toc_sticky: true

date: 2021-10-12
last_modified_at: 2021-10-12
---

## 실패율  

난이도 : 🌕  
푼횟수 : 🟢⚪⚪  

슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.  

이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.  

- 실패율은 다음과 같이 정의한다.  
-- 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수  

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.  

**<u>제한사항</u>**  
- 스테이지의 개수 N은 1 이상 500 이하의 자연수이다.  
- stages의 길이는 1 이상 200,000 이하이다.  
- stages에는 1 이상 N + 1 이하의 자연수가 담겨있다.  
- 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.  
- 단, N + 1 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.  
- 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.  
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0 으로 정의한다.  

**<u>일출력 예</u>**  
N	        stages	            result  
5	[2, 1, 2, 6, 2, 4, 3, 3]	[3,4,2,1,5]  
4	[4,4,4,4,4]	[4,1,2,3]  

**<u>입출력 예 설명</u>**  
입출력 예 #1  
1번 스테이지에는 총 8명의 사용자가 도전했으며, 이 중 1명의 사용자가 아직 클리어하지 못했다. 따라서 1번 스테이지의 실패율은 다음과 같다.  
- 1 번 스테이지 실패율 : 1/8  
2번 스테이지에는 총 7명의 사용자가 도전했으며, 이 중 3명의 사용자가 아직 클리어하지 못했다. 따라서 2번 스테이지의 실패율은 다음과 같다.  
- 2 번 스테이지 실패율 : 3/7  
마찬가지로 나머지 스테이지의 실패율은 다음과 같다.  
- 3 번 스테이지 실패율 : 2/4  
- 4번 스테이지 실패율 : 1/2  
- 5번 스테이지 실패율 : 0/1  
각 스테이지의 번호를 실패율의 내림차순으로 정렬하면 다음과 같다.  
- [3,4,2,1,5]  

입출력 예 #2  
모든 사용자가 마지막 스테이지에 있으므로 4번 스테이지의 실패율은 1이며 나머지 스테이지의 실패율은 0이다.  
- [4,1,2,3]  

> 나의 풀이  

```python
def solution(N, stages):
    answer = []
    answer_dict = dict()
    for i in range(1, N+1):
        total_player = 0
        not_clear_player = 0
        for stage in stages:
            if stage >= i:
                total_player += 1
            if stage == i:
                not_clear_player += 1
        calc = 0 if total_player == 0 else not_clear_player / total_player
        answer.append((i, calc))

    answer = sorted(answer, key = lambda t : t[1], reverse = True)
    answer = [i[0] for i in answer]
    return answer
```

> 문제 해설  

실패율의 정의에 따라서 실수 없이 구현을 잘해주면 된다.  

실패율은 '스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어의 수'로 정의된다.  

따라서 이 문제를 해결하기 위해서는 스테이지 번호 (i)를 1부터 N까지 증가시키며, 해당 단계에 물러 있는 플레이어의 수를 계산한다.  

그러한 플레이어의 수 정보를 이용하여 모든 스테이지의 따른 실패율을 계산한 뒤에 저장하면 된다.

> 문제 답안  

```python
def solution(N, stages):
    answer = []
    length = len(stages)

    # 스테이지 번호를 1부터 N까지 증가시키며
    for i in range(1, N + 1):
        # 해당 스테이지에 머물러 있는 사람의 수 계산
        count = stages.count(i)

        # 실패율 계산
        if length == 0:
            fail = 0
        else:
            fail = count / length

        # 리스트에 (스테이지 번호, 실패율) 원소 삽입
        answer.append((i, fail))
        length -= count

    # 실패율을 기준으로 각 스테이지를 내림차순 정렬
    answer = sorted(answer, key = lambda t : t[1], reverse = True)

    # 정렬된 스테이지 번호 출력
    answer = [i[0] for i in answer]
    return answer
```


<br>
기출 : 2019 카카오 신입 공채 1차  
링크 : <https://programmers.co.kr/learn/courses/30/lessons/42889>  

---
**🐢 현재 공부하고 있는 `이것이 취업을 위한 코딩 테스트다 with 파이썬 - 나동빈 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊