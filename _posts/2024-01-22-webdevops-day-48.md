---
title: (Day	48) 네트워크와 Concurrent 프로그래밍
author: 김준회
date: 2024-01-22 18:00:00 +0900
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

[PDF 강의 내용](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8B%A4%EC%8A%B5%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B84.pdf)

2024년 1월 22일, 월요일
오늘은 교육센터에서 PC가 업그레이드와 함께 스토리지가 초기화되었다.
나는 내 소유의 랩탑들을 쓰고 있어서 환경을 재구성할 필요는 없었지만,
혹시라도 이 컴퓨터를 사용할수도 있는 다른 훈련생들을 위해, 연습겸 개발환경을 구성했다.
* SDK 기반으로 JDK 설정함 
  * 가장 범용성 높은 방식은 도커를 사용하는 방법인 것 같다.
  * JDK 설치 및 JAVA_HOME 환경변수 생성, Path에 java/bin 경로 추가
* Windows이므로 Git SCM 별도로 설치함
* Editor, IDE 설치
  * VSCode, IntelliJ, Eclipse
* 



# Review
## Stateful / Stateless
### 차이
하나의 요청을 처리한 후 바로 연결을 끊는가, 끊지 않고 유지하는가의 차이!

### Stateless 클라이언트의 구분방법
IP 기준으로 개별 ID 부여
### Stateless 한계와 극복
클라이언트 요청 처리 회전율을 높이기 위해 도입한 Stateless도,
하나의 요청이 아주 길다면 다른 클라이언트를 기다리게 할 수 있다.
그를 극복하기 위해 개별적인 실행흐름인 `쓰레드` 개념이 도입된다.
### Thread 구현하는 방법
2가지가 있다. 
* Thread 클래스를 상속하는 경우
* Runnable 인터페이스를 구현하는 경우
# Study
## Multi-Threading Programming
Concurrent Programming 이라고도 한다.


## CO/CL
Connection Oriented, 번역어로 사용하는 단어는 '연결지향'이다. Connection-less와 대비하여 '연결성'으로 번역하기도 한다.
대표적인 프로토콜: TCP

Connection-less, 번역하면 '비연결성'이다 .
대표적인 프로토콜: UDP

### DatagramSocket
ServerSocket 클래스는 TCP 연결을 지원하기 위한 클래스였다.
DatagramSocket은 UDP 연결을 지원하기 위한 클래스다.

CO에서는 서버가 대기하고 있는 상태가 아니라면 (Blocking method에서 읽기를 위해 대기)
서버가 연결을 거부하는 예외가 발생하여 데이터 송신이 불가능하다.

그런데 DatagramSocket을 사용하여 CL 방식으로 데이터그램을 전송하는 경우라면
서버와 연결이 되었는지를 확인하지 않고, 데이터그램 패킷(DatagramPacket)을 송신한다.

데이터그램 패킷은 바이트 배열을 포함하고 있다.
클라이언트는 보내는 경우라면, 클라이언트에는 데이터를 포함한 바이트 배열이 데이터그램 패킷 객체에 포함되어 있을 것이며,
서버는 받는 경우라면, 서버에는 빈 바이트배열이 데이터그램 패킷 객체에 포함되어 있을 것이다.


포트번호는 TCP와 마찬가지로 부여한다.
서버는 개발자가 포트번호를 부여하며, 클라이언트는 OS가 포트번호를 자동으로 할당한다.

