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