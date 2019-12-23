## Docker

http://www.pyrasis.com/docker.html

> 2013년 3월 Docker, Inc에서 출시한 오픈 소스 컨테이너 프로젝트
>
> 현재 전세계적으로 큰 인기

#### 1. 가상머신과 도커

- AWS, Google cloud, MS Azure 등이 클라우드 서비스에서 공식 지원
- 가상화 기술을 이용하여 서버를 임대해주는 서비스가 클라우드 서비스

- But, 가상 머신의 문제점 발견

  -  컴퓨터를 통째로 만들다 보니, 각종 성능 손실 발생

  - CPU에서 제공해도 느림

  - 오픈소스 가상화 소프트웨어는 OS 가상화에만 주력(배포와 관리 기능 부족)

    ![img](http://www.pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/5.png)

- 호스트와 커널을 공유하는 반가상화 기술 등장

![img](http://www.pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/4.png)



#### 리눅스 컨테이너

> 컨테이너 안에 가상 공간을 만들지만 시행 파일을 호스트에서 직접 실행

- 가상화가 아닌 `격리`

- 도커는 리눅스 컨테이너를 사용
  - 초기에서 LXC(LinuX Container)를 기반으로 구현
  - 버전 0.9부터는 LXC를 대신하는 libcontainer를 개발하여 사용
  - 실행 옵션으로 선택 가능

---



### 2.도커의 특징

- 도커는 게스트 OS를 설치하지 않음

  - 이미지에 서버 운영을 위한 프로그램과 라이브러리만 격리해서 설치
  - 이미지 용량이 크게 줄어듬
  - 호스트와 OS자원(시스템 콜)을 공유

  ![img](http://www.pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/6.png)

- 도커는 하드웨어 가상화 계층이 없음

  - 메모리 접근, 파일 시스템, 네트워크 전송 속도가 가상 머신에 비해 월등히 빠름
  - 호스트와 도커 컨테이너 사이의 성능 차이가 크지 않음(오차 범위 안)

- 이미지 생성과 배포에 특화

- 이미지 버전 관리도 제공하고, 중앙 저장소에 이미지를 올리고 받을 수 있음(push, pull)

- 다양한  API를 제공하여 원하는 만큼 자동화 가능 개발과 서버 운영에 매우 유용



`이미지`

> 이미지는 서비스 운여에 필요한 서버 프로그램, 소스 코드, 컴파일된 실행 파일을 묶은 형태

> 저장소에 올리고 받는건 이미지(push, pull)



`컨테이너`

> 컨테이너는 이미지를 실행한 상태

> 이미지로 여러 개의 컨테이너를 만들수 있음

> 운영체제로 치면 이미지는 실행파일이고 컨테이너는 프로세스



- 도커의 이미지 처리 방식

  - 유니온 처리 방식
  - 베이스 이미지에서 바뀐 부분만 이미지로 생성

  ![img](http://www.pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/10.png)

  - 컨테이너로 실행할 때는 베이스 이미지와 바뀐 부분을 합쳐서 실행
  - Docker Hub 및 개인 저장소에서 이미지를 공유할 때 바뀐 부분만 주고 받음
  - 각 이미지는 의존 관계 형성(해쉬로 구분)

  ![img](http://www.pyrasis.com/assets/images/DockerForTheReallyImpatientChapter01/11.png)

---



### 4. 서비스 운영 환경과 도커

> 지금 까지는 물리서버를 직접 운영

> 호스팅 또는 IDB 코로케이션 서비스 사용

> 서버 구입과 설치에 돈이 많이 들고 시간이 오래 걸림

> 가상화가 발전하면서 클라우드 환경으로 변화 => 가상 서버를 임대하여 사용한 만큼한 요금 지불

- Immutable Infrastructure
  	- 한 번 설정한 운영 환경은 `변경하지 않는다(Immutable)`는 개념
  	- 서비스 운영 환경 이미지를 한 번 쓰고 버림



> 포트 포워딩
>
> 외부에서 Host PC의 IP를 통해 Guest PC의 apache2 localhost IP로 접속하기 위해서는 Virtual Network Editor의 NAT Settings의 포트 포워딩을 사용 
>
> 외부에서 Guest PC로의 가는 포트를 Host PC로 나누는 것.