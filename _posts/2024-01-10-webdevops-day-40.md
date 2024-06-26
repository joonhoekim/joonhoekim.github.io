---
title: (Day	40) i18n, l10n, 제네릭과 DAO, Annotation 문법
author: 김준회
date: 2024-01-10 23:00:00 +0900
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
## Terminology
### Functional Interface
### Lambda Grammer
### Static Method Reference
### Instance Method Reference
### Constructor Reference
### forEach( [lambda] )

* 람다 문법과 메서드 레퍼런스 문법은 별개의 문법이며, 모두 functional interface를 위한 문법이다. 
* 람다 문법과 메서드 레퍼런스 문법이 없는 경우에도 기존의 문법들(로컬클래스, 중첩클래스, 익명클래스)로 구현이 가능하나 **간결함**을 위하여 사용된다. 그리고 다른 개발자들이 *아주 많이* 사용하기 떄문에 선택의 영역이 아니다.

# TIL
### "%c%d%c"
첫글자와 마지막 글자만 남기고 가운데 알파벳들은 그 개수로 대체한다. 왜 이렇게 표현하는가? 짧게 쓰려고...
자주 쓰는 용어가 너무 길면 이렇게 줄여 말한다.
```
i18n : Internationalization
l10n : Localization
k8n : kubernetes
```
i18n은 줄일 만 하지 않나요? 🤣🤣🤣 너무 길다.

---
### i18n (국제화/Internalization)
Internationalization
: 국제화로 번역되는데, `다국어 지원`이 훨씬 와닿는 단어.

콘솔에서 다국어 지원을 한다면,  이런 방식이 가능할 것이다.
```java
System.out.printf("%s: %s", getS1(), getS2());
```
이렇게 적절한 `라벨`을 가져와서 쓸 수 있게 만든다면 `i18n`을 수행한 것이다. 

---
### l10n (지역화/Localization)
그러면 localization은 다국어 지원을 못하게 막는 것인가? 그렇지 않다. 다국어 지원을 하려면 결국 각 언어별 정보가 필요하다. 이런 언어별 정보를 작성하는 경우 `l10n`을 수행한 것이라고 할 수 있다.

`*.xml` 파일이나 `*.properties` 파일 등에 `l10n`을 수행한 결과를 저장해두고 사용한다.

자바에서는 `System.getProperty("user.country");` 를 수행하면 (정보가 없으면 null) JVM이 OS에게 설정된 국가 정보를 받아오게 한다.

## 어떻게 i18n 하는가?
자바 기준으로 원리를 말하면,
* 사용자가 사용하는 언어 코드를 JVM을 통해 알아낸다.
	* 이를 Properties 클래스에서 지원한다. 
	* 코드는 `System.getProperty("user.language");`
* 사용자가 사용하는 국가 코드를 JVM을 통해 알아낸다.
	* 이 또한 Properties 클래스에서 지원한다.
	* 코드는 `System.getProperty("user.country");`
* JVM을 통해, 혹은 사용자가 설정한 국가/언어 정보에 따라 문자열을 출력하도록 한다.
	* `"%s-%s_%s.properties", filename, userLanguage, userCountry` 같이 규칙에 따라 `l10n` 파일을 읽어올 수 있도록 한다.
	* 이러한 파일 형식을 Properties 클래스가 지원한다.
	* 그러나 xml이나 json을 사용하는 경우가 더 많다. 이점이 더 많기 때문이다.

## 제네릭 (generic/ex02)
제네릭 또한 코드 중복을 해소하기 위한 방법이다.

### 제네릭 명명(Naming)의 관례
T - Type이라는 의미를 표현할 수 있어 많이 사용하는 이름이다.
E - Element라는 의미로 목록의 항목을 가리킬 때 사용한다.
K - Key 객체를 가리킬 때 사용한다.
N - Number의 의미로 숫자 타입을 가리킬 때 주로 사용한다.
V - Value의 의미로 값의 타입을 가리킬 때 사용한다.
S,U,V 등 - 한 번에 여러 타입을 가리킬 때 두 번째, 세 번째, 네 번째 이름으로 주로 사용한다.

