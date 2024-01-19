---
title: (Day	35) 익명클래스와 파일 입출력
author: 김준회
date: 2024-01-03 23:00:00 +0900
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
# 복습
## 중첩클래스
## 로컬클래스
### 로컬클래스에서 enclosing 메서드의 변수를 사용하는 메커니즘을 설명할 수 있는가?
그것을 직접 사용하는 것이 아니라, 그 변수의 값을 할당하는 변수를 자동으로 컴파일러가 생성해주기 때문이다. (Non-Static Local Class)
이러한 변수는 바이트코드를 해석한 IDE의 윈도우에서 `Synthetic` 이라고 붙은 변수로 확인할 수 있다. 

`합성된` 이라는 의미의 Synthetic 키워드는 사용자가 직접 만든 게 아니라는 의미에서 붙는 단어이다. 그럼 누가 만들었을까? 말했듯이 컴파일러다.

## 익명 클래스

# 학습
## 30. File Input/Output
지금까지 Array, Linked List 를 자료구조로 사용했는데, 이들을 모두 Memory 에 저장했다. 프로그램이 종료되면 (메모리에서 제거되면) 추가한 정보가 모두 사라지는 문제가 있는 것이다.

데이터를 입력하고 그 데이터를 유지할 수 있는 능력이 있다면 그것은 데이터에 대하여 Persistent 하다고 할 수 있다. 데이터에 영속성이 있다는 것이다. 이러한 persistance를 구현하려면 Non-volatile storage에 해당 데이터를 저장할 수 있어야 한다. 데이터를 Non-volatile storage에 저장하면 그것을 파일이라고 부른다. 이러한 파일 저장 및 읽기 기능을 java에서 추가해보자.

이는 자바에서 제공하는 `FileOutputStream` Class를 통해서 가능하다.
또한 저장된 파일을 읽어들이는 것은 `FileInputStream` Class를 통해서 가능하다. 이들은 `java.io` 패키지에 소속된 클래스들이다.

## 31. Refactoring `App` class
* `main()` 메서드에 들어있는 코드를 기능에 따라 묶어 여러 메서드로 분리
* 공유하는 변수는 인스턴스 필드로 전환

## FIle I/O 는 대부분의 고급 언어에서 유사하다
운영체제의 펑션을 직접 콜하는 것이 아니면, 대부분의 언어에서 파일입출력은 유사하다. 왜 유사하다고 할까? (물론 주관적인 것이지만)

어떤 프로세스도 ==자원에 직접 접근할 수 없다==. **운영체제**를 거치지 않고 자원에 직접 접근하지 못하다보니 그렇다.

(C나 C++ 같이 운영체제를 작성하는데도 쓰인 언어는 시스템펑션을 콜 할 수 있어서 다르다)

자바의 경우에는
Java Application이 파일입출력을 위해 call 하면 JVM이 그것을 받을 것이고, JVM이 OS에 해당 펑션을 Call 할 것이다. 

C, C++는 운영체제에 직접 Call 할 수 있다. 문제는, OS마다 시스템 펑션을 콜하는 방법이 다 다르다는 것이다. 속도가 빠르지만 운영체제에 종속된다. 운영체제마다 프로그램을 따로 짜줘야 한다.

결국 모든 언어는 운영체제의 펑션에 종속될 수 밖에 없다. 그래서 파일 입출력을 하는 방법이 유사한 것이다. 

참고로 파일 입출력은 유닉스에서 처음 그 기능이 만들어졌다. 윈도우도 리눅스도 유닉스 이후 나온 것이고 유닉스를 참고하여 만들었기 때문에 그 명령어들도 같거나 유사한 면이 있다. OS의 입출력의 방법 또한 마찬가지다. 유닉스에서 사용되던 현재의 메인스트림 고급 언어들도 마찬가지다.

### 유닉스의 파일 입출력 방법
write(int)
: 무조건 1바이트를 출력한다. long을 주건 int를 주건 char를 주건 short를 주건... 뒤에 있는 1바이트만을 출력한다. 

write(byte[])
: N개의 바이트 배열을 argument로 주면, 그대로 전체 바이트 배열을 출력한다.

write(byte[], offset, len)
: offset 을 인덱스의 시작점으로 하여 len(length) 개수 만큼만을 출력한다.

이는 java에서 FileOutputStream 클래스에서도 동일하게 적용된다.


## 32. Primitive Type & String Type -> File Input/Output Stream

Via `DataOutputStream`, we can manage primitive and String type with File.
Inheritance can be applied to this case. CAUSE I have to make the `DataOutputStream` class.

* writeShort
* writeInt
* writeLong
* writeUTF
* ...

With Inheritance, we can make useful class with minimum code modification.