(이 내용들은 개발자가 코드로 구현해줘야 한다. 코드 예시는 아래와 같다.)
서버측 코드는 아래와 같다.
```java
// connectionless 클라이언트 - 연결없이 데이터 수신
package com.eomcs.net.ex05;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.util.Scanner;

// 비연결(connectionless)
// => 연결없이 데이터를 송수신한다.
// => 상대편이 네트워크에 연결되어 있지 않다면, 그 데이터는 버려진다.
// => 그래서 전송 여부를 신뢰할 수 없다.
// => 실생활에서 "편지"와 같다.
// => 예) ping
// => DatagramSocket, DatagramPacket을 사용하여 처리한다.
public class Server0210 {
  public static void main(String[] args) throws Exception {
    Scanner keyScan = new Scanner(System.in);

    System.out.println("서버 실행 중...");

    System.out.println("소켓 생성 전 잠깐!>");
    keyScan.nextLine();

    // 데이터 송수신을 담당할 소켓을 먼저 준비한다.
    // => 보내는 쪽이나 받는 쪽이나 같은 소켓 클래스를 사용한다.
    //    서버 소켓이 따로 없다.
    // => 받는 쪽에서는 소켓을 생성할 때 포트번호를 설정한다.
    DatagramSocket socket = new DatagramSocket(8888);

    // 받은 데이터를 저장할 버퍼 준비
    byte[] buf = new byte[8196];

    // 빈 패킷 준비
    DatagramPacket emptyPacket = new DatagramPacket(buf, buf.length);

    System.out.println("데이터를 읽기 전에 잠깐 멈춤>"); 
    keyScan.nextLine(); // Blocking method


    // 빈 패킷을 사용하여 클라이언트가 보낸 데이터를 받는다.
    // => 데이터를 받을 때까지 리턴하지 않는다.
    socket.receive(emptyPacket); // Blocking method
    System.out.println("데이터를 받았음!");

    socket.close();
    keyScan.close();

    // 빈 패킷에 저장된 클라이언트가 보낸 데이터를 꺼낸다.
    // 패킷에 저장된 UTF-8로 인코딩된 바이트 배열을 가지고 String 객체(UTF-16)를 만든다.

    // 1) 패킷 객체에 보관된 바이트 배열을 꺼낸다.
    byte[] bytes = emptyPacket.getData();

    // getData()가 리턴한 배열은 DatagramPacket 을 만들 당시 넘겨준 배열이다.
    System.out.println(buf == bytes);

    // 2) 바이트 배열에 보관된 데이터의 개수를 알아낸다.
    int len = emptyPacket.getLength();

    // 3) 클라이언트에서 받은 바이트 배열을 가지고 String 객체를 생성한다.
    String message = new String(bytes, 0, len, "UTF-8");

    // 실무에서는 다음과 같이 로컬 변수를 사용하지 않고 직접 패킷 객체를 사용하는 방식으로 코딩한다.
    //    String message = new String(//
    //        emptyPacket.getData(), // ==> buf, 패킷에서 바이트 배열을 꺼낸다.
    //        0, // 버퍼에서 데이터를 꺼낼 때 0번째부터 꺼낸다.
    //        emptyPacket.getLength(), // 패킷에서 받은 바이트의 개수만큼 데이터를 꺼낸다.
    //        "UTF-8" // 바이트 배열로 인코딩된 문자표의 이름을 지정한다.
    //        );

    System.out.println(message);

  }
}
```
클라이언트측 코드는 아래와 같다.
```java
// connectionless 클라이언트 - 연결없이 데이터 송신
package com.eomcs.net.ex05;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

// Connectionless
// => 서버와 연결없이 데이터를 보내고 받을 수 있다.
// => DatagramSocket, DatagramPacket을 사용하여 처리한다.
// => 예) 편지, ping 등
// => 응용) 모니터링 프로그램에서 많이 사용한다.
//
public class Client0210 {
  public static void main(String[] args) throws Exception {
    // connectionless 방식으로 통신을 수행할 소켓 생성
    // - 클라이언트 쪽은 포트 번호를 지정하지 않는다.
    // - 물론 OS가 자동으로 부여할 것이다.
    DatagramSocket socket = new DatagramSocket();

    // 데이터를 받을 상대편 주소와 포트 번호
    // String receiver = "localhost";
    // int port = 8888;

    // 보낼 데이터를 바이트 배열로 준비
    // String message = new String("Hello"); // Heap에 String 객체 생성
    // String message = "Hello"; // constant pool에 String 객체 생성
    // byte[] bytes = message.getBytes("UTF-8");
    byte[] bytes = "Hello".getBytes("UTF-8");

    // 보낼 데이터를 패킷에 담는다.
    // => 패킷 = 데이터 + 데이터크기 + 받는이의 주소 + 받는이의 포트번호

    DatagramPacket packet = new DatagramPacket(bytes, // 데이터가 저장된 바이트 배열
        bytes.length, // 전송할 데이터 개수
        InetAddress.getByName("localhost"), // 데이터를 받을 상대편 주소
        8888 // 포트번호
    );

    // 데이터 전송
    socket.send(packet);
    System.out.println("데이터 전송 완료!");

    // 자원해제
    socket.close();

    // 상대편이 네트웍에 연결되었는지 따지지 않고 무조건 데이터를 보낸다.
    // 만약 상대편이 연결되어 있지 않다면, 보낸 데이터는 그 쪽 네트웍에서 버려진다.
    // => 데이터 송수신을 보장하지 않는다.
  }
}
```