### Autoboxing, Unboxing
계속 나온다~ 제네릭을 학습할 때도 나온다.
리터럴을 객체에 넣을 때 -> 자동으로 박싱 (오토박싱)
객체 내부의 값을 받을 때 -> 자동으로 언박싱 (언박싱)
***`자동으로` 라는 말은, 컴파일러가 그것에 필요한 코드를 추가하여 변환한다는 것이다.***

### 메서드에 제네릭이 없다면
메서드를 받을 파라미터의 타입별로 아주 많이 만들어야 한다면?
그런 비효율은 아주 끔찍하지요. 그렇다고 Object 타입으로 받는다면? 다시 강제 타입캐스팅을 해줘야 하는 불편함(+위험성)이 생깁니다.

### 클래스에 제네릭 문법이 없다면
클래스 안에 사용할 필드의 타입이나 메서드의 파라미터의 타입별로 클래스를 아주 많이 만들어야 한다면? 그런 비효율은 아주 끔찍하지요. 그렇다고 Object 타입으로 만들어서 Object 타입으로 리턴하는 경우라면? 마찬가지로 강제 타입캐스팅의 불편함이 생깁니다.

### 바이트 코드를 뜯어보면
제네릭 문법으로 선언된 타입들의 바이트코드를 뜯어보면, Object 타입인 것을 확인할 수 있다.
제네릭 문법의 유효성은 `컴파일러`가 검사하는 것이다. 제네릭은 컴파일러가 코드를 검사하게 하는 방법을 지정한 문법이다. 컴파일이 끝나고 나면 제네릭 문법은 바이트코드에 흔적을 남기지 않는다.

## 제네릭과 상속
레퍼런스 변수는 레퍼런스의 타입의 서브클래스들에 해당하는 인스턴스들도 담을 수 있다. 그런데 제네릭은 다르다. 제네릭으로 타입 파라미터를 잡아주면, 타입파라미터는 그 서브클래스를 포함하지 않는다. (컴파일러의 해석)

받은 타입 파라미터의 서브클래스까지 제네릭에 사용가능하게 하려면 명시해야 한다. \<? extends A\> 같은 방식이다.

반대로 슈퍼 클래스를 제네릭의 타입파라미터로 허용하는 방법도 있다. \<? super B1\> 와 같이 코드로 명시하면 된다. 

## 제네릭과 생략
제네릭을 사용하는 클래스임에도, 그 객체(인스턴스)를 생성할 때 타입파라미터를 알려주지 않고 생략해버리면, 컴파일러가 검사하지 않는다. 타입파라미터는 실제로는 (바이트코드는) Object 타입이나 컴파일러가  제네릭 문법에 따라 검사해주는 것이다. 컴파일러에게 어떻게 검사하는지 타입 파라미터를 알려주지 않으니 그냥 Object 타입에 대한 검사만 하는 것이다. 

그외 생략 관련해서, 이미 어떤 타입 파라미터를 쓰는지 레퍼런스 변수를 선언할 때 알려준 경우 **굳이** new 생상자 부분에서는 제네릭 안에 동일하게 반복해서 작성해 주지 않아도 된다.

## \<?\>
\<?\> 는 어떤 제네릭 타입을 지정하더라도 상관없이 받겠다는 것이다. 그런데 \<?\>로 레퍼런스를 선언하면, 그 레퍼런스로 메서드 사용시 유효성을 컴파일러가 검사할 수 없을 때 오류를 낸다.

그래서 \<?\>를 사용할 때는 보통 super, extends 키워드와 함께 쓴다.

## 제네릭과 다형성
제네릭을 사용한 클래스를 타입으로 사용하여 메서드 파라미터를 받는다.
이 때, 타입 파라미터로 \<?\>를 사용한 경우에 일어나는 일들을 이해하려면
컴파일러의 입장에서 생각해주어야 한다.

