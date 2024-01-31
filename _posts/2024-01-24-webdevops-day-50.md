---
title: (Day	50) Thread, Mutex와 Semaphore
author: 김준회
date: 2024-01-24 17:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---
[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8B%A4%EC%8A%B5%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B84.pdf)
# Review
클라우드에 리눅스 서버 생성하기
* VPC (Virtual Private Cloud) 설정하기
  * ACL 설정하기 (Access Control List)
  * Subnet mask 설정하기 (VPC 안에서 사용할 비트)
* 서버 생성하기
  * Server ACG 설정하기 (Access Control Group)
  * 커널 업데이트를 지원하지 않으므로 생성시 주의
* SSH (Secure Shell) 접속하기
  * 맥이나 리눅스: `OpenSSH`가 기본 설치되어 있음
  * 윈도우: 전에는 `Putty`를 SSH 클라이언트로 사용했는데, 윈도우 터미널에서 OpenSSH를 제공하기 시작함.
    * 터미널은 윈도우11부터는 기본 설치되어 있음 (없는 경우는 MS Store에서 설치하면 됨)

### VPC
### ACG
### Network ACL
### Subnet
### SSH

## 병행처리
### Fork VS Thread
fork는 프로세스의 힙메모리까지 복사하기 때문에 메모리 낭비가 크다.
### JVM Thread Structure
JVM이 프로세스이며,
main 스레드가 main() 호출한다.

### Thread, Runnable


### Thread, Thread Group

### Virtual Thread(JDK 21 ~)

### Context Switching




# TIL
## Linux

### 기호
터미널에서 유저 권한과 현재 경로 바로 알 수 있다.
```bash
student@bitcampncp-server1:~$ ls
```
`유저이름` `@` `서버이름` `:` `경로` `권한` `CLI 명령어 입력란`

이런 모양을 보여준다.


경로에 대한 기호는,
* 홈 경로에 있는 경우는 `~` 기호로 줄여 표현한다.
* 이외의 경우는 `/`으로 루트를 표현한다.


권한은 아래 기호 차이로 알 수 있다.
* 일반 사용자인 경우, `$` 을 `Prompt 기호`로 사용한다.
* 관리자(루트)인 경우, `#` 을 `Prompt 기호`로 사용한다.
### sudo
`sudo`는 super user do를 줄인 말이다. 슈퍼유저(관리자)로서 행하겠다고 알려주는 것이다. 프롬프트 기호가 #으로 시작하는 경우라면 붙일 필요가 없다.
'수도'라고 발음하기보단, '슈두' 정도로 읽어주는 게 적절하다. super user do 라서 그렇다.

### 계정 추가 및 권한 설정
관리자 권한에서,
```bash
adduser 유저이름 #유저 추가하기 (홈폴더 자동 생성)
...
usermod -aG sudo studnet #관리자 목록에 추가하기
```

## IP 주소할당 방법
### CIDR (Classless Inter-Domain Routing)
`주소/프리픽스 길이` 형식으로 표현한다. IPv4 주소가 고갈 예정이었던 1990년대, #TODO 사이더 블록


## Thread
### Thread Life Cycle

