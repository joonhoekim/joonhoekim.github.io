---
title: (Day	26) OOP; 추상클래스, 추상메서드, 인터페이스, 템플릿 메서드 패턴, modifier
author: 김준회
date: 2023-12-19 23:00:00 +0900
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
## 추상 클래스의 용도는? 
1. 상속을 위해 존재하는, `generalization`을 위한 클래스이다.
2. 인스턴스화를 할 수없다. (인스턴스 생성이 불가능하다.)
3. 레퍼런스 선언은 가능하다. 다형적 변수를 구현한다. 인터페이스도 레퍼런스 선언은 가능하다.
	- 컴파일러: 레퍼런스가 선언된 클래스를 기준으로 `형식`만을 검사한다.
	- JVM:  실제 생성된 인스턴스에 해당 메서드와 필드가 있는지를 검사한다.


## 추상 메서드의 용도는?
추상클래스에서, 구현하지 않은 메서드를 서브클래스에서 구현하는 것을 강제하기 위한 문법이다. 작성할 때는 메서드 시그너처만 작성하고, 바디는 `{}` 괄호도 작성하지 않아야 한다. 이 없는 바디는 서브클래스에서 구현하라는 의미다.

C++ 이나 C#에서는 Function Prototype이라고 부른다. 여기서는 메서드라는 말을 안쓰고 함수라고 부른다. 여기서도 펑션 바디가 없이, 상속만을 위한 것을 만들 수 있다. (java의 추상 메서드)

## 인터페이스의 용도는?
추상 클래스는 일부 필드와 일부 메서드는 구현해두고, 나머지는 서브클래스에서 재정의하거나 추가하는 등 활용하라는 것인데, 어떤 추상 클래스는 구현하는 필드나 메서드가 아예 없을 수도 있다. 이 경우는 인터페이스를 쓰면 된다. 인터페이스는 완전히 껍데기만 있는 추상클래스라고 생각하면 된다. 

추상클래스는 접근제어자가 public 일수도 있고, protected 일 수도 있고, (default) 일 수도 있다.  (private는 서브클래스에서 사용 불가하므로 아니겠지..?) 인터페이스는 전부 public이다. 다른 패키지에서도 확실히 접근 가능한, 상속을 주기 위한 것만이 목적인 것이 인터페이스다.

## 템플릿 메서드 패턴은?
슈퍼클래스는 알고리즘의 골격을 메서드들로 잡아둔다. 이 부분을 '템플릿 메서드' 라고 한다. 그리고 알고리즘의 요소를 이루는 이 메서드들을 서브클래스에서 구현하도록 한다. 추상 클래스인 슈퍼클래스를 상속한 서브클래스들은, 구현되지 않은 메서드들을 반드시 구현해야 할 것이다. 슈퍼클래스에서 추상메서드로 선언하여 구현을 강제했을테니 말이다.
이렇게 템플릿이 잡혀있고, 서브클래스에서 필요에 따라 템플릿의 내용을 채워넣는 패턴을 `템플릿 메서드 패턴`이라고 한다. (GoF)


### 컴포지트 패턴
폴더 트리, 메뉴처럼 계층 구조로 객체가 객체를 포함하는 구조를 가지고 있을 때 이것들을 가장 편하게 관리하기 위해 적용하는 패턴.
- 메뉴, 메뉴아이템, 메뉴 그룹
### 싱글톤 패턴
단 한번만 인스턴스를 생성하고, 그 인스턴스만을 사용하고 싶은 경우
- 달력
### 팩토리 메서드 패턴
인스턴스 생성 시, 유효한 인스턴스로 설정하기 위한 과정이 복잡한 경우
new 명령어를 이용하여 인스턴스를 생성하지 않고 메서드를 통해 인스턴스를 생성하면서, 인스턴스의 설정 또한 한번에 하도록 하기 위한 패턴.
### 싱글톤 패턴과 팩터리 메서드 패턴의 공통점
생성자를 private로 막아서 new 명령어로 직접 인스턴스 생성하지 못하도록 한다.
메서드를 통해서 인스턴스를 생성해야 한다. 
인스턴스 생성을 위한 메서드는, 당연히 인스턴스 생성을 하기 전에 접근가능해야 하므로, public static 메서드여야 한다.

## 필드나 메서드의 접근 범위를 조정해야 하는 이유와 방법
private (default) protected public .. modifier, 접근제어자를 생각할 때는 Score 클래스를 생각하자. 각 과목 점수에 직접 값을 할당하거나, 평균이나 합계에 값을 직접 할당하는 경우는 문제가 생긴다. 각 과목 점수에 따른 평균과 합계가 일치하지 않는 것이다.

**이러면 Integrity(무결성)이 깨졌다. 혹은 Synchronous하지 않다**  

이러면 문제가 발생할 수 있다! 
합계나 평균을 꺼내봤더니, 원하지 않은 결과가 나온 것이다.

그래서 접근을 제어해야 하는 것이다. 이는 접근제어자를 통해 가능하다.
그런데 접근을 제어하고 나면, 필드에 접근하여 읽거나 할당하는 기능도 당연히 필요한데 이는 메서드로 구현하자.

