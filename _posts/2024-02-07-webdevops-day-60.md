---
title: (Day	60) JDBC와 DB 모델링에 관하여
author: 김준회
date: 2024-02-07 17:00:00 +0900
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

# Review
## DB 모델링에 대해서
도서관이나 수족관같이 엄청난 하중을 지탱해야 하는 경우 구조상 슬래브나 컬럼 단면이 일반 주거용 건물과는 달라지게 된다. 소프트웨어도 목적에 따라서 DB가 달라진다. 
### 요구-개념-논리-물리


## 제3정규화
제3정규화는 `이행적 함수 종속성`이라는 용어가 나오면서 사람들이 쉽게 이해하는 것을 방해한다. 

>아래는 ChatGPT로 생성한 결과를 다듬은 것이다. 잘못된 예시를 가져오길래 수정하고 내용을 덧붙였다..

다음과 같은 제품 주문 테이블이 있다고 가정해 봅시다.
```
주문 (Order)
주문번호 (OrderID)   제품코드 (ProductCode)   제조사 (Manufacturer)
1                   LAP001                  ABC 주식회사
2                   PHN002                  XYZ 주식회사
3                   TAB003                  ABC 주식회사
```
여기서 "제품코드"는 각 제품을 고유하게 식별하는 데 사용됩니다. 그리고 "주문번호"는 주문을 고유하게 식별합니다.
이 경우 "제조사"는 "제품코드"에 종속됩니다. 즉, 특정 제품 코드는 특정 제조사에 의해 만들어진 제품을 식별합니다. 또한 "주문번호"는 주문을 식별합니다.
따라서 "제품코드"가 주어지면 "제조사"를 알 수 있습니다. 그리고 "주문번호"가 주어지면 해당 주문에 속한 제품의 "제조사"를 알 수 있습니다.
이것이 이행적 함수 종속성의 예시입니다. 데이터베이스 설계에서 이러한 종속성을 해결하기 위해 데이터를 적절하게 분리하여 정규화를 수행합니다.

위의 예시에서 "제조사"가 주문 테이블에 중복하여 저장되어 있기 때문에 데이터 중복 문제가 발생할 수 있습니다. 이는 데이터 무결성을 해치는 요인이 될 수 있습니다.

예를 들어, ABC 주식회사의 이름을 변경해야 하는 경우라고 가정해봅시다. 이 경우 주문 테이블의 모든 해당 제품의 "제조사" 열을 수정해야 합니다. 중복 데이터를 수정하는 것은 번거롭고, 데이터 일관성을 유지하기 어려울 수 있습니다.

또한, 만약 같은 제조사가 여러 제품을 생산하는 경우에는 데이터 중복이 더 심각한 문제가 될 수 있습니다. 예를 들어, ABC 주식회사가 여러 제품을 생산하는 경우에는 ABC 주식회사의 이름이 여러 번 중복 저장됩니다. 이는 데이터를 효율적으로 사용하지 않는 것이며, 저장 공간의 낭비로 이어질 수 있습니다.

이러한 데이터 중복은 데이터의 무결성을 위협할 뿐만 아니라 데이터베이스의 성능에도 영향을 미칠 수 있습니다. 중복된 데이터를 저장하면 데이터베이스 크기가 커지고, 데이터 검색 및 업데이트 작업이 더 복잡해질 수 있습니다.

따라서 이러한 이행적 함수 종속성을 해결하기 위해 데이터베이스 설계자들은 정규화 과정을 통해 데이터를 적절하게 분리하여 중복을 최소화하고 데이터의 일관성과 무결성을 유지합니다.

## Engineering
### Forward Engineering
ERD등 논리적인 모델을 물리적인 코드 등으로 변환
(보통 새로운 소프트웨어를 만들 때)
### Reverse Engineering
물리적인 코드 등을 논리적인 모델로 변환
예시: DDL을 모델로 변환
(보통 기존 소프트웨어를 분석할 때)

| **비교 항목**   | **Forward Engineering**                                                  | **Reverse Engineering**                                                                 |
| --------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| **정의**        | 새로운 소프트웨어를 개발하기 위해 설계된 모델을 코드로 변환하는 프로세스 | 기존의 소프트웨어나 시스템을 분석하여 모델을 생성하거나 역으로 코드를 생성하는 프로세스 |
| **목적**        | 설계된 모델을 구현하여 소프트웨어를 개발 및 배포                         | 기존 소프트웨어를 이해하고 문서화하거나, 수정, 업그레이드, 재디자인하는 데 사용         |
| **프로세스**    | 모델링 → 코드 생성 → 테스트 → 배포                                       | 소스 코드 또는 실행 코드를 분석 → 모델 생성 또는 수정 → 문서화 또는 역 코드 생성        |
| **대상**        | 새로운 소프트웨어나 시스템                                               | 기존 소프트웨어나 시스템                                                                |
| **입력**        | 모델 또는 설계 문서                                                      | 소스 코드, 실행 파일, 데이터베이스 스키마, 문서, 다이어그램 등                          |
| **출력**        | 소스 코드, 실행 파일, 데이터베이스 스키마 등                             | UML 다이어그램, 문서, 코드 등                                                           |
| **적합한 상황** | 새로운 소프트웨어를 개발할 때                                            | 기존 소프트웨어를 분석하거나 변경할 때                                                  |

