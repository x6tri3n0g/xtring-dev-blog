+++ 
title= "1. 도커란 무엇인가?" 
date= 2025-04-20
tags= ["docker"] 
draft= false 
+++

## Section 1) 도커란 무엇인가?

### 도커의 정체

> 도커를 잘 모르면 "편리해 보이지만, 뭔지 잘 모르겠다"는 이미지를 가지고 있다.

'도커' 개발 초기에는 BE를 중심으로 개발환경에서 사용됐으나, 지금은 운영 환경은 물론이고 FE의 개발환경에도 널리 도입되었다. -> 이제는 많은 곳에서 도커가 사용중이다.

#### 도커는 '데이터 또는 프로그램을 격리시키는' 기능을 제공한다

- 주로 서버에서 이용하기도 하지만 클라이언트 컴퓨터에서도 사용됨
- `도커`는 **다양한 프로그램(소프트웨어)과 데이터를 각각 독립된 환경에 격리하는 기능**을 제공

#### 컨테이너와 도커 엔진

- `컨테이너`: 서버 환경을 '조립형 창고'와 같이 분할시킨 공간
- `도커`: 컨테이너를 다루는 기능을 다루는 기능을 제공하는 소프트웨어
- `도커 엔진`: 도커를 사용하기 위한 소프트웨어 본체, 컨테이너를 생성하고 구동시키는 역할

#### 컨테이너를 만들려면 이미지가 필요함

- `컨테이너`를 만들기 위해서는 `이미지`와 `도커 엔진`이 필요
- `이미지`의 종류는 다양
  - 아파치 컨테이너 만들기 -> 아파치 이미지 사용
  - MySQL 컨테이너 만들기 -> MySQL 이미지 사용
- 하나의 이미지로 여러개의 컨테이너를 생성할 수 있음
  - 용량이 허용하는한 하나의 도커에서 여러 개의 컨테이너 생성 가능

#### 도커는 리눅스 컴퓨터에서 사용

- 도커를 사용하기 위한 제약 사항
  - `리눅스 운영체제`
    - 윈도우나 macOS에서도 도커를 구동할 수는 있지만, 이 경우 내부적(?)으로 리눅스가 사용됨
    - '컨테이너에서 동작시킬 프로그램도 리눅스용 프로그램'
      - 도커가 리눅스 운영체제에서 사용하는 것을 전제로 만들짐

### 데이터나 프로그램을 독립된 환경에 격리해야하는 이유

> 데이터나 프로그램을 독립된 환경에 격리해야하는 이유?

- 소프트웨어는 여러 프로그램으로 구성되며, 다른 프로그램과 정보를 공유하기도 함
- 프로그램을 실행하기 위해서는 각 프로그램의 실행환경이 필요함
  - !) 시스템 A, B 사이의 필요 조건의 실행환경 버전이 일치하지 않는 경우가 발행할 수 있다
    -> 공유하는 대상이 있는 두 시스템 사이에 어느 한쪽만을 위해 버전을 수정하면 다른 쪽에서 오류가 발생하게됨
    -> **문제의 원인은 프로그램 사이의 실행환경 공유에 있음**
    - 프로그램이 고도화되는 과정에서 이는 문제로 직면함

### 프로그램의 격리란?

- **도커 컨테이너는 다른 컨테이너와 완전히 분리된 환경**
  - 컨테이너 안에 들어있는 프로그램은 다른 프로그램과 격리된 상태
- '프로그램의 격리'로 해결되는 것은
  - 여러 프로그램이 한 서버에서 실행되면서 발생하는 문제 대부분
  - 예시) 시스템 A - 어떤 프로그램 5.0 버전 사용, 시스템 B - 어떤 프로그램의 8.0 버전 사용 을 사용하여 각각 세트로 묶어(컨테이너로) 따로 격리한다

## Section 2) 서버와 도커

> 도커를 설명하기 위해서는 서버 이야기를 하지 않을 수 없다

### 서버의 두 가지 의미

서버란 무엇일까?
-> **어떤 서비스(service)를 제공(serve)하는 것**

#### 기능적 의미의 서버와 물리적 컴퓨터로서의 서버

서버의 두 가지 의미

- '기능적 의미의 서버'
  - 예) "웹 서버에 올려줘", "메일 서버가 죽었어" 등에서 말하는 서버
  - '무슨무슨 서버'라는 말은 '무슨무슨 기능을 제공한다'는 의미
    - 웹 기능을 제공하는 서버
- '물리적 컴퓨터로서의 서버' - 예) "신입 직원이 올 테니 저 책상 위의 서버 좀 치워라" - 실물(; 컴퓨터) - 데스크톱 컴퓨터와 같이 어딘가에 물리적으로 존재하는 컴퓨터
  -> **이렇게 의미를 구분하는 이유는 하나의 '물리적 컴퓨터로서의 서버'에 여러 개의 '기능적 의미의 서버'를 함께 둘 수 있기 때문이다.**

