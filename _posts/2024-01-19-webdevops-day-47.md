---
title: (Day	47) Stateless의 단점, 그리고 Thread의 도입
author: 김준회
date: 2024-01-19 18:00:00 +0900
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
## I/O Stream
* 스트림: 데이터가 단방향으로 흘러가는 (전송되는) 것
* ByteStream VS CharacterStream
	* 둘 다 스트림이라는 것은 공통점
	* 바이트단위로만 읽고 쓰거나, 자료형에 맞춰서 읽고 쓰거나의 차이
	* 즉 입출력의 단위가 핵심적인 차이
	* Reader/Writer는 `버퍼`를 내장함 
		* 그래서 `flush()`를 해야 버퍼의 내용을 내보낼 수 있다.
	* ByteStream
		* InputStream/OutputStream
			* FileInputStream/FIleOutputStream
			* ByteArrayInputStream/ByteArrayOutputStream
			* PipedInputStream/PipedOutputStream
				* stdin/stdout을 사용한다. 
				* 프로세스들끼리 스트림 연결이 가능하다.
	* CharacterStream
		* Reader/Writer (abstract class)
			* FileReader/FileWriter
			* CharArrayReader/CharArrayWriter
			* StringReader/StringWriter

## String
### getBytes() 메서드의 동작방식
### byte[] 데이터로 String Instance를 생성할 수 있는가?
* 문자 집합을 다룰 수 있어야 한다.
* JVM이 내부적으로는 항상 UCS2 를 사용한다는 것을 안다.
* `file.encoding` 환경변수의 의미를 알아야 한다.
* OS별 기본 사용하는 문자집합은? 
	* Win:CP949(MS949)
	* MacOS/Linux: UTF-8
### 상속의 한계점
요구되는 경우의 수가 많을 때, 각각의 클래스를 상속을 통해서 만드는 경우
* 클래스 수가 경우의 수만큼 늘어난다.
* 각 클래스의 유사 코드들이 중복된다.

# Study
## I/O
### Concrete Component VS Decorator
데이터 싱크 스트림 클래스란? (Data Sink Stream Class)
* 데이터가 어딘가에 있는 경우
* 파일, 메모리, 프로세스 등에 가라앉아(sink) 있는 경우
* 단독적으로 쓸 수 있는 클래스다.
* Concrete Component 클래스에 해당한다.

데이터 프로세싱 스트림 클래스란? (Data Processing Stream)
* 데코레이터로 사용된다.
* 단독적으로 사용할 수 없다.
	* PrintStream은 예외다.

### Decorator 구분하기
* Decorator 필요조건: 생성자에서 파라미터로 완제품 인스턴스를 받아야 한다.
* 단독적으로 사용할 수 있는 경우는 (완제품인 경우는) 데코레이터가 아니다.
	* 다만 PrintStream은 단독적으로도 쓸 수 있고, 데코레이터로도 쓸 수 있다.
