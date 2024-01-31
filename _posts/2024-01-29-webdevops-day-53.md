---
title: (Day	53) DBMS Programming
author: 김준회
date: 2024-01-29 17:00:00 +0900
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
[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8B%A4%EC%8A%B5%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B85.pdf)

이번 강의는 따라가느라 적은 내용이 상당히 적다... 반 강제적으로 한번 지식을 주입받았으니 내가 책이나 예제들을 보고 다른 포스팅을 남기면서 채워야겠다.

# Review
## DBMS Programming
* MySQL 설치 / 설정
  * 사용자 등록
  * 권한 설정

DBMS 프로그래밍이란? DBMS와 직접 통신하지 않는다. API를 호출하여, 대리하여 DBMS를 사용하게 한다. (이런 API들은 Vendor API, Native API라고 불리며 보통 C/C++로 작성된다.)

이러한 방법은 아래 순서처럼 발전했다.

* DBMS마다 다른 Native API에 맞춰 프로그래밍
* MS 주도하에 ODBC 등장하여, API 호출 규칙을 통일함
  * ODBC Driver는 프록시 역할을 하여 Vendor API가 동작
  * ODBC Driver는 ODBC API Spec 구현체를 지칭하는 용어
  * ODBC Driver는 사용법이 spec에 따라 통일되어 있으나, 구현체는 당연히  DBMS마다 다름.

* java
  * Type1: C/C++ 직접호출 불가능하므로 ODBC 구현체(드라이버)를 호출하기 위한 인터페이스가 만들어짐 (JDBC)
  * Type2: Type1의 성능 저하를 해결하기 위해, JDBC 드라이버를 벤더사가 작성하여 배포함
  * Type3: 중계서버를 둔 경우이다. 로컬(중계서버 JDBC Driver) - 중계 서버(JDBC API) - DB서버(DBMS) 형태이다. (속도상 문제로 잘 사용되지 않는다.)
  * Type4: App-JDBC Driver-DBMS 형태로, Native API를 대체하는 JDBC API를 벤더사가 제공한 경우이다. 대부분 이 방식을 택한다.

### MySQL 사용방법
* DBMS 설치 및 설정
* 사용자 추가 및 조회
* 데이터베이스 추가/삭제
* GRANT 명령어 (권한 설정)




# TIL
## Integrity Integration(?)
데이터의 온전함 (무결성)
  * 들어와야 할 형식에 맞는지
  * 필수로 들어와야 하는 값 중 빠진 것은 없는지
  * 동시성 제어 (세마포어 1 (Mutex))

## JDBC API Programming
### 순서
TODO

### ResultSet에 대해서
버거킹에서 와퍼를 주문하면 그 즉시 햄버거가 나오지 않는다.
영수증과 주문 번호표를 받는다.
DB도 그렇다. 쿼리를 날리면, 해당 데이터를 받는게 아니라 그 번호표를 받는다.
`ResertSet rs = stmt.executeQuery("SQL문");` 하게 되면,
 그 결과를 바로 받는 게 아니라 번호표를 받는 것이다. 한번에 하나의 레코드만 가져올 수 있다. (한 줄, 한 튜플, 1개 row...)
실제 가져올 때는 `rs.next()` 로 번호표를 제시하고 가져오는 것이다.


### CRUD

백엔드 프로그래밍은 결국 SQL문을 다루는 것
Create = INSERT
Retrieve = SELECT
Update = UPDATE
Delete = DELETE

## 조언
계속하여 해커톤이나 각종 대회 참여 (필요도 필요이나 재밌으니까!)
고객 니즈 파악에 대한 것이 중요. 
리팩토링, 디자인패턴도 중요하겠지만 고객니즈파악을 잘 할 수 있는 것이 아주아주 중요

조직이 크면 클수록 더 중요. 팀이 업무단위로 쪼개지면 니즈파악이 어려워짐.
(이전 직장에서 경험한 것..ㅋㅋ)

요새 IT 기업 팀은 3~4명 정도
그 팀이 UI UX Fullstack Monitoring 다한다. (자체 서비스)
그냥 요구사항 받아서 만들어주기만 하는 것은 예전 트렌드

해당 팀이 기획한 서비스가 잘 되면, 팀이 유지되거나 커진다.
근데 잘 안되면 팀이 공중분해되고 다른 팀으로 **이직**한다..🥹 

그래서 팀이 운영하는 서비스가 잘 안되는데, 그쪽 팀원이 맘에 들면 미리 **물밑작업🤣**으로 데려갈 준비를 한다.

근데 그게 마땅치 않으면 (데려가려는 사람이 없으면) 가고 싶은 팀에 면접을 통해서 들어가야 한다.

몇 천명이 있는 큰 조직이지만, 그 안에서 작은 소형 팀들이 마치 벤처기업처럼 서로 경쟁도 하고 협업도 하는 그런 형태다. 비대해진 조직의 관료주의적 폐해를 방지하기 위한 방법 중 하나..





