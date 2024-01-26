---
title: (Day	52) ODBC, JDBC, DBMS(MySQL)
author: 김준회
date: 2024-01-26 17:00:00 +0900
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
# TIL
## Framework
### Netty
ExecutorService와 같이 스레드풀 관리를 위한 API가 있듯이,
서버 관리를 위한 프레임워크가 있다. 한국인 개발자가 만들었다!!

## DBMS
[아주 자세한 설명](https://en.wikipedia.org/wiki/Database)

Database는 번역이 필요없어진 외래어다.
데이터베이스는 아주 간단하게 말하면 데이터를 모아둔 것이다.

DBMS는 Database Management System이다.
데이터를 모아둔 것을 편하게 쓸 수 있도록 도와주는, 미리 설계된 것이다.

### NoSQL?
DBMS의 히스토리에 따라서 RDMBS 다음 세대로 NoSQL 이야기가 나온다. 그 다음에는 New SQL 이야기가 나오는데, 각각의 DBMS들은 반드시 서로를 대체해나가는 것이 아니다. 이전 세대의 문제점이 발견된 사례를 극복하기 위해 다음 세대가 만들어지는데, 이전 세대의 장점이 여전한 경우는 그대로 사용될 수 있다. 많은 부분이 그렇게 여전히 사용된다. 그리고 이전 세대가 다음 세대보다도 더 나은 성능을 보일 수도 있다.

### Management?
* 데이터 정의
* 업데이트
* 검색
* 관리
  * 사용자 등록/모니터링
  * 데이터 보안
  * 성능 모니터링
  * 데이터 무결성 유지
  * 동시성 제어
  * 오류 제어

이렇게 나열한 것 말고도 많은 기능들이 있다.

### DBMS / App 간의 프로토콜
DBMS 제조사는 프로토콜을 오픈하지 않는다. DBMS 자체의 소스코드도 공개하지 않는다. 
(물론 오픈소스 프로젝트도 있지만 말이다.)

보안도 이유지만, 오버헤드를 제거하는 방법 등 핵심 기술력이 노출되기 때문이다.
그래서 API를 제공해준다. API-DBMS 간에 private protocol로 통신하고, App-API 간 call/return을 주고받는 것이다.

DBMS에 전용 프로토콜을 사용하여 통신하는 Client API는 C/C++ 같이 시스템콜 바로 사용하여 빠르게 동작하게 할 수 있는 언어로 작성되어있다. Client API는 Vendor API라고도 불리며, Native API라고도 불린다. 

### Native API
Native API를 사용하는 경우 이런 특징이 생긴다.
* 비공개 프로토콜을 사용하기 위해 API를 호출하면 된다. 
* DBMS마다 각각 작성해주어야 하기에, 여러 DBMS를 쓴다면 비효율이 생긴다.
  * 변수명, 파라미터 타입, 메서드 이름 등등... API마다 다르다..
  * 비용이 증가한다.
  * 게빌의 일관성이 떨어진다.

물론 이런 단점들이 있는 걸 알아도 대안이 없던 경우엔 각각 Native API와 통신하는 방식으로 프로그래밍했다.
나중에 인터페이스처럼 중간에 하나를 더 두어서 사용방법을 통일하는 대안이 나왔다.

### ODBC (Open Database Connectivity)
[wiki](https://en.wikipedia.org/wiki/Open_Database_Connectivity)

처음에 ODBC가 등장했다.
각각의 Native API를 위한 ODBC구현체를 만든 것이다. 
(별도의 디자인패턴이라기보다는 `표준 API` 적용이라고 볼 수 있다.)

DBMS를 쓰려는 앱은 ODBC를 호출하여 동일한 방식으로 각 벤더들의 DBMS를 쓸 수 있다.
그런데, ODBC도 C/C++로 작성된거라 java에서는 쓸 수가 없었다...
java App은 C/C++ API를 직접 호출할 수 없기 때문이다.
java server app developers 역시, native api에 종속되지 않는 표준적인 API를 쓰고 싶을텐데 말이다!!
그래서, ODBC를 java에서도 쓸 수 있게 JDBC라는 걸 만들었다.


## JDBC API
이렇게 JDBC API가 등장했다. (JRE에 기본으로 포함되었다!)
`DBMS - Native API - ODBC - JDBC Driver - JDBC API - java App` 이렇게 길다고?
실제로도 그렇게 길다... 그래서 비효율성이 발생한다. 아래 사진에서 Client App -> JDBC Driver -> ODBC Driver -> DBMS 형태이다.

![](https://techdifferences.com/wp-content/uploads/2016/11/JDBC-Vs-ODBC.jpg)

[출처](https://techdifferences.com/difference-between-jdbc-and-odbc.html)는 TechDifferences인데, 여기 좋은 내용이 아주 많다!

그래서 그런 비효율을 해결하기 위해, JDBC Driver에서도 Native API를 Call 할 수 있도록 한 단계를 제거하였다. 이것은 JRE에 기본적으로 포함되는 내용이 아니며, vendor에서 직접 단운로드 해야한다. local에 설치된 Native API를 사용한다. 이것을 이전의 드라이버와 비교하기 위해 Type2 드라이버라고 부른다. ODBC를 java에서 사용하기 위한 드라이버는 Type1 드라이버라고 부른다.

다음에는 JDBC API가 DBMS와 `전용 프로토콜`을 통해 **직접** 통신해버리게 만든다.
이건 Type4 드라이버라고 부른다. 네트워크 프로토콜로 직접 호출해버리는 네트워크 드라이버다. Vendor가 제공해주기에 유저가 직접 다운로드 받아야 한다.
Native API를 경유하지 않으므로, Local에 C/C++로 작성된 Native API를 설치할 필요가 없다.
다른 이름으로, 순수 자바 드라이버(pure java 혹은 pure java driver)라고도 한다.

> 참고로, Type2, Type3, Type4 드라이버는 DBMS를 교체하면 로컬에 설치한 드라이버도 교체해야 한다.
> 그러나 어플리케이션의 코드는 바꿀 필요가 없다. API를 호출하는 방법은 모두 동일하기 때문이다.

Type3도 당연히 있다. 네트워크 드라이버를 써서, 중계 서버를 통해서 특정 DBMS에 종속되지 않는 DBMS와의 통신을 구현하는 방법을 Type3 드라이버라고 부른다. 
>중계/중개 헷갈릴 수 있는데, 여기서는 중계가 맞다. 대리로 무언가를 행할 때는 `중계`라는 용어가 맞고, 중개는 부동산 중개처럼 두 당사자 사이에서 일을 주선하는 것을 말한다 (보통 수수료를 위해)

ODBC 같이, 표준 API를 이용하여, Vendor API마다 개발하는 일을 피할 수 있게 되었다.

### 이런 DBMS의 교체(변경)가 자주 일어나는 일일까?
가상의 일로 생각하면 잘 다가오지 않는다.
기사들을 보자.

[삼성전자, 차세대 ERP 시스템 구축…SAP S/4HANA적용 글로벌 첫 사례](https://m.ddaily.co.kr/page/view/2021040514191680156)

[교통안전공단, 전사 `오라클 퇴출` 작업 착수..국산DB 대체](https://www.etnews.com/20151218000443)

DBMS 벤더들은 일반적인 소비자용 소프트웨어랑 달리, 지속적인 비용이 발생한다. (사실 이것도 옛말이다. 2010년 중반을 넘기며 구독제로 지속적인 이익을 창출하는 소프트웨어 개발사가 많아졌으니)

소프트웨어에 대한 유지보수, 기술지원 등 서비스가 계속 제공되며, 그러한 소프트웨어를 사용하는 기업들이 소프트웨어를 통해서 지속적인 이익을 창출하니 사실 이상한 일은 아니다.

어쨌든 주기적으로 소프트웨어 비용을 계속 내다보니 DBMS에 대한 교체욕구도 발생할 수 밖에 없다.
* 오라클이 너무 비싸니, 더 저렴하고 기술지원 빠른 국내사로...
* SAP에서 온메모리 DMBS 겁나게 빠르다던데? ERP 쓰고 있으면 할인도 해주고...
그러다보니 DBMS 교체 사업이 고도화 프로젝트 같은 이름을 달고 기업에서 시행이 되는 것이다.
그때마다 서버 프로그래밍을 다시 해줘야 한다면? 너무 힘들 것이다.

## MySQL
MySQL은 교육용, 개인은 무료 에디션 사용이 가능한데, 매출이 발생하면 무료 제공이 중단된다. 이 매출은 광고와 같이 간접적인 경우도 모두 포함이니 주의할 것.
### 설치
brew update
brew install mysql
brew service mysql start

### 로그인
mysql -u root -p

## 글로벌 경쟁 심화
* 제품 라이프 사이클의 감소

태스크포스
* 작업 중심의 팀 빌딩
* 제품이 바뀌면 작업도 바뀐다.

=> 제품 라이프 사이클이 길 때는 1년~2년에 한번씩 아웃소싱하는 식으로 개발했다면, 라이프사이클이 짧아지면서 직고용/직개발하는 형태로 변경.

## 아키텍처의 변화
Standalone Application
Client Application / Server Application
Web Client Application / Web Server / Web Application / DBMS
클라이언트는 웹브라우저가 하면 되잖아?
Web Browser / Web Server / Application / DBMS


### SQL도 클라이언트 쓰면 C/S App이다.
1. 직접 클라이언트(mysql.exe) 써서 통신하는 경우 특정 프로토콜로 요청과 응답을 반복하는 C/S 어플리케이션이다.
2. mysql-connector-j.jar 같은, 타입4 드라이버를 사용하는 경우에도 JDBC Driver - Server 간 CS App 이다.
3. 


# DBMS DAO 
* JDBC 드라이버 준비하고, java.sql.Driver 구현체를 생성하여 DriverManager에 등록한다.
```java
  Driver driver = new com.mysql.jdbc.Driver();
  DriverManager.registerDriver(driver);
```
사실 위 코드는 필요하지 않다.
external library에서, mysql-connector-j-8.0.33.jar!\META-INF\services\java.sql.Driver
를 보면 안다.
(META-INFL 폴더명도 관례다)

* DriverManager를 통해 DBMS 연결을 요청한다.

## 컨벤션, 트렌드보다 중요한 것
같이 일하는 사람들에게 예측가능성을 주는 것
더 나은 트렌드가 있어도 팀이 받아들일 준비가 된 상태가 아니면
개인이 혼자 그것을 끌고갈 수 없음!