예시
: 아래의 코드에서 add() 메서드는 둘 다 컴파일 오류가 발생한다.
```java
// Object - A - B - C 순으로 슈퍼-서브 클래스가 있다고 가정하자.
static void m1 (ArrayList<?> list) {
	list.add(new Object); // 컴파일 오류
	list.add(new A); // 컴파일 오류
	list.add(new B); // 컴파일 오류
	list.add(new C); // 컴파일 오류
```
왜? 
: 컴파일러 입장에서 생각해야 알 수 있다.

ArrayList는 어떤 타입 파라미터를 받을지 모르는 상태이므로, 어떤 객체가 와도 되는지 (= 타입 파라미터) 알 수 없는 상태이기 때문이다. 이렇게 맞는지 틀린지 모르는 상황에서, 컴파일러는  모르는 건 틀렸다고 판단하도록 만들어졌다. 이것이 컴파일러의 입장이다.

더 풀어 말하면, 제네릭을 사용해서 만들어진 `ArrayList<E>()` 클래스는 ***나중에 컴파일 된다면*** 필드 및 메서드 파라미터를 Object 타입에 따라 만든다. **그렇게 컴파일이 되려면,** 컴파일러가 타입 파라미터 E에 맞춰서 나머지 코드가 잘 작성되었는지를 검사를 통과해야 한다. **그런데 `E`가 주어지지 않은 것이다. 그러면,** 컴파일러는 검사를 하지 못하므로, **타입이 결정되지 않은 상태**에서 **유효하지 않은 코드**가 있는 경우 **컴파일을 해주지 않는다.**
(E는 타입 파라미터를 지칭한다. 자주 사용되는 타입 파라미터의 이름을 내가 빌려왔다.)

`extends` 키워드를 사용하는 경우, 그러니까 `<? extends B>`와 같이 사용하는 경우에도 마찬가지다.

반대로 `super` 키워드는 컴파일러에게 판단 근거를 제공할 수 있다. `<? super B>` 같이 판단의 근거를 제공하는 경우는 컴파일러가 검사를 할 수 있다.

extends
---
```java
// Object - A - B - C 순으로 슈퍼-서브 클래스가 있다고 가정하자.
static void m1 (ArrayList<? extends B> list) {
	list.add(new Object); // 컴파일 오류
	list.add(new A); // 컴파일 오류
	list.add(new B); // 컴파일 오류
	lsit.add(new C); // 컴파일 오류
```	

super
---
```// Object - A - B - C 순으로 슈퍼-서브 클래스가 있다고 가정하자.
static void m1 (ArrayList<? super B> list) {
	list.add(new Object); // OK!
	list.add(new A); // OK!
	list.add(new B); // 컴파일 오류
	lsit.add(new C); // 컴파일 오류
```
extends 인 경우는 다형성에 따라 크게 문제없이 사용될 수 있겠으나, super 키워드로 판단 근거를 제공한 경우 컴파일러의 검사는 통과할 수 있을지라도 강제타입캐스팅이 필요할 수 있다.

### 비유하면
창고클래스< ? super SUV > 창고
창고.add(new SUV());   //OK
창고.add(new 모하비()); //OK
창고.add(new 승용차()); //NO
  
### 용어
Generic Type
: 메서드 사용할 때 Argument와 같이, 제네릭을 사용하는 쪽에서 주는 타입이 `generic type` 이다.

Type Parameter
: 메서드나 클래스에서 제네릭 문법을 사용할 때, \<\>로 선언한 타입은 `type parameter`가 된다. angle bracket(\<, \>)으로 감싸서 선언한다. 사용할 때는 그대로 쓴다. 타입파라미터를 선언할 때 네이밍은 보통 대문자 영어 한 글자를 사용한다.


