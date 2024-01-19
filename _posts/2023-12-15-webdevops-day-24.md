---
title: (Day	24) OOP; 추상클래스, 추상메서드, 오버라이딩
author: 김준회
date: 2023-12-15 23:00:00 +0900
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
## 생성자에 대한 설명?
클래스를 객체로 생성할 때(사용할 때) 단 한번 실행되는 메서드가 생성자다.
왜 생성자가 필요한가? 객체가 유효한 상태로 존재하기 위해서는 기본적인 설정이 필요하다. 
그 기본적인 설정을 하는 것이 생성자다. 만약 기본적인 설정을 할 게 아무것도 없다면, 아무것도 하지 않고 아무 정보도 받지 않는 생성자인 기본생성자를 쓰면 된다.

### String 생성자


## 클래스와 객체지향
클래스는 데이터와 데이터를 다루는 연산자를 설계한 것이다.

## 공부에 대해서
물건을 볼 때도 위에서 볼 때 앞에서 볼 때 옆에서 볼 때 다 다르듯이 계속 다양한 시각에서 봐야 이해하게 된다.

## 인스턴스 메서드와 스태틱 메서드의 구분

## 싱글톤 패턴의 목적과 용도?
인스턴스를 딱 하나만 만들고 싶을 때.
생성자를 private으로 만들고, 그 생성자 실행을 public static method로 처리. 한번 생성하고 더 생성 안하게는 그 메서드에서 처리한다.

## 팩토리 메서드 패턴의 목적과 용도?
인스턴스의 생성 과정(설정 과정)이 복잡한 경우, 그것을 간단하게 해주는 메서드를 만들어서 사용하는 것을 말한다. 

## 상속의 용도를 이해하고 구현하기
기존 코드를 손대지 않고 기능을 추가하려고 만든 문법이 상속이다. 추가나 복제후 변경은 유지보수가 어려워지는 문제가 있는데, 상속은 기존 코드를 그대로 두고 extends 키워드로 해당 코드의 변수와 메서드를 쓰겠다고 JVM에게 알려준다. (기존 코드를 복사해오는 것은 아니다)

### 상속은 다형적 변수를 가능하게 해준다.
다형적 변수는 무엇인가? 왜 쓰나?우리가 말을 할 때, 정확한 의미를 갖는 용어를 사용하기 보다는 그것을 포함하는 단어를 써서 말한다. 그것이 더 편하기 때문이다. 점심 먹었어? 라고 하지 짬뽕먹었어? 짜장먹었어? 이런식으로 물어보진 않잖아. 그게 프로그래밍에서 구현된 것이지.

### C++ Bjarne Stroustrup
비야네 스트로스투룹이 C++ 를 만들었다. 그가 인터뷰에서 말한 것이, 다중 상속을 허용한 것을 후회한다고 했다. 다중 상속이 가진 문제점과 컴파일러의 복잡성 증가가 그 원인이라고 한다. C++ 이후에 나온 Java는 다중 상속을 허용하지 않는다.

## 상속의 두 가지 목적
상속의 두 가지 목적은 서로 반대되는 것 같이 보인다. Specialization과 Generalization 두 단어는 서로 반대되는 것처럼 보인다.

### Specialization, 전문화
좀 더 전문화된 클래스를 만들고 싶은 것이다. 슈퍼 클래스의 메서드를 `Overriding`, 즉 `재정의`하여 전문화된 클래스를 만들고자 할 때 상속을 쓸 수 있다. 
이 경우 상속은 일반적인 클래스에 기능을 추가하거나 재정의하면서, 각각의 서브클래스를 전문화하는 것이다.