서버 컴퓨터는 결국 우리가 사용하는 '컴퓨터'다. 개인용 컴퓨터는 개인이 사용하지만 **서버는 여러 사람이 원격으로 접근해 사용**한다.

- 서버용 운영체제 위에 여러 소프트웨어가 동작

#### 서버의 기능은 소프트웨어가 제공한다

> 따라서 서버의 기능 역시 특별한 것이 없다.

**서버의 기능은 소프트웨어가 제공하는 것으로,** 소프트웨어를 설치하면 '서버'의 기능을 갖게 된다
즉, '무슨무슨 서버를 만든다'는 말은 '무슨무슨용 소프트웨어를 설치해 이 기능을 갖춘다'는 말과 같다.
-> **'여러 가지 소프트웨어를 한 컴퓨터에 설치할 수도 있다'**

#### 서버의 대표적인 예

서버의 대표적인 예

- 웹 서버
- 메일 서버
- 데이터베이스 서버
- 파일 서버
- DNS 서버
- DHCP 서버
- FTP 서버
- 프락시 서버
- 인증 서버

#### 서버의 운영체제로는 주로 리눅스가 사용된다

> 개인용 컴퓨터와 서버는 크게 다르지 않다

- 서버는 사용 목적과 역할에 따라 구성이 다를 수 있음
  - 소프트웨어를 설치한다는 점에서는 개인용 컴퓨터와 같음
- 서버는 역할 특성상 운영체제는 `서버용 운영체제`를 사용하는 경우가 많음
  - 서버용 운영체제는 `리눅스` 또는 `유닉스` 계열을 주로 사용
    - 소프트웨어 역시 리눅스용 소프트웨어가 대다수를 차지
    - 윈도우 전용 서버 버전이 있지만 리눅스와 유닉스가 더 많이 사용됨
- 서버 운영 체제의 종류

```
윈도우 계열
	ㄴ Windows

유닉스 계열
	ㄴ 리눅스 계열
		ㄴ Red Hat, CentOS
		ㄴ Debian, Ubuntu
		ㄴ SUSE, openSUSE
		ㄴ 기타
	ㄴ BSD 계열
		ㄴ macOS
		ㄴ FreeBSD
		ㄴ NetBSD
	ㄴ 솔라리스 계열
		ㄴ Solaris, OpenSolaris
	ㄴ 기타...
```

- BSD 계열?
- 우리 회사는 어떤 서버 운영체제를 사용할까?

### 컨테이너를 이용해 여러 가지 서버 기능을 안전하게 함께 실행하기

> 도커 환경에서 컨테이너를 사용하면 프로그램을 완전히 격리시킬 수 있다

**컨테이너 기술을 활용하면 하나의 물리 서버에 여러 개의 웹 서버를 올릴 수 있다.** 한 대의 서버에서 실행하던 웹 서버, 메일 서버, DB 서버를 각각 독립적인 환경에서 안전하게 운용할 수 있을 것이다.

하나의 물리 서버에 하나의 웹 서버를 띄우면 `웹서버:물리서버=1:1` 와 같은 형태로 구성된다. 필요한 용량이 크지 않은데 이러한 형태로 물리 서버가 많아지면 그 만큼 낭비가 발생한다. 만약, 두 웹 서버를 하나의 물리 서버에 함께 둔다면 프로젝트 하나의 비용이 절반으로 감소할 것이다.

질문?) 다양한 종류의 서버를 다수 개 운용 해야한다면 하나의 물리 서버 컴퓨터에 같은 종류의 서비스를 묶어 하나의 물리 서버에서 제공할까?

개발 측면에서 이점
: 개발환경을 갖추거나 운영 환경으로 쉽게 넘어갈 수 있다
-> 컨테이너가 그저 격리된 환경이 아니라 **쉽게 옮길 수 있다**는 특성에서 비롯됨

### 자유로이 옮길 수 있는 컨테이너

> 컨테이너는 자유로이 옮길 수 있다

-> 실제로는 컨테이너의 정보를 내보내기한 다음, 다른 도커 엔진에서 복원하는 형태
-> 이런 특성을 이용하면 동일한 상태로 튜닝한 컨테이너를 팀원 전원에게 배포해 모두가 동일한 개발환경을 사용할 수 있음

- 도커만 설치되어 있으면 이러한 형태의 공유가 가능함
- 도커를 이용하면 **물리적 환경의 차이, 서버 구성의 차이를 무시**할 수 있으므로 운영 서버와 개발 서버의 환경 차이로 인한 문제를 원천적으로 방지할 수 있음