### HTTP
HTTP1, 2는 TCP/IP로 동작한다. CO 방식을 쓴다는 것이다. Handshake로 연결을 먼저 생성한다.
HTTP3은 UDP에 기반한 CL방식을 사용한다. Handshake 과정 자체를 아예 뺴버려서 더 나은 반응성을 기대하는 것이다.
> 그 다음은 QUIC 프로토콜이 나온다. 지금 QUIC을 도입하는 사례가 많다.

HTTP(HyperText Trnasfer Protocol) 을 통해 여러 문서들을 요청하고 응답을 받아서 보다보면 거미줄처럼, 그물처럼 연결된 문서들 사이를 옮겨다니는 것 같아 보인다. 그래서 웹(Web)이라는 애칭이 붙었다. 그리고 그 웹을 돌아다니기 위한 소프트웨어는 Web Browser(웹 브라우저)라는 애칭이 붙었다. HTTP 요청에 응답하여 서비스를 제공하는 서버는, 즉 HTTP Server는 Web Server라는 애칭이 붙었다.
HTTP Server를 지원하기 위한 소프트웨어로, Apache HTTP Server, IIS(MS), NginX 같은 제품들이 있다.
HTTP Client를 지원하기 위한 소프트웨어로는 GUI 브라우저(Chrome, Edge, Firefox, Safari, Opera, Brave, Whale 등)이 있고, CLI를 지원하기 위한 wget, curl 같은 소프트웨어도 있다.

프로토콜을 지원하는 서버에 따라 이름도 다르게 붙는다. FTP를 지원하는 서버는 FTP 서버로 불리며, FTP로 서버를 이용하는 경우는 FTP Client라고 부른다. SMTP를 사용하는 경우는 SMTP Server, SMTP Client라는 용어를 쓴다. HTTP를 사용하는 경우는 HTTP Client, HTTP Server라는 용어를 쓰는데, (Web) Client, Web Server라는 용어가 더 자주 쓰인다.

## HTTPS
End-to-end encryption (번역하자면, 종단 간 암호화)를 HTTP에 적용한 것이다. S는 Security.
HTTPS는 보다 `안전한 사이트`라고 하는데, 뭐가 안전한걸까?
HTTP를 통해 클라와 서버가 [요청/응답]을 하는데, 해커가 이 패킷들을 빼돌려서 본다고 생각해보자. 그러면 그 내용을 조작해서 서버에 사기를 친다거나, 서버가 클라에게만 주려고 했던 민감한 내용을 해커가 볼 수 있을 것이다. 그래서 패킷을 빼돌려서 보더라도 무슨 내용인지 알 수 없게 암호화한 게 HTTPS다.

암호화한 걸 정상적인 클라&서버는 어떻게 해독할까? 해독할 수 있는 비밀번호인 `private key(개인키)`가 있기 때문에 가능하다. 개인키는 클라이언트만 가지고 있다. `public key (공개키)`는 키라는 단어가 들어가서 헷갈릴 수 있는데, 공개키는 실제로 생각해야 하는 이미지는 키라기보다는 자물쇠에 가깝다. 암호화를 위해서 사용하는 키가 공개키 이기 때문이다.

`클라이언트`, `서버 & 인증서버` 로 이루어진다. 
인증서버는 B의 `공개키`를 제공한다. 
1. 클라이언트는 공개키를 인증서버로부터 받고,
2. 자신의 개인키로  


### 암호에 대한 인식
* 어떤 알고리즘을 가지고 암호를 만들건간에 **언젠가는** 암호가 풀린다.
* 악의적 목적을 가진 사람들이 풀어내기 얼마나 어려운지가 중요한 것이다. (시간과 자원)
* 반대로 정상적인 경우에는 푸는데 리소스를 적게 쓰는 것이 중요한 것이다.
* 패킷의 탈취 또한 완전한 내부망이 아니라면 막아내기 힘들다.
  * 많은 라우터를 경유해서 패킷들이 전송되기 때문이다.
    * 패킷 탈취를 위한 라우터를 다른 라우터들과 구분할 방법이 없다.
    * 전류/전압을 모니터링하여 임의 라우터가 추가되는 것을 막는 방법도 있지만, 해당 라우터가 손실된 전력을 보충해줄 수 있는 발전된 형태라면?

## URI (Uniform Resource Identifier)
* URI: Idnetifier
  * URL: Locator
  * URN: Name
URI라고 하면 URN, URL 둘 다를 말한다. URI의 하위개념이 URN, URL 이기 때문이다.
그리고 현재 대부분은 URL 방식을 사용한다. 
이 방식들은 웹상에서 자원의 위치를 표현하는 방법들이다.