## @ Annotation 문법
### 흔한 오해
주석은 컴파일 하고 나면 없어질텐데,
그렇지 않은 주석이 있다. 그것이 애노테이션이다. 
▲ 라는 설명이 있는데, `주석`이라는 번역어를 써서 발생한 문제.

영어를 쓰면 간단하게 생각할 수 있다.
`comment`와 `annotation`은 애초에 다르다.

그러면 `애노테이션 주석` 이라는 표현이 잘못된 것이라는 것을 알 수 있다.

### @interface
Annotation을 사용하려면 .java 파일로 소스코드를 작성해줘야 한다. 문법이 인터페이스랑 비슷해서 (아주 많이)  선언하는 키워드도 비슷하게 @interface 로 선언하면 된다. 

### Annotation's Method Naming
* 인터페이스에서 메서드 정의하는 것과 유사하다.
* 메서드 이름을 필드 이름 짓듯이 명명한다. 동사가 아닌 명사로 한다.
* 내부의 메서드는 메서드라 부르지 않고 '프로퍼티'라고 부른다.
* 값을 설정할 떄는 변수에 값을 설정하듯이 value="hello" 처럼 설정한다. 
* 클래스, 필드, 메서드의 선언이나 파라미터, 혹은 로컬 변수의 선언에도 붙일 수 있다.

### Annotation
* 값을 할당할 때는 선언과 동시에 가능하다. 
`@MyAnnotation(property=...)`
* property 이름이 `value`이고, 그것이 유일한 property인 경우에는 `value =` 을 생략하고 할당할 값만 적어줘도 된다.
`@MyAnnotation("Hello Annotation")`
* 어노테이션을 어디까지 유지할지 설정할 수 있다.
SOURCE / CLASS / RUNTIME 
설정하는 방법은 `@Retention(value=RetentionPolicy.SOURCE)`
	* RUNTIME으로 retention이 설정된 경우 Reflection API를 통해 값을 추출할 수 있다.
* 

# MyApp
## DAO (Data Access Object) 도입하기
자료구조나 데이터 저장 방식이 변경되더라도 다른 코드에 영향을 끼치지 않게 하자.

지금까지는 각 vo(Value Object)마다 핸들러 클래스를 만들어서 다뤘다.

### 장점
* 일관되게 데이터를 다룰 수 있다.

### 단점
* 다른 자료구조를 사용한다면 핸들러도 변경해야 한다.
* 다른 기술로 데이터를 다룬다면 핸들러도 변경해야 한다.
(vo 마다 핸들러가 필요한 건 물리적인 한계..)

왜 이런 단점이 있나? 캡슐화가 덜 된 것이다.

자동차 운전자는 핸들, 엑셀, 브레이크라는 공통된 `인터페이스`만을 사용할 줄 알면 그것이 전기차건, 디젤이건, 휘발유건, 하이브리드건 운전할 수 있다.

캡슐화를 더 진행하여 핸들러의 데이터 목록을 다루는 방식을 변경하더라도 핸들러는 변경할 필요가 없애볼 것이다.

### DAO 도입하기 (Data Access Object) 
추상클래스를 두고 vo마다의 DAO 클래스로 확장하도록 하자. 왜? 각 DAO마다 공통된 기능이 있을 것이니 슈퍼클래스로 일반화(Generalization)하고, 그 슈퍼 클래스는 직접 사용하지 않을 것이니 추상 클래스로 두려는 것이다.






# 그 외
### 수학 리부트에 대한 이야기를 해주심
concrete mathmatics 책을 추천해주셔서 보았다가 너무 어려워서 다른 책을 선행해서 봐야겠다 싶었다. 그래서 찾아보다가 수학 리부트라는 책을 찾고 그것을 보고 있는데, 엄진영 강사님하고 그 얘기를 했었다. 강사님이 그 책을 사서 보셨는데 얘기를 해주셨다. 

용어의 중요성, 그리고 그에 대한 기억력에 대한 이야기. 기억력이 좋지 않아도 익숙해지고 하다보면 된다는 경험담을 이야기 해주셨다. 