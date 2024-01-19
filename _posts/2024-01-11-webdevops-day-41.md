---
title: (Day	41) Annotation, 네트워크 변천사
author: 김준회
date: 2024-01-11 23:00:00 +0900
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
## Generic
### 제네릭이란?
**타입에 종속되지 않는** 메서드와 클래스를 만들어 동일 기능을 여러 타입에 대해 보편적으로 사용할 수 있게 해주는 문법이다. 다르게 말하면, General, 범용적인 클래스와 메서드를 만들 때 쓰는 문법이 generic이다. 
줄여 말하면, 범용 클래스를 제작하는 전용 문법이다.

### 제네릭의 사용법?
`<>` Angle bracket 안에 `Type parameter`를 선언하고 그를 변수처럼 쓴다. 타입을 변수처럼 쓰는 것이다.
제네릭 메서드에서는 `<타입파라미터>메서드명 (타입파라미터 로컬변수명)` 같은 형태로 쓸 수 있다. 이 때 argument로 들어온 (반대로 말하면 parameter로 받은) 로컬 변수의 타입을 타입 파라미터로 받아와서 쓸 수 있다.
클래스에서는 `클래스타입<타입>` 형태로 사용한다.

### 제네릭의 바이트코드?
generic 문법으로 작성하여 타입파라미터를 사용하는 경우, 그 타입이 실제로 컴파일 되면 항상 `Object`가 된다. 이것이 무슨 말인가? generic 문법은 컴파일러에게 검사규칙을 알려주는 문법이라는 것이다. 바이트코드로 실제로 무언가가 구현되는 문법이 아니라는 것이다. 원래 모든 타입을 다 받을 수 있는 Object 클래스를 타입으로 하는 레퍼런스이나, 컴파일 전 컴파일러 검사를 시키는 문법이 generic 문법이다.

### 클래스의 타입파라미터에 전달된 클래스 정보의 추출
```java
Class<?> genericDataType = (Class) ( // 클래스 타입으로 강제캐스팅  
  (ParameterizedType) this.getClass() // 필요한 캐스팅
  .getGenericSuperclass()). //  이 메서드 호출한 클래스 정보를 알아냄
  getActualTypeArguments()[0]; // 제네릭 타입 알아냄
```
풀어서 작성하면
```java
   
   void loadData(String filepath) {  
    System.out.println("loadData Test");  
    Class<?> clazz = this.getClass();  
    System.out.println(clazz.getName());  
    System.out.println(clazz.getGenericSuperclass());  
    ParameterizedType classInfoWithTypeParameters = (ParameterizedType) clazz.getGenericSuperclass();  
    System.out.println(classInfoWithTypeParameters);  
  
    System.out.println(classInfoWithTypeParameters.getActualTypeArguments());  
    // 제네릭으로 넘어간 전체 타입파라미터 배열을 요구한다면  
//    Type[] typeList = classInfoWithTypeParameters.getActualTypeArguments();  
//    for (Type type : typeList) {  
//      System.out.println(type.getTypeName());  
//    }  
  
// 0번 인덱스만 필요로 한다면  
  System.out.println(classInfoWithTypeParameters.getActualTypeArguments()[0].getTypeName());  
    Type type = classInfoWithTypeParameters.getActualTypeArguments()[0];  
    Class clazz1 = (Class) classInfoWithTypeParameters.getActualTypeArguments()[0];  
  
    // 그래서 결론, 제네릭타입을 받아오려면  
  Class<?> genericDataType = (Class) ((ParameterizedType) this.getClass()  
        .getGenericSuperclass()).getActualTypeArguments()[0];  
  
  
  }
```

### 와일드카드 타입 파라미터


## Annotation
### Annotation 정의
소스파일(*.java)를 만들고 @Interface로 선언한다.
### Annotation 사용
@AnnotationName 형태로 다른 소스코드에서 사용한다.
### Retention Policy
Annotation은 언제까지 유지(Retain) 할 것인지 정책을 설정할 수 있다.
`@Retention(value =  RetentionPolicy.CLASS)`
여기서 CLASS, SOURCE, RUNTIME을 정할 수 있다.
소스파일에만 보일 것인지, 컴파일 결과인 클래스 파일에도 남길 것인지, JVM에게도 visible한 RUNTIME으로 할 것인지를 결정할 수 있는 것이다.

