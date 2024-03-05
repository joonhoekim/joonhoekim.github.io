---
title: (Day	74) MVC 모델 구현하기
author: 김준회
date: 2024-02-29 17:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---
* 실습 따라할 부분이 많아서 내용이 연습장 수준입니다 ㅠㅠ 

# MVC 모델1, 모델2
모델1: 뷰와 로직을 JSP 페이지 하나에서 처리. 서블릿을 전부 JSP에 옮겨버림. 그만큼 JSP 코드가 길어지는데 JSP는 출력을 편하게 하려고 만든 것인만큼, java 소스코드를 작성하기엔 불편한점이 있음.

모델2: 요청을 서블릿만 받는다. 출력(뷰)는 JSP가 담당한다. 로직에 대한 처리는 자바 클래스들 (서비스나 DAO)이 처리한다.

# JSP 태그
## Directive Element
* <%@ page [ ] %>
  * JSP 페이지에 대한 설정
* <%@ include [ ] %> 
  * 다른 파일의 컨텐츠를 삽입
* <%@ taglib [ ] %>
  * 외부 태그 라이브러리를 가져오기
## Scriptlet
* <% [ java code ] %>
  * 서블릿 클래스에 삽입할 자바 코드 
* <% =[표현식(expression)] %>
  * 표현식의 리턴 값을 출력 (Expression element)
* <%! [필드 및 메서드]>
  * 서블릿 클래스에 필드 및 메서드를 삽입한다. (Declaration element)
* <jsp:[ ] [ ]/>
  * JSP Action tag

# EL: Expression Language
${ [객체].[프로퍼티].[프로퍼티]... } 같은 방식으로 사용된다. 이렇게 연결된 모양에서 Object-Graph Navigation Language 표현식 이라고 한다. 길다보니 두문자어로 축약해 OGNL Expression 이라고 표현된다. 

```
class Car {
  Engine engine;
}

class Engine {
  Maker maker;
}

class Maker {
  String name;
}

Car car = getCar( ... );

String name = car.engine.maker.name;
```

마지막 문장을 보면 객체 관계를 따라 올라가는 것을 볼 수 있다. 이렇게 객체간 관계를 따라가는 경우의 표현을 OGNL 표기법을 이용한다고 한다. 객체들간에 연결된 관계인 그래프(Object Graph)를 탐색하는(Navigation) 언어(Language)를 OGNL이라고 한다.

여기서 그래프는 객체관의 관계를 나타내는 것을 의미한다.

이게 JSP와 무슨 상관인가?
## JSON과 EL
```
<%객체 가져오기%>
<%=객체.게터()%> 
```
이렇게 안해도 된다..

```
${requestScope.객체.변수명}

혹은

${객체.변수명}

라고만 적어도 된다.
```
그러면 리퀘스트스콥이 알아서 범위를 바꿔가며 객체를 찾고 게터를 호출한다... 프라이빗 인스턴스 변수여도 게터가 아닌 저렇게 변수명을 OGNL로 불러도 된다는 것..

# Front Controller
프론트 컨트롤러가 무엇인가? 앞에서 요청을 각각의 서블릿이 받도록 만들었다.

거기서 한번 더 발전시켜서, 하나의 서블릿이 모든 요청을 받도록 한다. 그렇게 해서 각각의 서블릿에서 중복되는 코드를 그 서블릿으로 이전시킨다. 요청을 받아서 다른 서블릿으로 배달해주는 역할을 한다는 점에서, 이 서블릿을 DispatcherServlet이라는 이름을 쓰는데 Spring Framework도 동일하게 동작한다. (스프링 프레임워크의 프론트 컨트롤러 구현은 훨씬, 훨씬 복잡하지만..)

이는 GoF의 디자인패턴으로 명명되었다. Front Controller 패턴. 반대로 요청을 배달받는 다른 컨트롤러는 Back Controller라고 할 수 있다.

Front Controller 역할을 하는 서블릿은 들어온 요청을 분석하여 `getPathInfo()` 어떤 서블릿을 요청하는 것인지 확인한다. 그러면 (보내기전에 필요한 작업이 있다면 하고 나서) 해당 서블릿을 include 한다. Back Controller는 필요한 작업을 하고, DispatcherServlet과 공유하는 요청 객체에 `setAttribte()`로 필요한 객체나 뷰 역할을 하는 jsp 주소를 담아서 리턴한다.

# 상대경로 / 절대경로
경로의 시작에 `/`를 붙이냐 안붙이냐가 중요하다
안붙으면 상대경로다. 상대경로는 현재 웹브라우저의 경로를 기준으로 계산된다. 현재 읩브라우저의 경로 뒤에 붙는다는 것이다.

`http://abc.com:1234/app/member/[자원이름]` 같은 URL에서, 엄밀히 말할 때 경로는 자원이름을 제외한 것이다.

`http://abc.com:1234/app/member` 까지가 경로다. 그리고 이것이 `상대경로`에서의 `현재 웹브라우저의 경로`를 말한다.

절대 경로는 포트번호 다음의 부분에 붙는 경로다.

# Front Controller 도입 이후 차이
그것은 `POJO` 로 전환이 가능해졌다는 것이다. (Plain Old Java Object) 이름 잘 지었네... Plain Old는 특별한 확장 라이브러리를 사용하지 않은 Java 클래스를 의미한다.

왜인가? PageController는 서블릿이 아닌 다른 일반 자바 클래스가 되어도 문제가 없어졌기 때문이다.

