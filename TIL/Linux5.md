### Linux 

QUIZ1. 다음 명령어의 실행 결과가 나머지와 다른 것은?

root@server:/bin# ls

root@server:/bin# ls .

root@server:/bin# ls ./

root@server:/bin# ls /

root@server:/bin# ls /bin

root@server:/bin# ls /bin/*

root@server:/bin# ls /bin/

---

QUIZ2. 특정 디렉터리에서 root 사용자 홈 디렉터리로 이동 방법

root@swarm-manager:~/dira# cd 

root@swarm-manager:~/dira# cd ~

root@swarm-manager:~/dira# cd ..

root@swarm-manager:~/dira# cd /

rootroot@swarm-manager:~/dira# cd $HOME

---

QUIZ3. 다음 명령어의 실행 결과는?

root@server:/tmp# touch aaa

root@server:/tmp# touch bbb

root@server:/tmp# touch ccc

root@server:/tmp# mkdir ddd

root@server:/tmp# ls 

aaa 

bbb

ccc 

ddd ← 디렉터리

root@server:/tmp# mv aaa bbb ccc ddd

​           					---------------------------- 3개 파일을 ddd 디렉터리로 옮김

---

QUIZ4. >와 >> 차이점

```
> 는 덮어쓰기
>> 는 추가하기
```

---

QUIZ5. cat all 명령어의 실행 결과가 아래와 같이 나오도록 >를 이용해서 all 파일을 생성해 보세요.

nanjini@linux:~$ date > aaa

nanjini@linux:~$ cat aaa

2019-12-29 (일) 20:58:38 KST

nanjini@linux:~$ date > bbb

nanjini@linux:~$ cat bbb

2019-12-29 (일) 20:58:49 KST

nanjini@linux:~$ cat all

2019-12-29 (일) 20:58:38 KST

2019-12-29 (일) 20:58:49 KST
root@swarm-manager:~# cat aaa bbb > all

root@swarm-manager:~# cat all

2019-12-31 (화) 09:38:27 KST

2019-12-31 (화) 09:38:34 KST

---

#### RAID

> RAID는 여러 개의 하드디스크를 하나의 하드디스크처럼 사용하는 방식.

> 비용을 절감하면서도 더 신뢰성을 높이며, 성능까지 향상시킬 수 있다.

> RAID는 기본적으로 구성하는 방식에 따라 Linear RAID, RAID 0, RAID1, RAID 2, RAID3, RAID4, RAID5까지 7가지로 분류 할 수 있다.

![raid 종류에 대한 이미지 검색결과](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F274AB8385961D68428)

- Linear RAID와 RAID 0

![linear raid에 대한 이미지 검색결과](https://2.bp.blogspot.com/-9inhAmYQ10M/Wq5T3efswII/AAAAAAAABnY/QOg_JZq-N3MQTo-hKxbFYewGnNoNef0_gCLcBGAs/s1600/RAID%2BLinear.png)

> 최소 2개의 HDD가 필요
>
> 2개 이상의 HDD를 1개의 볼륨으로 사용
>
> 앞 디스크부터 차례로 저장
>
> 100% 공간 효율성 = 비용 저렴



- RAID0 = Stripping\

![raid0에 대한 이미지 검색결과](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/RAID_0.svg/150px-RAID_0.svg.png)

> 최소 2개의 HDD 필요
>
> 모든 디스크에서 동시에 저장
>
> 100% 공간 효율성 = 비용 저렴
>
> 낮은 신뢰성 → 빠른 성능을 요구하되, 손실되어도 무관한 데이터 저장에 적합



- RAID1 = Mirroring

![raid1에 대한 이미지 검색결과](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/RAID_1.svg/150px-RAID_1.svg.png)

> 결함 허용(fault-torerance)을 제공 = 신뢰성↑
>
> 데이터 저장에 두배의 용량 필요 = 공간 효율성↓ = 비용↑
>
> 저장 속도(성능)은 변함 없음
>
> 중요 데이터 저장에 적합



-  RAID5 = RAID0 + 1 Parity

![raid5에 대한 이미지 검색결과](https://i.stack.imgur.com/jBdAm.png)

> RAID0의 공간 효율성 + RAID1의 데이터 안전성
>
> 최소 3개 이상의 HDD가 필요
>
> 전체 HDD 개수 -1 만큼의 공간을 사용
>
> 요류가 발생했을 때 Parity를 이용해서 데이터를 복구
>
> HDD 2개가 동시에 문제가 생기면 복구가 불가능

![img](http://cfile209.uf.daum.net/image/200659324CE3622B4AD6DB)



- RAID6

![raid6에 대한 이미지 검색결과](https://upload.wikimedia.org/wikipedia/commons/7/70/RAID_6.svg)

> RAID 5 방식의 개선으로, RAID 5는 1개의 Parity를 사용하지만 RAID6는 2개의 Parity를 사용
>
> 최소 4개 이상의 HDD가 필요
>
> RAID5 보다 공간 효율성과 성능(속도)가 떨어짐
>
> 2개의 하드디스크가 동시에 고장 나도 데이터에는 이상이 없도록 함



- RAID 1+0

![raid1+0에 대한 이미지 검색결과](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/RAID_10_01.svg/220px-RAID_10_01.svg.png)

> 신뢰성(안정성)과 성능(속도)를 동시에 확보하는 방법

