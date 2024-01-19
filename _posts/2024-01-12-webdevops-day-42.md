---
title: (Day	42) C/S App으로 진화! RMI란?
author: 김준회
date: 2024-01-12 23:00:00 +0900
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
## DAO
### 왜 썼니? (무슨 역할(목적)?)
이전에 핸들러는 UI처리 (콘솔 입출력) 및 데이터 처리 코드가 한 클래스 안에 있었다.
그러면 유지보수가 너무너무너무 힘들어진다. (=불가능해진다.)
UI처리를 콘솔 입출력에서 GUI로, 혹은 웹 페이지로 변경한다면 그 때마다 데이터 처리 코드도 수정이 되어야 하는 것이다. 반대로 데이터 처리 코드도 ArrayList 에서 Map을 쓴다거나 하는 식으로 변경하면 UI처리 코드도 변경이 되어야 한다.
그래서 분리한다. 분리해서 서로 영향을 주지 못하게 `캡슐화`한다. 
한 쪽의 코드를 수정해도 다른 코드들은 영향을 받지 않거나 최소한만 받게 한다.
핸들러가 데이터를 직접 접근(처리)하지 못하게 DAO로 그 부분을 감춘 것이다.
> DAO는 데이터 처리 코드를 캡슐화하는 것이 목적이다.
> 캡슐화의 목적은 유지보수의 가능화다.

### 구현하려면?
다루는 vo 별로 클래스를 만들어주어야 한다. (데이터 처리 방법은 다르므로 이걸 범용화할 수는 없다...)
추상클래스를 통해 어떤 vo를 다루는지에 대한 부분을 처리하기 좋다. Generalization(일반화)이 적용된 추상 클래스를 만들고, 인터페이스를 하나 더 정의한다. 인터페이스는 진짜 말 그대로 인터페이스를 제공하는 것이 목적이다. 어떤 사용방법을 써야 하는지 인터페이스에서 정의를 해주는 것이다.

`인터페이스타입 레퍼런스명 = new 구상클래스생성자()`
여기서 구상클래스는 관례상 OOOImpl 형태로 명명한다.

### 인터페이스, 추상클래스를 사용하는 목적
인터페이스 => 하나의 핸들러에서 인터페이스 규칙에 따라 동작하는 클래스를 쓰겠다고 지정하여, 핸들러를 변경하지 않고도 DAO를 변경할 수 있다. (어떤 DAO를 쓸 것인지를 선언하는 부분의 코드는 당연히 변경해야 한다.)

추상클래스 => 유사한 인터페이스 구현체들의 메서드 및 필드를 상속을 통해 일반화시켜 중복 코드를 제거할 수 있다.

# STUDY
## Standalone App ➡️ Clinet/Server App
Standalone App 에서 C/S App 으로 변경하려면 App을 쪼개야 한다.
Client App, Server App 두 개로 쪼개야 한다. 여기서 좀 더 발전시키면 Common App 같이 한번 더 쪼갤 수도 있다. 

### IDE 를 위해 쪼개는 방법을 알아야 한다.
`IntelliJ` 는  편하고,  `Made with 🩷` 한 프로그램이다 (ㅋㅋ)
인텔리제이는 빌드 자동화 도구 `gradle`을 기반으로 동작하는데, 프로그램을 쪼개는 경우에도 gradle 설정을 맞춰줘야 한다.

이를 위해서는 이전에 학습한 `settings.gradle`, `build.gradle` 두 빌드 스크립트 파일에 대해서 이해하고 있어야 한다.

프로젝트의 루트 경로(최상단경로)에는 `settings.gradle` 빌드 스크립트 파일이 있다.
gradle은 이 파일을 가장 먼저 참조하여, 각 경로에 있는 `build.gradle` 을 참조하게 된다.
`gradle init`할 때 파일의 내용이 자동으로 생성되는데, 어떤 폴더들을 프로젝트로 포함할지는 `include()` 명령어를 통해 알려줘야 한다.
 
위 내용에 따라 프로젝트 구조를 변경하기 위해 실제로 설정해야 하는 부분은 아래 코드를 참조하자.
```groovy
...
//include('app') //app 경로 하위의 build.gradle 파일을 참조하라!  

rootProject.name = 'myapp'  
//위 rootProject 하위의 include에 포함되지 않은 경로들의 gradle settings들은 전부 무시된다.  
  
include('app-client')  
include('app-server')  
//include('app-common')
...
```

