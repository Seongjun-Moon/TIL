### Docker

- 아파치 웹 서버 설치하고 로컬에 있는 hello.html. 파일을 컨테이너의 /var/www/html 디렉터리로 복사

#1 작업 디렉터리 생성 및 이동

```
root@server:~/docker# cd ~
root@server:~# mkdir webserver
root@server:~# cd webserver
root@server:~/webserver#

```



#2 Docekrfile 생성

```
root@server:~/webserver# gedit Dockerfile

---
FROM ubuntu

RUN apt-get update

RUN apt-get install apache2 -y  ⇐ ##1 

ADD hello.html /var/www/html/   ⇐ ##2

WORKDIR /var/www/html			⇐ ##3

RUN [ "/bin/bash", "-c", "echo hello2 >> hello2.html" ]		⇐ ##4

EXPOSE 80						⇐ ##5

CMD apachectl -DFOREGROUND		⇐ ##6

---

```

> \##1 -y : docker build 과정에서 사용자 입력이 발생하면 오류로 처리하므로 사용자 입력이 발생하지 않도록 하기 위한 옵션
>
> \##2 ADD, COPY : 호스트의 파일 또는 디렉터리를 이미지 내부로 복사 
>
> - COPY는 호스트의 로컬 파일만 복사가 가능
>
> - ADD는 호스트의 로컬 파일 뿐 아니라 외부 URL 또는 tar 파일도 복사가 가능 (tar 파일인 경우 압축을 해제해서 복사)
> - 일반적으로 COPY 사용을 권장
>
> ##3 WORKDIR : cd 명령어와 동일. 명령어를 실행 위치를 지정
>
> ##4  [ ] 형식의 인자 = JSON 배열 형식 → 쉘을 실행하지 않음을 의미
>
> ​		RUN command 형식은 /bin/sh -c command 형식으로 실행
>
> ##5 EXPOSE : 이미지에서 노출할 포트를 설정
>
> ##6 CMD : 컨테이너가 실행될 때 마다 실행할 명령어



#3 hello.html 파일을 생성

```
root@server:~/webserver# echo hello >> hello.html
root@server:~/webserver# ls hello.html
hello.html
root@server:~/webserver# cat hello.html
hello
```



#4 Dockerfile을 이용하여 이미지를 생성

```
root@server:~/webserver# docker build -t myimage .
```



#5 생성된 이미지로 컨테이너 실행

```
root@server:~/webserver# docker run -d -P --name myserver myimage
```

> -P : 호스트의 빈 포트를 컨테이너에 EXPOSE 된 포트로 매핑



#6 포트 확인

```
root@server:~/webserver# docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
5e24372e0fa4        myimage             "/bin/sh -c 'apach..."   4 minutes ago       Up 4 minutes        0.0.0.0:32768->80/tcp   myserver
root@server:~/webserver# docker port myserver
80/tcp -> 0.0.0.0:32768
```



#7 웹 서버 접속

```
Firefox에서
localhost:32768/hello.html => hello
localhost:32768/hello2.html => hello2
```

---



- 컨테이너 중지 ⇒ docker container stop *CONTAINER_ID_or_NAME*

```
root@server:~/webserver# docker container stop myserver
myserver
root@server:~/webserver# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

root@server:~/webserver# docker ps -a
```



- 컨테이너 실행 ⇒ docker container start *CONTAINER_ID_or_NAME*

```
root@server:~/webserver# docker container start myserver
myserver
root@server:~/webserver# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
5e24372e0fa4        myimage             "/bin/sh -c 'apach..."   30 minutes ago      Up 3 seconds        0.0.0.0:32769->80/tcp   myserver


```



- 컨테이너 삭제 ⇒ docker container rm *CONTAINER_ID_or_NAME*

> 컨테이너를 중지하고 삭제

```
root@server:~/webserver# docker container stop myserver
myserver
root@server:~/webserver# docker container rm myserver
myserver
root@server:~/webserver# docker container ls -a
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS                    PORTS               NAMES
844d85720786        example/echo:latest   "/bin/bash"              2 days ago          Exited (0) 2 days ago                         ccc
c3e780f8d6c9        example/echo:latest   "/bin/bash"              2 days ago          Created                                       zen_montalcini
4cc0da7d66ad        example/echo:latest   "/bin/bash"              2 days ago          Exited (0) 2 days ago                         nostalgic_wozniak
8633b8e96c8a        example/echo:latest   "/bin/bash"              2 days ago          Exited (130) 2 days ago                       lucid_clarke
d9f9914cdea4        example/echo:latest   "go run /echo/main.go"   2 days ago          Exited (1) 2 days ago                         elastic_swirles
90ddefbb7952        example/echo:latest   "go run /echo/main.go"   2 days ago          Exited (2) 2 days ago                         admiring_lichterman
3e8caf67aaba        example/echo:latest   "go run /echo/main.go"   2 days ago          Exited (2) 2 days ago                         vigorous_brown

```



