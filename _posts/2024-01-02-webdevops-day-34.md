---
title: (Day	34) 중첩클래스 및 실습
author: 김준회
date: 2024-01-02 23:00:00 +0900
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
24년이라니!!

# 복습
## `Iterator` Design Pattern (GoF)
자료구조에서 값을 꺼내는 방법에 `통일성`을 부여하기 위해 발견된 패턴.
어떤 자료구조에서 값을 꺼내건 동일한 방식으로 꺼내기 위해 사용한다.

장점
: 통일성

단점
: 오버헤드

## 중첩 클래스(Nested Class)
사용 용도
: 어떤 하나의 클래스에서만 사용하기 위해 클래스를 만들면 불편한 점들이 있다.
1. 패키지 안에 클래스들이(자바 소스파일) 많아져서 이해하기 힘들어진다.
2. 어떤 클래스가 어떤 클래스를 사용하는지 이해하기 힘들어진다.
3. 이해하기 힘들어지면 유지보수하기 힘들어진다.

패키지를 뜯어보니 하나의 클래스에서만 쓰이는 클래스들이 많다는 것을 알게 되었다. 그래서, 그렇게 단 하나의 클래스만을 위해서 쓰는 경우, 자바 소스파일 안에 그 클래스 안에 중첩해서 작성할 수 있도록 한 것이 중첩 클래스다.

병렬이 아니다. 말 그대로 사용하는 클래스 바디 안에다가 다른 클래스를 넣은 것이다.

### 중첩클래스의 단계
1. 외부에서 구현한 패키지 멤버 클래스를 사용함 (Nested Class 사용하지 않은 상태)
2. `Static Nested Class`
정적 영역에 생성하는 중첩 클래스. 바깥 클래스의 인스턴스 주소가 필요한가? 로 Non-static 여부를 결정하면 된다.
3. `Non-Static Nested Class` = Inner Class
외부(바깥) 클래스의 **인스턴스** 주소를 가져오는 코드를 생략해도 컴파일러가 추가해준다.
바이트코드를 보면 this$0 라는 주소를 가져온다. `바깥클래스이름.this.` 으로 접근할 수 있다.

***중첩클래스를 정확히(바이트코드가 어떻게 생성되는지) 알고 있는 자바 개발자가 많지 않을 수도 있다.***

### 로컬클래스
>TODO

### 익명클래스
익명클래스는 바로 어떤 일을 하는지 알 수 있는 게 장점이다. 보통 중첩클래스에서 객체를 만들어서 바로 반환하는 경우에 쓰는데 코드가 짧을 때 쓴다. (10~20줄 미만 정도?)
코드가 길면 차라리 바깥으로 빼는 게 가독성이 더 좋을 것이다.


# 학습
## Eclipse, VSCode 동시 사용시 문제
이클립스는 오류가 있는 소스파일이 있으면 건너뛰고 bin에 컴파일한 클래스파일 생성한다.
VSCode는 그레이들을 통해서 빌드한다. 오류있는 파일이 있으면 빌드를 중단한다.


스태틱 멤버는 인스턴스 멤버를 사용할 수 없다. 레퍼런스 선언은 가능한데 인스턴스 생성은 불가능하다.

## this 찾는 순서 (중첩클래스)
this 를 명시하지 않았을 때 변수를 찾는 순서
1. 로컬 변수를 찾는다.
2. 메서드가 소속된 클래스의 인스턴스 필드를 찾는다.
3. 바깥 클래스의 인스턴스나 스태틱 필드를 찾는다.

찾으면 컴파일러가 그렇게 컴파일한다. JVM이 그 순서로 찾는 게 아니다. JVM은 바이트코드를 그냥 있는 그대로 실행할 뿐이다.

```
inner class 는 스태틱 멤버를 가질 수 없다.
스태틱 멤버는 오직
- top level class 나
- static nested class
만이 가질 수 있다.
- Java16 부터는 inner class도 스태틱 멤버를 가질 수 있다.
```
```
NESTMEMBER com/eomcs/oop/ex11/d/A$1X
NESTMEMBER com/eomcs/oop/ex11/d/A$2X
// access flags 0x0
m1()V
L0
LINENUMBER 23 L0
NEW com/eomcs/oop/ex11/d/A$1X <<<요 녀석 주목!
DUP
ALOAD 0
INVOKESPECIAL com/eomcs/oop/ex11/d/A$1X.<init>(Lcom/eomcs/oop/ex11/d/A;)V <<<요 녀석 주목!
ASTORE 1
L1
LINENUMBER 24 L1
RETURN
L2
LOCALVARIABLE this Lcom/eomcs/oop/ex11/d/A; L0 L2 0
LOCALVARIABLE obj Lcom/eomcs/oop/ex11/d/A$1X; L1 L2 1
MAXSTACK = 3
MAXLOCALS = 2
```
로컬 클래스를 만들 때 컴파일러가 넣어주는 `제한`이다.
바깥클래스에서만 쓸 수 있도록 제한하는 것이다.


메서드에 선언된 클래스 = 로컬 클래스.
로컬클래스의 바깥클래스 사용 가능 여부는 메서드의 non-static 여부에 따라 달라진다.
non-static 이라면 바깥클래스의 객체 주소를 같이 받으며 (생성자에 컴파일러가 자동으로 넘겨줌) 마찬가지로 `바깥클래스명.this` 로 사용할 수 있다.


로컬 클래스에서 바깥 클래스에 있는 변수를 사용한다면
그것을 위한 필드를 컴파일러가 자동으로 추가해준다. (사용하는 것만)
이름도 val$변수명 같이 만들어준다. (구현체마다 다를 수 있음)

로컬클래스에서 인클로징 메서드 (바깥 메서드) 의 파라미터를 사용하게 되면,
그것을 사용할 수 있도록 내부적으로 파라미터에 대한 필드를 자동생성해준다.
그 필드는? **인스턴스 필드** 다 !!

중첩클래스를 배울 때 중요한 것은,
컴파일러가 그것을 어떻게 변환하는지를 아는 것이다.
모든 케이스를 암기하려고 하면 안된다.
컴파일러가 경우에 따라 어떻게 컴파일하는지 알면
나머지 케이스들을 이해할 수 있게 된다.
즉 컴파일러가 어떻게 하는지를 암기해야지
전체 케이스를 다 암기하려고 하면 안된다는 것이다.


## 컴파일러가 만들어 준 class 파일의 이름
클래스명으로 이름이 생성된다.
그런데,
`클래스명$1` 이라던가 (로컬 클래스, 익명 클래스)
`클래스명$중첩클래스명` 같은 형태가 있다.