![](https://static.javatpoint.com/core/images/life-cycle-of-a-thread.png)
[출처](https://www.javatpoint.com/life-cycle-of-a-thread)

스레드는 Running, Not Runnable 두 가지 상태가 존재한다.
* Running: CPU 자원을 사용할 수 있다!
* Not Runnable: CPU 자원을 사용할 수 없다!

한번 죽은 스레드는, 즉 run() 메서드가 종료된 스레드는 다시 running 상태로 돌아갈 수 없다. (다시 run() 메서드를 실행하는 것이 불가하다!) 이 경우 스레드 객체는 예외를 반환하는데, `Java.lang.IllegalThreadStateException` 예외를 반환한다.

### Race Condition
`Race Condition`는 `경쟁 상태`로 번역된다.
1. 공유 자원(CPU 등)에 대해 여러 포르세스가 동시에 접근을 시도할 때
2. 순서, 간섭이 가능할 때

두 조건이 모두 만족될 때 race condition이 성립한다.

## Critical Section(=Critical Region, 임계 영역)

아래 코드에서 `add()`가 임계 영역이다.
여러 스레드가 같은 변수의 값을 변경할 때, 기존 값을 덮어 쓸 수 있는 문제가 발생할 수 있는 코드를 임계 영역이라고 한다.

Critical Section 혹은 Critical Resion 이 있는 코드는 Thread-unsafe 하다.

```java
// 멀티 스레딩(비동기 프로그래밍)의 문제점 - 사례 1 
package com.eomcs.concurrent.ex5;

public class Exam0110 {

  static class MyList {
    int[] values = new int[100];
    int size;

    public void add(int value) {
      // 여러 스레드가 동시에 이 메서드에 진입하면 
      // 배열의 값을 덮어쓰는 문제가 발생한다.
      // 이렇게 여러 스레드가 동시에 접근했을 때 문제가 발생하는 코드 부분을
      // "Critical Section" 또는 "Critical Region" 이라 부른다.
      if (size >= values.length) {
        delay();
        return;
      }
      delay();
      values[size] = value;
      delay();
      size = size + 1;
      delay();
    }

    public void print() {
      for (int i = 0; i < size; i++) {
        System.out.printf("%d:  %d\n", i, values[i]);
      }
    }

    public void delay() {
      int count = (int)(Math.random() * 1000);
      for (int i = 0; i < count; i++) {
        Math.atan(34.1234);
      }
    }
  }

  static class Worker extends Thread {
    MyList list;
    int value;

    public Worker(MyList list, int value) {
      this.list = list;
      this.value =  value;
    }

    @Override
    public void run() {
      for (int i = 0; i < 20; i++) {
        list.add(value);
      }
    }
  }

  public static void main(String[] args) throws Exception {
    MyList list = new MyList();

    Worker w1 = new Worker(list, 111);
    Worker w2 = new Worker(list, 222);
    Worker w3 = new Worker(list, 333);

    w1.start();
    w2.start();
    w3.start();

    Thread.sleep(10000);

    list.print();
  }

}
```

위 코드에서 100개의 int 배열에 size 를 인덱스로 하여 3개의 스레드가 race condtition으로 값을 할당한다.

그런데 `size = size + 1;` 코드가 뒤에 있어서, 만약 이 라인이 실행되기 전에 다른 스레드에게 CPU를 뺏기는 경우 size가 증가하기 전인, 같은 인덱스에 덮어쓰는 문제가 생긴다.

이렇게 여러 스레드가 같은 변수의 값을 변경하는 경우 문제가 생길 수 있다.

* Thread-unsafe 는 값을 쓰는 코드에서만 발생한다.
* 값을 조회만 하는 경우는 Thread-safe 하다.

## Thread-Safe(Semaphore, Mutex)
스레드에 안전한 코드로 변경하기 위해서는
* 경쟁 상태에서 덮어쓰는 일이 없도록 만드는 방법
* 임계 영역에는 한번에 하나의 스레드만 진입가능하도록 제한하는 방법이 있다. 
---
* Semaphore
  * `Semaphore N`` 이라고 하면, 동시에 들어올 수 있는 스레드가 N개 라는 의미다.
  * 자바에서는 오로지 Semaphore 1 만을 지원한다. 1이 아닌 다른 Semaphore N을 구현하는 방법이 없다. 
* Mutex(Mutually )
  * Semaphore 1 의 다른 이름이 Mutual Exclusion이다. 줄여 말해서 Mutex.
---
자바에서 Semaphore 1 을 메서드에 구현하는 방법은 간단하다.
`synchronized` 키워드를 modifier 앞에 붙여주면 된다.

위 임계영역 예제의 add()에 적용하면 다음과 같다.
```java
synchronized public void add(int value) {
      if (size >= values.length) {
        delay();
        return;
      }
      values[size] = value;
      size = size + 1;
    }
```

### P(Wait)연산과 V(Signal)연산
정수 변수 하나를 스레드 진입과 종료를 구별하기 위해 쓴다고 생각하면 된다. 세마포어에 스레드가 나갈 때 P연산으로 세마포어 값을 줄이고, 스레드가 나갈 때 V연산하여 세마포어 값을 증가시킨다.


### Dead lock
간단하게 말하면, synchronized 임계영역에서 wait하는 코드들이 있다면 위험하다! 길게 말하면...


### Mutex 구현을 위한 synchronized 쓴다면
병행처리의 장점이 없어진다....! 순차 처리랑 다를 게 없어진다. 
그런데 인스턴스와 결합하면 이야기가 달라진다.
인스턴스 메서드가 synchronized 된다는 것은 아래와 같은 의미가 된다.
* 같은 변수에 대해서는 여러 스레드가 동시에 진입하는 것을 막지만
* 다른 인스턴스라면 여러 스레드가 동시에 진입하는 걸 막지 않는다. 
(병행 처리를 하게 된다.)

```java
// synchronized - 인스턴스 메서드에 적용
package com.eomcs.concurrent.ex5;

public class Exam0610 {
  public static void main(String... args) {
    Job job1 = new Job();
    Job job2 = new Job();
    Job job3 = new Job();

    Worker1 w1 = new Worker1("HongGilDong", job1);
    Worker2 w2 = new Worker2("ImGgeokJeong", job2);
    Worker3 w3 = new Worker3("GimGoo", job3);

    w1.start();
    w2.start();
    w3.start();
  }


  static class Worker1 extends Thread {
    Job job;

    public Worker1(String name, Job job) {
      super(name);
      this.job = job;
    }

    @Override
    public void run() {
      try {
        for (int i = 0; i < 10; i++) {
          job.play(getName());
        }
      } catch (Exception e) {
        e.printStackTrace();
      }
    }
  }

  static class Worker2 extends Thread {
    Job job;

    public Worker2(String name, Job job) {
      super(name);
      this.job = job;
    }

    @Override
    public void run() {
      try {
        for (int i = 0; i < 10; i++) {
          job.play(getName());
        }
      } catch (Exception e) {
        e.printStackTrace();
      }
    }
  }

  static class Worker3 extends Thread {
    Job job;

    public Worker3(String name, Job job) {
      super(name);
      this.job = job;
    }

    @Override
    public void run() {
      try {
        for (int i = 0; i < 10; i++) {
          job.play(getName());
        }
      } catch (Exception e) {
        e.printStackTrace();
      }
    }
  }

  static class Job {
    synchronized void play(String name) throws Exception {
      System.out.println(name);
      Thread.sleep(200);
    }
  }
}
```

여러 Worker(스레드)가 Job을 처리하려고 한다면,
* synchronized job이라면 
  * 동일 인스턴스에 대해서
    * 하나의 worker만이 접근가능하다.
    * 나머지는 대기해야 한다.
  * 다른 인스턴스에 대해서
    * 1개의 인스턴스에 1개의 스레드만 접근 가능하다.
    * 다른 인스턴스에 대한 것이라면 대기할 필요가 없다.

다른 synchronized 메서드에 대해서도 막는다.

# Others
## 인터넷 강의의 특징
수동적으로 보게 됨. 집중하기 힘듦.
반복시에 매번 같은 컨텐츠가 제공됨.
그래서 반복해서 보기에는 좋음.
낯선 경우는 더 반복을 해야 함.

## N-Time
매직에꼴 백엔드 개발자 김양수님
* 이전 기수 수료하신 분
  * 죽기살기로 임해야 취업이 가능할 것
* 캠프에서 6개월간 조언
  * 기술블로그  
    * 선택이 아닌 필수
    * 이슈사항 해결 등 **고민한 흔적/해결과정**이 보이는 글이 중요함
* 기술 스택
  * 선택과 집중 필요
* 클라우드 역량 쌓기
  * 도커, 쿠버네티스, 리눅스 등...
  * 진짜 필요하다. 배울 수 있을 때 배워둬라.
  * 단순 코딩은 GPT도 잘 한다.
* 캠프에서 제공하는 컨텐츠 적극 활용
  * N타임
  * 모의면접
    * 원래 프로젝트 시점에 같이 했는데,
    * 프로젝트에 수강생들이 너무 집중해서 후반부로 분리함!
  * CI/CD 강의
    * Docker, Linux, etc..
* 개발스터디
  * 각자 발표할 파트를 정하고 10분 정도
  * 이해시키기 위한 공부는 아주 선명하게 기억에 남는다.
* 수료후
  * **완벽한 준비라는 건 애초에 없다!**
    * 자소서, 면접 준비
    * 노션, 깃으로 포트폴리오 작성
    * KDT로만 해도 웹개발은 **한달에 1000명** 정도가  공급된다.
  * 클라우드 서비스 관리 역량
    * NCA, NCP, NCE
  * 기존 강점과 개발역량을 융합 (비전공)
    * 단점을 장점으로
    * 기술 블로그에 추가 카테고리를 가질 것
      * ex. 금융, 수학 등등... 원하는 분야로
  * CS
    * 비전공이면 오히려 더 많은 질문이 들어올 것
    * 문제 해결시 필요하니 꾸준히 공부해나가야 할 내용!
  * 포트폴리오 사이트 만들기
    * HTML/CSS/JS 배우고 나면 만들기!
    * PDF/GIT Repo로 만들지 말고, 웹페이지로 만들자!
      * 템플릿 이용하는 경우도 있고
      * 직접 만드는 경우도 있고
* 동기들하고 연락하기
* 아쉬웠던 점
  * 단순하게 여러 기능을 구현하기보다 하나를 구현하더라도 완벽하게 
  (보안, 성능, 유지보수 용이성)
    * 대부분 CRUD 의 반복이다보니..
    * API 이것저것 붙이는 것보다 보안, 성능, 유지보수 고려해보기
    * 예시
      * Login Flow
        * 단순 구현
          * DB에 사용자 개인정보 그대로 저장
          * 사용자가 입력한 데이터를 DB 데이터와 비교
        * 깊게 생각하기
          * 암호화하여 해시값만 DB 저장
          * 모든 데이터 통신은 SSL 통해 보안
          * Spring Security, JWT 방식을 이용해 사용자 인증 및 인가 절차