- 실행중인 모든 컨테이너를 중지 ⇒ docker container stop $(docker container ls -q)

- 모든 컨테이너를 삭제 ⇒ docker container rm -f $(docker container ls -aq)

- myimage 이미지를 이용해서 mywebserver 컨테이너를 실행

```
root@server:~/webserver# docker run -d -P --name mywebserver myimage 
7fc896026d799e38d79d32e0f9b92478c54f47cdbe70202c6fa982496dd0c524
root@server:~/webserver# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
7fc896026d79        myimage             "/bin/sh -c 'apach..."   5 seconds ago       Up 5 seconds        0.0.0.0:32779->80/tcp   mywebserver
root@server:~/webserver# docker run -d -P --name mywebserver myimage  ⇐ 동일한 이름의 컨테이너가 존재하면 컨테이너 실행시 오류가 발생
docker: Error response from daemon: Conflict. The container name "/mywebserver" is already in use by container "7fc896026d799e38d79d32e0f9b92478c54f47cdbe70202c6fa982496dd0c524". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.


```

```
root@server:~/webserver# docker container stop mywebserver
mywebserver
root@server:~/webserver# docker container ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS               NAMES
7fc896026d79        myimage             "/bin/sh -c 'apach..."   2 minutes ago       Exited (137) 6 seconds ago                       mywebserver
8c4f9e337753        golang              "/bin/bash"              2 days ago          Exited (100) 2 days ago                          zealous_newton
root@server:~/webserver# docker run -d -P --name mywebserver myimage 
docker: Error response from daemon: Conflict. The container name "/mywebserver" is already in use by container "7fc896026d799e38d79d32e0f9b92478c54f47cdbe70202c6fa982496dd0c524". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.

```

```
root@server:~/webserver# docker container rm -f mywebserver ; docker run -d -P --name mywebserver myimage
mywebserver  ⇐ 이전 컨테이너를 강제적으로 삭제하는 과정에서 나온 로그
6d99d4273aa57a1e3c03beaa1d31fa2b6c56f588c98eaf4549a9153e03deb737 ⇐ 새로운 컨테이너가 실행

root@server:~/webserver# docker container ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                    PORTS                   NAMES
6d99d4273aa5        myimage             "/bin/sh -c 'apach..."   10 seconds ago      Up 9 seconds              0.0.0.0:32780->80/tcp   mywebserver
8c4f9e337753        golang              "/bin/bash"              2 days ago          Exited (100) 2 days ago                           zealous_newton

```

---

- 동일한 이름의 컨테이너를 삭제 후 실행하는 쉘 스크립트 작성

> root@server:~/webserver# gedit run.sh

```
#!/bin/bash

#1 명령어 형식 체크
if [ $# == 0 ] 
then 
	echo 명령어 형식이 잘못되었습니다.
	echo [사용법] ./run.sh container_name_or_id
	exit 1
fi 

#2 컨테이너 실행 전 컨테이너 리스트 출력
docker container ps -a

#3 동일 이름의 컨테이너를 조회
cid=$(docker container ps -a --filter="name=^/$1$" -q)

#4 동일 이름의 컨테이너가 존재하는 경우 해당 컨테이너를 삭제 후 메시지를 출력
if [ -n "$cid" ]
then
	docker container rm -f $cid
	echo $1 이름의 컨테이너\($cid\)를 삭제했습니다.
fi

#5 컨테이너를 실행
docker container run --name $1 -d -P myimage

#6 컨테이너 실행 후 컨테이너 리스트 출력
docker container ps -a

#7 쉘 종료
exit 0

```