## 익명 객체

Anonymous Objects CANNNOT be declared static! GPT Said:
Java에서 익명 객체(Anonymous Objects)를 `static`으로 선언하는 것은 불가능합니다. 익명 객체는 주로 메서드 내에서 생성되어 사용되는데, 이 객체는 메서드 블록 내에서만 유효하므로 `static`으로 선언할 수 없습니다.

`static` 멤버는 클래스 수준에서 존재하며, 익명 객체는 특정 메서드 내에서만 사용되기 때문에 클래스 수준에서 존재하지 않습니다. 따라서 익명 객체를 `static`으로 선언하는 것은 의미가 없습니다.

만약 `static`으로 사용하고자 하는 경우에는 일반적인(named) 클래스를 정의하고 해당 클래스의 객체를 `static`으로 선언하여 사용하는 것이 적절합니다.

# 커리어 관련
## 경험치를 많이 쌓을 수 있는 곳
선임자가 있을 것 (혹은 조언을 받을 오프라인 인원이 있는 경우)
선임자도 없이 파견 / 인력장사 하는 곳은 피할 것


# 공부 일기
1. 무엇이 중요한지 앞으로 어떻게 배워야 할지 안개가 아주 조금은 걷힌 것 같다. 문법과 그 문법으로 OOP를 어떻게 구현하는지, 왜 이런 문법이 존재하는지와 같은 것이 가장 중요한 기초이다. 
2. 문법 및 문법의 존재 이유보다 더 아래로 파고 들어가면 운영체제, 회로설계, 전자공학, 물리학, 수학, 논리학과 같은 bedrock 들을 만나게 되겠지만, 지금은 강사님이 조언하시는 것처럼 언어 자체에 대해 익숙해지고, 두려움을 없애고, 존재 이유를 느껴야 하는 시기일 것으로 생각된다. `혼자 공부하는 컴퓨터 구조 및 운영체제`로 운영체제는 맛봤고, 잊혀진(?) 수학을 수학 리부트로 다시 시작해야겠다.
3. 그래서 문법 교과서라고 할 수 있는 `이것이 자바다` 를 훈련과 동일 진도로 맞추는 것이 지금 하고 있는 학습이다.
4. 교과서 따라가기를 약 이주 정도 꾸준히 진행했고 현재 진도에 꽤 많이 따라왔다. 다만 지금 수업에서도 나가는 속도가 빨라서 계속해서 교과서에서 진도를 따라가야 할 것이다.
5. 자바 문법과 객체지향의 목적(유지보수를 편리하게)에 대해 이해하면서, 지금 하고 있는 것처럼 계속 알고리즘 테스트를 단계별로 진행하면서 잔디를 심자.
6. 그러다 보면 수학이 필요해지는 시점이 올 수 있겠다 싶다. 자기 전에 `수학 리부트` 요 책을 읽자. 한 챕터를 읽어보니 수긍할 수 밖에 없는 사실이 있다, 강사님이 추천해주신 Prof. Donald Knuth의 `Concrete Mathematics` 는 내가 지금 흡수할 수 있는 레벨이 아니라는 것이다.
7. 자료구조와 알고리즘에 대해서도 강사님 강의가 진행될 걸로 보이는데 (자료가 있으니까) 이 때 책을 하나 같이 읽자.
8. 그러면 순서를 정리해보자.
9. Main Quest: java 언어 문법이 끝날 때까지 `이것이 자바다`
10. Daily Quest: Algorithm Judge → 잔디 심기 ㅋㅋ 
11. Sub Quest:  `수학 리부트` 자기전에 읽기. 감탄하면서 읽고 있다. 이걸 읽다보니 유클리드 Element를 읽어보고 싶다. 쌓아가기... 차곡차곡...
12. 메인 퀘스트가 끝나면 `리팩터링`, 그리고 `헤드퍼스트 디자인 패턴` 이 책들의 지식을 흡수하는 게 목적이 될 것 같다.
13. `코딩 인터뷰 완전분석` 이라는 책도 좋은 책 같은데, 기본적인 자료구조와 그에 대한 알고리즘들을 먼저 보고 나서야 흡수할 수 있는 책 같다.


오늘 `이것이 자바다`는 Chapter 9 중첩 선언과 익명 객체까지 끝냈다. 나중에 빠르게 복습하면서, 전체 내용이 아닌 주요 내용에 대해 **Mind Map 형식**으로 포스팅을 하면 나에게도 좋고 혹시라도 있을 방문자분들에게도 아주 좋을 것 같다. 프로그램은 Thinkwise 혹은 FreeMind 를 쓸 예정이다. 요새 시각화의 중요성을 절대적으로 크게 체감중이다. 