### 속성 추가
### 속성 배열로 정의, 추가
### Annotation 
### 데이터 가져오기
Reflection API > getAnnotation()
```

# TIL
## Handler & DAO
Handler와 Data Access Object는 어떤 관계가 있을까?
Hander와 DAO는 어떤 관계가 있을까?

지금은 콘솔 기반의 프로그램을 작성해보면서 연습하고 있어서 잘 와닿지는 않지만,
GUI 기반의 프로그램을 작성하면 UI 와 관련된 코드가 아주 분량이 많아질 것이다.

DAO가 데이터 처리 코드만을 가지고 있다면 재사용성이 높아질 것이다.
지금과 같이 명령줄 기반의 인터페이스를, CLI를 쓰는 경우에도 같은 DAO를 call(호출)하여 사용하고,
GUI 핸들러를 쓰는 경우에도 같은 DAO를 콜하고,
웹페이지 핸들러를 쓰는 경우에도 같은 DAO를 콜한다면
지속적인 유지보수가 가능한 프로그램이 될 것이다.

> 유지보수는 `쉽다, 어렵다` 가 아니라 결국엔 `가능하다, 불가능하다`로 판단하게 될 것이다.
> 레딧에서 전직 CA직원이 토탈워 관련 AMA(Ask Me Anything) 진행하면서 기술부채 이야기를 하던데..

### `UI 처리 코드`와 `데이터 처리 코드`를 분리하면
DAO의 데이터 처리 방식이 바뀌더라도 Handler의 `UI 처리 코드`를 변경할 필요가 없다.
DAO의 데이터 처리 방식이 List를 쓰건, Map을 쓰건, DBMS를 쓰건, Excel을 쓰건 상관할 게 없어진다.
분리한다는 말 대신, 더 멋진 표현을 사용하면, DAO가 데이터 처리 과정을 `Encapsulation` 한 것이다.

DAO를 쓰지 않을 때는 이렇게 코드가 나온다.
```java
public class BoardAddHandler extends AbstractMenuHandler {  
  
  private List<Board> objectRepository;  
  
  public BoardAddHandler(List<Board> objectRepository, Prompt prompt) {  
    super(prompt);  
    this.objectRepository = objectRepository;  
  }  
  
