+++ 
title= "4. 컨테이너를 실행해 보자" 
date= 2025-05-06
tags= ["docker"] 
draft= false 
+++

## Section 1)  도커 엔진 시작/종료하기

### 도커 엔진을 시작/종료하는 방법
> 도커 엔진은 설치와 함께 실행되며, 계속 동작 상태로 남아 있지만 **컨테이너를 실행 중이 아니라면 컴퓨터의 리소스를 거의 차지하지 않으므로 문제가 없다.**

- 도커 엔진이 종료되면 모든 컨테이너는 종료됨
	- 도커 엔진은 자동 실행 설정 가능
	- 컨테이너는 자동 실행 설정 없음
		- 예를 들어 정전으로 인해 서버 컴퓨터가 꺼지게 되면 컴퓨터를 다시 실행하고 컨테이너를 실행하는 스크립트를 통해 재실행해야함

## Section2) 컨테이너의 기본적인 사용 방법
### 컨테이너 사용의 기본은 도커 명령d어
- 터미널에서 다음 명령어를 입력을 통해 컨테이너를 다루는 명령을 할 수 있음
```shell
> docker ~
```
#### 명령어와 대상
- `docker` 명령어 뒤에 오는 키워들은 '커맨드'라고 함
	- '커맨드'는 상위 커맨드와 하위 커맨드로 구성
	- 상위 커맨드는 '무엇을'을 의미
	- 하위 커맨드는 '어떻게'를 의미
- `docker <무엇을> <어떻게> <대상>` 식으로 구성
	- `docker <command(상/하위 커맨드)> <target>`
		- command는 12종류가 있음
- 예를 들어 penguin인 이미지를 실행
```shell
> docker image pull penguin
> docker container start penguin
> docker container run penguin
```
#### 옵션과 인자
- 위에서 설명한 '커맨드'와 '대상' 외에도 **'옵션'과 '인자'라는 추가 정보**가 있음
- 예를 들어 `-d` 옵션(백그라운드 실행, 옵션)과 `--mode==1`(모드 1로 실행, 인자)를 사용하면 아래와 같음
```shell
> docker container run -d penguin --mode=1
```
- 자주 사용하는 옵션과 인자는 기억해두면 좋음
### 기본적인 명령어 - 정리
- `docker <커맨드> (<옵션>) <대상> (<인자>)`
#### 커맨드(상/하위 커맨드)
- '컨테이너'를 '실행'하고 싶다면 `container run` 커맨드를 사용
	- 다만 역사적 이유로 start나 run처럼 **'container'를 붙이지 않아도 실행 가능한 명령어가 있으며, 관례상 이렇게 많이 사용함**
		- `docker container run` --> `docker run`
#### 옵션
- 옵션은 커맨드에 세세한 설정을 지정하는 용도
- 커맨드의 실행 방법 또는 커맨드에 전달할 값을 지정
- 백그라운드 실행 : `-d`
- 키보드를 통한 조작 실행 : `-i` 또는 `-t`
- 커맨드에 어떤 값을 전달하고 싶다면 `--name` 같은 옵션 뒤에 옵션의 값을 지정
```shell
> ~ --name penguin
```
- `-d`와 같이 - 하나만 사용하는 옵션은 한꺼번에 모아서 사용 가능
	- `-dit`
#### 대상
- 커맨드와 달리 구체적인 이름 지정
```shell
> docker container start <옵션> penguin
```
#### 인자
- 대상에 전달할 값을 지정함
- 문자 코드 또는 포트 번호 등을 전달할 수 있음
```
--mode=1
--style nankyoku
```
##### Column: 상위 커맨드는 생략 가능하다?
- 도커 1.13 부터 커맨드가 재편되면서 상위 커맨드와 하위 커맨드의 조합 형태로 일원화됨
	- 일부 커맨드는 커맨드 자체가 변경됨
	- `run` --> (TOBE)`contianer run` 

### 간단한 명령어 사용해보기
- 도커 버전 확인하기
```shell
> docker version
Client:
 Cloud integration: 1.0.17
 Version:           20.10.8
 API version:       1.41
 Go version:        go1.16.6
 Git commit:        3967b7d
 Built:             Fri Jul 30 19:55:20 2021
 OS/Arch:           darwin/arm64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.8
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.6
  Git commit:       75249d8
  Built:            Fri Jul 30 19:53:34 2021
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
#### 대표적인 명령어
```
docker 명령어 
	 ㄴ 상위 명령어 
		 ㄴ하위 커맨드 
			 ㄴ 옵션
			 
