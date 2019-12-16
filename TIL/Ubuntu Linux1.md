## Ubuntu Linux

### 1. 실습환경 구축

#### - 가상머신(Virtual Machine)

> Virtual Machine Tool : VMware, Virtual Box ...

- Windows를 그대로 사용하면서도 여러대의 리눅스 서버를 운영하는 효과를 낼 수 있음

- 쉽게 생각하면 '가짜 컴퓨터'

  > 가상머신은 멀티부팅(Multi Booting)과는 개념이 다르다. 멀티부팅은 하드디스크의 파티션을 분할한 후에, 한번에 하나의 운영체제만 가동시킬 수 있는 환경

  > 가상머신은 파티션을 나누지 않고, 동시에 여러 개의 운영체제를 가동


---

##### 1. Program 기법

- 절차 중심적 기법(C)

- 정보 공학 기법(DBMS)

- 객체 지향 기법(JAVA)

Garbage Collector를 쓸 때 살아있는 객체는 5~15% 정도로 분석, 설계 단계에서부터 고민해야함

- CBD 기법

- Framework 기법
- Functional 기법

> Function과 Method의 차이 : Function은 독립적인 객체, Method는 독립적이지 않은 객체



##### 2. 구조적 기법

- Main Frame => Network(Client - Server) 시대

> 개발자 편의성이 떨어지게 됨

- Web site 시대

> Protocol이 나오면서 HTTP가 각광받게 됨 => 개발자들의 편의성 상승 때문

- Web site + CGI => Web Application 시대

> HTTP는 멀티유저를 수용하려고 만든 Protocol
>
> 하지만 Client마다 request가 생기기 때문에 수용해야 하는 request가 Client 수대로 늘어나기 때문에 비효율적이게 됨

- Thread 구현

> NS의 NS API(Application Program Interface)
>
> MS의 IS API

- 이후

> 하나의 서버에서 받을 수 있는 request 한정
>
> Scale out을 하기에는 시간, 규범, 환경적 제한에 걸림
>
> Platform Independent를 개발자들이 바라게 됨
>
> Sun에서 Survlet API를 제공하며 개발자 편의성을 극대화 하고 Thread도 구현

=> 개발 편의성이 높아지는 형태

---

|          OOAD(방법론)          |
| :----------------------------: |
|              EJB               |
|           Framework            |
|     Web 기본 / Servly, JSP     |
|         JDBC / CS구조          |
| 보안 - JAVA Syntax, Semantics  |
| 보안 - Object-Oriented Concept |

---

