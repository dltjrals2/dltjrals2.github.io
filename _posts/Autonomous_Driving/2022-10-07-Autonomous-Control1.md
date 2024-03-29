---
title: "PID 제어"

categories:
  - Autonomous_Control
tags:
  - [Autonomous Drive, Control, PID, Robotics]

use_math: true

toc:  true
toc_sticky: true

date: 2022-10-07
last_modified_at: 2022-10-07
---

### 제어(Control)  

제어(Control)란, `시스템의 상태 x를 원하는 목표치 $ X_d $로 도달시키는 과정이다.` 시스템을 로봇팔이라고 한다면 관절의 각도가 시스템의 상태가 된다.  

제어에서 중점사항으로 보는 것은 `1. 성능(Performance)`과 `2. 외란에 대한 안정성(Stability)`이다. 성능은 `시스템의 상태가 목표치에 부합한 정도`이며 안정성은 `시스템에 외란이 가해져도 발산하지 않는 것`을 말한다.  

용어를 정리하기 위해, 현재 A인 값을, B로 조정해 C로 만들고자 한다고 가정해보자.  

- 설정값(SV, Set point Variable), 목표값: 제어의 목표가 되는 값이다. C에 해당한다.  
- 제어량: 제어 대상에 대해 제어해야 하는 양, A에서 C로 가는 변화값이 된다.  
- 조작량(MV, Manipulated Variable): 제어량에 영향을 주는 것 중, 목표값에 도달하도록 이용하는 것이다. B에 해당한다.  
- 측정값(PV, Process Variable): 제어가 진행중일 때 현재 측정(출력)되고 있는 값이다. A, C와 같은 단위를 가진다.  
- 편차, 오차(e, error): 측정값과 설정값의 차이값이다. A에서 C로 가는 도중 D 값이 측정되었다 하면, C와 D의 차이이다.  
- 외란: 사람의 힘이 미치지 못하지만 제어량을 변화시키는 원인이다.  

자동차의 속도 제어를 예로 들자면, 제어 대상은 자동차가 되고, 조작량은 가속 페달에 의해 조절되는 엔진의 힘, 제어량은 속도가 되며, 외란은 공기의 저항이나 노면의 상태, 경사 등이 된다.  

### 피드백 제어기와 PID 제어기  

피드백 제어(Feedback Control)란, 입력(Input)에 대해 특정 프로세스(Process)를 거친 출력(Output)이 다시 입력에 영향을 미치고 이것이 다시 프로세스로 들어가 내는, 반복적인 제어 방식이다. 출력이 목표값에 도달하려면 어떻게 되어야 하는지(줄여야 할지, 늘여야 할지 등) 피드백을 한다고 이해하면 된다.  