---

docker 
	ㄴ container
		ㄴ start : 컨테이너 실행, 생략 O
			ㄴ -a
			ㄴ -i
		ㄴ stop : 컨테이너 정지, 생략 O
			ㄴ -t
		ㄴ create : 도커 이미지로부터 컨테이너 생성
			ㄴ -a
			ㄴ -e
			ㄴ -i
			ㄴ -p
			ㄴ -v
			ㄴ ...
		ㄴ run : 도커 이미지를 내려받고 컨테이너를 생성해 실행(다운로드는 필요한 경우에만).
		        docker image pull, docker container create, docker container start라는 세 개의 명령어를 하나로 합친 것과 같음.
			ㄴ -d
		ㄴ exec : 실행 중인 컨테이너 속에서 프로그램을 실행, 생략 O
		ㄴ rm : 정지 상태의 컨테이너를 삭제, 생략 O
		ㄴ ls : 컨테이너 목록을 축력, docker ps로도 가능
		ㄴ cp : 도커 컨테이너와 도커 호스트 간에 파일을 복사, 생략 O
		ㄴ commit : 도커 컨테이너를 이미지로 변환, 생략 O
		ㄴ ...
	ㄴ image
		ㄴ pull : 도커 허브 등의 리포지토리에서 이미지를 내려받음, 생략 O
		ㄴ rm : 도커 이미지를 삭제, docker rmi로 가능
		ㄴ ls : 내려 받은 이미지의 목록을 출력, 생략 X
		ㄴ build : 도커 이미지를 생성, 생략 O
		ㄴ search
		ㄴ ...
	ㄴ volume
		ㄴ create : 볼륨을 생성
		ㄴ inspect : 볼륨의 상세 정보를 출력
		ㄴ ls : 볼륨의 목록을 출력
		ㄴ prune : 현재 마운트되지 않은 볼륨을 모두 삭제
		ㄴ rm : 지정한 볼륨을 삭제
		ㄴ ...
	ㄴ network
		ㄴ connect : 컨테이너를 도커 네트워크에 연결
		ㄴ disconnect : 컨테이너의 도커 네트워크 연결을 해제
		ㄴ create : 도커 네트워크를 생성
		ㄴ inspect : 도커 네트워크의 상세 정보를 출력
		ㄴ ls : 도커 네트워크의 목록을 출력
		ㄴ prune : 현재 컨테이너가 접속하지 않은 네트워크를 모두 삭제
		ㄴ rm : 지정한 네트워크를 삭제
		ㄴ ...
	ㄴ checkpoint : 현재 상태를 일시적으로 저장한 후, 나중에 해당 지점의 상태로 되돌릴 수 있음. 현재 실험적 기능
	ㄴ node : 도커 스웜의 노드를 관리하는 기능
	ㄴ plugin : 플러그인을 관리하느 기능
	ㄴ secret : 도커 스웜의 비밀값 정보를 관리하는 기능
	ㄴ service : 도커 스웜의 서비스를 관리하는 기능
	ㄴ stack : 도커 스웜 또는 쿠버네티스에서 여러 개의 서비스를 합쳐 구성한 스택을 관리하는 기능
	ㄴ swarm : 도커 스웜을 관리하느 기능
	ㄴ system : 도커 엔진의 정보를 확인하는 기능
