---
title: (Day	22) 객체지향(GRASP, Composite Pattern)
author: 김준회
date: 2023-12-13 23:00:00 +0900
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
# 22일(2023-12-13)

# 복습
## GRASP (일반적 책임할당 지침) 의 Information Expert 적용
정보를 가지고 있는 클래스가 그 정보를 다루게 해라. 그렇게 해야 클래스를 전문화시켜서 높은 응집력을 가지게 할 수 있다. 

## GRASP: High Cohesion
응집력을 높여야 다른 프로젝트에서 사용가능하며, 기능 변경시 작업해야 하는 코드가 적어진다.

## 상속관계의 클래스를 가리키는 용어
상위: super class, parent class
하위: child class, sub class

## 상속관계에서 다형성은 어떻게 나타나는가?
자식은 부모의 기능을 물려받는다. 부모 클래스의 데이터타입으로 정의한 레퍼런스 변수는 자식 클래스의 인스턴스 주소를 담을 수 있다. 부모는 자식을 담을 수 있다. 그러나 자식은 부모를 담을 수 없다. 그리고 자식끼리는 서로 담을 수 없다.

`부모클래스명 레퍼런스명 = new 자식클래스명();` 

위 코드처럼 부모클래스로 레퍼런스를선언하고 자식클래스의 인스턴스 주소를 담는 것은 가능하나 아래는 불가하다.

`자식클래스명 레퍼런스명 = new 부모클래스명();` 
`자식클래스명 레퍼런스명 = new 자식클래스명();` 

## 코딩의 일관성
어떤 조직에 들어갔을 때 기술적으로 낡았음이 느껴지거나 개선 가능성이 높은 코드를 보게 될 수도 있다. 그런데 혼자 개선작업을 하는 건 유지보수에 결코 도움이 되지 않는다.
코드는 일관성이 필요하다. 코드에 일관성이 없으면 사람이 바뀌었을 때 문제가 생긴다. 조직의 모든 관련 구성원이 날잡고 리팩터링을 하는 게 아니라면, 일관성을 유지하는 게 낫다. 혼자 일관성없는 코드를 만들지 마라.

## java.lang.Object
자바의 모든 클래스들은 Object의 후손이다. 
부모 클래스 데이터타입에 대한 레퍼런스는 자식 클래스의 인스턴스를 담을 수 있다. 그래서 `Obejct 레퍼런스명` 은 범용적으로 인스턴스를 담을 수 있는 레퍼런스다.

### 이를 이용하여 범용 클래스를 만들다.
모든 클래스의 부모인 Object 클래스를 활용한다. 이전 myapp 에서는 각각의 핸들러에 Repository 클래스를 두었던 형태였다. 근데 그럴 필요가 없다. **코드 중복을 하나 더 해소한 것이다!** 

 `Board[] boards = (Board[]) this.objectRepository.toArray();`
 위 문장이 이상한 이유는?

```java
Object obj = new Board();
Board b = (Board) obj;
```
이건 Board 객체를 b에 넣는거라 문제 없다

```java
Object[] arr = new Object[3];
Board[] arr2 = (Board[]) arr;
```
arr에 들어있는 건 Object 배열의 주소이다.
부모를 자식이라고 할 수 없다. 
컴파일러는 이걸 검사하지 않는다. 타입이 맞는지만 검사한다.
이러면실행 중 오류(예외) (Runtime Exception) 가 발생한다.

## 컴포지트 패턴의 비유
컴포지트 패턴을 현실 세계의 일화를 통한 비유로 설명해 보겠습니다.
**일화: 기업 조직 구조와 조직 부서**

가상의 기업을 상상해 봅시다. 이 기업은 여러 부서로 구성되어 있고, 각 부서는 여러 팀으로 세분화됩니다. 이 조직 구조는 컴포지트 패턴과 유사한 원리를 따릅니다.

1. **기업 (Component):** 기업 전체가 컴포지트 패턴에서의 Component에 해당합니다. 기업은 여러 부서로 이루어져 있으며, 각 부서는 특정 역할과 책임을 갖고 있습니다.

2. **부서 (Composite):** 각 부서는 여러 팀(Team)과 하위 부서(Sub-department)로 구성될 수 있습니다. 부서는 또한 특정 역할을 수행하며, 부서 내의 모든 팀과 하위 부서를 관리합니다.

3. **팀 (Leaf):** 팀은 더 이상 세분화되지 않는 최하위 단계의 구성원입니다. 팀은 개별적인 업무나 프로젝트를 수행하며, 더 이상 하위 요소가 없는 단말 노드입니다.

이 구조에서는 기업이라는 큰 단위에서 각 부서가 부서 내의 팀이나 하위 부서까지 포함하는 구조를 가집니다. 이때, 각 부서는 또 다른 부서일 수도 있고, 팀이나 하위 부서일 수도 있습니다. 따라서 전체 조직 구조를 유연하게 확장하거나 축소할 수 있습니다.

컴포지트 패턴과 마찬가지로, 이 기업 조직 구조에서는 클라이언트가 부서, 팀, 또는 개별 구성원에게 특정 작업을 지시할 수 있으며, 각 구성원은 자신에게 할당된 작업을 수행합니다. 이로써 전체적인 조직이 효율적으로 운영되고 유지보수가 쉽게 이루어집니다.

 
## Generic - 범용클래스를 위한 타입을 받는 변수
제네릭이 선언된 경우, 레퍼런스를 선언하는 시점에 지정된 타입이 아닌 값을 넣으려고 하면 컴파일 오류가 발생한다. 즉 특정 타입만 사용하도록 제한할 수 있는 문법이 제네릭(generic)이다.

## 자료구조에 대해서
지금 이렇게 자바에서 기본으로 제공해주는 클래스를 구현하면서 가고 있다. 어렵다. 근데 필요한게 맞다. 쉬운 게 아니라서 가치있는 것이다. 
어레이리스트의 단점은 무엇인가? 배열의 단점은 무엇인가? 삽입과 삭제의 불편함이다. 배열 더 큰 것이 필요하면 배열을 복사해야하니 가비지도 많이 생긴다.