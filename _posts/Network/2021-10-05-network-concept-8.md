---
title: "[Network] 스패닝 트리 프로토콜 2, 카타르시스 스위치, VLAN 개념 정리"

categories:
  - Network
tags:
  - [Network, STP, Catalyst Switch, VLAN]

toc:  true
toc_sticky: true

date: 2021-10-05
last_modified_at: 2021-10-07
---

## 6. 스위치를 켜라!  

### 10. 스패닝 트리에 변화가 생기던 날  

스패닝 트리가 완성이 된 후, 루트 브리지는 2초 마다 헬로(Hello) BPDU를 Non Root Bridge로 전송하고, 이 헬로 BPDU를 받은 Non Root Bridge들은 이것을 자신의 데지그네이티드 포트를 통해 다시 전달한다.  

여기서 Non Root Bridge들은 매 2초마다 들어오는 루트 브리지의 헬로 패킷을 보면서 루트 브리지까지 가는 길이 살아있구나 라는 걸 알게 됩니다.  

이때 만약 Non Root Bridge들이 지정된 시간 동안 헬로패킷을 받지 못하면 중간 경로에 뭔가 문제가 발생했다고 생각하고, 스패닝 트리를 재편성하는 모드로 들어가게 된다.  

- Hello Time(헬로타임) : 루트 브리지가 얼마 만에 한 번씩 헬로 BPDU를 보내는지에 대한 시간이다. 즉 루트 브리지는 자신에게 연결된 브리지들에게 헬로 BPDU를 헬로타임마다 한 번식 보내게 되는데, 디폴트 헬로타임은 2초이다.  
- Max Age(맥스 에이지) : 브리지들이 루트 브리지로부터 헬로패킷을 받지 못하면 맥스 에이지 시간 동안 기다린 후 스패닝 트리 구조 변경을 시작한다. 즉 맥스 에이지란, 브리지들이 루트 브리지로부터 얼마 동안 헬로패킷을 받지 못했을 때 루트 브리지가 죽었다고 생각하고 새로운 스패닝 트리를 만들기 시작하는가에 대한 시간이다.  
- Forwarding Delay(포워딩 딜레이) : 브리지 포트가 블로킹 상태에서 포워딩 상태로 넘어갈 때까지 걸리는 시간이다. 여기서 중요한 점은 블로킹 포트에서 리스닝 상태로 넘어간 포트는 포워딩 딜레이 시간 동안 기다린 후 러닝 상태로 넘어가고, 러닝 상태에서 다시 포워딩 딜레이 시간동안 기다린 후 포워딩 상태로 넘어가기 때문에 사실 블로킹에서 포워딩으로 넘어가는 데 걸리는 시간은 포워딩 딜레이 시간의 두 배가 된다는 점이다.  

루트 브리지는 자기와 연결된 나머지 브리지들에게 헬로패킷을 매 2초마다 뿌리고, 이 패킷을 받은 브리지들은 자신의 데지그네이티드 포트로 다시 그 헬로패킷을 전달한다.  

만약 링크에 문제가 생기게 되면 끊어진 Non Root Bridge는 헬로 패킷을 받지 못하게 된다. 만약 맥스 에이지인 20초 동안 헬로 패킷을 받지 못하게 되면 스패닝 트리의 변경을 시작하게 된다.  

그러면 다른 스위치에서는 계속 헬로 패킷을 받고 있기 때문에 해당 헬로 패킷을 끊긴 쪽의 블로킹 포트로 전달하게 된다.  

그러면 끊긴 스위치에서는 블로킹 포트를 루트 포트로 변경하고 블로킹에서 리스닝을 거치고 포워딩 상태로 가게 된다.  

정리하자면 다음과 같다.  

- 맨 처음 루트 브리지로부터 헬로패킷을 2초마다 받던 스위치 C가 갑자기 헬로패킷이 들어오지 않게 된다.  
- 참을성 많은 스위치 C는 자신의 맥스 에이지 시간인 20초 동안 루트 브리지로부터의 헬로패킷을 기다려보지만, 20초가 지나도 헬로패킷은 E0 포트를 통해 들어오지 않는다.  
- 이렇게 되자 스위치 C는 B에서 전달해 준 헬로패킷을 자신의 E1 포트로 받아들여 E1 포트를 루트 포트로 세팅하게 된다.  
- 물론 Non Designated 포트로 블로킹 상태에 있던 스위치 C의 E1 포트를 루트 포트로 선정했다고 해서 바로 포워딩 상태로 넘어가진 않는다. 디폴트 포워딩 딜레이 시간인 15초를 먼저 리스닝 상태에서 기다리고, 다시 한 번 러닝 상태에서 15초를 추가로 기다린 후 드디어 데이터 전송이 가능한 포워딩 상태로 넘어가게 된다. 이떼 기존의 루트 포트로 포워딩 상태였던 스위치 C의 E0 포트는 블로킹이 된다.  

