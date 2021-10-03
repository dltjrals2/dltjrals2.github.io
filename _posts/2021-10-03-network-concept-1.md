---
title: "[Network] 인터넷, 인트라넷, 엑스트라넷, 이더넷 개념"

categories:
  - Network
tags:
  - [Network, Internet, Intranet, Extranet, Ethernet]

toc:  true
toc_sticky: true
show_date: true
read_time: false
use_math: false

date: 2021-10-03
last_modified_at: 2021-10-03
sitemap :
  changefreq : daily
  priority : 1.0
---

## 1. 네트워크 세상에 들어서며  

### 1. 인터넷, 인트라넷, 엑스트라넷  

#### 1. 인터넷  
인터넷(Internet)이란, '여러 개의 네트워크를 묶었다'는 의미를 가지고 있다.  
인터넷의 시작은 각각의 회사나 단체에서 자신들의 정보를 공유하고자 만들었던 네트워크를 좀 더 많은 사람들과 정보를 공유하고자 서로 연결하기 시작하면서 시작되었다.  
- 특징  
-- 인터넷에서는 하나의 언어, 즉 하나의 프로토콜만을 사용하는데 인터넷에서 사용하는 이 하나의 프로토콜이 TCP/IP이다.  
-- 악스플로러, 크롬, 파이어폭스 같은 웹 브라우저를 이용해 인터넷을 탐험한다는 것이다.  
-- 인터넷에는 없는 정보가 없다.  

#### 2. 인트라넷  
인트라넷(Intranet)이란, 내부의 네트워크를 의미한다.  
인트라넷의 시작은 웹 브라우저만 가지고 사용하다 보니 편리해서 사내 업무도 웹 브라우저를 사용하고 싶어 시작되었다.  
- 특징  
-- 인트라넷 역시 인터넷과 똑같이 TCP/IP 프로토콜을 사용한다.  
-- 인터넷을 사용하듯 사내 업무를 처리하게 되었다.(업무 보고, 휴가 신청 등)  
-- 인터넷과의 차이점은 회사 사람 말고 다른 사람은 인터넷을 통해 접속이 불가능하다.  

#### 3. 엑스트라넷  
엑스트라넷은 기업의 인트라넷을 그 기업의 종업원 이외에도 협력 회사나 고객에게 사용할 수 있다.  

📌**<u>내용 정리</u>**📌  
- 인터넷은 네트워크를 여러 개 묶어놓은 네트워크 연합을 말하고, TCP/IP라는 공통의 프로토콜을 사용한다.  
- 인트라넷은 회사에서 쓰는 여러 가지 프로그램들을 마치 인터넷을 사용하는 것처럼 쓰도록 만들어 놓은 것이다. 인트라넷은 그 회사의 직원 이외에는 사용할 수 없다.  
- 엑스트라넷은 사용 범위를 직원 외에도 협력 회사나 고객까지로 확대한 개념이다.  

## 2. 네트워크와 케이블, 그리고 친구들  

### 1. LAN(Local Area Network)이란?  
LAN이란, 'Local Area Network'의 약자로 즉, '어느 한정된 공간에서 네트워크를 구성한다'는 것이다.  
예를 들어, 한 사무실에 컴퓨터가 30대가 있는데, 이것을 네트워크로 구성한다면 '사무실에 LAN을 구축한다'라고 말합니다.  
LAN과 비교되는 말로 WAN이 있다.  
WAN은 'Wide Area Network'의 약자로서 '멀리 떨어진 지역을 서로 연결하는 경우'에 사용합니다.  
요즘은 모두 인터넷을 쓰는 세상이니 인터넷에 접속하는 것은 WAN 이라고 봐야 한다. 그래서 요즘은 LAN과 WAN이 공존한다.  
정리하자면, **<u>LAN은 한정된 지역 안에서의 네트워크 구축이고, WAN은 서로 멀리 떨어진 곳을 네트워크로 연결하는 것이다.</u>**