  @Override  
  protected void action() {  
    // MenuHandler 인터페이스에 선언된 메서드 대신  
 // AbstractMenuHandler 클래스에 추가된 action() 추상 메서드를 구현한다.  Board board = new Board();  
    board.setTitle(this.prompt.input("제목? "));  
    board.setContent(this.prompt.input("내용? "));  
    board.setWriter(this.prompt.input("작성자? "));  
    board.setCreatedDate(new Date());  
  
    objectRepository.add(board);  
  }  
}
```
데이터 처리 과정에서 `List<Board>` 객체를 쓴다는 것이 드러난다. 데이터 처리 방식이 변경되면 핸들러도 모두 고쳐야 하는 것이다.

## myApp-39
* 기존 방식은 인덱스를 사용했다.
	* 데이터를 삭제하면 인덱스가 변경되는 방식이었다.
	* 데이터 조회시 일관성이 없어지는 문제가 있었다.
* 개선 방식은 고유의 식별번호를 부여하는 방식으로 한다.
	* 데이터를 삭제하더라도 삭제하지 않은 다른 데이터들의 식별 번호는 그대로 유지된다.
	* 데이터 조회시 식별 번호의 일관성이 유지된다.

=> private int lastKey 필드를 추가하고 각각의 아이템들이 uniqueNumber를 갖게 한다.

## myApp-40 인터페이스 적용하기
Interface를 적용하자. json, XML, Excel, DBMS... 어떤 데이터를 다루건간에 하나의 인터페이스로 다룰 수 있도록 하자.

`핸들러 - DAO인터페이스 - DAO추상클래스 - DAO 콘크리트클래스 - 데이터`

여기서 핵심은 단일 핸들러를 통해 다양한 데이터 타입을 위한 여러 DAO를 사용할 수 있게 된다는 것이다.

> DAO를 교체하더라도 사용법(인터페이스)은 바뀌지 않으므로 핸들러는 교체할 필요가 없다. 
> 차(DAO)가 바뀌어도 엑셀/브레이크/핸들(인터페이스)의 사용법은 동일하므로 운전자(핸들러)를 바꿀 필요가 없다.

### 그러면 어떻게 구현하는게 좋을까?
* 모든 DAO 클래스는 BoardDAO 인터페이스를 구현해야 한다.
* 모든 구상 DAO 클래스는 적합한 DAO 추상 클래스를 상속받아야 한다.

## myApp-41 드디어 네트워킹
`Personal Application` 이라면 `파일`로 데이터를 `혼자서` 저장하고 불러오면 되겠지만,
`Business Application` 이라면 `여러 사람`이 `데이터`를 `공유`하면서 사용해야 할 것이다.

예를 들면 인사팀의 직원 A가 인사정보를 바꿨다면, 인사팀 직원 B 혹은 다른 팀 직원 C도 그 정보를 공유받아 조회할 수 있어야 할 것이라는 이야기다.

Standalone Application Program ➡️ C/S Application Program, 초기에 배웠던 내용이 드디어😃 나왔다. 영광스런 아키텍처의 변경 과정을 목도하라. Architecture goes on.🤣 
Standalone ➡️ C/S ➡️ C/S+DBMS ➡️ Application Server ➡️ Web App ➡️ AJAX
* Standalone: 언어 기본 사용법, OOP 개념, I/O
* C/S: networking, threading
* C/S + DBMS: SQL, JDBC Programming
* Application Server: IoC Container usage, Spring IoC Container
* Web App: Servlet/JSP, HTML/CSS/JavaScript
* AJAX: AJAX, advanced Javascript, React

실습은 내일!


### 네트워킹 변천사
네트워크를 이용하는 방식도 당연히 역사가 있다.
1. 공유 폴더나 공유 디스크를 사용하는 방법
여러 컴퓨터에서 동일한 프로그램을 각각 실행한다. 그리고 네트워크를 통해 공유하는 `디스크` 혹은 `폴더`의 파일을 사용했다.
여러 어플리케이션이 같은 파일을 동시에 변경하면 다른 어플리케이션이 변경한 내용을 덮어쓰는 문제가 생길 수 있다. (Thread-Safe와 유사하다. mutex & semaphore)
2. 파일 관리를 위한 어플리케이션을 사용하는 방법
파일에 대한 read, write 등의 요청을 파일 관리 App에게 위임하는 방법이다. 파일 입출력에 대한 통제를 파일관리 Application에게 위임하여, 하나의 파일에 대해 여러 App이 동시에 wrtie하여 데이터를 서로 덮어 써버리는 문제를 해결한 것이다. 여기서부터 `client`와 `server` 라는 개념이 생긴다. 파일 관리라는 서비스를 받는 `client`와 그 서비스를 제공하는 `server`가 구분된 것이다. 이렇게 client/server 구조의 App을 C/S App 혹은 C/S Program이라고 부른다.

### Local App에서 C/S App으로 전환하기
1. 프로그램을 분리해야 한다. 클라이언트를 위한 앱, 서버를 위한 앱 둘로 분리한다.
2. 클라이언트는 요청을 할 수 있어야 한다.
3. 서버는 요청을 듣고 응답을 할 수 있어야 한다. 요청에 따른 데이터 처리는 read/write 기능으로 처리한다.

### 전환을 위해 필요한 기술
1. Networking Programming
2. Multi-Threading (for multiple client)

connection oriented: tcp
ㄴ stateful / stateless
connectionless: udp




## 나중에 연습 프로젝트를 할 때
요구사항 바꿔서 기능 수정하는 걸 두려워 하지 말자!
바꾸는 이유가 타당하면 코드 전체를 뜯어 고치는 한이 있더라도 바꿔라!
제품 만드는 것도 아니고 초보때는 많이 바꾸면 바꿀수록 실력이 늘어난다.
실력을 늘리려고 시간을 투자하고 있는데
**바꾸기가 귀찮다**는 생각을 하면 아주 아주 멍청한거다.
목적을 상실한 것이다!

# 잘못 표현했던 것
클래스를 `new 생성자()` 로 `인스턴스화`하면 힙 메모리에 클래스를 설계도로 한 인스턴스가 생성된다.
=> 이거 엄밀히 말하면 틀렸다. 클래스는 필드와 메서드가 있는데, 메서드는 항상 Method Area 영역에만 존재한다. 그것이 static method 이거나 instance method 상관없이 그렇다. 인스턴스 필드만 힙 영역에 메모리를 할당받는다.