스패닝 프로토콜을 이용해 다른 경로를 살리는데 걸리는 시간은 대략 50초 정도 소요되는데 시간이 오래 걸려 개선할 많은 기법이 소개되고 있다.  

예를 들어, RSTP(Rapid Spanning Tree Protocol), Port Fast, Up-link Fast, Backbone Fast 등이 있다.  

### 11. 카타리스트 스위치 바라보기  

### 12. 카타르시스 스위치 구성하기

### 13. 맥 어드레스는 어디에 저장되어 있을까요?  

맥 어드레스는 스위치나 브리지가 출발지에서 들어오는 맥 어드레스를 보고 그 것을 자신의 맥 어드레스 테이블에 저장한 후 그 주소 테이블에 있는 맥 어드레스를 찾으면 그 쪽 포르로만 보내고 나머지 포트는 막아줌으로써 스위치의 기본 기능 중 하나의 콜리전 도메인을 막는 역할을 한다.  

따라서 맥 어드레스를 배우고 저장하는 것은 어떻게 보면 스위치의 가장 큰 기능 중의 하나라고 볼 수 있다.  

맥 어드레스를 저장하는 방식은 크게 2가지가 있다.  

- 첫 번째 : Dynamic 방식  
- 두 번째 : Permanent 방식  

Dynamic 방식은 자동으로 배우는 방식이다. 맥 어드레스를 하나를 배우고 나면, 그것을 맥 어드레스 테이블에 저장하고 사용한다. 이 때 이 주소를 사용한 지 300초(디폴트)가 지나도록 다시 사용하지 않으면 이 주소는 맥 테이블에서 지워진다.  

맥 테이블은 그 용량에 한계각 있기 때문이다.  

Permanent 방식은 절대 지워지지 않도록 맥 어드레스를 저장한다. 이 경우는 사람이 수동으로 맥 어드레스를 넣어준다.  

스태틱 맥 어드레스란 항상 맥 어드레스 테이블에 저장되어 지워지는 일이 없는 주소이다. 예를 들어 서버나 고정 장비 등은 이렇게 스태틱으로 지정해 주면 시간이 지나도 지워지지 않을 뿐만 아니라 다시 이 맥 어드레스를 알기 위해 Learning 과정을 거칠 필요가 없다는 장점이 있다.  

하지만, 주소가 바뀐다고 해도 자동으로 수정이 불가능하고 맥 어드레스 테이블을 계속 사용하기 때문에 메모리가 낭비된다는 단점도 있다.  

### 14. 가상의 랜(Virtual Lan)이란?  

스위치의 기능 중에서 요즘 가장 많이 사용된다.  

예전에는 스위치가 단순히 콜리전 영역을 나눠주는 정도의 역할을 하면 충분했지만 콜리전 도메인을 나눠주는 스위치의 기능에 별로 만족하지 못하게 되었다.  

브로드캐스트의 영향이 점차 커지면서 라우터에 의한 네트워크 영역의 분류는 필수가 되었고 이런 네트워크 영역의 구분은 스위치의 능력을 뛰어넘는 기능이였다.  

그래서 하나의 스위치에 연결된 모든 장비들이 모두 같은 브로드캐스트 도메인 안에 있게 되었다.  

이러한 브로드캐스트 도메인을 나누려면 중간에 라우터를 두고 양쪽으로 스위치를 라우터에 연결해야만 한다. 그런데 가상 랜, 즉 VLAN을 사용하면 한 대의 스위치를 마치 여러 대의 분리된 스위치처럼 사용하고, 또 여러 개의 네트워크 정보를 하나의 포트를 통해 전송할 수 있다.  

예를 들어, 만약 어느 회사에서 본관과 별관, 이렇게 2개의 건물이 있고, 회사 전체는 3개의 네트워크로 나누어져 있다. 따라서 본관에 있는 라우터에서는 3개의 이더넷 인터페이스가 나와야 하고, (라우터는 원래 네트워크 개수만큼 인터페이스가 필요하다.) 이 3개의 인터페이스는 3개의 서로 다른 스위치 (스위치 A, B, C)에 연결되어야 한다.  

만약 이 회사의 별관쪽에도 똑같이 3개의 네트워크가 필요하다면 다시 스위치를 설치하고 맨 처음 스위치에서 같은 네트워크에 속한 스위치끼리 연결해줘야 한다.  

하지만 VLAN이 지원되는 라우터와 스위치를 사용하면 라우터는 스위치로 하나의 링크만을 이용해서도 3개의 네트워크 정보를 같이 실어보낼 수 있다.  