### Generalization, 일반화
전문화의 반대로, 일반화에 상속을 사용할 수도 있다.
차량으로 치면, 먼저 클래스들을 세단, 트럭과 같이 개별적인 객체를 나타낼 수 있게 설계한다.
그렇게 클래스들을 설계하고 나면, `코드 중복` 문제가 발생한 것을 알 수 있다. 
세단과 트럭은 유사성이 있는 차량이라는 범주 내에 있다보니, 데이터(모델명, 연비)나 연산자(가속하다, 감속하다) 같은 것들이 공통적으로 있는 것이다.
이런 공통되는 코드들을 뽑아서 차량이라는 슈퍼클래스를 만들어주면, 세단과 트럭에서 공통되는 부분을 차량이라는 하나의 클래스에서 공통으로 관리할 수 있게 되는 것이다.

### 정리하면
공통성이 있는 부분을 먼저 뽑아놓고, 각 세부 요소를 추가한 클래스를 만들어주면 그것이 Specialization이다. 

각 요소들을 만들고 보았더니 공통점이 있어서, 그것들의 코드 중복을 해소하기 위해 슈퍼클래스를 만들어주면 그것은 Generalization이다.

**이 두 가지가 상속의 목적이다.**

## 추상 클래스, 추상 메서드
Generalization 을 목적으로 상속을 활용하여 슈퍼클래스를 설계한 경우, 이 클래스는 직접 사용하는 것을 제한하는 것이 나을 것이다. 그를 위해서 등장한 것이 추상 클래스이다. 추상 클래스는 코드 관리를 목적으로 만든 슈퍼클래스다. 

### 리턴 데이터타입에 대해서
다형적 변수와 연계된 내용으로, 리턴 타입에도 다형적 변수 개념이 적용되어 그 리턴타입 및 리턴 타입의 서브클래스들 또한 리턴이 가능하다! 추상클래스와 연계해서 생각해보면, 리턴 타입이 클래스인 경우 아래와 같은 의미로 이해하면 된다.
1. 추상클래스이므로 해당 타입으로 리턴될 일은 없다. (인스턴스를 못만드니까)
2. 다형적 변수에 따라 그 추상클래스를 상속받아 만든 서브클래스들을 리턴타입으로 쓰겠다는 의미다.

### 추상 메서드에 대해서
공통 변수와 메서드를 뽑아서 슈퍼클래스를 만들었고, 이를 추상 클래스로 만들어서 공통 코드의 유지관리를 위해 사용하는 상황이다. 여기서 편리함에 대한 욕심(?)이 하나 더 추가된다. 공통으로 가지고 있는 메서드가 있어서 슈퍼클래스에 정의되어 있으나, 매번 재정의`오버라이딩`해줘서 사용해야 하는 클래스가 있다. 공통되는 코드라는 것을 알려주는 점에서 슈퍼클래스에 정의를 해 줘야 하는 메서드다. 

1. 서브클래스들의 메서드 이름의 일관성이 필요해서
2. 서브클래스들의 메서드 구현의 일관성이 필요해서

근데! 문제는 *실수로* 그 메서드를 재정의하지 않은 경우 슈퍼클래스의 공통 메서드를 실행하게 된다. 즉 재정의를 하지 않은 실수를 해도 나중에 알게 된다.
> 이건 재정의하지 않으면 실수야!

그래서 재정의를 필수로 해야 하는 메서드의 경우를 위한 문법이 발생했다. 그것이 추상 메서드 문법이다.

추상 메서드 문법은 서브클래스에서 재정의할 메서드를 나타내기 위한 문법이다. 서브클래스에서, 슈퍼클래스의 추상 메서드를 재정의하지 않으면 컴파일 오류가 발생한다. 개발자가 실수하지 않도록 재정의를 강제하는 것이다.

어떻게 쓰는가? 바디를 가지지 않은, 메서드 시그너처`Method Signature`만 가진 메서드를 `abstract` 키워드를 붙여주면 된다. 추상 메서드는 이러한 목적으로 고안되었기에 추상클래스나 인터페이스에서만 선언될 수 있다. 다시 말하면, 추상메서드는 실행이 불가능한, **미구현 된 메서드** 이기에 인스턴스로 구현하지 않는 추상클래스나 인터페이스에서만 가질 수 있는 메서드다.