이렇게 포함할 경로들을 알려주고, 인텔리제이에서 gradle을 리로드 해주자. (우측 코끼리 아이콘 눌러서 사이드탭을 열고 Reload 버튼을 눌러주거나, Settings.gradle 파일로 들어가면 에디터 탭의 우측상단에 리로드를 위한 코끼리 아이콘의 버튼이 있는데 그걸 클릭해도 된다. **Gradle Reload를 수행하면 좌측 Explorer 사이드탭에서 Project Files로 보면 깔끔하게 정리되어 보이게 된다.** (이 문장을 쓰면서 느끼는건데, 그냥 기술분야는 영어로 작성하는게 더 낫겠다...)

## 초기 C/S App의 재현
클라이언트앱이 요청을 하면, 서버 앱은 응답한다.
요청 규칙은 아래와 같이 정의한다. 이 요청 규칙은 protocol 이라고 한다.

요청규칙
1. 데이터명
2. 명령(CRUD.. create, read, update, delete)
3. 데이터 (Entity 라고 부른다.) -> json 포맷을 쓴다.

1, 2, 3은 개행문자로 구분된다. 예시는 이렇다.
```
board
findAll

```

```
board
add
{"title":"this is title", "contents":"Hello Network!"}
```

### 서버 앱에서 하는 일
서버 앱은 뭉뚱그려 말하면 이런 일들을 한다.
1) 네트워크 장비와 연결하기 위한 데이터를 준비한다. (ServerSocket 클래스)
2) 클라이언트의 연결을 기다림  
3) 클라이언트와 통신함

* ServerSocket 클래스의 생성자는 portNumber를 파라미터로 받는다. 포트번호. 공유기나 NAS 쓸 때 많이 봤던 단어다. 포트번호는 랜카드가 데이터를 수신할 때 사용할 `식별 번호`이다.
* 클라이언트 대기 목록에서 먼저 연결된 순서대로 클라이언트의 연결 정보를 꺼낸다. (바로 큐가 생각난다. 선착순 처리.) 
* 클라이언트 대기 목록에 아무것도 없다면 연결이 될 때까지 기다린다. (리턴하지 않고 기다린다.)


위 내용을 코드로 작성하면,
```java
try {  
 // 1) 네크워크 연결을 위해서, 랜카드의 연결 정보를 객체로 만들어서 준비한다.  
 // => 랜카드를 통해 네트워크와 연결되는 작업을 수행하는 것은 OS이다. 
 // => JVM은 OS가 작업하여 연결된 결과를 가져오는 것이다. 
 // 이는 new Server(portNumber) 형태인데, 포트 번호는 랜카드를 통해 데이터를 들여올 때 고유한 번호이다.  
  ServerSocket serverSocket = new ServerSocket(8888);  
  System.out.println("서버 실행 중...");  
  
  System.out.println("클라이언트 연결을 기다리는 중");  
  serverSocket.accept();  
  System.out.println("대기목록에서 클라이언트 연결정보를 꺼냈음!");  
  
} catch (Exception e) {  
  e.printStackTrace();  
  System.out.println("통신 예외발생 ");  
}
```
소켓 인스턴스 안에는 상대편(클라이언트)의 IP주소와 포트번호가 필드로 들어가 있다.

### 클라이언트 쪽에서는 무엇을 해야 하나
```java
try {  
 // 1) 서버와 연결한 후 연결 정보를 준비한다.  
 // 연결 정보는 new Socket(서버주소, 포트번호) 
 // - 서버주소: IP Address / Domain Name 둘 다 가능 
 // - 포트번호: 연결할 포트 번호 
 // => 로컬 컴퓨터를 가리키는 주소가 있다. 이는 IP Address: 127.0.0.1 또는 도메인으로 localhost  
  System.out.println("서버 연결 중...");  
  Socket socket = new Socket("localhost", 8888);  
  System.out.println("서버에 연결 됨!");  
} catch (Exception e) {  
  e.printStackTrace();  
  System.out.println("통신 예외 발생");  
}
```
localhost에서 돌아가지만 처음으로 클라이언트-서버 앱을 만들어 본 것이다!
여기서 클라이언트의 연결을 위한 큐를 직접 만들었나? 아니.. 그런 적이 없다! 제공되는 클래스가 처리한다. 