* 80 또는 443 포트번호는 생략할 수 있다. 

대략적인 구조는 이렇다.
![](https://www.hostinger.in/tutorials/wp-content/uploads/sites/2/2022/07/the-structure-of-a-url.png)

### Dynamic vs Static
Static, Dynamic: 요청시마다 응답해줘야 하는 컨텐츠가 변경되는지로 구분된다.


## BASE64
`BASE64`는 이진 데이터를 문자화하는 방법이다. BASE64가 이진 데이터를 문자화시키는 방법은 이렇다.

 6비트 (경우의 수가 64가지) 로 잘라서 A~Z, a~z, 0~9, +, = 
즉 26 + 26 + 10 + 1 + 1 가지의 문자에 대응시킨다.
6비트 단위로 자르고 각각의 경우에 따라 문자를 배정한 것이므로 모든 이진데이터를 표현할 수 있다. 끝 비트들이 6개보다 모자란 경우 모자란 수 만큼 `=` 를 끝에 채워 넣어 해당 정보를 표현해준다.

영어 대소문자와 `+`, `=` 으로 끝나는 문자열들을 주소창에서 많이 보았을 것이다. 그것은 이진 데이터다.

### 왜 이진데이터를 문자화시키는가?
URL은 이진 데이터가 아니라 Charset을 기준으로 만들어지기 때문이다. 바이너리 데이터를 URL에서 다루고 싶기 때문에 BASE64 같은 방법이 생긴 것이다. URL에서 이미지같은 바이너리 데이터를 그대로 문자로 줘 버리고 싶을 때... 그럴 때 사용된다. 그렇게 사용하는 사람들이 만든 서비스를 이용하기 위해서도 알아야 하고.

예시를 보고 싶다면, 구글에서 이미지 검색을 하고, 나오는 작은 썸네일에 우클릭을 하여 이미지 주소를 확인해보자 그러면 이런 결과를 볼 수 있다.
```
data:image/png;base64,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
```

### BASE64 변환
변환은 아래 테이블을 이용하면 된다.
![BASE64](https://media.geeksforgeeks.org/wp-content/uploads/20200520142906/1461.png)

참고로, BASE64는 바이너리를 공개된 테이블에 따라 텍스트 데이터로 변환하는 방법이다. 암호화하고는 관계가 없다.

그리고 남용하는 것도 좋지 않다. 모바일 기기에서 디코딩시 전력을 사용하게 될 것이기 때문이다. 바이너리를 한번에 받을 수 있는데도 BASE64가 있는 이유는 바이너리를 문자로 변환해서 URL에 포함해야 할 어떤 이유가 있을 때다.

## 동시성 처리 (Concurrency)
### 선형(Linear) 처리 VS 병행(Parallel) 처리
### cf: Concurrency VS Parallelism
![](https://techdifferences.com/wp-content/uploads/2017/12/Untitled.jpg)
https://techdifferences.com/difference-between-concurrency-and-parallelism.html

### 병행처리
하나의 작업을 동시에 여러개 실행하는 것을 말한다.
그래서, 그냥 '동시 실행' 이라고 생각해도 다를 게 없다...

* CPU가 여러 쓰레드를 지원하는 경우는 자원을 더 효율적으로 쓸 수 있으므로 같은 시간 안에 더 많은 처리를 할 수 있다. 
* 각 작업이 **대기해야 하는 시간**도 줄어든다.
* 동시성 이슈가 있다.
  * 작업들간에 지켜야 할 순서가 없는 경우,
  * 서로 간섭하지 않는 작업들인 경우에 적절하다.

```C
// 프로세스 복제하여 병행처리하는 예시
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
  int i = 0;
  pid_t processId = 0;
  
  processId = fork(); // 현재 실행 실행 위치에서 프로세스 복제
  
  for (i = 0; i < 10000; i++) {
    printf("[%d] ==> %d\n", processId, i);
    int temp = rand() * rand();
  }
  
  return 0;
}
```
### 병행처리
* 작업 순서에 상관이 없는 여러 작업


### 프레임 동시 처리
머신의 파워에 따라서 게임의 결과가 달라지면 안된다.
FPS가 잘나온다고 더 좋은 성능으로 움직이면 안된다는 것이다.
게임엔진에서의 핵심기술이다. [이 사례는](https://www.inven.co.kr/board/black/3584/52570) 그 문제가 현실로 나온 일일까..?