```
#### 컨테이너 조작 관련 커맨드(상위 커맨드 container)
- 컨테이너를 실행하거나 종료하고, 컨테이너 목록을 확인하는 등 컨테이너를 다루기 위해 사용하는 커맨드
#### 이미지 조작 관련 커맨드(상위 커맨드 image)
- 이미지를 내려받거나 검색하는 등 이미지와 관련된 기능을 실행하는 커맨드
- 이미지를 대상으로 어떤 일을 할지는 하위 커맨드를 통해 지정
#### 볼륨 조작 관련 커맨드(상위 커맨드 volume)
- 볼륨 생성, 목록 확인, 삭제 등 볼륨(컨테이너에 마운트 가능한 스토리지)과 관련된 기능을 실행하는 커맨드
#### 네트워크 조작 관련 커맨드(상위 커맨드 network)
- 도커 네트워크의 생성, 삭제, 컨테이너의 네트워크 접속 및 접속 해제 등 도커 네트워크와 관련된 기능을 실행하는 커맨드
	- 도커 네트워크 : 도커 요소 간의 통신에 사용하는 가상의 네트워크
#### 그 밖의 상위 커맨드
- 대부부은 도커 스웜(Docker Swarm)과 관련된 커맨드로서 초보자 수준에서 사용할 일이 없음
	- `도커 스웜`이 뭡니까?
		- ...
#### 단독으로 쓰이는 커맨드
- 도커 허브 검색, 로그인에 사용되는 커맨드
```
docker
	ㄴ login : 도커 레지스트리에 로그인
	ㄴ logout : 도커 레지스트리에 로그아웃
	ㄴ search : 도커 레지스트리를 검색
	ㄴ version : 도커 엔진 및 명령행 도구의 버전을 출력
```

## Section 3) 컨테이너의 생성과 삭제, 실행, 정지
> 실제로 컨테이너를 생성하고 실행해보자

### docker run 커맨드와 docker stop, docker rm 커맨드
`docker run`은 컨테이너를 생성하고 실행한다.
-  `docker run (docker container run)`
	- 도커 컨테이너를 생성하고 실행
		- 컨테이너를 생성하기 위한 이미지가 없다면 이미지를 내려받는 기능도 겸함

- `docker pull (docker image pull)`
	- 이미지 내려받기
- `docker create (docker container create)` 
	- 도커 컨테이너를 생성
- `docker start (docker container start)`
	- 도커 컨테이너 실행
--> create, start, pull을 한꺼번에 run으로 실행하는 것이 일반적이다.

컨테이너 정지하기와 폐기하기
- `docker stop (docker container stop)`
	- 컨테이너 정지하기
- `docker rm (docker container rm)`
	- 컨테이너 삭제하기
--> 컨테이너를 폐기하려면 먼저 컨테이너를 정지시켜야 한다.
--> **동작 중인 컨테이너를 그대로 삭제할 수는 없다.**

#### 컨테이너를 생성하고 실행하는 커맨드: docker run (docker container run)
> 컨테이너를 생성해 실행하는 커맨드

- docker image pull --> docker container create --> docker container start 의 기능을 하나로 합친 것과 같음
	- 이미지가 없는 경우 먼저 이미지를 내려 받음
	- 컨테이너의 '대상'은 아래 옵션을 사용할 수 있음 
		- 이름: `--name` 
		- 포트 번호: `--p`
		- 볼륨 마운트: `-v`
		- 컨테이너를 연결할 네트워크: `--net`
	- 데몬 형태로 동작하는 소프트웨어의 컨테이너에 사용하는 옵션
		- 백그라운드 실행: `-d`
		- 컨테이너에 터미널(키보드)을 연결함: `-i`
		- 특수 키를 사용 가능하도록 함: `-t` 
		- 환경 변수 설정: `-e`
		- 합쳐서 사용: `-dit`
	- 사용할 수 있는 인자는 이미지의 종류에 따라 달라짐
		- 예를 들어 로그인 필요한 MySQL 이미지는 아이디, 패스워드, 인코딩이나 정렬 순서를 인자로 받음
	- **정리: `docker run (옵션) 이미지 (인자)`**
#### 컨테이너를 정지하는 커멘드: docker stop (docker container stop)
- **컨테이너를 삭제하기 위해서는 반드시 '정지'해야함**
- `docker stop (대상; 컨테이너_이름)`

#### 컨테이너를 삭제하는 커맨드: docker rm (docker container rm)
- 컨테이너를 삭제함
- 정지 상태가 아닌 컨테이너에 사용하는 경우 오류가 발생하며 컨테이너가 삭제되지 않음
- `docker rm (대상; 컨테이너_이름)`

##### Column 한 번만 실행되는 컨테이너와 데몬 형태로 동작하는 컨테이너
- 컨테이너에 따라 지정 가능한 옵션이나 인자도 달라짐
- 한 번만 실행되는 컨테이너는 실행하자마자 종료되므로 컨테이너가 터미널의 제어를 차지하더라도 일시적인 것이므로 문제되지 않음
	- 이런 경우 -it와 같은 옵션은 불필요함
- 데몬처럼 계속적으로 실행되는 프로그램은 저절로 종료되지 않으므로 한번 터미널의 제어를 넘기면 이를 되찾아오기 번거로움
	- *데몬*: 유닉스 또는 리눅스에서 동작하는 프로그램 중에서 백그라운드에서 항상 동작하는 프로그램을 관례적으로 데몬(demon)이라고 한다.

### docker ps 커맨드
- 컨테이너의 생애주기를 관장하는 커맨드 외에 자주 사용하게 될 커맨드
- `docker ps (docker container ls)`
- 도커 컨테이너의 목록을 출력
- `docker ps -a`: 현재 존재하는 컨테이너(**정지 상태를 포함**)의 목록을 출력
- **컨테이너를 실행하거나 정지시킬 때 컨테이너의 상태가 기대했던 대로인지 확인, 컨테이너의 상세 정보 확인**

#### 컨테이너 목록의 정보
```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
- `CONTAINER ID`: 각 컨테이너를  구분하는 무작위 값
- `IMAGE`: 컨테이너를 만들 때 사용한 '이미지 이름'
- `COMMAND`: 컨테이너 실행 시 실행하도록 설정된 프로그램
- `CREATED`: 컨테이너 생성 후 경과된 시간
- `STATUS`: 컨테이너의 '현재 상태', 실행중: Up / 종료: Exited
- `PORTS`: 컨테이너에 할당된 포트 번호. '호스트 포트 번호 -> 컨테이너 포트 번호' 형식, 포트 번호가 동일한 경우 '->' 뒷 부분은 출력되지 않음
- `NAMES`: 컨테이너의 '이름'