### 2. 이더넷은 인터넷의 친구?  
이더넷은 네트워킹의 한 방식입니다.  
즉, 네트워크를 만드는 방법 중 하나라고 생각하면 된다. 이러한, 이더넷 방식의 가장 큰 특징은 CSMA/CD라는 프로토콜을 사용해서 통신을 한다는 것이다.  
지금 대한민국에서 사용하고 있는 네트워킹 방식의 대부분이 바로 이더넷 방식이다.  
네트워킹 방식은 얼마 전까지만 해도 우리가 말한 이더넷 방식 말고도 토큰링 방식, FDDI 방식, ATM 방식도 있었다.  

> CSMA/CD  

CSMA/CD는 'Carrier Sense Multiple Access/Collision Detection'을 줄여서 부르는 방식입니다. 이 통신 방식을 한 마디로 이야기하자면 '대충 알아서 눈치로 통신하자' 입니다.  
이더넷 환경에서 통신을 하고 싶은 PC나 서버는 먼저 지금 네트워크상에 통신이 일어나고 있는지를 확인합니다. 즉, 캐리어가 있는지를 감지하는 것이다. 이것을 바로 'Carrier Sense'라고 한다. 이때 만약 캐리어가 감지되면, 다시 말해서 누군가가 네트워크상에서 통신을 하고 있으면 자기가 보낼 정보가 있어도 못 보내고 기다린다 그러다가 네트워크에서 통신이 없어지면 눈치를 보다가 무조건 자기 데이터를 네트워크상에 실어서 보낸다.  
여기서 만약 두 PC나 서버가 보낼 데이터를 가지고 눈치를 살피고 있었다고 가정해보자.  
그러다가 네트워크상에서 통신이 일어나지 않고 있다는 것을 알아내 바로 자신의 데이터를 네트워크상에 실어서 보냈다. (두 PC나 서버가 동시에) 이더넷에서는 이렇게 2개 이상의 PC나 서버가 동시에 네트워크상에 데이터를 실어 보내는 경우가 발생할 수있는데 이런 경우를 'Multiple Access'라고 한다.  
통신에서 이렇게 2개의 장비들이 동시에 데이터를 보내다 부딪치는 경우를 충돌(Collision)이 발생했다고 한다.  
따라서, 이더넷에서는 데이터를 네트워크에 실어서 보내고 나서도 혹시 다른 PC 때문에 Collision 이 발생하지 않았는지 잘 점검해야 한다. 그것이 바로 'Collision Detector' 이라는 것이다.  
그러다 만약 충돌이 발생하면 랜덤한 시간을 기다린 후 다시 데이터를 전송하게 된다. 이렇게 기다렸다가 보내고 또 충돌이 발생하면 또 기다렸다가 보낸다. 이렇게 15번을 반복했는데도 충돌이 나면 통신을 포기하게 된다.  
**<u>충돌이 발생하는 것은 이더넷에의 CSMA/CD라는 특성상 자연스러운 일이지만, 너무 많은 충돌이 발생하게 되면 통신 자체가 불가능해지는 경우도 있다.</u>**  

📌**<u>내용 정리</u>**📌  
- 이더넷이란, 네트워크를 구축하는 방식 중 하나로, 우리나라에서는 대부분이 이더넷 방식을 사용한다.  
- 이더넷의 가장 큰 방식은 CSMA/CD 방식으로 통신한다는 것이다.  
- CSMA/CD는 네트워크상에서 통신을 하고 있지 않으면 자기 데이터를 보내되, 다른 PC와 동시에 데이터를 보낼 경우 충돌이 발생하는 데 이를 충돌이라고 한다. 충돌이 발생하면 일정 시간 기다렸다가 다시 데이터를 보내게 된다.


---
**🐢 현재 공부하고 있는 `후니의 쉽게 쓴 CISCO 네트워킹 - 진강훙 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