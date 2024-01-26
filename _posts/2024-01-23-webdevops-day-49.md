---
title: (Day	49) 병행처리, 프로세스와 스레드, 그리고 네트워크(Gateway, VPC)
author: 김준회
date: 2024-01-23 17:00:00 +0900
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
[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8B%A4%EC%8A%B5%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B84.pdf)
# Review
## CO/CL
### DNS 서버와 야코
순간 이게 생각났다. 알아? 몰라! 알아? 몰라! 알아? 몰라! 알아? 알아!
[Kids now, Kids then](https://www.youtube.com/watch?v=fWOEhFu9H1U)

ISP 가 제공하는 DNS 서버에 물어보고, DNS 서버는 모르면 하위 서버에 물어보고, 하위 서버는 모르면 또 하위 서버에 물어보고, 이렇게 계속 물어보고 찾으면 IP주소를 반환하고!

### TCP/IP?

### HTTP 클라이언트와 서버?
프로토콜에 따라 서비스를 제공하는, 제공받는
HTTP의 애칭 : 웹
다른 경우: FTP, SMTP

### HTTPS
E2EE(종단간암호화)를 HTTP 프로토콜에 적용한 것
* 새 아이폰을 처음 로그인하면 개인키와 공개키가 생성된다. 그리고 로그인하면 그 공개키를 애플 서버에 보낸다. 애플은 자체 서버에 모든 유저의 공개키를 갖고 있다. 

### URI, URL, URN
인터넷상의 리소스들을 구별하기 위해 쓰는 방법이다.
(Uniform Resource Identifier / Locator / Name)
URI는 URL과 URL을 포함하는 개념이다.
URL을 아주 많이 쓴다. URL의 structure를 알아야 한다.

## BASE64
바이너리 데이터를 텍스트화시키는 인코딩 방법이 BASE64이다. 
바이너리 데이터를 텍스트와 **함께** 보내거나, 받거나 하고 싶어서 쓴다.
URL에 바이너리를 넣고 싶다거나 할 때 말이다.

## 병행처리(Concurrency) Concept
* 흔한 오해: 하나의 스레드를 여러 코어로 처리하는 건 병렬처리(Parallelism)으로 다른 개념이다. [더 자세한 내용](https://joonhoekim.github.io/posts/webdevops-day-48/#cf-%EB%B3%91%EB%A0%ACparallel%EC%9D%80-%EC%95%84%EB%8B%88%EB%8B%A4)
* 여러 개의 스레드를 여러 코어로 처리하는 것이 병행처리(Concurrency)이다. 
* 각 작업 간 처리 순서가 중요하지 않고, 서로 간섭하지 않는 작업들의 경우에 Concurrency를 적용하여 효율성을 높일 수 있다. (단위시간에 사용하는 자원의 양이 늘어난다.)

# TIL
## 디바이스
### 허브의 취약성
같이 연결된 디바이스한테도 패킷이 공유된다. 수신기에 들어온, 다른 기기의 패킷들은 무시하도록 설게되는데, 여기에 해킹 목적의 프로그램은 그 무시되는 패킷을 수집한다. 즉, 수집한 패킷이 종단간 암호화가 되어 있지 않다면, 같은 허브에 연결된 공격자에게 전부 노출된다.

### 스위치와 허브의 차이
스위치는 모든 연결된 디바이스에 패킷을 뿌리는 게 아니라 디바이스별로 구분해서 뿌린다. 예전에는 (~2010년 정도) 허브와 스위치의 가격 차이가 아주 심했다. 요즘에는 저렴한 공유기들도 허브가 아니라 스위치 방식을 택한다.

## 게이트웨이
우리가 통신사(ISP)와 계약하면 랜선을 받는 게 아니라 동축케이블(구리선)이나 광케이블을 받는다. 오는 건 랜선이 아닌데 쓰는 건 랜선이다. 통신방식이 다른 경우 (이종간 통신) 에 게이트웨이가 필요하다.

참고로 자주 사용하는 이더넷 랜 케이블은 구리선 8가닥이다.

![](https://i.namu.wiki/i/jJYPnugiA4GJ9r9kcx1OD8pC_XAtrvdHOsJ2rOskzbooh4IZIfz5wkyZYaRXXt-nTjYDNfsK0_fAh0vBsP8YuhwAUkemmT1tFFP_aeBk5s4O9qESp6JDKyLarurzP5PfFTRSIbWk8QbNsw5mfX7ZVQ.webp)

참고: [차재복님이 작성하신 내용](https://www.ktword.co.kr/test/view/view.php?no=399)

### 네트워크 연결도
그래서 
```
디바이스
|
랜선
|
게이트웨이
|
동축/광 케이블
|
라우터들
|
동축/광 케이블
|
게이트웨이
|
디바이스 
```
형태가 된다.

## 병행처리에 대해 좀 더 알아보자
병행처리를 구현하기 위해서 프로세스를 복제하는 방법이 처음에 제안되고, 그 후 메모리 효율을 위해 스레드를 추가하는 개념이 등장한다.
### 프로세스의 복제(fork)
프로세스는 코드 세그먼트와 데이터 세그먼트로 이뤄진다. (많이 간단하게 말하면 말이다.) 코드 세그먼트는 기계어들을 갖고 있고, 데이터 세그먼트는 데이터를 보관하며 힙 영역에 위치한다. 프로세스를 포크(복제)하면 코드세그먼트와 데이터세그먼트를 둘 다 복제한다. 그래서 메모리 사용량이 많다. (특히 힙메모리가 복제되는 것이 원인이다!)

### Threads Share Heap Memory
스레드는 프로세스 복제와 달리, 실행위치 정보와 스택 메모리만 가진다. 실행위치 정보를 따라가면 원래 프로세스의 힙 메모리 위치를 알 수 있다. 그렇게 메모리를 공유하는 것이다.

## IP Address
### Octet
> 옥텟(octet)은 컴퓨팅에서 8개의 비트가 한데 모인 것을 말한다. 초기 컴퓨터들은 1 바이트가 꼭 8 비트만을 의미하지 않았으므로, 8비트를 명확하게 정의하기 위해 옥텟이라는 용어가 필요했던 것이다.
> [위키백과](https://ko.wikipedia.org/wiki/%EC%98%A5%ED%85%9F_(%EC%BB%B4%ED%93%A8%ED%8C%85))


### Mask

123.123.123.123/***11***
슬래쉬 뒤에 붙는 숫자가 마스크다.
마스크 만큼의 비트를 앞에서부터 고정하겠다는 것이다.

> 추가 포스팅하여 정리 필요..

## VPC(Virtual Private Cloud)
가상의 개인 네트워크 공간이다.
VPC는 앞 16비트는 고정이다. 
즉, VPC는 언제나 10.0 으로 시작한다는 것이다.
마스크가 16인 경우 최대 개수의 비트를 쓴다.
그래서 VPC는 최대 256 * 256 = 65536 개의 주소를 할당할 수 있다.

### ACL(Access Control List)
> 네트워크 액세스 제어 목록(ACL)은 서브넷 수준에서 특정 인바운드 또는 아웃바운드 트래픽을 허용하거나 거부합니다. VPC에 대한 기본 네트워크 ACL을 사용하거나 보안 그룹에 대한 규칙과 유사한 규칙을 사용하여 VPC에 대한 사용자 지정 네트워크 ACL을 생성하여 VPC에 보안 계층을 추가할 수 있습니다.
>
> From [AWS Docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/vpc-network-acls.html)

### Subnet
VPC를 청주시라고 한다면
서브넷은 청주시 안에 봉명동을 만든 것이다.

**10.0** 청주시
10.0.**0** 봉명동
> 이 부분들은 따로 정리를 하면서 공부를 더 해야겠다.

### Server ACG (Access Control Group)
서버에 대한 접근 제어 그룹이다.
방화벽이라고 보면 된다.

## 윈도우 서버 VS 리눅스 서버
리눅스 서버는 소스코드가 아예 공개되어있으니, 소스코드 분석을 통한 공격도 자주 당함. 그리고 널리 확산됨. 그만큼 공개된 취약점의 업데이트도 아주 빠름.

윈도우는 소스코드가 비공개임. 네트워크만 뚫리면 MS가 **만들어둘 수 있는** 백도어로 모든 데이터가 공개될 것임.

2020년대 현재는 리눅스 서버 점유율이 80% 정도라고 보면 됨. 그 이유는 소스코드가 공개됨에서 발생하는 투명함(성능, 보안) 때문임.

## 리눅스
### 유닉스(혹은 리눅스)의 전제
유닉스는 처음부터 PC용으로 사용했던 적이 없다. 전문가 그룹이 사용할 목적이었다. 그래서 처음부터 여러 사람이 동시에 하나의 운영체제를 이용할 수 있어야 한다는 전제 하에 만들어졌다. 반대로 Windows는 개인용 사용을 전제하고 만들어졌다. 그래서 동시에 하나의 운영체제를 여러 사람이 사용하기 어렵다.

### 유저 만들기
유저를 만드는 방법이 여러개 있을 수 있는데, 한 가지만 알아두자. `adduser $username` 명령어를 기억하자. 참고로 `adduser`, `useradd` 두 가지가 있는데 `useradd`는 홈 경로를 만들어주지 않는 등 수동 설정이 많이 요구된다. 그래서 `adduser`를 많이 사용한다.

### sudoer 추가하기


### jdk 설치하고 cli 환경에 추가하기



## JVM's Thread
* JVM은 프로세스다.
  * Heap Memory, Method Area를 가진다.
* JVM은 main 스레드를 가지는데, 이 스레드가 main() 메서드를 호출하는 역할을 한다.
  * 각각의 스레드는 각자의 스택 메모리를 갖는다.
  * 스레드는 JVM의 Heap 메모리와 Method Area 주소(실행위치정보)를 갖고있어 여러 스레드가 그 메모리들을 공유한다.

### JVM's Thread Hierachy
Recursive Call
```java
// JVM의 전체 스레드 계층도
package com.eomcs.concurrent.ex2;

public class Exam0180 {

  public static void main(String[] args) {
    // JVM의 최상위 스레드 그룹인 system의 계층도 출력하기
    Thread mainThread = Thread.currentThread();
    ThreadGroup mainGroup = mainThread.getThreadGroup();
    ThreadGroup systemGroup = mainGroup.getParent();

    printThreads(systemGroup, "");
  }

  static void printThreads(ThreadGroup tg, String indent) {
    System.out.println(indent + tg.getName() + "(TG)");

    // 현재 스레드 그룹에 소속된 스레드들 출력하기
    Thread[] threads = new Thread[10];
    int size = tg.enumerate(threads, false);
    for (int i = 0; i < size; i++) {
      System.out.println(indent + " ㄴ " + threads[i].getName() + "(T)");
    }

    // 현재 스레드 그룹에 소속된 하위 스레드 그룹들 출력하기
    ThreadGroup[] groups = new ThreadGroup[10];
    size = tg.enumerate(groups, false);
    for (int i = 0; i < size; i++) {
      printThreads(groups[i], indent + "  ");
    }
  }
}
```

OUTPUT:
```
system(TG)
 ㄴ Reference Handler(T)
 ㄴ Finalizer(T)
 ㄴ Signal Dispatcher(T)
 ㄴ Notification Thread(T)
  main(TG)
   ㄴ main(T)
  InnocuousThreadGroup(TG)
   ㄴ Common-Cleaner(T)
```



### Thread를 만드는 방법
* Thread 클래스를 상속하기
  * run() 메서드를 재정의하여 병행으로 실행할 코드를 둔다.
  * new 생성자로 인스턴스 만들고 .start() 메서드 호출한다.

* Runnable 인터페이스를 구현하기
  * run() 메서드를 재정의하여 병행으로 실행할 코드를 둔다.
  * Thread 클래스의 인스턴스를 만들 때 생성자로 구현 클래스를 전달한다.
  * 해당 인스턴스의 start() 메서드를 호출한다.

두 방법 중 더 많이 사용되는 방법은 Runnable 인터페이스를 구현하는 것이다. 람다 문법을 적용할 수 있으며 확장성이 더 좋기 때문이다.

## Multitasking
* 멀티태스킹: 동시에 여러 작업을 실햏하는 것
  * 방법 1: 여러 코어가 여러 작업을 각각 병행처리
  * 방법 2: 하나의 코어가 여러 작업을 번갈아서 실행

많은 부분이 실제로는 방법2와 같이, 진짜 동시에 실행되는 게 아니라 조금씩 CPU의 처리를 나눠 받아서 **동시에 실행되는 것처럼 보이는** 것이다.

방법 2에서는 한가지 고민이 생긴다. 여러 작업들에게 CPU라는 자원을 나눠줘야 하는데, 어떻게 나눠주는게 좋은거지? 그것이 CPU Scheduling (CPU 스케줄링) 이라는 Concept로 발전했다.

### CPU Scheduling
여러 프로세스가 CPU 자원을 받는 규칙이다.
* Round-Robin
  * 일정 시간을 돌아가면서 시행하는 방법이다.
  * Windows에서 기반으로 채택하는 스케줄링 방법이다. 
  * Windows는 라운드 로빈을 쓴다! 라고 말할 수는 없다. 여러가지 기술들이 적용되어 단순한 라운드 로빈이라고 말할 수 없으므로..
* Priority + Aging
  * 프로세스간 우선순위를 부여하여 높은 우선순위의 프로세스에 더 많은 CPU 자원을 할당하는 방법이다.
  * Starvation(기아) 문제를 해결하기 위해 처리가 너무 오래 안된, 나이가 들어버린 (aging) 프로세스는 우선순위를 높혀주는 방법이다.
    * MIT의 유닉스 서버 중에 프린트 프로세스가 3년간 기아상태로 있던 사례가 있다고 한다. 이것이 aging 기법 개발의 계기가 되었다는 후문..

CPU Scheduling은 이 둘 말고도 많고, 아주 복잡한 방법들도 있다. OS의 성능과 전력 소비를 감소시키고자 하는 니즈는 없어지지 않기에 앞으로도 계속 연구가 지속될 것이다. 게다가 현대 프로세서의 발전 방향이 코어의 수를 높이거나, 저전력 코어와 성능 코어를 분리하는 형태로 발전하다보니 새로운 프로세서를 위한 새로운(발전된) 스케줄링 방법이 계속 등장할 것이다.

> 예전 기억이 난다. CPU 스케줄링 방법들 10가지 정도인가 외웠던 일...

참고: CPU 자체도 발전하지만 프로세스의 알고리즘도 발전한다. 화면 공유 기능만 해도, 화면 변화가 있어야만 화면 캡쳐를 계속 할 것이고, 캡처한 내용을 서버에 보낼 것이다. 변화가 있더라도 변화가 있는 부분의 정보만 서버에 보낼 것이다. 자원을 덜 쓰고도 사용자에게 좋은 경험을 주는 소프트웨어들이 살아남을 것이기 때문에 알고리즘이 기술력이고, 계속 발전하게 될 것이다.

## Context Switching
`문맥 교환`으로 자주 번역되는 `Context Switching`은 하나의 코어가 여러 작업을 번갈아서 실행하기 위해 필요한 기술이다. 프로그램이 중간에 멈췄다가 실행되려면 **실행되던 때의 정보를 불러와야** 할 것이다. 이 실행하던 때의 '상태 정보'를 작업이 번갈아서 실행될 때마다 같이 불러와서 사용하고, 반대로 중단하는 작업은 **실행하던 떄의 정보를 저장해야** 할텐데, 그 때 Context Swtiching 기술이 사용된다. 

### Caution for Context Switching (주의점)
Context Switching 또한 자원을 소모하는 작업이기 때문에 문제가 된다. 그래서 OS는 Context Switching을 언제, 얼마나 자주 할 지를 최적화해야 한다.

# OTHERS
### 경력 면접의 질문 난이도
2~3년차: 코딩 테스트 + 적당한 기술면접
5~6년차: OS 혹은 언어 등 가장 바닥에 있는, 핵심적인 내용들도 알아야 함! 알 걸로 기대되고! OS, 네트워크, 프로세스, 스레드, 언어에 업데이트되고 추가된 기능들.

### ChatGPT/copilot 나오고 나서
'프레임워크를 많이 써봤다' 보다 깊은 지식을 알고 있는 사람이 유리하다! 