## Section 04) 컨테이너의 통신
> 아파치 컨테이너를 만들고 웹페이지 확인하기
### 아파치?
> "웹 서버 기능을 제공하는 소프웨어"

--> 아파치가 동작 중인 서버에 파일을 두면 이 파일을 웹 사이트 형태로 볼 수 있다.

앞선 방식으로는 컨테이에 접근할 수 없음
--> 가능하게 하기 위해서는 컨테이너를 실행할 때 설정이 필요

### 컨테이너와 통신하기
- 웹브라우저를 통해 컨테이너에 접근하려면 외부와 접속하기 위한 설정이 필요
	- 이를 위해 포트(*port*)를 설정
		- '포트'란 통신 내용이 드나드는 통로를 의미
	- 아파치는 서버에서 정해둔 포트(80번 포트)에서 웹 사이트에 대한 접근을 기다리다가 사용자가 이 포트를 통해 접근해 오면 요청에 따라 웹 사이트의 페이지를 제공
		- 하지만 컨테이너 속에서 실행 중이 ㄴ아파치는 외부와 직접 연결되지 않았기 때문에 외부에서 접근할 수 없음
- 포트 설정 방법
	- `-p 호스트_포트_번호:컨테이너_포트_번호`
	- `-p 8080:80`
- 컨테이너를 사용하면 여러 개의 웹 서버를 함께 실행 할 수도 있음
	- **호스트 포트 번호를 모두 같은 것으로 사용하면 어떤 컨테이너로 가야할 요청인지 구분할 수 없기** 때문에 컨테이너 A에는 호스트 포트 8080, 컨테이너 B에는 호스트 포트 8081과 같은 식으로 호스트의 포트 번호를 겹치지 않게 설정
		- 여러 컨테이너로 연결되는 포트를 같게 설정하고 싶다면 **리버스 프락시**로 서버 이름을 통해 구별하도록 구성

#### 웹 사이트로 접근
- 현재 접속중인 컨퓨터: localhost
- `http://localhost:8080/`

### 실습: 통신이 가능한 컨테이너 생성
- `docker run --name apa000ex2 -d -p 8080:80 httpd`
	- `-p 8080:80`: 호스트의 포트 8080 컨테이너 포트 80으로 포워딩
- http://localhost:8080/ 으로 브라우저에서 웹서버 실행을 확인
- `docker stop apa000ex2`으로 컨테이너 종료
- `docker rm apa000ex2`으로 컨테이너 삭제

