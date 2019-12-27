### Docker

- 컨테이너 내부에서 명령어를 실행 -> docker exec

> docker exec CONTAINER_NAME 명령어



- 호스트 볼륨 공유

> -v 옵션을 이용해서 호스트의 볼륨을 공유
>
> 호스트의 디렉터리(또는 파일)을 컨테이너의 디렉터리(또는 파일)로 마운트



#1 MySQL 이미지를 이용해서 데이터베이스 컨테이너를 생성

```
root@server:~/docker# docker run -d \
> --name wordpressdb_hostvolume \
> -e MYSQL_ROOT_PASSWORD=password \
> -e MYSQL_DATABASE=wordpress \
> -v /home/wordpress_db:/var/lib/mysql \   
> mysql:5.7


-v /home/wordpress_db:/var/lib/mysql ⇒ 호스트의 /home/wordpress_db 디렉터리를 
   									   컨테이너의 /var/lib/mysql 디렉터리로 공유
   HOST               CONTAINER 
/var/lib/mysql ⇒ mysql db의 데이터를 저장하는 기본 디렉터리
```



#2 워드프레스 이미지를 이용해서 웹 서버 컨테이너를 생성

```
root@server:~/docker# docker run -d --name wordpress_hostvolume \
> -e WORDPRESS_DB_PASSWORD=password \
> --link wordpressdb_hostvolume:mysql \
> -p 80 \
> wordpress

```



#3 호스트 볼륨 공유를 확인

```
root@server:~/docker# ls /home/wordpress_db/
auto.cnf         client-key.pem  ibdata1             private_key.pem  sys
ca-key.pem       ib_buffer_pool  ibtmp1              public_key.pem   wordpress
ca.pem           ib_logfile0     mysql               server-cert.pem
client-cert.pem  ib_logfile1     performance_schema  server-key.pem
```

```
root@server:~/docker# docker exec wordpressdb_hostvolume ls /var/lib/mysql
auto.cnf
ca-key.pem
ca.pem
client-cert.pem
client-key.pem
ib_buffer_pool
ib_logfile0
ib_logfile1
ibdata1
ibtmp1
mysql
performance_schema
private_key.pem
public_key.pem
server-cert.pem
server-key.pem
sys
wordpress

```



#4컨테이너 삭제 후 볼륨 데이터가 유지되는지 확인

```
root@server:~/docker# docker container stop wordpress_hostvolume wordpressdb_hostvolume
wordpress_hostvolume
wordpressdb_hostvolume
root@server:~/docker# docker container rm wordpress_hostvolume wordpressdb_hostvolume
wordpress_hostvolume
wordpressdb_hostvolume

root@server:~/docker# ls /home/wordpress_db
auto.cnf         client-key.pem  ibdata1             public_key.pem   wordpress
ca-key.pem       ib_buffer_pool  mysql               server-cert.pem
ca.pem           ib_logfile0     performance_schema  server-key.pem
client-cert.pem  ib_logfile1     private_key.pem     sys

그대로 남아있다.
```



#5 파일 단위의 공유도 가능하고, -v 옵션을 여러개 사용하는 것도 가능

```
root@server:~/docker# echo hello1 >> hello1.txt && echo hello2 >> hello2.txt
root@server:~/docker# cat hello1.txt && cat hello2.txt
hello1
hello2

root@server:~/docker# docker run -it \
> --name volume_test2 \
> -v /root/docker/hello1.txt:/hello1.txt \ 앞에 경로 안잡아 주면 hello1&2.text가 디렉터리로 생성됨.
> -v /root/docker/hello2.txt:/hello2.txt \
> ubuntu:14.04


root@a272500867eb:/# ls
bin   dev  hello1.txt  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  hello2.txt  lib   media  opt  root  sbin  sys  usr
```

호스트에서 hello1.txt 내용을 변경 후 호스트와 컨테이너에서 확인

```
root@server:~/docker# echo HELLO CONTANIER >> hello1.txt
root@server:~/docker# cat hello1.txt 
hello1
HELLO CONTANIER
root@server:~/docker# 

root@6422868ffb86:/# cat ./hello1.txt                    
hello1
HELLO CONTAINER

```



컨테이너에서 hello2.txt 내용을 변경 후 호스트와 컨테이너에서 확인

```
root@6422868ffb86:/# echo HELLO HOST >> hello2.txt
root@6422868ffb86:/# cat ./hello2.txt 
hello2
HELLO HOST

root@server:~/docker# cat hello2.txt 
hello2
HELLO HOST
```



#6 컨테이너에 존재하는 디렉터리를 호스트 볼륨으로 공유하는 경우

```
root@server:~/docker# docker run -it --name dummy alicek106/volume_test

root@fb9c61b0e1c3:/# ls -l /home/testdir_2
total 4
-rw-r--r-- 1 root root 11 Sep  8  2016 test
```



- 도커 볼륨

  > 도커 자체에서 제공하는 볼륨 기능을 활용한 데이터 보존

  > docker volume 명령어를 사용



#1 볼륨 생성

```
root@server:~/docker# docker volume create --name myvolume

root@server:~/docker# docker volume ls
DRIVER              VOLUME NAME
local               12daea4641d75c792b7a5cdec95b13274a861726c0d14532aa58776e0fdef308
local               81fcc1bff371f6495ec3f5ca5c7a890444e4d1bd56b7b57f80c6b6cd5dd1b932
local               d5299679f988ce568e2367c2c172d378db24c11de8620845386413bf70e71146
local               hello1.txt
local               hello2.txt
local               myvolume

```



#2 생성한 볼륨을 사용하는 컨테이너를 생성

​	-v [볼륨이름]:[컨테이너 디렉터리]

```
root@server:~/docker# docker run -it --name myvolume_1 \
> -v myvolume:/root/ \
> ubuntu:14.04

root@0fe4a1dff4c5:~# echo Hello, Volume >> /root/hello
root@0fe4a1dff4c5:~# exit
```



#3 동일 볼륨을 사용하는 컨테이너를 생성해서 파일 공유가 되는지 확인

```
root@server:~/docker# docker run -it --name myvolume_2 -v myvolume:/temp/ ubuntu:14.0

root@ff84240a18d6:/# ls /temp
hello
root@ff84240a18d6:/# cat /temp/hello
Hello, Volume

```



#4 docker inspect 명령어를 이용해서 볼륨 정보를 조회

```
root@server:~/docker# docker inspect --type volume myvolume
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/myvolume/_data",
        "Name": "myvolume",
        "Options": {},
        "Scope": "local"
    }
]

```



- 도커 컴포즈(docker compose)