![image](https://user-images.githubusercontent.com/37467408/194344865-cfb735a5-59f2-481e-a87a-08cefab4a856.png)  

PID제어기(Proportional - Integration - Differential Controller) / PID 제어(control)는 피드백(feedback) 제어기의 형태를 가진다. PID 제어의 피드백 제어의 측면은 다음과 같은 특징을 가진다.  

- 제어 대상의 출력(output)을 측정해 원하는 값인 설정값(set value)와 비교하여 오차(error)을 계산한다.  
- 제어에 필요한 제어량을 도출된 오차값을 이용해 계산한다.  

![image](https://user-images.githubusercontent.com/37467408/194345475-a2e351e6-17a9-4b53-97f1-6480e2fc3962.png)  

아래 사진은 PID 제어기를 활용해 출력을 조절하는 예시이다.  

![image](https://user-images.githubusercontent.com/37467408/194345601-90ca32f7-3e9a-4042-a22d-2e85b6b7d6f2.png)  

### 제어값 계산 방식  

#### 제어량 계산식  

시간에 따른 제어량은 다음과 같이 계산한다.  

$ K_p e(t) + K_i \int_0^t e(t) dt + K_d \frac{de}{dt} $  

- $ K_p e(t) $ : `비례항` - 편차에 비례한다. 현재 상태에서 편차의 크기에 비례해 제어 작용을 한다.  
- $ K_i \int_0^t e(t) dt $ : `적분항` - 오차값의 적분(integral)에 비례하여, 정상 상태(Steady-state) 오차를 없앤다.  
- $ K_d \frac{de}{dt} $ : `미분항` - 오차값의 미분(derivative)에 비례한다. 출력값의 급격한 변화를 막아 Overshoot를 줄이며 안정성을 향상시킨다.  

여기서 $ e(t) = y_s (t) - y(t) $ = (제어량 - 목표값)으로, 편차에 해당한다.  

#### 이득(Gain)  

위 제어량 계산식에서 $ `K_p, K_i, K_d` $를 Gain 값이라고 한다. 각각 비례 제어, 적분 제어, 편차 제어를 조절하는 계수가 된다.  

때에 따라서는 변형 형태로 특정 항만을 가질 수 있다. 비례항만을 가질 때는 P제어기, 비례-미분항만을 가질 때는 PD 제어기라 하는 등의 방식이다.  

### 튜닝  

튜닝은 Gain 값을 수학적/실험적/경험적으로 계산해 조정하는 것을 말한다.  

무작정 $ K_p, K_i, K_d $를 조절할 수도 있지만, 정확성이 떨어지고 시간이 매우 오래 걸린다.  

PID 제어기의 튜닝의 방식에는 `저글러 - 니콜라스 방법` 등 여러가지가 존재한다. 이 방법은 뒤에서 설명하도록 한다.  

### 제어 매커니즘  

#### P(비례) 제어  

P(비례) 제어 는 `제어량과 목표값의 차이(편차)에 비례하여 제어하는 방식이다.` 편차가 크면 조작량을 크게, 편차가 작으면 조작량을 작게하여 목표값에 유연하게 도달시킨다.  

![image](https://user-images.githubusercontent.com/37467408/194348608-f0de7a4c-e9cc-41a4-9407-2e27624b5c42.png)

$ K_p $값을 크게 하면 편차에 대한 조작량이 크므로, `상승시간이 줄어 빠르게 목표값에 도달하는 대신, 오버슈트 값이 크고 시스템에 무리를 줄 위험`이 있다.  

반대로 $ K_p $을 작게 하면 상승 시간은 길어져 목표값에 느리게 도달하고, 대신 오버슈트는 감소한다.  

> `오버슈트(Overshoot)와 상승시간`  
> 오버슈트는 목표값보다 오차가 커지는 것을 말한다. 오버슈트 값이 과하게 커지면 시스템에 무리를 줄 수 있다.  
> 예를 들면, 제어하려는 대상이 전압이라면 오버슈트가 발생하면 과전압이 발생할 수 있는 것이다. 반대 개념은 언더 슈트(Undershoot)이다.  
> 상승시간이란 0에서 부터 처음 오버슈터가 될 때까지 걸리는 시간이다.  
 

> `정상 상태(steady state)와 정착시간`  
> 정상 상태란 목표값에 거의 가까워진 상태이며, 정착시간이란 시작부터 정상상태가 될 때까지 걸리는 시간이다. 정착시간과 $ K_p $는 관계 없는 값이다.  
> 제어는 실질적으로 (제어량) = (목표값)이 되긴 힘들다. 따라서 목표값의 일정 오차 범위내에 제어값이 위치한다면 이를 제어 완료로 보는데, 이때까지 걸리는 시간이 정착시간이 되기도 한다. 이 시간이 짧게 소요될수록 좋은 제어기이다.  


![image](https://user-images.githubusercontent.com/37467408/194349876-86dbfea2-9701-4ec9-8547-bcc31f6aa060.png)

P 제어의 문제점은, `정상 상태 오차(잔류편차)가 남는다`는 것이다. 정상상태 오차는 목표값의 일정 범위 내에 제어값이 들어오기는 했으나, 끝내 없어지지 않고 남아 있는 오차를 말한다. 목표치에 가까워진다는 것은 편차가 작아진다는 뜻이며, 이는 곧 조작량이 너무 작아 측정 오차 범위에 속하여 세밀한 제어가 힘들다는 의미이다.  

결국 목표치에 매우 가까운 상태로 안정되어 버리는 현상이 일어나고, 이때 목표치와의 편차가 잔류편차가 된다.  

즉, `설정값에 매우 근사한 상태에서 안정화 되어버려 측정값과 설정값이 같아질 수가 없다.`  

#### I(적분) 제어  

P 제어에서 `정상 상태 오차의 발생을 해결`하기 위한 방법으로 I 제어를 도입할 수 있다.  

I(적분) 제어는 편차를 시간에 대해 누적(적분)하고, 이 `누적값이 특정값이 되면 조작량을 증가시켜 편차를 없앰`으로써 목표값에 더욱 정밀하게 접근하도록 한다.  

![image](https://user-images.githubusercontent.com/37467408/194350717-d4deb93a-849a-4f91-8ca9-0e0661516b27.png)  

$ K_i $값이 크면 오버슈트가 커지고 상승시간이 약간 감소한다. $ K_i $값이 작으면 그 반대가 된다.  

![image](https://user-images.githubusercontent.com/37467408/194351129-ec0d55db-8521-4cf3-8c1b-0fbe243158c6.png)  

I 제어의 문제점은 다음과 같다.  

P 제어의 정상 상태 오차를 제어할 수 있는 대신, 그만큼 정착시간이 추가된다. 그리고, $ K_i $값이 클수록 오버슈트/언더슈트가 커지고 이에 따라 정착시간은 더 늘어난다.  

외란 발생 시 반응속도(응답시간)가 느려지는 것도 문제이다. I 제어를 통해 편차에 대한 조작량을 적게 해두었으므로, 외란으로 인해 제어량 변화가 급작스럽게 커져도 조작량 변화는 작을 수 밖에 없다. 그리고 편차를 시간 단위로 측정하므로 외란 신호를 다시 목표값 부근으로 복원시키는 데 시간이 오래 걸린다. 즉 응답시간이 오래 걸린다.  

또한 외란이나 오차의 누적이 계속된다면 $ K_i $값이 커져 제어량의 발산이 유발될 수 있다.  

누적에 의한 발산을 막기 위해 적분 값을 초기화하는 등 anit-wind up 기법을 사용하기도 한다.  

#### D(미분) 제어  

D(미분) 제어는 `목표량과 제어량의 편차를 비교해 이와 반대되는 쪽(기울기)으로 조작하는 방식`이다.  

편차가 +10 이라면 조작량 -10이 되도록 조작 방향을 설정하는 식이다. 급격히 어느 한 방향으로 힘이 쏠리면 반대방향으로 힘을 주어 그 힘을 상쇄하려는 제어이다.  

`외란과 목표값의 편차, 혹은 이번 편차와 직전 편차를 비교해 이 크기에 따라 조작량을 결정`하며 D 제어를 통해 외란에 대한 응답성을 개선하고 안정성을 높일 수 있다.  

목표값에 다가가다가 이대로면 설정값을 초과할 것 같아 제어값의 변화에 대해 그것을 억제하는 작업으로 이해하면 좋을 것 같다.  

> 짧은 샘플링 시간이나 갑작스런 외란이 미분값을 급작스럽게 변화시킬 수도 있으몰 보통 앞에 저주파 필터(Low Pass Filter)를 붙여 사용하기도 한다.  


![image](https://user-images.githubusercontent.com/37467408/194352746-2dfdb7c1-9b54-4fbb-a90c-8345ed2f16cc.png)  

$ K_d $값이 클수록 `안정화에 걸리는 시간이 줄어들어 정착시간이 감소된다.` 다만 정상상태에서는 D제어 보다 PI제어의 영향이 크므로 이 구간에서 정상상태 오차의 변화는 거의 없다.  

P, I제어의 경우 편차와 같은 방향으로 제어량을 조작시키므로 게인 값과 오버슈트 값은 비례하고 상승 시간이 반비례했다. 그러나 D 제어는 편차와 반대 부호로 조작을 하기 때문에 $ K_d $값이 클수록 오버슈트는 감소하고 오차를 빨리 교정해, 상승시간과 정착 시간이 감소한다.  

#### PID 제어  

PID 제어에서 각 제어의 역할은 다음과 같다.  

- P 제어 : 목표값 도달 시간(B) 감소, 목표값과 멀수록 많이, 가까울수록 적게 조작  
- I 제어 : 정상 상태 오차(C) 감소, 목표값에 딱맞게 주행하기 위해 미세하게 조정하여 편차 제거  
- D 제어 : 오버슈트(A) 억제, 제어값 변화 억제  

각 제어기의 조작을 위해 각 게인 값을 잘 조절해야 한다. 정리하면 아래와 같다.  

![image](https://user-images.githubusercontent.com/37467408/194353755-b7aa8585-a99d-49c9-8328-3768573e9666.png)  

제어기를 거쳐 최종적으로 변화시키는 값 u는 다음과 같이 다시 쓸 수 있다.  

$ K_p e(t) + K_i \int_0^t e(t) dt + K_d \frac{de}{dt} $  

$ = K_p(e(t) + \frac{K_i}{K_p} \int_0^t e(t) dt + \frac{K_d}{K_p} \frac{de}{dt}) $  

$ = K_p(e(t) + \frac{1}{T_i} \int_0^t e(t) dt + T_d \frac{de}{dt})  $  

각 항을 $ K_p $로 묶었을 때 나오는 $ T_i $는 Integral Time, $ T_d $는 Derivative Time이라고 한다. 맨 마지막 식을 Matlab 등의 시뮬레이션 툴에서 많이 사용한다.  

### 프로세스 게인, 제어 게인  

프로세스 게인(Process Gain)은 제어 대상이 가진 전체 능력이다.  

승용차가 총 150km/h의 속도까지 낼 수 있고, 스포츠카가 300km/h까지 가능하다고 할 때, 페달을 10% 밟으면 승용차는 15km/h, 스포츠카는 30km/h를 낼 수 있다. 이때 속도를 내는 능력이 프로세스 게인이고 스포츠카가 승용차의 두 배가 된다.  

제어 게인(Control Gain)은 제어를 할 수 있는 능력으로, 제어 대상의 응답을 일정량 변화시키는데 필요한 제어 출력의 비율이다.  

앞선 예에서는 30km/h를 내는 데 승용차보다 스포츠카가 페달을 밟는 양이 커야 하고, 이는 제어 게인이 승용차가 더 높다는 뜻이 된다.  

### 비례대  

비례대는 조작량을 비례시키는 폭을 말한다. 설정값을 중심으로 설정한 폭 이내로 측정값이 들어오면 그 때부터 설정값과 측정값과의 편차에 비례하여 제어한다.  

비례대 폭을 좁게 설정하면 제어 이득이 높아지고 감도는 올라간 안정성이 나빠지고, 넓게 설정하면 제어 이득이 낮아지고 감도는 떨어지나 안정성은 좋아진다.  

![image](https://user-images.githubusercontent.com/37467408/194355700-6f09713f-f9eb-47af-9cc3-0c33eda7edf2.png)  

### 지글러-니콜라스 방법(Zigler-Nichols Method)  

PID 제어기의 튜닝 방법 중 하나로, 휴리스틱(경험적) 방법에 속한다. 모델에 대한 정보가 없을 때 PID 게인을 조절할 수 있는 방법이다. 물론 이 방법이 모든 상황에서 최적이란 뜻은 아니다.  

- 적분 게인($K_p$)와 미분 게인($K_d$)를 0으로 한다.  
- P 제어기만 사용하여 $K_p$를 0부터 최종 게인 $K_u$까지 올린다.  
- 여기서 $K_u$은 제어 루프의 출력이 안정적이고 일관된 진동을 가질 때의 게인을 말한다. 여기서의 진동 주기는 $ T_u $이고, 진폭 비율을 M이라고 했을 때 $ K_u = \frac{1}{M} $이다.  
- 이후에 사용할 컨트롤러 유형, 원하는 동작에 따라 $K_p, K_i, K_d$ 값을 조정한다.  

![image](https://user-images.githubusercontent.com/37467408/194356519-97a49c1b-2fcb-4eef-94f4-375dc2d03c56.png)  

**🐢 현재 공부하고 있는 자율주행/자율운항을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}   

참고 블로그  
[PID 제어 내용](https://velog.io/@717lumos/Control-PID-%EC%A0%9C%EC%96%B4)

감사합니다.😊