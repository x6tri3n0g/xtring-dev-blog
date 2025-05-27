+++ 
title= "5. 여러 개의 컨테이너를 연동해 실행해보자" 
date= 2025-05-12
tags= ["docker"] 
draft= false
+++

## Section 01) 워드프레스 구축
### 워드프레스  사이트 구성 및 구축
> 워드프레스는 웹 사이트를 만들기 위한 소프트웨어로, 서버에 설치해 사용한다.
> 여러 개의 컨테이너를 다루는 엽습소재로서 워드프레스 사이트를 구축해 볼 것이다.

- 컨테이너는 워드프레스 공식 이미지를 사용
- 워드프레스 이미지에는 아파치, PHP 런타임을 함께 포함하고 있음
- 워드프레스 컨테이너와 MySQL 컨테이너가 있으면 워드프레스를 사용할 수 있음

### 도커 네트워크 생성/삭제
> 워드프레스는 워드프레스 컨테이너와 MySQL 컨테이너로 구성된다.

- 워드프레스 컨테이너와 MySQL 컨테이너가 연결되어 있어야 저장된 데이터를 읽고 쓸 수 있음
- **컨테이너를 두 개 만들기만 해서 두 컨테이너가 연결되지는 않음**
	- 가상 네트워크를 만들고 이 네트워크에 두 개의 컨테이너를 소속시켜 두 컨테이너를 연결해야 함
	- 가상 네트워크를 만드는 커맨드: `docker network create 네트워크_이름`
	- 네트워크를 삭제: `docker network rm`
	- 네트워크 목록 확인: `docker network ls`

#### 도커 네트워크를 생성하는 커맨드
- 옵션이나 인자를 추가하는 경우는 거의 없음
- 네트워크를 생성한 다음 컨테이너에서 네트워크에 접속하게 설정
- `docker network create 네트워크_이름`

#### 도커 네트워크를 삭제하는 커맨드
- `docker network rm 네트워크_이름`

#### 그 외 도커 네트워크 관련 커맨드
- `connect`: 네트워크에 컨테이너를 새로 접속
- `disconnect`: 네트워크에서 컨테이너의 접속을 끊음
- `create`: 네트워크를 생성
- `inspect`: 네트워크의 상세 정보를 확인
- `ls`: 네트워크의 목록을 확인
- `prune`: 현재 아무 컨테이너도 접속하지 않은 네트워크를 모두 삭제
- `rm`: 지정한 네트워크를 삭제

### MySQL 컨테이너 실행 시에 필요한 옵션과 인자
```shell
> docker run --name 컨테이너_이름 -dit --net=네트워크_이름 -e MYSQL_ROOT_PASSWORD=MySQL_루트_패스워드 -e MYSQL_DATABASE=데이터베이스_이름 -e MYSQL_USER=MySQL_사용자이름 -e MYSQL_PASSWORD=MySQL_패스워드 mysql --character-set-server=문자_인코딩 --collection-server=정렬_순서 --default-authentication-plugin=인증_방식
```
- `--net`: 컨테이너를 연결할 도커 네트워크
	- `--net=네트워크_이름`
- `-e`: 환경변수
	- `-e MYSQL_ROOT_PASSWORD=MySQL_루트_패스워드 -e MYSQL_DATABASE=데이터베이스_이름 -e MYSQL_USER=MySQL_사용자이름 -e MYSQL_PASSWORD=MySQL_패스워드`
- 인자
	- 문자 인코딩: `--character-set-server`
		- `utf8mb4`: 문자 인코딩으로 UTF8을 사용
	- 정렬 순서: `--collection-server`
		- `utf8mb4_unicode_ci`: 정렬 순서로 UTF8을 따름
	- 인증 방식: `--default-authentication-plugin`
		- `mysql_native_password`: 인증 방식을 예전 방식(native)으로 변경