![Design Patterns - Decorator Pattern](https://www.tutorialspoint.com/design_pattern/images/decorator_pattern_uml_diagram.jpg)

## Stateful/Stateless
### 기본 개념
존대로 작성해볼까요..?
* 연결한 후 연결을 끊기까지 계속 요청/응답 수행해요
* 클라이언트가 요청하지 않을 때도 계속 연결된 상태를 유지해요
	* 이런 망할! 뒤에 손님이 기다리고 있잖아!
	* 손님들이 매장에 쌓이니 매장 공간도 부족해지잖아!
* 그래서 클라이언트 회전율이 떨어지고, 접속가능한 클라이언트의 수가 제한돼요.
* 그렇지만 클라이언트 요청에 대한 작업결과를 서버에 유지할 수 있어요.


### Stateful/Single Thread

### Y/n, y/N
대문자: 기본값. 아무것도 입력 안하고 엔터 누르면 대문자인 녀석이 입력돼요.
소문자: 명시적으로 입력해줘야 해요.

## (거의 모든) 로그인의 원리
### 대부분이 Stateless 방식이에요
대부분의 서버는 요청에 응답하고 연결을 끊는 Stateless 방식입니다!
### 그런데 어떻게 상태를 기억하나요?
다음 요청에 필요한 어떤 정보가 있을 때, 고객이 누군지를 정보와 함께 저장해둡니다.
그래서 다시 연결되었을 때 내가 가지고 있는 고객이 누군지 안다면 저장된 정보를 가져옵니다.


### Stateless의 문제점
요청 1개를 완료하고 연결을 끊는다.
근데 요청 1개가 오래걸린다면? 그럼 다른 클라이언트도 대기해야 한다.
그래서 `Thread`라는 기술이 나왔다.

커피 매장이라고 한다면,
비효율적 주문접수를 개선하기위해서, 
stateful => 1명 처리 다 끝나기까지 기다려야 함. 그래서 
stateless => 1번에 1주문만 받기로 함.
thread => 점원을 늘려서 주문대를 분할하기로 함
이런식으로 클라이언트가 기다리는 시간을 줄이려고 했다.

## Thread
쓰레드가 뭘까? 왜 수많은 용어중에서 '실행흐름'을 의미하는 용어로 '실'을 의미하는 쓰레드라는 용어를 썼을까?
1. knitting(뜨개질)걸 보면, 하나의 훌륭한 제품이 결국 하나의 실(쓰레드)로 만들어졌다.
2. 끊기지 않고 (건너뛰지 못하고) 만들어야 한다.
3. 이게 프로그램이 실행되는 것이랑 엄청 닮았다.
4. 실행흐름도 하나의 연결된 선으로 진행한다.
5. 그래서 쓰레드라고 부른다.

이제야 알게 된 실체(?)
* JVM은 프로세스다.
* main()은 쓰레드다.
* JVM은 main() 쓰레드를 실행시킨 것이다.
* JVM이 main() 메서드를 호출한 것이 아니다.

### Multi-Thread
* java는 멀티스레드를 지원한다.
* main()과 같이 상위 스레드를 부모 스레드라고 부른다.
* new로 자식 스레드를 만들고 start()로 실행을 분리한다.
* 특정 구간에서 실행 흐름을 분리하여, 독립적으로 실행하려는 `코드`가 있다면
	* Thread 클래스를 상속한 클래스를 만들고
	* 재정의(Override)한 `void run()` 메서드 안에 그 `코드`를 둔다.

### 멀티스레딩
여러 스레드가 동시에 실행되고 있는 것을 말한다.

### 스레드를 분리하여 실행하려면?
`Thread` 클래스를 상속받은 클래스를 작성하고, 해당 클래스를 인스턴스화한다.
그 인스턴스에서 `Thread` 클래스에서 상속받은 `start()` 메서드를 호출한다. 
`start()` 메서드는 별도의 스레드로 실행을 분리하여 `run()`을 수행한다.

다른 방법도 있다. (한 단계를 더 끼는 방법)
`Runnable` 인터페이스를 구현하는 것이다.
이 경우도 마찬가지로 `run()`을 재정의해야 한다.
`Thread`객체를 만들 때, 생성자 아규먼트로 인터페이스 구현체 인스턴스를 준다.
사용할 때는 Thread 인스턴스에서 run()을 호출한다.

### 람다 이해하기 팁
-> 선언부 보러가면 (IDE 에서 컨트롤이나 커맨드 누른채로 클릭하면)
해당 펑셔널 인터페이스로 가준다.

### DBMS
-> 멀티스레드, 네트워크 다 적용되어있음.
그래서 그걸 다 배우고나서야 DBMS를 이해하면서 적용하는 것이 가능함.



# OTHERS
## 생각
강의 속도가 상당히 빠르고 양이 많아서 타이핑 하지 못하는 부분이 아주 많아지고 있다!
어차피 날것처럼, 내가 복습을 위해 적는 포스트들이긴 하지만, 코드를 기준으로 복습해야 하는 비중이 늘고 있다!
## 구조조정 -> 경력자의 시장 공급
1. 기업이 임금을 줄이려고 구조조정한다.
2. 구조조정으로 인해 나간 사람들은 기존 임금이 심리적인 하방이 된다.
3. 근데 기업은 임금 지급여력이 없으니 (그래서 구조조정했으니)
4. 시장에 공급된 경력자를 쓰는게 아니라 신입을 뽑을 것이다.
5. 물론 힘들어진 건 사실이겠으나 어디에도 살 길은 있다.