## JDBC
JDBC Programming은 JDBC API를 통해 DB를 다루는 프로그래밍을 말한다.
JDBC API는 사용방법이다. 인터페이스와 관련 클래스들이다. DBMS 벤더마다 사용방법이 너무 다르다보니 그걸 통합한 것이 (합집합으로 통합) JDBC API다.

사용방법에 불과하다보니 당연히 구현체가 필요하다. 구현체들은 벤더들이 라이브러리로 만들어서 배포한다. Maven이나 Gradle을 통해서 쉽게 받아올 수 있다.

### JDBC 주요객체
DriverManager
Driver
Connection
Statement
ResultSet

### 주요 포인트
* Driver, DriverManager의 관계
* Driver 등록하는 방법
  * 다양한 방법이 있음
  * Service-Provider를 통해 자동으로 드라이버를 로딩하는 것은 어떻게 진행되는가?
  * DriverManager를 통해 Connection 생성하는 방법은?
  * Statement를 이용해 DML, DQL에 해당하는 SQL문을 실행할 수 있는가? (쿼리하기)
  * ResultSet을 이용하여 SELECT 실행 결과를 다룰 수 있는가?

# TIL
## SQL Injection
사용자가 입력한 문자열을 이용해 SQL문을 완성하게 되면, 입력된 문자열이 SQL문을 왜곡시킬 수 있다. 테이블명, 컬럼명, 컬럼의 순서 같은 정보들이 있다면 (title, name, created_date 같이 거의 유사한 ...) 다른 SQL문을 왜곡할 수 있다.

왜곡된 SQL문은 원래 목적과 다른 결과를 발생시킬 수 있다. 가령 한개의 데이터를 삭제하는 SQL문이 테이블 내 전체 튜플들을 삭제한다거나, 한개 데이터의 제목을 바꾸는 SQL문이 전체 데이터의 제목을 바꿔버리는 일이 생길 수 있다. 공격자가 아주 좋아할 것이다.

그래서 String.format() 방식으로 SQL을 만드는 것이 잘못된 방법이 되는 것이다.

용어도 알아두자. 값을 가지고 SQL문을 만드는 것을 Dynamic SQL이라고 한다. SQL문을 값과 분리하여야 한다. Dynamic SQL은 SQL Injection 공격에 취약하다. SQL문을 먼저 만들어두는 방식이 필요하다.

### PreparedStatement, prepareStatement()
들어오는 문자열이 sql문을 왜곡시킬 수 있는 것인지 검사하는 클래스를 따로 만들지 않아도 된다.
SQL문을 만들 땐 Dynamic SQL을 피하기 위해서, Statement가 아닌 PreparedStatement 클래스를 사용하며, prepareStatement 메서드를 이용한다고 생각하면 된다.

prepareStatement 메서드는 Connection 클래스의 인스턴스 메서드이다. 주어진 SQL문을 값과 분리하여 DBMS가 이해할 수 있는 형식으로 미리 변환(컴파일)한다. 값은 따로 DBMS에 저장한다.

```java
    try (
        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/studydb", "study",
            "Bitcamp!@#123");
        PreparedStatement stmt =
            con.prepareStatement("insert into x_board(title,contents) values(?,?)")) {

      // 위에서 준비한 SQL 문에 값을 설정한다.
      // => ? : 값이 놓일 자리를 의미한다. 'in-parameter' 라 부른다.
      // => in-parameter 에 들어갈 값의 타입에 따라 적절한 setXxx() 메서드를 호출한다.
      //
      stmt.setString(1, title);
      stmt.setString(2, contents);

      // => 이미 SQL 을 준비한 상태이기 때문에 실행할 때는 SQL를 줄 필요가 없다.
      // => setXxx()로 설정된 값은 단순한 텍스트로 처리한 후
      // SQL을 실행할 때 파라미터로 전달되기 때문에
      // SQL 삽입 공격이 불가능 하다.
      int count = stmt.executeUpdate();

      System.out.println(count + " 개를 입력하였습니다.");
    }       
```

위에서 String값에 SQL 삽입을 시도해도 그 전체가 하나의 문자열 처리가 된다.
SQL 삽입 공격이 아닌데 오탐하는 것을 걱정하지 않아도 된다.

### 원리
터미널에서 SQL문을 줄 때, DBMS는 그 문자열 덩어리를 해석한다.
SQL문과 값들을 분리하고 처리한다.
SQL Injection은 SQL문과 값들을 분리하는 과정을 왜곡한다.

SQL문과 값을 처음부터 분리해서 DBMS에 준다면
SQL문이 왜곡되는 일이 원천차단된다.

처음에 터미널에서 문자열 덩어리 쿼리를 던져보면서 실습하기에
쿼리(SQL문 + 값) 말고는 명령을 내리는 방법이 없나 싶다.
터미널에서는 쿼리 말고는 다른 방법이 없다.
그러나 DBMS와 직접 통신할 수 있는 구현체들은, SQL문과 값을 분리해서 받을 수 있다.
DBMS가 따로 쿼리를 파싱하지 않아도 되게끔 할 수 있고, 이로서 SQL Injection을 방어할 수 있다.

* 커넥션 버릴 떄 완료 안된 게 있으면 롤백되도록 드라이버가 작성되어 있음
* 커넥션을 재사용하는 경우
  * 하나의 서버에 동시에 여러 클라가 들어올 때
  * 




# pj; Roadmap
로드맵의 수가 적게 제한된다면 로드맵별로 테이블을 새로 만들어주는게 좋은지?
혹은 하나의 테이블 안에 넣는게 좋은지가 고민이 된다.

