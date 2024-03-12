---
title: (Day	64) 서블릿이란?
author: 김준회
date: 2024-02-15 17:00:00 +0900
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
# TIL
Servlet Container

## Servlet
-let 은 작은 것을 의미하는 점미어다.
Bullet amulet...
server App + let
서버 앱의 작은 기능을 서블릿이라고 부른다.
서버 어플리케이션은 여러 작은 기능들을 모아서 만든다.
안사관리면 직원등록, 휴가신청, 퇴사관리, 경력증명서신청 등 여러 기능들이 있을 것이다. 작은 기능 각각은 서블릿이 될 수 있다.

## 기술면접
답변시 그걸 왜 하는지를 같이 말해줘야 함
ex. 오버라이딩: 상속받은 거 재정의하는 것. 이라고만 말하면 안됨. 왜 재정의하는가? 서브클래스에게 슈퍼클래스 메서드가 안맞는 상황이 발생할 수 있기 때문이다.
오버로딩은? 메서드 리턴타입이나 순서, 개수를 달리하여 같은 메서드 이름으로 사용하는 것, 이라고만 말하면 안됨. 메서드 사용 방법의 일관성을 유지하면서, 유사한 요구사항을 처리하기 위해 동일한 이름의 여러 메서드를 선언하는 것을 말한다.
인터페이스를 구현하는 상속클래스를 사용하는 이유는?