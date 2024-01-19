---
title: (Day	46) IO, CO vs CL (Connection Oriented VS Connection-less)
author: 김준회
date: 2024-01-18 23:00:00 +0900
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
## CO/CL
### `Connection Oriented` VS `Connection-less`
차이 : 연결이 있어야 통신이 가능하냐?
연결이 일단 되어야(검사를 해야) 통신이 가능하면 CO 이다.
연결 여부와 관계없이(검사 없이) 통신이 가능하면 CL 이다.

그럼 연결, 정확히는 연결 `검사`를 어떻게 하나?
여기서 `3-way Handshake` 혹은 `4-way Handshake` 이라는 절차가 나온다.
![](https://t1.daumcdn.net/cfile/tistory/225A964D52F1BB6917)

![](https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835)

`FIN`, `ACK` 라는 게 보이는데, 이게 뭐 대단한 게 아니다. 그냥 숫자를 보내는거다.
숫자를 서로 보내봐서, 서로가 방금 내가 통신한 기기가 맞는지 확인해보는거다. 그렇게 `검사`를 한 다음 보내면 CO고, `검사`를 생략하고 바로 데이터를 던지는 게 CL이다.
HTTP3에서 UDP를 택하며 CL 방식이 되었다. (그 다음에 QUIC이 이제 많이 도입되고 있다)

## Statetful VS Stateless
차이: `상태`를 요청 처리한 이후에도 `유지`하는가의 차이다.
상태를 계속 유지한다면 연결가능한 클라이언트의 회전율을 늘리기 어려운 문제가 생긴다. 그럼에도 반드시 상태를 유지해야 할 경우도 있다. 전화통화나 계속 연결되어있어야 하는 게임 로그인 등이 그렇다.

# Study
## File 클래스
### `com.eomcs.io` 패키지 공부함
### close() 메서드가 있는 경우는 한정적 자원을 사용하는 경우다.
Resource Leak이 발생할 수 있다. 24시간 365일 계속 실행되어야 하는 프로그램은 close() 메서드가 존재하는 경우 ***반드시*** close() 수행할 타이밍을 잘 정해야 하고 빠트리면 안된다.

### 기본 단위
C는 운영체제가 32비트면 int도 그 크기고, 64비트면 int도 그 크기다. int가 기본 단위라 그렇다. 그래서 64비트 운영체재면 long이랑 int가 둘 다 64비트 크기의 자료형이다. (그래서 `long long` 이라는 자료형이 생겼다.

자바도 read()가 리턴타입이 int지만 끝 두바이트만 읽어서 반환한다. 유닉스 시절부터 그렇게 써온 레거시이기 때문에 그렇다. 

### EoF (End of File) 검출
read() 메서드는 읽어온 것이 없으면 -1을 반환한다.
그래서 2바이트 단위 read()를 사용하여 반복문을 돌리는 경우 이런 문장들을 자주 볼 수 있다.
```java
  int readed;
  while ( (readed = in.read()) != -1 ) {
   ...
  }
```

### 자바의 기본 단위
UCS2 (UTF-16 BE)를 쓴다. 이 Character Set은 모든 문자를 2바이트로 처리한다.
getBytes()를 호출해서 바이트 배열로 뽑아낸다고 하면
JVM 환경변수 값 중에서, `file.encoding` 정보에 따라 달라진다.
해당 정보는 Character Set 정보를 가지고 있다. 만약 해당 정보도 없다면, OS가 사용하는 CharSet을 쓴다.
그 윈도우 charset이 한국에서 윈도우는 MS-949고, 맥이나 리눅스는 UTF-8 이다.

### getBytes()의 인코딩
getBytes() 는 JVM 환경변수인 `file.encoding` 에 따라 인코딩하여 바이트 배열을 읽어온다.

### new String( byte[] )
바이트 배열이 들어있는 데이터가 어떤 charset 에 따라 encoding 되어있는지 모른다면
1. JVM 환경변수인 `file.encoding`에 따라서 인코딩한다.
2. `file.encoding` 또한 정보가 없다면 OS가 사용하는 charset을 사용한다.

그런데 IDE에 따라서 file.encoding이 설정될 수 있다.
* Eclipse는 `file.encoding`을 UTF-8로 잡는다.

## 주요 문자의 캐릭터 정수 값


| 문자 | UTF-16BE | UTF-8      | EUC-KR | MS949  |
| ---- | -------- | ---------- | ------ | ------ |
| '가' | 0xAC00   | 0xEA B0 80 | 0xB0A1 | 0xB0A1 |
| 'A'  | 0x0041   | 0x41       | 0x41   | 0x41   |
| 'a'  | 0x0061   | 0x61       | 0x61   | 0x61   |
| '0'  | 0x0030   | 0x30       | 0x30   | 0x30   |
| '1'  | 0x0031   | 0x31       | 0x31   | 0x31   |

* 각 인코딩은 아스키(ASCII) 문자에 대해서는 동일한 값을 가진다. 
* MS949는 EUC-KR을 포함한다.


### FIS FOS / FR FW
FIS/FOS 는 있는 그대로
FR/FW 는 문자집합에 따라 자동 인코딩/디코딩
텍스트가 아닌 파일을 주고받는 경우라면 FR/FW를 쓰면 안된다.


## `DataFileInputStream`, `DataFileOutputSteram`
1. 바이트단위로만 읽고 쓸 수 있는 read(), write()는 1바이트를 넘어가는 자료형을 다룬다면 비트쉬프트 연산을 여러번 이용해야만 한다.
2. 그건 너무너무 힘들다. 그래서 그걸 상속해서, 자료형에 따라 비트쉬프트 연산을 하는 메서드를 추가한 클래스를 만들었다. 그게 DFIS, DFOS다.
3. 근데 점점 상속의 문제점이 드러났다.


### 상속의 문제점
Buffer 써보니까 굉장히 좋다! 입출력 횟수를 줄이니까 아주 빨라졌다!
1. 그래서, `FileInputStream`을 상속하여 `BufferedFileInputStream`을 만들었다. 
2. 그러고나서, `FileInputStream`을 상속해서 `DataFileInputStream` (1바이트 이상의 자료형을 위해 만든 클래스)에다가 버퍼를 적용한 클래스인 `DataBufferedFileInputStream`을 만들었다. 
3. 그리고 각각의 클래스에 코드를 유사한 코드들을 작성했다. writeUTF(), writeInt() 등등등.... 그리고 기존 클래스에 있던 유사한 코드들이 미역처럼 불어나기 시작했다.
4. ... 왜 이렇게 됐지?
	5. 클래스가 아주 많아졌다.
	6. 각각의 클래스에서 중복되는 코드가 늘어났다.

상속을 이용하여 기능을 확장하다보니, 경우의 수가 증가함에 따라 중복코드도 증가하게 된다. 기능을 쉽게 추가하거나 삭제할 수 없게 되어버렸다. 😭😭😭

이를 고찰해보면,
* 경우의 수가 많은 경우에 이런 일이 일어난다.
* 현실세계로 비유해본다면, (ex. 자동차나 노트북 같은 제조업)
	* 다양한 옵션이 있어서 여러 경우의 수가 가능한 상품이 있는데
	* 그 경우의 수만큼 틀을 만드는 비효율적인 행동을 하게 된 것이다.
	* 현실에서는 소수의 틀을 갖고 만든 기본 제품에, 주문 옵션에 따라 나머지를 조립한다.
* 이 문제를 해결하려면, 현실세계(?)의 해법처럼 소수의 틀을 갖고, 기본 제품에 옵션들을 조립하는 구조로 클래스를 설계할 수 있도록 해야 한다.
(참고로, 다중 상속은 좋다고 보기 힘들다!
참고: https://velog.io/@dlwoguq0928/%EB%8B%A4%EC%A4%91-%EC%83%81%EC%86%8D%EC%9D%84-%EC%A7%80%EC%96%91%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0 ) 

이 해법 또한 노하우로 공개가 되었는데, 이것이 GoF의 Decorator Pattern이다.
GoF(Gang of four, 사총사 정도로 번역)가 Design Pattern으로 정리한 패턴 중 하나다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/400px-Decorator_UML_class_diagram.svg.png)

데코레이터(꾸미기, 혹은 악세사리)에 포함된 기능들을 컴포넌트에 붙이는 형태다.
실제 제품에 기능들을 붙이는 형태다.


# Self Study
## 이것이 자바다
* 제네릭 복습 완료. (570-589)
* 람다식 복습함.
* 어제, 오늘 배웠던 내용인 데이터 입출력파트 (780~819) 책으로 복습함.