## Section 05) 컨테이너 생성에 익숙해지기
### 다양한 유형의 컨테이너
> 아파치 이외의 소프트웨어가 담긴 여러 가지 컨테이너를 생성해보며 컨테이너를 생성하는 연습을 진행한다

### 실습: 아파치 컨테이너를 여러 개 실행하기
- 컨테이너를 여러 개 실행할 때 **호스트 컴퓨터의 포트 번호가 중복돼서는 안됨**
	- 따라서, 1씩 차이나도록 번호를 지정
- **컨테이너 포트는 중복돼도 무방하므로 모두 80으로 설정**

> 아파치 컨테이너 여러 개를 실행해본다.
#### run 커맨드 실행
```shell
> docker run --name apa000ex3 -d -p 8081:80 httpd
6241b2165bfee7a1b006368ab91c3d9354d246a02d26865b88e881c533d46414
> docker run --name apa000ex4 -d -p 8082:80 httpd
db73665906ab179bef3a71a41526d4cfafc1bb7b34348f8be4134b09994a3db4
> docker run --name apa000ex5 -d -p 8083:80 httpd
fa07f90adaf97ee03949ca6f4c6b8eeaf62240cc80f43cdda1361203bbd61980
> docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                                   NAMES
fa07f90adaf9   httpd     "httpd-foreground"   7 seconds ago    Up 6 seconds    0.0.0.0:8083->80/tcp, :::8083->80/tcp   apa000ex5
db73665906ab   httpd     "httpd-foreground"   14 seconds ago   Up 13 seconds   0.0.0.0:8082->80/tcp, :::8082->80/tcp   apa000ex4
6241b2165bfe   httpd     "httpd-foreground"   21 seconds ago   Up 20 seconds   0.0.0.0:8081->80/tcp, :::8081->80/tcp   apa000ex3
```
- `docker ps` 확인 시 최신 실행 순으로 컨테이너가 나열됨
- 웹 브라우저에서 http://localhost:8081 , http://localhost:8082 , http://localhost:8083 으로 확인 가능
#### stop 커맨드를 사용해 컨테이너 종료 -> 삭제하기
```shell
> docker stop apa000ex3
apa000ex3
> docker stop apa000ex4
apa000ex4
> docker stop apa000ex5
apa000ex5
> docker rm apa000ex3
apa000ex3
> docker rm apa000ex4
apa000ex4
> docker rm apa000ex5
apa000ex5
> docker ps -a
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          
```

## Section 06) 이미지 삭제
> 컨테이너를 다루는 데 필요한 일련의 조작과 더불어 이미지를 삭제하는 방법도 알아보자

### 이미지 삭제
- 컨테이너 사용 후 삭제를 하더라도 이미지는 그대로 남아 쌓임
- 이미지가 늘어나면 스토리지 용량을 차지하게 됨
	- 따라서, 쓰지 않는 경우 그때그때 삭제

### docker image rm 커맨드
- `docker rm`은 컨테이너 삭제
- `docker image rm 이미지_이름 [이미지_이름] [이미지_이름] [이미지_이름]`

### docker image ls 커맨드
- 이미지 목록 확인
- `docker ps`와 달리 `-a`는 사용할 수 없음
	- 이미지는 컨테이너와 달리 '실행 중', '종료' 등의 상태를 가질 수 없기 때문
- `docker image ls`

#### 이미지 목록의 정보
```shell
docker image ls
REPOSITORY                                                  TAG             IMAGE ID       CREATED         SIZE
nginx                                                       latest          a5cdc6683c84   3 weeks ago     198MB
mysql                                                       latest          1ba038aab342   3 weeks ago     876MB
httpd                                                       latest          3405bcd9d2a6   3 months ago    178MB
registry.dev.kurlycorp.kr/local-dev-proxy/local-dev-proxy   latest          c3c7b19612ee   4 months ago    184MB
registry.dev.kurlycorp.kr/local-dev-proxy/local-dev-proxy   <none>          d33519bda791   9 months ago    1.15GB
kurly-www-v2_web                                            latest          4ca5df343e49   11 months ago   18.9MB
kurly-www-v2_app                                            latest          f673f95dedc0   11 months ago   987MB
nginx                                                       1.20.0-alpine   1ef2477c082c   4 years ago     21.2MB
```
##### Column: 이미지 버전과 이미지 이름
- 이미지_이름:버전_넘버
- `httpd:2.2`