### 순서 파악하기
1. 서버 앱이 `ServerSocket` 객체를 생성한다.
2. 서버 앱은 `.accept()` 메서드를 통해 대기한다.
3. 클라이언트 앱이  `Socket` 객체를 생성한다.
4. `Socket` 객체는 랜카드의 정보(포트)와 상대편의 연결정보(IP Address/Domain Name)를 가진다.
5. 객체 생성시 연결을 시도한다.

랜카드도 dsecriptorID로 (식별자ID)로 OS에게 구분된다.

### 소켓 = 상대편의 연결 정보
소켓은 상대편의 (다른 컴퓨터의) IP주소 및 포트번호다.
InputStream, OutputStream을 소켓에서 메서드로 get 할 수가 있다.
스트림에는 데코레이터 패턴이 적용되어있므로 조금 더 편하게 입출력을 할 수가 있고.

### 소켓 입출력
소켓을 통해서 입출력을 한다는 것은? InputStream 및 OutputStream 클래스를 다룬다는 것. 물론 저 클래스를 그대로 쓰면 바이트 단위로만 작업을 할 수 있다. 1바이트 이상의 byte 혹은 int, long, float, long 같은 정보를 제대로 읽어오려면 처리를 해줘야 한다. 그래서 그 처리도 공부했는데, 당연히 JDK를 개발하는 사람들도 바보 병신이 아니다. (신에 가깝지) 기존에 적용해봤던 것처럼 데코레이터 패턴이 적용되어 있는 API 이므로, DataInputStream 및 DataOutputStream의 생성자로 전달하여 편하게 쓸 수 있다. 버퍼도 당연 적용 가능하고.

## 이런 일들은 어떻게 일어나는가?
JVM은 OS에게 요청한다. OS는 랜카드를 제어한다.
**랜카드에도  RAM 이 있다.** 이 RAM에 저장해달라는 요청을 write로 하는 것이고 읽어달라는 것을 read로 하는 것이다. read는 랜카드 RAM에 저장된 내용이 없으면 block되고 기다린다. 그러나 write는 바로 쓰고 return 해 버린다. 읽었는지 읽지 않았는지는 관심사가 아니다.

### 주의점
write()는 block되지 않는다? 위 내용을 다시 작성한 것이다.
 소켓 클래스 -> I/O Stream 통해서 write() 하는 경우 (writeUTF() 건 뭐건 간에 쓰는 경우) 상대편이 다 읽을 때까지 block 하지 않고 (기다리지 않고) 바로 리턴한다. 받는 쪽에서 읽었는지 읽지 않았는지 관심이 없다. write() 메서드는 랜카드의 RAM에 값을 쓰고 바로 리턴한다

## 네트워크 DAO 적용하기
인터페이스를 구현하고, 추상클래스를 상속하는 구조의 DAO를 설계하고 적용했었다. 그러면 이 설계의 이점을 누려보자.
ServerApp과 Read, Write할 때도 인터페이스 사용방법을 그대로 쓰면서 재활용해보자.

### GSON 은 Thread-Safe가 적용되었다.

### HTTP 응답 프로토콜
https://developer.mozilla.org/ko/docs/Web/HTTP/Status

### 인터페이스의 장점
```java
boardDao = new BoardDaoImpl("board", in, out);  
greetingDao = new BoardDaoImpl("greeting", in, out);  
assignmentDao = new AssignmentDaoImpl("assignment", in, out);  
memberDao = new MemberDaoImpl("member", in, out);
```
요렇게, boardDao 는 BoardDao (인터페이스) 타입이라 바꿔 끼우기면 하면 사용법은 그대로다. DAO를 바꾸더라도 나머지 코드(핸들러)는 안바꿔도 된다는 것이다.

## 오늘 배운 건 프레임워크를 만드는 법을 배운 거다.
서버는 Reflection API 이용해서, 클래스의 메서드 이름을 몰라도 처리할 수 있다.
