---
title: (Day	79) 미니 스프링 프레임워크
author: 김준회
date: 2024-03-08 17:00:00 +0900
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

[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8B%A4%EC%8A%B5%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B88.pdf) 

# 코드에 대해 익숙해지면
객체를 하나의 주체, 사람처럼 이해해서 읽게 된다. 코드를 더 빠르게 읽게 해주며, OOP의 방향을 따라가는 방법이다. 반면에 실제 코드를 따라가는 경우라면 객체가 아닌 메서드를 중심으로 흐름을 읽게 된다.

처음에는 메서드를 중심으로 읽게 된다. 메서드가 무슨 행동을 하는지에 대해서 익숙하지 않기 때문에. 

메서드도 연산자다. 인스턴스는 힙에 들어가 있는 데이터이다. 힙에는 클래스가 가진 생성자 목록, 인스턴스/스태틱 블럭, 멤버 같은 정보가 들어가 있다. 

```java
Constructor<?> constructor = clazz.getConstructors()[0];
```

`.getConstructors()[0]` 메서드가 호출되면, 이 기능은 `Constructors[]` 배열을 반환한다. 

## 객체지향

clazz.getConsntructor() 에서 clazz = i, getConstructor() = ++ 로 봐서 i++와 같은, 피연산자와 연산자의 조합이라고 볼 수 있다. 

- 클래스 객체(clazz)에게 메시지를 준다. 생성자들 찾아서 첫번째 꺼 담아줘.

객체지향 시점에서는 다른 용어를 쓴다. `Object & Message`. 객체에게 명령을 메시지로 주는 것이다. 

코드가 몇천줄 정도일 때는 method 위주로 작성하면 되지만, n만줄이 되면... 절차적으로 해석하는 것의 한계가 온다.

# 경험 누적
인류의 발전은 경험의 누적과 동일했다. 경험의 누적이 끊긴다는 것은 발전이 퇴보한다는 것이다. 

기술은 계속해서 누적되었으나 사회에서 관계에 대한 지혜들은 누적이 끊기고 있다. 친구, 이성간의 관계, 인생관 등..

스스로를 아끼고 관계에 대한 지혜를 누적시키는 집단에 들어가야 한다.

## 오류 사례 추가
files 같은, 도메인 객체와 컨트롤러 객체의 이름이 충돌하는 경우.
 

# 웹 개발자가 하는 일들
도메인, DAO, 컨트롤러 클래스 만들기
## 에러 해결
### 스프링에서 null을 못받으므로 발생하는 로그인 오류 수정
Controller에서 쿠키에 원하는 값이 없어서 발생하는 문제를 수정 (필수 요구를 하지 않거나 기본값을 주는 방식임. 애너테이션 문법에 따라 작성하면 됨)

아래 두 방법 모두 적용 가능함. 적절하게 사용.

@CookieValue(value = "email", required = false)
@CookieValue(value = "email", defaultValue= "")

넘어오는 데이터가 잘못된 것인지, DB쪽에서 잘못된건지 무엇이 잘못된건지 찾는 것이 중요한데, 에러 해결 경험이 중요함. 

추가예시: 체크박스는 체크하지 않으면 값이 아예 안넘어가고, 인풋박스는 넣은 값이 없으면 빈문자열이 넘어감.