```
root@server:~/webserver# ls -l
합계 12
-rw-r--r-- 1 root root 214 12월 26 10:33 Dockerfile
-rw-r--r-- 1 root root   6 12월 26 10:50 hello.html
-rw-r--r-- 1 root root 752 12월 26 13:35 run.sh
root@server:~/webserver# chmod 755 run.sh
root@server:~/webserver# ls -l
합계 12
-rw-r--r-- 1 root root 214 12월 26 10:33 Dockerfile
-rw-r--r-- 1 root root   6 12월 26 10:50 hello.html
-rwxr-xr-x 1 root root 752 12월 26 13:35 run.sh

root@server:~/webserver# ./run.sh mywebserver
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                    PORTS                   NAMES
6d99d4273aa5        myimage             "/bin/sh -c 'apach..."   2 hours ago         Up 2 hours                0.0.0.0:32780->80/tcp   mywebserver
8c4f9e337753        golang              "/bin/bash"              2 days ago          Exited (100) 2 days ago                           zealous_newton
6d99d4273aa5
mywebserver 이름의 컨테이너(6d99d4273aa5)를 삭제했습니다.
75549b3173a93019c1ac4e094503d3740923947089d28d7471711ca79a90b93e
CONTAINER ID        IMAGE               COMMAND                  CREATED                  STATUS                    PORTS                   NAMES
75549b3173a9        myimage             "/bin/sh -c 'apach..."   Less than a second ago   Up Less than a second     0.0.0.0:32781->80/tcp   mywebserver
8c4f9e337753        golang              "/bin/bash"              2 days ago               Exited (100) 2 days ago                           zealous_newton

```

---

> Ex. 아래와 같은 형태로 기존의 컨테이너를 삭제하고 새롭게 컨테이너를 생성하는 스크립트를 작성하시오.[사용법] ./run.sh IMAGE_NAME 
>
> 1) CONTAINER_NAMECONTAINER_NAME 일치하는 컨테이너가 존재하는지 확인
>
> 2) 존재하는 경우 해당 컨테이너를 삭제
>
> 3) IMAGE_NAME 이미지를 이용해서 CONTAINER_NAME 이름의 컨테이너를 생성

```
#!/bin/bash

#1 명령어 형식 체크
if [ $# == 0 ] 
then 
	echo 명령어 형식이 잘못되었습니다.
	echo [사용법] ./run.sh IMAGE_NAME CONTAINER_NAME
	exit 1
fi 

#2 컨테이너 실행 전 컨테이너 리스트 출력
docker container ps -a

#3 동일 이름의 컨테이너를 조회
cid=$(docker container ps -a --filter="name=^/$2$" -q)

#4 동일 이름의 컨테이너가 존재하는 경우 해당 컨테이너를 삭제 후 메시지를 출력
if [ -n "$cid" ]
then
	docker container rm -f $cid
	echo $2 이름의 컨테이너\($cid\)를 삭제했습니다.
fi

#5 컨테이너를 실행
docker container run --name $2 -d -P $1

#6 컨테이너 실행 후 컨테이너 리스트 출력
docker container ps -a

#7 쉘 종료
exit 0
```

---

- 호스트와 컨테이너 간 파일 복사 

  > docker container cp HOST_FILE_PATH CONTAINER_ID_or_NAME:CONTAINER_FILE_PATH

- Ex.mywebserver의 웹 루트 리렉터리(/var/www/html/)에 있는 index.html 파일을 가져와서 자신의 이름을 출력하도록 페이지를 변경한 후 다시 웹 서버에 적용http://localhost:mywebserverPortNumber접속했을 때 자신의 이름이 출력되는지 확인

```
root@server:~/webserver# docker container cp mywebserver:/var/www/html/index.html .
root@server:~/webserver# ls
Dockerfile  hello.html  hello3.html  index.html  run.sh  run2.sh
root@server:~/webserver# gedit index.html
root@server:~/webserver# docker container cp ./index.html mywebserver:/var/www/html/index.html
root@server:~/webserver# 

```

---

Ex. \#1 작업디렉터리(lab) 생성 
\#2 아래 작업을 수행하는 Dockerfile을 생성ubuntu 최신 버전의 이미지를 베이스 이미지로 사용apache2 설치apache2를 백그라운드에서 실행
\#3 docker build를 통해 이미지를 생성이미지 이름 : myapache이미지 태그 : latest
\#4 호스트에서 생성한 index.html 파일을 컨테이너 내부 아파치 웹 루트에 복사  <html><body><h1>Hello, 본인이름</h1></body></html>
\#5 호스트에서 웹 브라우저를 통해서  http://localhost:??????/index.html 로 접속했을 때 Hello, Docker가 출력되는 것을 확인
\#6 현재 상태의 컨테이너의 이미지를 생성이미지 이름 : myapache이미지 태그 : latest단, 기존 이미지의 태그를 1.0으로 변경
\#7 #6에서 생성한 이미지(2개)를 자신의 도커 허브에 반영
\#8 #7에서 반영한 이미지를 다른 사람의 자리에서 가져와서 실행 후 브라우저를 통해서 확인

