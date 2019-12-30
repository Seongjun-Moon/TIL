### Docker swarm

- docker 에서 제공해주는 분산 코디네이터

- 3대의 서버 필요
  - swarm-manager
  - swarm-worker1
  - swarm-worker2
- 외부 컴퓨터에서 Docker swarm 으로 접속해보기

```
- swarm manager
방화벽 내리고, swarm init

- 외부 컴퓨터 3대
docker swarm join \
    --token SWMTKN-1-1qke1gwcvaq1gzzhcmcctrsc6n6tu0gosth5k1fj6wd321d64e-cvg7cnz83c52ln88k880v6jjm \
    70.12.113.172:2377

swarm manager에서 swarm init 후 나온 docker swarm jon을 통해 join

---

join 후 swarm manager에서 service create 하면서 replicas 설정 후 각 join한 외부컴퓨터에서 container ls 되는지 확인
```