### 워드프레스 컨테이너 실행 시 필요한 옵션과 인자
```shell
> docker run --name 컨테이너_이름 -dit --net=네트워크_이름 -p 포트_설정 -e WORDPRESS_DB_HOST=데이터베이스_컨테이너_이름 -e WORDPRESS_DB_NAME=데이터베이스_이름 -e WORDPRESS_DB_USER=데이터베이스_사용자_이름 -e WORDPRESS_DB_PASSWORD=데이터베이스_패스워드 wordpress
```

## Section 02) 워드프레스 및 MySQL 컨테이너 생성과 연동
### 실습
1. 네트워크 생성
2. MySQL 컨테이너 생성
3. 워드프레스 컨테이너 생성
4. 컨테이너 및 네트워크 확인
5. 정리

#### 1. 네크워크 생성
```shell
> docker network create wordpress000net1
241053fc96e4d1980b7096281c47cb2360b50c5a7efe1b4475d87724d9bb2bc5
```

#### 2. MySQL 컨테이너 생성
```shell
> docker run --name mysql1000ex11 -dit --net=wordpress000net1 -e MYSQL_ROOT_PASSWORD=myrootpass -e MYSQL_DATABASE=wordpress000db -e MYSQL_USER=wordpress000kun -e MYSQL_PASSWORD=wkunpass mysql --character-set-server=utf8mb4 --collection-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
88a33dc89062: Pull complete 
b923213a633c: Pull complete 
50e5ea4c44e8: Pull complete 
7e11d7832ad7: Pull complete 
f3200e30bcf8: Pull complete 
0564cbd28fe8: Pull complete 
4c6cf5759eb5: Pull complete 
f9436da2cfde: Pull complete 
238130ab3d54: Pull complete 
ca1a77cbea06: Pull complete 
Digest: sha256:2247f6d47a59e5fa30a27ddc2e183a3e6b05bc045e3d12f8d429532647f61358
Status: Downloaded newer image for mysql:latest
bb0399c3c34e494679c6c93aa87e9f0791b092bafd55047db8e87eecfae3a8a4
```
#### 3. 워드프레스 컨테이너 생성
```shell
> docker run --name wordpress000ex12 -dit --net=wordpress000net1 -p 8085:80 -e WORDPRESS_DB_HOST=mysql000ex11 -e WORDPRESS_DB_NAME=wordpress000db -e WORDPRESS_DB_USER=wordpress000kun -e WORDPRESS_DB_PASSWORD=wkunpass wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
943331d8a9a9: Pull complete 
afff2173127a: Pull complete 
63c7677ee413: Pull complete 
96614c9ab0cf: Pull complete 
5090bc40a8f8: Pull complete 
90d4a4206944: Pull complete 
78fb123ed78b: Pull complete 
71538b8cf6d8: Pull complete 
a5c4e0cd05e3: Pull complete 
7fe143c9de07: Pull complete 
fb3de79ceae5: Pull complete 
dbfbbd03a020: Pull complete 
a0609ccaecd6: Pull complete 
4f4fb700ef54: Pull complete 
f1e5786874c0: Pull complete 
14d32666beeb: Pull complete 
60c0859ef803: Pull complete 
d9ef6a0ae242: Pull complete 
b770c37bfa4d: Pull complete 
2ec73213abe4: Pull complete 
7c681d5cda1d: Pull complete 
6b4d7d725ce0: Pull complete 
Digest: sha256:626ea197cb3f17747b24de6ba8f52a5a841efbb68a11167a115a6401ccb6cd3c
Status: Downloaded newer image for wordpress:latest
c15e47366947a374c4d70951b6c20dd9f63f8cb82878a8fbb3d9428662e28912
```
#### 4. 컨테이너 및 네트워크 확인
```shell
> docker ps -a
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS                      PORTS                                   NAMES
aa5e692195ed   mysql       "docker-entrypoint.s…"   14 seconds ago   Exited (1) 11 seconds ago                                           mysql1000ex11
c15e47366947   wordpress   "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes                0.0.0.0:8085->80/tcp, :::8085->80/tcp   wordpress000ex12
```
#### 5. 정리
- 컨테이너 중지
- 컨테이너 제거
- 이미지 삭제
- 네트워크 삭제