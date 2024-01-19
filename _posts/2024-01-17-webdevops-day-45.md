---
title: (Day	45) 네트워크를 다루기 위한 기본
author: 김준회
date: 2024-01-17 23:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt:
---
# Review

## Network

### Proxy

### Stateful, Stateless

한번 요청하고 연결을 계속 유지하는 경우, 즉 상태(State)를 유지하는 경우는 Stateful 통신이다. 한번 요청을 처리하고 연결을 끊고, 필요시 다시 연결하도록 하는 경우는 Stateless 통신이다.

### ServerSocket의 대기열과 accept() 메서드의 동작
서버소켓은 큐 자료구조를 내장하고 있다. 여러 클라이언트의 요청이 들어오면 큐에 담기고, accept() 해주는 순서대로 큐를 처리한다.


### Socket
두 컴퓨터 사이의 연결 정보를 소켓이라고 한다.
### Port Number
포트번호는 들어오는 통신들을 구별하기 위해 붙이는 꼬리표다.
서버측 애플리케이션은 본인이 사용할 포트번호를 직접 정한다.
0~1024 는 예약 포트번호다. 충돌 방지를 위해 쓰지 않는 것이 관례다. (근데 666 같이 이제는 거의 안쓴다 싶은건 써도 되지 않을까..?) 그 외에도 오라클DB, MySQL 같이 자주 사용되는 어플리케이션들이 사용하는 포트번호도 쓰지 않는 것이 좋다.
클라이언트측 애플리케이션은 49XXX ~ 65535 사이에서 운영체제가 포트번호를 부여한다.
IP Address는 컴퓨터를 구별하기 위한 번호고, 포트번호는 어플리케이션들을 구별하기 위한 번호라고 생각하면 된다.
JVM에서 중복된 포트번호를 가지고 연결을 OS에 요청하는 경우 Binding Exception이 난다. 포트번호를 묶을 때 예외가 발생했다는 것이다.

### 소켓기반 File I/O
소켓이 가진 IO Stream을 가져와서 입출력하면 된다.
버퍼를 쓰지 않으면 10MB 파일 기준 대략 70~200배쯤 느렸다.
입출력에서 버퍼는 선택이 아니라 필수다. 

### 네트워크 프로그래밍에서 read(), write()의 차이
write()는 수행하고 즉시 리턴한다. read()는 들어오는 값을 읽기 전까지 blocking 상태로 있게 된다. 값이 없을 때 기다리는 건 잘 이해가 되는데, 왜 write()는 리모트에서 읽기를 기다리지 않나? 네트워크 카드에도 RAM이 있다. 그 RAM에 데이터를 쓰고 바로 빠져나오는 것이다.

# Study
## Network
### DNS(Domain Name System)
www.naver.com 을 브라우저에 입력하면 무슨 일이 일어나나?
1. DNS 서버에 연결한다. (DNS 서버는 아주 많이 있다)
2. 내 컴퓨터는 도메인을 전달한다.
3. 도메인 서버는 해당 도메인을 기반으로 IP Address를 전달한다.
4. 
5. 그럼 공유기는 어떻게 여러 기기들을 구별하는가?
6. MAC 고유번호가 있다. 랜카드 협회에서 관리한다. 

### 기본 대기열 사이즈
자바에서 서버소켓의 기본 대기열 큐 사이즈는 50개이다. 정확한 변수명은 `backlog`이다.
```java
new ServerSocket(포트번호, 대기열크기);
// 다음과 같이 대기열의 개수를 지정하지 않으면, 기본이 50개이다.
```

클라이언트도 무한정 기다리지는 않는다. 서버 응답을 기본 30초 기다려본다. 타임아웃을 넘기면 예외를 던진다. 

### accept()
대기열에 기다리고 있는 클라이언트를 꺼내온다.

## `Connection Oriented` VS `Connectionless`
CO와 CL은 프로토콜을 구분하는 분류이다.
상대방과 연결되었음을 확인하냐, 확인하지 않느냐의 차이이다.

Connection Oriented: firstly, client **make a connect** with the server. and have a transfer. after transfer, cut the connection.

Connectionless:  no connection made. just send a data. and receive the response.

비유 : 전화는 항상 연결이 발생해야 데이터 송수신이 가능하다. 그러니까 CO라고 할 수 있다. 근데 편지나 택배같은 경우는 연결이 발생하건 발생하지 않건 일단 보낼 수 있다. (등기우편 말고~)

### Pros and Cons
Connection oriented: 데이터 송수신의 신뢰성이 있다.
대표적인 규약(프로토콜): TCP

Connectionless: 데이터 송수신의 신뢰성을 보장하지 않는다.
대표적인 규약(프로토콜): UDP

서버를 경유하는 메신저(대부분의 상용 메신저)는 TCP를 사용하는 Connection oriented 방식이다.


