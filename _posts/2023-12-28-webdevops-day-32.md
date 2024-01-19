---
title: (Day	32) (실습) 자료구조 추가 적용
author: 김준회
date: 2023-12-28 23:00:00 +0900
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
# 학습
## Linked List, Array List의 remove(Object) 메서드에서 구현 방법이 다른 이유?
Linked List 에서 삭제하려는 값과 동일한 값을 가진 노드를 발견하고서,
index를 받아서 삭제하는 remove() 메서드를 실행하여 나머지를 처리하는 경우
traverse를 두 번 해야 하는 비효율이 생긴다.


## 가비지컬렉션을 도와주는 코딩
가비지로 만드는 객체는 다른 객체를 참조하지 못하도록 한다.
요즘 가비지컬렉터는 가비지가 참조하는 것 또한 고려한다만...


## 인터페이스 활용
메뉴를 ArrayList로 만들었는데, 이를 LinkedList로 변경하려면 Handler 클래스 또한 변경해야 한다. (그 코드의 내용을)
만약 ArrayList, LinkedList 두 클래스가 동일한 규칙에 따라 작성되었다면 Handler코드를 변경하지 않고, 리스트를 위해 사용하는 클래스를 변경할 수 있다.

동일한 규칙에 따라 작성하였다? 두 클래스는 타입이 같다(문법적으로)
이는 두 클래스의 사용방법이 같다면 이라는 말로 바꿔 말할 수 있다.
이것은 List 인터페이스를 정의하여 실현할 수 있다.

## 추상 클래스 끼우기
중복 코드를 더 줄이기 위해서, 인터페이스를 직접 구현하기보다는 추상 클래스를 한번 거치도록 한다. concrete class 들이 공통적으로 구현해야 하는 코드는 중복이 된다. 이를 추상클래스에서 구현해서 상속으로 전달해주는 것이다.

## 컴포지트 패턴에 인터페이스 적용
컴포지트 패턴에서 App.java의 main에서 리파지토리 변수를 만들어줄 때, 
인터페이스를 사용함으로 인해

```java
    List<Board> boardRepository = new LinkedList<>();  
    List<Assignment> assignmentRepository = new LinkedList<>();  
    List<Member> memberRepository = new ArrayList<>();  
    List<Board> greetingRepository = new ArrayList<>();
```
아래와 같이 사용할 수 있게 되었다.

인스턴스를 생성하는 App.java에서만 어떤 리스트를 쓸 지 결정해주면
Handler와 같은 다른 코드에서는 List의 서브클래스에 해당하는 concrete class를 받으면 되기에 문제가 없어진다. 

즉 LinkedList를 쓸 것인지, ArrayList를 쓸 것인지는 저 한줄에서만 변경이 발생한다는 것이다.

# 자료구조
### 학습 순서
링크드리스트가 기본이다. 그리고 스택과 큐는 링크드리스트를 상속받아 구현하면 된다.
구현할 때 메서드 이름을 잘 기억해야 한다. 짓는게 아니라 기억해야 한다. API에서 제공하고 있는 이름을 동일하게 사용해라. 그래야 인터페이스문법, 추상클래스 문법에서 다형적 변수를 활용하여 유지보수를 줄인 프로그램을 작성할 수 있다.

기술면접에서는 그 자체에 대한 설명과 함께 언제 쓰는지를 설명할 수 있어야 한다.

## 링크드리스트


## 스택 extends 링크드리스트
LIFO (Last-In First-Out)
push(), pop(), peek(), empty()

사용예시
1. Bread Crumb 
웹 브라우저에서 방문 기록을 남길 때와 같이, 하나씩 되돌아와야 하는 상황에 스택이 적절하다.
Explorer, Finder와 같은 소프트웨어에서 디렉토리 방문 목록등을 작성할 때
2. 메서드 호출 목록을 저장할 때


## 큐 extends 링크드리스트
FIFO (First-In First-Out)
offer(), poll(), peek()

사용예시
요청 순서대로 처리해야 할 때
1. 예약, 구매 등 선착순으로 요청을 처리해야 할 때


## 메뉴 그룹에 stack 적용하여 BreadCrumb 구현하기
MenuGroup 에서 Stack을 사용하여 현재 사용하고 있는 메뉴에 대한 BreadCrumb 구현하기

## Iterator Design Pattern
Iterator? What is the Iterator?
자료구조에서 값을 직접 꺼내지 못하게 캡슐화(감추기) 하고,
그 값을 꺼내는 다른 객체(Iterator)를 두는 것을 이터레이터 디자인 패턴이라고 한다.
값을 꺼내주는 객체만을 사용하게 만들어서 값을 꺼내는 부분을 감추면 무엇이 좋은가? 자료구조가 무엇이냐에 관계없이 일관된 방법으로 값을 조회할 수 있게 된다. java의 collection 에는 기본적으로 적용되어 있다.

## 연봉 1억의 벽
수학으로, 혹은 다른 기술로, 혹은 학위로 벽을 뚫어야 한다.