```
root@server:~/lab# gedit Dockerfile 
-------------------------
FROM ubuntu

RUN apt-get update

RUN apt-get install apache2 -y   

ADD index.html /var/www/html/   

WORKDIR /var/www/html			

RUN [ "/bin/bash", "-c", "echo hello2 >> hello2.html" ]		

EXPOSE 80						

CMD apachectl -DFOREGROUND	
-------------------------
root@server:~/lab# docker build -t myapache:latest .
Sending build context to Docker daemon  3.072kB
Step 1/8 : FROM ubuntu
 ---> 549b9b86cb8d
Step 2/8 : RUN apt-get update
 ---> Using cache
 ---> 5eb38be2cbfe
Step 3/8 : RUN apt-get install apache2 -y
 ---> Running in 14b940c0d916
	:
	:
	:
	:
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Processing triggers for libc-bin (2.27-3ubuntu1) ...
 ---> 5e39e85d38b0
Removing intermediate container 14b940c0d916
Step 4/8 : ADD index.html /var/www/html/
 ---> cc92f46997c2
Removing intermediate container 4efcf9256afc
Step 5/8 : WORKDIR /var/www/html
 ---> 1057ffef384b
Removing intermediate container b83a801faa92
Step 6/8 : RUN /bin/bash -c echo hello2 >> hello2.html
 ---> Running in c7ce580701e8
 ---> 523bae039ee1
Removing intermediate container c7ce580701e8
Step 7/8 : EXPOSE 80
 ---> Running in 230352b622a8
 ---> 1275e98b8255
Removing intermediate container 230352b622a8
Step 8/8 : CMD apachectl -DFOREGROUND
 ---> Running in 2fa39dada9f1
 ---> 50349c879870
Removing intermediate container 2fa39dada9f1
Successfully built 50349c879870
Successfully tagged myapache:latest
root@server:~/lab# docker container start myserver
myserver
root@server:~/lab# docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
root@server:~/lab# docker container ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
e212bf755ebd        14aa6eaf4d6d        "/bin/sh -c 'apach..."   8 minutes ago       Exited (0) 8 minutes ago                        myserver2
50454ab575fe        14aa6eaf4d6d        "/bin/sh -c 'apach..."   9 minutes ago       Exited (0) 14 seconds ago                       myserver
root@server:~/lab# echo index >> index.html
root@server:~/lab# ls index.html 
index.html
root@server:~/lab# cat index.html 
<html>
<body>
	<h1>Hello, Docker</h1>
</body>
</html>

index
root@server:~/lab# docker build -t myapache2:latest .
Sending build context to Docker daemon  3.072kB
Step 1/8 : FROM ubuntu
 ---> 549b9b86cb8d
Step 2/8 : RUN apt-get update
 ---> Using cache
 ---> 5eb38be2cbfe
Step 3/8 : RUN apt-get install apache2 -y
 ---> Using cache
 ---> 5e39e85d38b0
Step 4/8 : ADD index.html /var/www/html/
 ---> b5f9548ea045
Removing intermediate container 70298a012d3e
Step 5/8 : WORKDIR /var/www/html
 ---> 884d28080f84
Removing intermediate container 64dccabfb722
Step 6/8 : RUN /bin/bash -c echo hello2 >> hello2.html
 ---> Running in bd719b9831ef
 ---> b0dd8262e2fb
Removing intermediate container bd719b9831ef
Step 7/8 : EXPOSE 80
 ---> Running in 85c59c102f0d
 ---> fa4b1dc27af4
Removing intermediate container 85c59c102f0d
Step 8/8 : CMD apachectl -DFOREGROUND
 ---> Running in 47b20ef0fa16
 ---> 5215f419d7d7
Removing intermediate container 47b20ef0fa16
Successfully built 5215f419d7d7
Successfully tagged myapache2:latest
root@server:~/lab# docker run -d -P --name myserver3 myapache2:latest
ec6075c30adbb5186c95b71c23e14a75ee1accb12e92e5025aaa0bfc816e941e
root@server:~/lab# docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
ec6075c30adb        myapache2:latest    "/bin/sh -c 'apach..."   13 seconds ago      Up 12 seconds       0.0.0.0:32789->80/tcp   myserver3
root@server:~/lab# gedit index.html 
root@server:~/lab# 
root@server:~/lab# 
root@server:~/lab# 
root@server:~/lab# 
root@server:~/lab# 
root@server:~/lab# cat index.html 
<html>
<body>
	<h1>Hello, Docker</h1>
</body>
</html>
root@server:~/lab# docker container cp /var/www/html/index.html ./lab
must specify at least one container source
root@server:~/lab# docker container cp myserver3:/var/www/html/index.html .
root@server:~/lab# ls
Dockerfile  index.html
root@server:~/lab# gedit ine
root@server:~/lab# gedit index.html 
root@server:~/lab# docker container cp ./index.html myserver3:/var/www/html/index.html
root@server:~/lab# gedit run.sh
root@server:~/lab# docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
ec6075c30adb        myapache2:latest    "/bin/sh -c 'apach..."   9 minutes ago       Up 9 minutes        0.0.0.0:32789->80/tcp   myserver3
root@server:~/lab# gedit index.html 
root@server:~/lab# gedit Dockerfile 
root@server:~/lab# docker commit -m "add hello3.html" myserver3 dlglgl32/myapache2:latest
sha256:536df94b9d7ddadb78050c67c1db55c5ee7da1de3eb2a70c9932b1ad540c15f1
root@server:~/lab# docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
ec6075c30adb        myapache2:latest    "/bin/sh -c 'apach..."   13 minutes ago      Up 13 minutes       0.0.0.0:32789->80/tcp   myserver3
root@server:~/lab# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
dlglgl32/myapache2   latest              536df94b9d7d        14 seconds ago      188MB
myapache2            latest              5215f419d7d7        14 minutes ago      188MB
myapache             latest              50349c879870        16 minutes ago      188MB
<none>               <none>              14aa6eaf4d6d        28 minutes ago      188MB
ubuntu               latest              549b9b86cb8d        7 days ago          64.2MB
root@server:~/lab# docker commit -m "add hello3.html" myserver3 dlglgl32/myapache2:1.0
sha256:fc51361e6dbbf13f9cf503c394719fa5b12c11880a8f42f924daa1de32824da0
root@server:~/lab# docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
ec6075c30adb        myapache2:latest    "/bin/sh -c 'apach..."   15 minutes ago      Up 15 minutes       0.0.0.0:32789->80/tcp   myserver3
root@server:~/lab# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED              SIZE
dlglgl32/myapache2   1.0                 fc51361e6dbb        5 seconds ago        188MB
dlglgl32/myapache2   latest              536df94b9d7d        About a minute ago   188MB
myapache2            latest              5215f419d7d7        16 minutes ago       188MB
myapache             latest              50349c879870        18 minutes ago       188MB
<none>               <none>              14aa6eaf4d6d        29 minutes ago       188MB
ubuntu               latest              549b9b86cb8d        7 days ago           64.2MB
root@server:~/lab# docker run --name myserver_1.0 -d -P myapache
755484ecf1683f30c7212b66cffcdf43f1cd0c87b8c4ca10a9fa62ec1515913f
root@server:~/lab# docker ls
docker: 'ls' is not a docker command.
See 'docker --help'
root@server:~/lab# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
755484ecf168        myapache            "/bin/sh -c 'apach..."   7 seconds ago       Up 6 seconds        0.0.0.0:32790->80/tcp   myserver_1.0
ec6075c30adb        myapache2:latest    "/bin/sh -c 'apach..."   16 minutes ago      Up 16 minutes       0.0.0.0:32789->80/tcp   myserver3
root@server:~/lab# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED              SIZE
dlglgl32/myapache2   1.0                 fc51361e6dbb        About a minute ago   188MB
dlglgl32/myapache2   latest              536df94b9d7d        3 minutes ago        188MB
myapache2            latest              5215f419d7d7        17 minutes ago       188MB
myapache             latest              50349c879870        19 minutes ago       188MB
<none>               <none>              14aa6eaf4d6d        31 minutes ago       188MB
ubuntu               latest              549b9b86cb8d        7 days ago           64.2MB
root@server:~/lab# docker login -u dlglgl32
Password: 
Login Succeeded
root@server:~/lab# docker image push dlglgl32/myapache2:1.0
The push refers to a repository [docker.io/dlglgl32/myapache2]
c5a9318911c8: Pushed 
762dac99dad4: Pushed 
9beb4da118bf: Pushed 
906cd9ec7293: Pushed 
e21e3b1959c5: Pushed 
918efb8f161b: Mounted from library/ubuntu 
27dd43ea46a8: Mounted from library/ubuntu 
9f3bfcc4a1a8: Mounted from library/ubuntu 
2dc9f76fb25b: Mounted from library/ubuntu 
1.0: digest: sha256:9940b495f3f291762048c9a43dc734174accc7c766fb4eeb0beda8bc977991d5 size: 2197

```