### localhost로 네트워크 실습을 하면 생길 수 있는 일
OS에 따라 다르기도 한데 여러 프로세스들로 실습을 하면
윈도우의 경우는 `앱-랜카드-허브-랜카드-앱` 순서가 아니라, `앱-랜카드-앱`으로 바로 가버린다. 그러면 타임아웃 시간을 기다리지 않고 바로 대기열 상태에 따라 예외를 던져버린다.
맥은 `앱-랜카드-허브-랜카드-앱` 순서로 흘러가기에 로컬호스트로 실습해도 네트워크 환경하고 유사하다.

## TCP/IP 
TCP/IP란? 소프트웨어이다.
NIC(Network Interface Card)에 내장된 소프트웨어다.

어플리케이션이 데이터를 TCP/IP를 통해 보내는 경우를 생각해보자.
1. 어플리케이션이 데이터를 보낸다.
2. TCP 계층을 거치면서 데이터를 패킷으로 분할한다. 각 패킷은 번호를 갖는다.
3. IP 계층을 거치면서 패킷들에 필요한 주소를 부여한다. 처리된 패킷은 프레임이라고 한다.
(받는이의 주소, 보내는이의 주소)
4.  NIC이 프레임에 NIC의 주소 = MAC(Media Access Control) Address)를 붙인다.

* 패킷은 데이터 전송의 최소 단위다.

반대로 NIC가 TCP/IP로 보내진 정보를 받아서 데이터로 만드는 경우는, 마치 조립의 역순과 같다.
1. 프레임에서 NIC 정보를 추출한다.
2. IP 계층을 지나면서 받는 이, 보내는이 정보를 제거한 패킷들을 추출한다.
3. TCP 계층을 지나면서 패킷들을 순서에 따라 조립한다. 
(패킷들의 수신/발신은 순차적으로 이뤄지지 않는다)
4. 패킷이 조립되어 처음 보내진 데이터로 합쳐진다.

## flush()
byte stream, character stream 클래스에는 차이가 있다.
character stream에는 자체적인 버퍼가 있다. 데코레이터로 붙이지 않아도 버퍼가 있다는 말이다. 그래서 flush() 호출하지 않으면 write()를 끝내지 않으므로, flush() 해줄 것을 반드시 생각해줘야 한다. 

바이트 스트림이어도 flush()해도 문제 없으니 습관적으로 작성해주는 것이 좋다.

### 프로그램은 됐다 안됐다 하지 않는다
flush() 를 까먹는 것 같은 문제들과 같이 개발자들이 자주 하는 실수들이 있다.
비동기처리도 그렇다. 네트워크 속도에 따라서 먼저 가져왔기 때문에 데이터를 잘 가져온 경우도 있고, 속도 차이로 인해 데이터를 못 가져온 상태에서도 화면을 만들어버리는 문제가 발생할 수도 있다. 그래서 나중에 promise 가 제안된다 (JS) 
결론적으로, 프로그램은 됐다 안됐다 하지 않는다. 그건 그냥 99.9%는 잘못 작성한 것이다. 물리적인 하드웨어 문제같은 게 아니라면.

## File
File 클래스를 사용하여 디렉토리나 파일을 다룰 수 있다.
File 자체는

### 재귀함수 적용할 사례들
1. 삭제: 하위 경로를 포함하여 모든 파일과 경로를 지우는 방법
```java
// 이거 손코딩 맛집 (빈칸채우기 같은) 
// 출제이유: 1. 파일클래스 메서드아니? 2. 재귀호출 쓸 줄아니?
static void deleteFile(File dir) { 
    System.out.println(dir.getAbsolutePath());
    // 주어진 파일이 디렉토리라면 하위 파일이나 디렉토리를 찾아 지운다.
    if (dir.isDirectory()) {
      File[] files = dir.listFiles();
      for (File file : files) {
        deleteFile(file);
      }
    }

    dir.delete(); // 현재 파일이나 폴더 지우기
  }
```
2. 조회: 하위 경로를 포함하여 모든 파일과 경로를 출력하는 방법
```java
```
이런 기능들은 직접 File Class를 다루면서 작성해야 할 경우가 많다.
IoC 컨테이너를 만들 때도 이런 기술이 사용된다.
패키지에 해당되는 클래스의 인스턴스를 자동으로 생성하고 해쉬맵에 보관하는 것이 IoC 컨테이너인데 이런 기능을 구현할 수 없으면 불가능하다.

### 손코딩 맛집
배열의 add/insert/delete
링크드리스트/스택/큐

# OTHERS
돈 버는 방법을 배우는 건 힘들다.
돈 버는 건 더 힘들다.
힘든 건 당연한 것.
그런데 근본적인 경쟁력을 가지는 순간부터는
돈 버는 것이 힘들어보이지 않게 된다.