즉, 한 선에 여러 개의 네트워크 정보를 보내는 것이 가능해진다. 또 스위치도 여러 개의 브로드캐스트 영역을 나누어줄 수 있게 된다.  

다시 예를 들어, 어느 회사에는 인사팀과 생산팀이 있는데 사용자에 따라 인사팀 사람들은 A 네트워크에, 생산팀 사람들은 B 네트워크에 속하게 되어 있다.  

![image](https://user-images.githubusercontent.com/37467408/136273933-4bb21f4f-7c12-4124-a199-0394a20a39d4.png){: width="500" height="500"}  

그런데 인사팀에 있던 김대리가 책상을 생산팀으로 옮기게 되었다. 책상만 옮기고 인사팀 업무를 계속 수행한다.  

그렇다면, 김대리의 PC는 이제 생산팀이 사용하고 있는 스위치 중에서 한 포트를 쓰게 되는데, 이렇게 되면 인사팀 네트워크에 속한 김대리가 생산팀 스위치를 사용해도 될까?  

여기서 만약 김대리의 네트워크 IP 주소를 생산팀에 속하도록 하고 생산팀 스위치에 연결하게 되면 가능은 하지만 인사팀 업무를 보기 때문에 여러가지 측면에서 인사팀 네트워크에 속해 있는 것이 편하다.  

왜냐하면 네트워크를 나누어서 보안을 적용하기 때문에 생산팀에 속하면 아무래도 관리가 힘들다. 즉, 보안 등의 문제가 생기는 것이다.  

그럼 만약, VLAN이 없을 경우 김대리를 인사팀 네트워크에 연결하는 방법을 생각해보자.  

일단 김대리를 위한 스위치가 한 대 필요하게 된다. 인사팀의 스위치에서 포트를 하나 뽑자니 거리가 너무 멀기 때문이다.  

그런데 여기서 기존 스위치가 VLAN이 지원 된다면 어떻게 될까?  

먼저 라우터와 인사팀 스위치 사이를 트렁크로 연결한다. 즉 하나의 포트를 사용해서 여러 개의 네트워크 정보를 전달하는 것이다. 따라서 라우터는 이전과 달리 인사팀 스위치에만 트렁크로 연결된다. 하지만 네트워크 정보는 A, B가 같이 흐르게 된다.  

그다음은 인사팀 스위치와 생산팀 스위치를 트렁크로 연결한다. 이렇게 하면 인사팀 스위치 한 포트와 생산팀 스위치 한 포트를 연결하고, 이 링크를 이용해서 A 네트워크 정보와 B 네트워크 정보를 동시에 전송할 수 있다.  

이렇게 트렁크 구성을 마치고 나서 생산팀 스위치의 포트를 VLAN으로 세팅하는데, 한 포트는 인사팀용으로 써야 하니 A 네트워로 세팅하고, 나머지 포트는 B 네트워크로 세팅한다.  

모든 세팅을 마치면 인사팀의 김대리는 따로 스위치를 사용하지 않고도 A 네트워크를 이용할 수 있게 된다.  

![image](https://user-images.githubusercontent.com/37467408/136274032-d111ffa6-ca41-4b49-add3-ef3f7baa6d14.png){: width="500" height="500"}  

### 15. VLAN에서 꼭 기억해야 할 몇 가지  

먼저 VLAN은 스위치에서 지원하는 기능이다. 허브나 브리지에서는 지원하지 않는 기능이다. 따라서 VLAN을 지원한다면 이 장비는 스위치이다.  

다음으로 VLAN은 한 대의 스위치를 여러 개의 네트워크로 나누기 위해서 사용한다. 따라서 스위치가 일단 VLAN으로 나누어지면 나누어진 VLAN 간의 통신은 오직 라우터를 통해서만 통신이 가능하다.  

여기서 '네트워크를 나눈다'는 의미는 브로드캐스트 도메인을 나눈다는 의미이다.  

하나의 포트를 통해 서로 다른 여러 개의 VLAN을 전송할 수 있게 하는 포트를 '트렁크 포트(Trunk Port)'라고 한다.  

VLAN 간의 통신은 오직 라우터를 통해서만 가능하다.  

트렁크 포트가 가능하기 때문에 VLAN은 여러 대의 스위치에 구성이 가능하다. 즉 같은 VLAN끼리는 스위치를 건너서도 통신이 가능하다. (VLAN1은 VLAN1 끼리, VLAN2는 VLAN2 끼리, VLAN3는 VLAN3 끼리만 스위치를 건너서 통신이 가능하다.)  

트렁크에서 패킷이 전송될 때는 패킷에 VLAN 정보도 같이 전송된다. 그 때문에 어느 VLAN에 속한 패킷인지를 구분할 수 있다. 이처럼 트렁크에서 패킷에 VLAN 정보를 넣는 몇 가지 방법이 있다.  

### 16. VLAN에서의 트렁킹과 VTP(VLAN Trunking Protocol)  

트렁킹은 여러 개의 VLAN들을 함께 실어나르는 것을 말한다. 즉 각 스위치에 여러 개의 VLAN이 있기 때문에 원래는 각 VLAN별로 링크를 만들어주어야 하지만, 그렇게 되면 너무 많은 링크가 필요하기 때문에 마치 셔틀버스처럼 모든 VLAN이 하나의 링크를 통해 다른 스위치나 라우터로 이동하기 위해 트렁킹이란 것을 만든 것이다.  

버스를 내릴 때 자기 집을 제대로 찾아가려면 각 VLAN별로 별도의 이름표가 필요하다. 그래서 각 VLAN들은 트렁킹이라는 셔틀버스에 자기들의 패킷을 태울 때 각각 이름표를 붙여준다.  

이 이름표를 어떻게 붙여주냐에 따라 트렁킹도 2가지 방식이 있다. 바로 ISL 트렁킹과 IEEE 802.1Q 방식의 트렁킹이다.  

ISL 트렁킹은 시스코에서 만든 트렁킹 프로토콜로, 시스코 장비끼리만 사용하는 방식인 반면, IEEE 802.1Q 방식은 트렁킹에 대한 표준 프로토콜이다.  

802.1Q에서는 네이티브 VLAN(Native VLAN)이라는 한 가지 특색있는 VLAN이 나온다.  

네이티브 VLAN은 무엇일까? 트렁킹은 셔틀버스에 올라타난 모든 VLAN 패킷에 각각의 VLAN 정보를 써서 이름표로 붙여준다. 그런데 IEEE 802.IQ에서는 이름표를 붙이지 않는 VLAN이 하나 있다.  

그럼 어떻게 이 패킷의 VLAN을 찾을까? 딱 하나의 VLAN만 이름표를 달지 않았으니 이름표를 달지 않은 패킷은 모두 그쪽 VLAN이다. 바로 이 VLAN을 '네이티브 VLAN'이라고 한다.  

즉 네이티브 VLAN은 패킷에 VLAN 정보를 붙이지 않고 보내는 VLAN('Untagged 트래픽'이라고도 말한다)으로, 모든 스위치 네트워크에서 유일하게 한 개의 VLAN만을 네이티브 VLAN으로 세팅할 수 있다.  

그래야 나중에 이름표 없는 패킷이 어느 VLAN인지 알 수 있기 때문이다.  

ISL(Inter-Switch Link) 방식은 시스토만의 트렁킹 프로토콜로 스위치와 스위치 간의 링크, 스위치와 라우터 간의 링크에서 여러 개의 VLAN 정보를 함께 전달하는 방식이다.  

IEEE 802.1Q와 큰 차이는 없지만 ISL은 네이티브 VLAN이라는 개념이 없이 모든 VLAN에 이름을 붙인다. VLAN 정보는 ISL 트렁크를 따라 전송한다.  

물론 같은 링크를 통해서 전송이 이루어지지만 이 패킷들은 서로 다른 VLAN에 속해 있기 때문에 통신은 라우터를 통해서만 가능하다.  

쉽게 **<u>트렁킹이란, 여러 개의 VLAN을 한 번에 전송하는 방식이다.</u>**  

VTP(VLAN Trunking Protocol)는 무엇일까? VTP는 스위치들 간에 VLAN 정보를 서로 주고받아 스위치들이 가지고 있는 VLAN 정보를 항상 일치시켜 주기 위한 프로토콜이다.  

VTP도 시스코만의 프로토콜이다.  

만약, VTP 기능이 지원되지 않는 스위치라고 가정하면 이 스위치들에게 새로운 VLAN 하나를 추가할 필요가 생기면 스위치 A부터 스위치 E까지 5대의 스위치에 모두 VLAN 구성을 변경해 주어여 한다. 또 VLAN 하나를 지우려면 역시 각 스위치의 구성에 들어가서 VLAN을 지워줘야 한다는 문제가 있다.  

그럼 VTP 기능이 있다면 어떻게 될까?  

VTP가 Enable되도록 구성하면, VTP 서버에서 한번만 VLAN 정보를 설정해도 VTP 서버는 다른 스위치와의 트렁크 링크를 통해서 VLAN 정보를 자동으로 업데이트한다. 따라서 나머지 모든 스위치에 일일이 VLAN 정보를 업데이트할 필요가 없는 장점이 있다.  

### 17. VLAN의 구성  

### 18. 실제상황! VLAN  

---
**🐢 현재 공부하고 있는 `후니의 쉽게 쓴 CISCO 네트워킹 - 진강훙 저자` 의 책을 학습하며 기록 및 정리를 하기위한 내용들입니다. 🐢**
{: .notice--primary}

감사합니다.😊