```java
abstract class AbstractClass {
  ...
  abstract void abstractMethod();
  //추상메서드는 바디가 없이 시그너처만 가진다.
  ...
}
```

### 콘크리트 클래스
비유를 위해 쓰는 말이 아니라 공식적인 용어다. 추상클래스가 아닌, 인스턴스를 만들 수 있는 클래스들을 콘크리트 클래스라고 부른다. 추상클래스의 반대말이다. 

### 레퍼런스는 서브클래스만 가리킬 수있다.
슈퍼클래스는 항상 서브클래스보다 변수와 메서드가 적거나 같다. 그래서 슈퍼클래스에서는 서브클래스의 메서드나 변수를 쓸 수 없는 경우가 생긴다. 그래서 그걸 막아두었다. (컴파일러가)

### 오버로딩(overloading)?
파라미터가 다른, 같은 이름의 메소드를 사용하는 것이 오버로딩이다. 
파라미터의 형식(타입과 개수)은 다르지만 같은 기능을 수행하는 메서드에 대해 같은 이름을 부여함으로써 프로그래밍의 일관성을 제공하기 위한 문법이다.

기술면접이 빡세지고 있다. 내가 뭘 배웠고 뭘 할 수 있는지를 정확히 말해야 서류합이 되고, 면접에서는 기술면접을 봐야 하니 용어와 개념, 목적에 대해서 잘 말할 수 있어야 한다.

### 오버라이딩(Overriding)?
오버라이딩은 서브클래스가 상속받은 메서드를 재정의하는 것을 말한다. 서브클래스는 변수가 추가되는 등 슈퍼클래스랑 다른 점이 생기기 때문에, 상속받은 메서드를 변경하여 쓰고 싶은 욕구가 생긴다. 이것을 매번 새로운 이름의 메서드로 선언해야 한다면, 개발자들이 슈퍼클래스, 서브클래스 각각의 메서드들이 무엇이 잇는지 암기해야 할 것이다. 그래서 오버라이딩이라고, 서브클래스에서 상속받은 메서드를 재정의하는 기능을 만들었다.


부모 클래스로부터 상속  받은 메서드를 재정의할  수 없어서
새 메서드를 만들어야  한다면,
같은(또는  유사한)  일을  하는  메서드에 대해
안타깝게도  다른 이름으로 메서드를  만들어야 하기 때문에
개발자는 여러 개의  메서드를 암기해야 하는  번거로움이  생긴다.
=> 이런 문제를 해결하기 위해 등장한 문법이 "오버라이딩(overriding)"이다!

### 필드 오버라이딩 
요약 : 쓰지마세요.


![Do Not Use Sign Designs [PDF] - Free Printable Sign Designs](data:image/png;base64,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)

슈퍼클래스와 서브클래스는 같은 이름의 변수를 가질 수 있다. (파라미터의 순서나 타입이 달라야하는, 메서드의 오버로딩처럼 변수의타입은 달라야 한다.) 


이런 경우에, 해당 변수를 참조하는 경우, **레퍼런스가 선언될 때 타입으로 사용된 클래스에서 먼저 찾는다.** 슈퍼클래스의 변수를 덮어 써 버린 것이다. (Overriding)  

슈퍼클래스의 필드를 감춰야 할 이유가 있을까? 메서드는 유사기능을 재정의해서 써야 하는 경우가 많지만 필드는 그렇지 않다. 혼동을 가하기 쉽다. 

그리고 복잡하다. this 사용하는 경우 레퍼런스의 데이터타입을 말하는 클래스를 따라간다.  super 사용하는 경우 슈퍼클래스의 필드를 먼저 찾는다.

### 오버라이딩 실수가 발생할 수 있기에..
애노테이션 주석에서 컴파일러에게 검사를 요청할 수 있다
@Overrride 라고 컴파일러에게 알려주면 된다. 그러면 컴파일러가 오버라이딩해야 한다고 전제하고 문법 검사를 한다.