### 게터와 세터
접근이 제어된 필드에 접근하여 읽거나 할당(쓰는) 기능을 구현하는 것이 게터와 세터이다. 게터와 세터 메서드는 외부 클래스에서 접근 가능하여야 한다.

### 프로퍼티?
필드에 대해 설명할 때 사용되는 용어다.
1. 읽기만 가능하면 Read only property
2. 쓰기만 가능하면 Write only property
3. 읽기, 쓰기 둘 다 가능하면 Read/Write property

실제로는 더 다양한 의미가 있으나 일단은 이 깊이로만...

### 프로그래밍의 일관성, trade-off
굳이 접근제어를 할 필요가 없는 변수나,
게터와 세터의 필요성이 없는 변수더라도
그 필드(변수)들 또한 게터와 세터로 사용하도록 하자.
그것이 프로그래밍의 일관성을 높이고, 통일성을 높여 직관성을 만든다.
직관성은 유지보수 비용을 낮춘다.

## Protected 


##  초보와 디자인패턴
작성되어 있는 코드들이 어떤 패턴을 따르고 있는지를 **알아볼 수 있는 것**이  중요하다! 패턴을 사용할 수 있으면 좋겠지만, 우선 패턴을 알아보는 것이 먼저다.

## 추상화
컴퓨터로 뭔가를 다루려면, 데이터화 해야 한다. 환자를 컴퓨터로 다룬다면, 환자의 데이터를 다룬다는 것이다. 실제로 컴퓨터가 다루는 건 데이터다. 그 데이터를 물리적인 영역까지 넓히면 로봇이 주사놓고 수술하고 그렇게 되겠지만.. 

그런데 현실세계는 데이터가 아니다. 그래서 데이터로 만드는 과정이 필요하다. 여기서 인간이 가진 아주 중요한 능력이 사용된다. 그것이 `추상화`다.

컴퓨터를 위한 추상화는 무엇일까? 어떤 실제 존재하는 것을, `데이터`와 그것을 다루는 `연산자`로 표현하는 것이다. 현실세계의 물리적인 사물이나, 비물리적인 개념들을 데이터와 연산자로 표현하는 것이다.

> 비물리적인 개념들은, 업무 행위 등이 될 수 있다.

### 캡슐화
캡슐화는 추상화가 무너지지 않도록 해주는 기법이다.
접근제어자를 통해 변수들을 감추고, (캡슐화를 지원)
추상화에 유효한 값들을 받도록 하기 위한 기법이 캡슐화다.

그래서 자바는 필드나 메서드의 외부 접근 범위를 조정하는 문법을 제공한다.
그 문법을 '캡슐화(encapsulation)'라 부른다.


### 학습 조언
일반화를 위한 상속과 추상클래스, 추상메서드의 활용에 대한 강의를 하면서 요구하는 것은, 클래스 다이어그램을 보고 그것을 실제로 바로 구현할 수 있기를 바라는 것이 아니다. 이러한 클래스 다이어그램이 어떤 것이고, 코드를 보고 클래스들의 구조와 목적을 이해하기를 바라는 것이다. '왜 저렇게 했지?' 라는 질문에 답을 할 수 있는, 알아볼 수 있는 역량을 키워주고 싶은 것이다.

### Protected
```java
package bitcamp.menu;  
  
import bitcamp.util.AnsiEscape;  
import bitcamp.util.Prompt;  
  
public abstract class AbstractMenuHandler implements MenuHandler {  
  
  protected Prompt prompt;  
  protected Menu menu;  
  
  protected AbstractMenuHandler(Prompt prompt) {  
    this.prompt = prompt;  
  }  
  
  @Override  
  public void action(Menu menu) {  
    this.printMenuTitle(menu.getTitle());  
    //서브클래스에서 menu 객체를 필요로 할 수 있으므로 보관한다.  
  this.menu = menu;  
  
    this.action();  
  }  
  
  private void printMenuTitle(String title) {  
    System.out.printf(AnsiEscape.ANSI_BOLD + "[%s]\n" + AnsiEscape.ANSI_CLEAR, title);  
  
  }  
  
  //서브 클래스가 구현해야 할 메서드!  
 //슈퍼클래스의 action(Menu)를 통해 호출되는 메서드이다. 직접적으로 호출되지 않는다.  
 //그래서 접근범위를 제한할 것이다. 추상 메서드는 일단 상속은 되어야 하니까, protected 쓰자.  
 //문서들을 보면 protected 접근제어자를 쓰는 추상클래스들을 쉽게 찾아볼 수 있을 것이다.  
  protected abstract void action();  
  
  //메서드와 추상 메서드가 오버로딩 되어 있다!  
 //오버로딩 안해도, 슈퍼클래스의 코드가 먼저 실행되도록 작성 가능하지만  
  //오버로딩을 통해서 통일성을 부여할 수 있다.  
  
  
}
```

### 복습하면서
이건 예제를 다시 보지 않으면 다시 만들기가 아주 힘들 것이다. 아주 많이. 그걸 바라는 것도 아니다. 왜 이렇게 바꾸는지를 알면 되는 것이다. 
***두려워 말라***