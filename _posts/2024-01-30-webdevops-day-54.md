---
title: (Day	54) SQL Basic
author: 김준회
date: 2024-01-30 17:00:00 +0900
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
# TIL
## SQL(Structured Query Language)
구조적 질의 언어라고 번역되는 SQL은 쉽게 말하면 DBMS에게 내릴 명령을 작성하는 프로그래밍 언어이다.

SQL은 C, java, python 같이 통일된 컴파일러나 인터프리터가 있는 언어가 아니다. DBMS 제작사마다 어떤 SQL 명령은 지원하고, SQL 문법에 존재하지 않는 개별 DBMS 전용 문법도 존재한다. 


### 표준 문법
SQL 문법 중에는 표준 문법이라고 불리는 집합이 있다. 말이 'Standard' 이지만, 이것은 모든 DBMS 실행될 수 있다는 이야기가 아니다. 여러 DBMS들이 지원하는 문법들의 합집합에 가깝다. 교집합이 아니다. 

* 교집합이었다면 DBMS의 하향평준화가 발생했을 것이다.

### 전용 문법
표준 문법이 각 DBMS 문법들의 합집합이었는데, 왜 전용 문법이 따로 있는가? DBMS의 영역을 벗어나는 문법으로 보일 때는 표준 문법에 넣지 않았다.

* DBMS 제조사마다 제품 경쟁력을 위한 전용문법들을 추가하며 경쟁했다.
* 전용문법은 해당 제품의 경쟁력으로 내세우는 차별점이다보니 쓰는 것이 효율적이다.

### 실무
실무에서는 DBMS마다 지원율이 다른 표준문법, 호환되지 않는 전용문법을 혼합해서 사용한다.

하나의 DBMS를 쓰지 않고 상황에 따라 여러 DBMS를 혼합해서 사용하게 될 것이다.

그래서 DB가 바뀌면 SQL 작성했던 것들이 100% 호환되지 않는다. 
따라서 재개발과 테스트가 계속해서 발생한다. DAO를 DBMS 별로 만들어준다거나 하는 일이다.

AI가 발전해서 이런 포팅을 아무런 문제없이 해준다면 좋겠지만, 실상은 그렇지 않다... 그래서 끊임없이 업무가 발생할 것이다.



## SQL 문법의 종류
* DDL - Definition
* DML - Manifulation
* DQL - Query
---

## 키
Key가 무엇인가? 데이터를 구분할 때 사용할 수 있는 컬럼들의 집합을 키라고한다. 중복되지 않아 데이터를 구분할 수 있는 집합을 키라고 한다.

예를 들면, 이메일, 주민번호, ID 같은 경우는 애초에 중복생성을 불가능하게 하므로 각각의 컬럼 자체로도 데이터를 구분할 수 있다. 그러므로 키가 될 수 있다. {이름, 생일, 이메일} 과 같은 집합 정보가 있어도 구분이 가능하다. {이름, 전화번호} 같은 정보가 있어도 구분이 가능할 것이다. 혹은, 전화번호만 있더라도 가능할수도 있다. 이런 키 중에서 최소항목으로 줄인 집합들을 `Candidate Key` (후보키나 최소키로 번역된다.) 라고 한다.

이렇게 후보키를 선정하고 그것을 `키`로 사용하기 시작하면 그 값을 쉽게 변경할 수 없게 된다. `키`로 데이터들을 구분하고 있는데, `키`를 변경하려면 해야 하는 일들이 아주 많아진다...

### Primary key
candidate key 중에서 데이터를 구분하는 키로 결정한 키를 primary key라고 한다. PK로 선택되지 않은 모든 candidate key는 alternate key가 된다.

![diagram](https://media.geeksforgeeks.org/wp-content/uploads/20230314093236/keys-in-dbms.jpg)

* Primary Key
* Candidate Key (Minimum Key)
* Alternative Key
* Surrogate Key (Artificial Key) 대리키
  * PK로 적절한 컬럼 없을 때, PK로 사용하기 위해 직접 추가한 컬럼

### Surrogate Key 활용
ID, 전화번호 등을 PK로 쓰지 말고 Surrogate Key를 만들어서 (일련번호 등) 쓰는 것이 낫다.
연결되어 있는 다른 데이터들이 유효성을 유지하면서도 개인정보 파기가 가능한 방법이 필요하기 때문이다. 개인정보를 PK로 사용하지 마라. 변경될 수 있는 값이라면 더더울 PK로 사용하면 안된다. 임의의 번호를 PK로 사용하는 것이 훨씬 낫다. Surrogate Key를 만들어서 써라.

## Index
인덱스는 DBMS가 관리한다.
인덱스 컬럼을 지정하면, DBMS가 인덱스 테이블을 생성한다. 인덱스 테이블을 rowid DBMS가 할당한 rawid와 인덱스 컬럼만을 컬럼으로 갖는다. (rawid는 사용자에게 보이지 않는다.)

그리고 튜플이 추가될 때마다 인덱스 컬럼의 튜플들을 정렬한다. 이후 인덱스 컬럼을 기준으로 select(조회)할 때, 이미 정렬된 인덱스 테이블을 기준으로 rowid 를 찾기 때문에 빠른 조회 성능을 보여준다. 

* 빠른 조회가 중요할 때 중요 (아주 많은 차이)
  * I/O에서 버퍼를 사용하냐 하지 않느냐의 정도
  * 10000건 정도가 되어야 체감이 가능함
* 데이터의 추가/삭제가 잦은 경우 매번 다시 정렬하는 것이 오히려 성능 감소로 이어질 수 있음.

### GUI (workbench)
DBForge, DBeaver 같은 GUI도 있다. MySQL은 오피셜로 제공되는 workbench라는 소프트웨어도 있다.

### DBMS의 목적
무결성의 유지
constraint등을 추가하는 DBMS의 기술들

### auto_increment
auto_increment 컬럼은 키여야만 가능하며, 입력하지 않으면 마지막 값에서 1씩 증가시켜서 자동으로 입력된다.

## VIEW 동작원리
```sql
CREATE VIEW worker AS SELECT no, name, class WHERE working = 'Y';
```
위와 같이 VIEW (가상의 테이블)을 만드는 문장이 실행된다면, 실제로 DBMS는 어떤 처리를 할까?
`SHOW TABLES` 에서 보이는 뷰 인만큼 실제로 테이블이 만들어지는걸까?

* 뷰를 만드는 명령을 실행하면 테이블을 바로 만들지 않는다.
* 뷰를 만드는 명령은, 해당 명령을 변수로 저장해두는 것과 유사하다.
  * 왜냐면, 매번 길게 쿼리를 작성하는 것이 힘들기 때문이다.
* 이것을 확인해보고 싶으면, 뷰를 만들고나서 튜플을 추가하거나 삭제해보면 된다. 다시 뷰에 대해 SELECT 해보면, 추가/삭제된 내역이 마찬가지로 적용되어있음이 확인된다.

### 풀스택
풀스택이라고 해도, 프론트(html css js)하고 백엔드(스프링부트)하는게 기본인데, 사실 java는 스프링부트에서 대부분을 다 다뤄준다. 그래서 SQL문을 오히려 거기에 맞춰 작성하는 일이 많은 비중을 차지하게 된다. 일부 교육과정에서는 SI현장에 바로 투입하는 것을 목적으로, 기초를 무시해버리고, 디자인패턴, IoC 개념 등 필요한 부분을 다 갖추지 못한 채 프레임워크 사용 훈련만 한다. 그러면... 기초가 없는 개발자가 된다. 

OOP의 목적이 무엇인지, java가 어떻게 동작하는지, 그리고 그 과정에서 발생한 디자인패턴들을 이해하면서 가야지 기초가 있는 사람이 된다. 그래서 2주완성, 4주완성이나 프레임워크 위주의 클론코딩 과정으로 시작하면 문제가 되는 것이다...



## SQL 상세
(github.com/eom-cs)
```sql
# DDL(Data Definition Language)
- DB 객체(테이블, 뷰, 프로시저, 함수, 트리거) 등을 생성, 변경, 삭제하는 SQL 명령이다.
- 데이터베이스(database) = 스키마(schema)
- 테이블(table)
- 뷰(view)
- 트리거(trigger=listener)
  - 특정 조건에서 자동으로 호출되는 함수
  - 특정 조건? SQL 실행 전/후 등
  - OOP 디자인 패턴에서 옵저버에 해당한다.
- 함수(function)
- 프로시저(procedure)
- 인덱스(index)

## 데이터베이스
데이터베이스 생성
  create database 데이터베이스명 옵션들...;

데이터베이스 삭제
  drop database 데이터베이스명;

데이터베이스 변경
  alter database 데이터베이스명 옵션들...;

## 테이블
테이블 생성
  create table 테이블명 (
    컬럼명 타입 NULL여부 옵션,
    컬럼명 타입 NULL여부 옵션,
    ...
    컬럼명 타입 NULL여부 옵션
  );

예)
  create table test01 (
    name varchar(50) not null,
    kor int not null,
    eng int not null,
    math int not null,
    sum int not null,
    aver float not null
  );

테이블 정보 보기
  describe 테이블명;
  desc 테이블명;
  예) describe test01;
  예) desc test01;

테이블 삭제하기
  drop table 테이블명;
  예) drop table test01;

### 테이블 컬럼 옵션

#### null 허용
데이터를 입력하지 않아도 된다.
  create table test1 (
    no int,
    name varchar(20)
  );

데이터 입력 테스트:
  insert into test1(no, name) values(1, 'aaa');
  insert into test1(no, name) values(null, 'bbb');
  insert into test1(no, name) values(3, null);
  insert into test1(no, name) values(null, null);
  select * from test1;

#### not null
데이터를 입력하지 않으면 입력/변경 거절!
  create table test1(
    no int not null,
    name varchar(20)
  );

데이터 입력 테스트:
  insert into test1(no, name) values(1, 'aaa');
  insert into test1(no, name) values(null, 'bbb'); /* 실행 오류 */
  insert into test1(no, name) values(3, null);

#### 기본값 지정하기
입력 값을 생략하면 해당 컬럼에 지정된 기본값이 대신 입력된다.
  create table test1(
    no int not null,
    name varchar(20) default 'noname',
    age int default 20
  );

  insert into test1(no, name, age) values(1, 'aaa', 30);

값을 입력하지 않는 컬럼은 이름과 값 지정을 생략한다.
  insert into test1(name, age) values('aaa', 30); /* 오류! no는 not null*/

컬럼에 default 값이 설정된 경우, 컬럼 값의 입력을 생략하면 기본값이 사용된다.
  insert into test1(no, age) values(3, 30);
  insert into test1(no, name) values(4, 'ddd');
  insert into test1(no) values(5);

컬럼에 default 옵션이 있는 경우,
- 컬럼 값을 생략하면 default 옵션으로 지정한 값이 사용된다.
- 컬럼 값을 null로 지정하면 기본 값이 사용되지 않는다.
  insert into test1(no, age, name) values(6, null, null);

### 컬럼 타입

#### int
- 4바이트 크기의 정수 값 저장
- 기타 tinyint(1바이트), smallint(2바이트), mediumint(3바이트), bigint(8바이트)

#### float
- 부동소수점 저장

#### numeric = decimal
- 전체 자릿수와 소수점 이하의 자릿수를 정밀하게 지정할 수 있다.
- numeric(n,e) : 전체 n 자릿수 중에서 소수점은 e 자릿수다.
  - 예) numeric(10,2) : 12345678.12
- numeric : numeric(10, 0) 과 같다.

입력 테스트:
  create table test1(
    c1 int,
    c2 float,
    c3 numeric(6,2), /* 소수점 자릿수를 
    지정하면 
    부동소수점으로 사용 */
    c4 numeric -- decimal 과 같다
  );

  insert into test1(c1) values(100);
  insert into test1(c1) values(3.14); /* 소수점 이하 반올림하고 짜름 */
  insert into test1(c1) values(100.98); /* 소수점 이하 반올림하고 짜름 */

  insert into test1(c2) values(100);
  insert into test1(c2) values(3.14);
  insert into test1(c2) values(3.14159);

  insert into test1(c3) values(100);
  insert into test1(c3) values(123456789); /* 입력 오류. 5자리 초과 */
  insert into test1(c3) values(12345); /* 입력 오류. 1자리 초과 */
  insert into test1(c3) values(1234);
  insert into test1(c3) values(3.14);
  insert into test1(c3) values(3.14159); /* 2자리를 초과한 값은 반올림. */
  insert into test1(c3) values(3.14551); /* 2자리를 초과한 값은 반올림. */

  insert into test1(c4) values(1234567890);
  insert into test1(c4) values(12.34567890); /* 소수점은 반올림 처리됨 */
  insert into test1(c4) values(12345678.90); /* 소수점은 반올림 처리됨 */

#### char(n)
- 최대 n개의 문자를 저장.
- 0 <= n <= 255
- 고정 크기를 갖는다.
- 한 문자를 저장하더라도 n자를 저장할 크기를 사용한다.
- 메모리 크기가 고정되어서 검색할 때 빠르다.

#### varchar(n)
- 최대 n개의 문자를 저장.
- 0 ~ 65535 바이트 크기를 갖는다.
- n 값은 문자집합에 따라 최대 값이 다르다.
- 한 문자에 1바이트를 사용하는 ISO-8859-n 문자집한인 경우 최대 65535 이다.
- 그러나 UTF-8로 지정된 경우는, n은 최대 21844까지 지정할 수 있다.
- 가변 크기를 갖는다.
- 한 문자를 저장하면 한 문자 만큼 크기의 메모리를 차지한다.
- 메모리 크기가 가변적이라서 데이터 위치를 찾을 때 시간이 오래 걸린다.
  그래서 검색할 때 위치를 계산해야 하기 때문에 검색 시 느리다.

  create table test1(
    c1 char(5),
    c2 varchar(5),
    c3 varchar(21000)
  );

입력 테스트:
  insert into test1(c1) values('');
  insert into test1(c1) values('abcde');
  insert into test1(c1) values('가나다라마'); /* 한글 영어 상관없이 5자 */
  insert into test1(c1) values('abcdefghi'); /* 입력 크기 초과 오류! */
  insert into test1(c1) values('가나다라마바'); /* 입력 크기 초과 오류! */

  insert into test1(c2) values('');
  insert into test1(c2) values('abcde');
  insert into test1(c2) values('abcdefghi'); /* 입력 크기 초과 오류! */

고정 크기와 가변 크기 비교:
  insert into test1(c1) values('abc');
  insert into test1(c2) values('abc');

  select * from test1 where c1='abc';
DBMS 중에는 고정 크기인 컬럼의 값을 비교할 때 빈자리까지 검사하는 경우도 있다.
즉 c1='abc'에서는 데이터를 찾지 못하고, c1='abc  '여야만 데이터를 찾는 경우가 있다.
그러나 mysql은 고정크기 컬럼이더라도 빈자리를 무시하고 데이터를 찾는다.

#### text(65535), mediumtext(약 1.6MB), longtext(약 4GB)
- 긴 텍스트를 저장할 때 사용하는 컬럼 타입이다.
- 오라클의 경우 long 타입과 CLOB(character large object) 타입이 있다.

#### date
- 날짜 정보를 저장할 때 사용한다.
- 년,월,일 정보를 저장한다.
- 오라클의 경우 날짜 뿐만 아니라 시간 정보도 저장한다.

#### time
- 시간 정보를 저장할 때 사용한다.
- 시, 분, 초 정보를 저장한다.

#### datetime
- 날짜와 시간 정보를 함께 저장할 때 사용한다.

  create table test1(
    c1 date,
    c2 time,
    c3 datetime
  );

입력 테스터:
  insert into test1(c1) values('2022-02-21');
  insert into test1(c2) values('16:12:35');
  insert into test1(c3) values('2022-2-21 16:5:3');
  insert into test1(c1) values('2022-02-21 16:13:33'); /* 날짜 정보만 저장*/
  insert into test1(c2) values('2022-02-21 16:13:33'); /* 시간 정보만 저장*/
  insert into test1(c3) values('2022-02-21'); /* 시간 정보는 0을 설정된다.*/
  insert into test1(c3) values('16:13:33'); /* 실행 오류!*/

#### boolean
- 보통 true, false를 의미하는 값을 저장할 때는 정수 1 또는 0으로 표현한다.
- 또는 문자로 Y 또는 N으로 표현하기도 한다.
- 실제 컬럼을 생성할 때 tinyint(1) 로 설정한다.

  create table test1(
    c1 char(1),
    c2 int,
    c3 boolean
  );


  insert into test1(c1) values('Y'); /* yes */
  insert into test1(c1) values('N'); /* no */
  insert into test1(c1) values('T'); /* true */
  insert into test1(c1) values('F'); /* false */
  insert into test1(c1) values('1'); /* true */
  insert into test1(c1) values('0'); /* false */

  insert into test1(c2) values(1); /* true */
  insert into test1(c2) values(0); /* false */

  insert into test1(c3) values('Y'); /* error */
  insert into test1(c3) values('N'); /* error */
  insert into test1(c3) values('T'); /* error */
  insert into test1(c3) values('F'); /* error */

  insert into test1(c3) values(true); /* 저장할 때 1 */
  insert into test1(c3) values(false); /* 저장할 때 0 */
  insert into test1(c3) values('1'); /* true -> 1 */
  insert into test1(c3) values('0'); /* false -> 0 */
  insert into test1(c3) values(1); /* true -> 1 */
  insert into test1(c3) values(0); /* false -> 0 */

- 숫자 컬럼인 경우 값을 설정할 때 문자로 표현할 수 있다.
- 즉 문자열을 숫자로 바꿀 수 있으면 된다.

### 키 컬럼 지정

key column : 데이터를 구분할 때 사용하는 값

테이블:
- name, email, jumin, id, pwd, tel, postno, basic_addr, gender

#### key vs candidate key

- key
  - 데이터를 구분할 때 사용하는 컬럼들의 집합
  - 예)
    - {email}, {jumin}, {id}, {name, tel}, {tel, basic_addr, gender, name}
    - {name, jumin}, {email, id}, {id, name, email} ...
- candidate key (후보키 = 최소키)
  - key 들 중에서 최소 항목으로 줄인 키
  - {jumin}, {email}, {id}, {name, tel}

#### alternate key vs primary key

- primary key (주 키)
  - candidate key 중에서 DBMS 관리자가 사용하기로 결정한 키
  - 예) DBMS 관리자가 id 컬럼의 값을 데이터를 구분하는 키로 사용하기로 결정했다면,
    - 주 키는, {id} 가 된다.
    - 주 키로 선택되지 않은 모든 candidate key는 alternate key가 된다.
- alternate key (대안 키)
  - candidate key 중에서 primary key로 선택된 키를 제외한 나머지 키.
  - 비록 primary key는 아니지만, primary key 처럼 데이터를 구분하는
    용도로 대신 사용할 수 있다고 해서 **대안 키(alternate key)** 라 부른다.

#### artificial key (인공키)
- Primary key로 사용하기에 적절한 컬럼을 찾을 수 없다면,
  - 예) 게시글 : 제목, 내용, 작성자, 등록일, 조회수
- 이런 경우에 key로 사용할 컬럼을 추가한다.
- 보통 일련번호를 저장할 정수 타입의 컬럼을 추가한다.
  - 예) 게시글 : 게시글 번호
- 대부분의 SNS 서비스들은 일련의 번호를 primary key 사용한다.
- 왜?
  - 회원 탈퇴의 경우,
    - 회원 탈퇴할 때 아이디도 제거한다.
    - 아이디를 지우면 그 아이디와 연결된 게시글을 지워야 한다.
    - 그런데 회원 아이디 대신 일련 번호를 사용하면,
    - 그 회원이 쓴 게시글은 일련번호와 묶인다.
    - 따라서 아이디가 삭제되더라도 해당 글은 계속 유효하게 처리할 수 있다.
  - 이메일 변경,
    - primary key 값은 다른 데이터에서 사용하기 때문에,
      - 예) 게시글을 저장할 때 회원 이메일을 저장한다고 가정하자.
    - pk 값을 변경하면 그 값을 사용한 모든 데이터에 영향을 끼친다.
    - 그래서 PK 값을 다른 데이터에서 사용한 경우,
      DBMS는 PK 값을 변경하지 못하도록 통제한다.
    - 이렇게 변경될 수 있는 값인 경우, PK로 사용하지 말라.
    - 대신 회원 번호와 같은 임의의 키(인공 키)를 만들어 사용하는 것이 좋다.

#### primary key
- 테이블의 데이터를 구분할 때 사용하는 컬럼들이다.
- 줄여서 PK라고 표시한다.
- PK 컬럼을 지정하지 않으면 데이터가 중복될 수 있다.

- PK를 지정하기 전:
  create table test1(
    name varchar(20),
    kor int,
    eng int,
    math int
  );

- 입력 테스트:
  insert into test1(name,kor,eng,math) values('aaa', 100, 100, 100);
  insert into test1(name,kor,eng,math) values('bbb', 90, 90, 90);
  insert into test1(name,kor,eng,math) values('aaa', 100, 100, 100); /* 중복 허용*/

- PK를 지정한 후:
> 컬럼명 타입 primary key
  create table test1(
    name varchar(20) primary key,
    kor int,
    eng int,
    math int
  );

- 입력 테스트:
  insert into test1(name,kor,eng,math) values('aaa', 100, 100, 100);
  insert into test1(name,kor,eng,math) values('bbb', 90, 90, 90);
  insert into test1(name,kor,eng,math) values('aaa', 100, 100, 100); /* 중복 오류!*/
  insert into test1(kor,eng,math) values(100, 100, 100); /* PK는 기본이 not null 이다. */

- 한 개 이상의 컬럼을 PK로 지정하기
  create table test1(
    name varchar(20) primary key,
    age int primary key,
    kor int,
    eng int,
    math int
  ); /* 실행 오류 */

- 두 개 이상의 컬럼을 묶어서 PK로 선언하고 싶다면
  각 컬럼에 대해서 개별적으로 PK를 지정해서는 안된다.
- 여러 개의 컬럼을 묶어서 PK로 지정하려면 별도의 문법을 사용해야 한다.
  - constraint 제약조건이름 primary key (컬럼명, 컬럼명, ...)
  - 제약조건이름은 생략 가능.
  - 제약조건이름을 지정하지 않으면 이름이 자동으로 부여된다.
    그래서 나중에 제약조건을 찾기 힘들다.

  create table test1(
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int,
    constraint test1_pk primary key(name, age)
  );


- 입력 테스트:
  insert into test1(name, age, kor, eng, math) values('aa', 10, 100, 100, 100);
  insert into test1(name, age, kor, eng, math) values('bb', 20, 90, 90, 90);
  insert into test1(name, age, kor, eng, math) values('aa', 11, 88, 88, 88);
  insert into test1(name, age, kor, eng, math) values('ab', 10, 88, 88, 88);

/* 이름과 나이가 같으면 중복되기 때문에 입력 거절이다. */
  insert into test1(name, age, kor, eng, math) values('aa', 10, 88, 88, 88);

- 여러 개의 컬럼을 묶어서 PK로 사용하면 데이터를 다루기가 불편하다.
  즉 데이터를 찾을 때 마다 name과 age 값을 지정해야 한다.
- 그래서 실무에서는 이런 경우 '학번'처럼 임의의 값을 저장하는 컬럼을 만들어 PK로 사용한다. (인공 키의 예!)
  create table test1(
    no int primary key, /* 학번 */
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int
  );

  insert into test1(no,name,age,kor,eng,math) values(1,'a',10,90,90,90);
  insert into test1(no,name,age,kor,eng,math) values(2,'a',11,91,91,91);
  insert into test1(no,name,age,kor,eng,math) values(3,'b',11,81,81,81);
  insert into test1(no,name,age,kor,eng,math) values(4,'c',20,81,81,81);

/* 번호가 중복되었기 때문에 입력 거절 */
  insert into test1(no,name,age,kor,eng,math) values(4,'d',21,81,81,81);

/* 번호는 중복되지 않았지만, name과 age값이 중복되는 경우를 막을 수 없다*/
  insert into test1(no,name,age,kor,eng,math) values(5,'c',20,81,81,81);

- 위와 같은 경우를 대비해 준비된 문법이 unique이다.
- PK는 아니지만 PK처럼 중복되어서는 안되는 컬럼을 지정할 때 사용한다.
- 그래서 PK를 대신해서 사용할 수 있는 key라고 해서 "대안키(alternate key)"라고 부른다.
- 즉 대안키는 DBMS에서 unique 컬럼으로 지정한다.

#### unique = alternate key(대안키)
  create table test1(
    no int primary key,
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int,
    constraint test1_uk unique (name, age)
  );

/* 다음과 같이 제약 조건을 모든 컬럼 선언 뒤에 놓을 수 있다. */
  create table test1(
    no int,
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int,
    constraint primary key(no),
    constraint test1_uk unique (name, age)
  );

- 입력 테스트:
  insert into test1(no,name,age,kor,eng,math) values(1,'a',10,90,90,90);
  insert into test1(no,name,age,kor,eng,math) values(2,'a',11,91,91,91);
  insert into test1(no,name,age,kor,eng,math) values(3,'b',11,81,81,81);
  insert into test1(no,name,age,kor,eng,math) values(4,'c',20,81,81,81);

/* 번호가 중복되었기 때문에 입력 거절 */
  insert into test1(no,name,age,kor,eng,math) values(4,'d',21,81,81,81);

/* 비록 번호가 중복되지 않더라도 name, age가 unique 컬럼으로 지정되었기
   때문에 중복저장될 수 없다.*/
  insert into test1(no,name,age,kor,eng,math) values(5,'c',20,81,81,81);

/* 또는 다음과 같이 테이블 정의 다음에 제약 조건을 둘 수 있다. */
  create table test1(
    no int,
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int
  );

  alter table test1
    add constraint test1_pk primary key (no);

  alter table test1
    add constraint test1_uk unique (name, age);


##### index
- 검색 조건으로 사용되는 컬럼인 경우 따로 정렬해 두면 데이터를 찾을 때 빨리 찾을 수 있다.
- 특정 컬럼의 값을 A-Z 또는 Z-A로 정렬시키는 문법이 인덱스이다.
- DBMS는 해당 컬럼의 값으로 정렬한 데이터 정보를 별도의 파일로 생성한다.
- 보통 책 맨 뒤에 붙어있는 색인표와 같다.
- 인덱스로 지정된 컬럼의 값이 추가/변경/삭제 될 때 인덱스 정보도 갱신한다.
- 따라서 입력/변경/삭제가 자주 발생하는 테이블에 대해 인덱스 컬럼을 지정하면,
  입력/변경/삭제 시 인덱스 정보를 갱신해야 하기 때문에
  입력/변경/삭제 속도가 느려지는 문제가 있다.
- 대신 조회 속도는 빠르다.

  create table test1(
    no int primary key,
    name varchar(20),
    age int,
    kor int,
    eng int,
    math int,
    constraint test1_uk unique (name, age),
    fulltext index test1_name_idx (name)
  );

  insert into test1(no,name,age,kor,eng,math) values(1,'aaa',20,80,80,80);
  insert into test1(no,name,age,kor,eng,math) values(2,'bbb',21,90,80,80);
  insert into test1(no,name,age,kor,eng,math) values(3,'ccc',20,80,80,80);
  insert into test1(no,name,age,kor,eng,math) values(4,'ddd',22,90,80,80);
  insert into test1(no,name,age,kor,eng,math) values(5,'eee',20,80,80,80);

- name 컬럼은 인덱스 컬럼으로 지정되었기 때문에
  DBMS는 데이터를 추가하거나 삭제할 때 name 컬럼의 색인표를 갱신한다.
- 단점, 이런 이유로 이름으로 검색할 때 찾기 속도는 빠르지만,
  입력,변경,삭제 속도는 느리게 된다.

#### 인덱스 컬럼의 활용
검색할 때 사용한다.

select * from test1 where name = 'bbb';


### 테이블 변경
기존에 있는 테이블을 변경할 수 있다.

- 테이블 생성

  create table test1 (
    name varchar(3),
    kor int,
    eng int,
    math int,
    sum int,
    aver int
  );


- 테이블에 컬럼 추가

alter table test1
  add column no int;

alter table test1
  add column age int;

alter table test1
  add column no2 int,
  add column age2 int;


- PK 컬럼 지정, UNIQUE 컬럼 지정, INDEX 컬럼 지정

alter table test1
  add constraint test1_pk primary key (no),
  add constraint test1_uk unique (name, age),
  add fulltext index test1_name_idx (name);


- 컬럼에 옵션 추가

alter table test1
  modify column name varchar(20) not null,
  modify column age int not null,
  modify column kor int not null,
  modify column eng int not null,
  modify column math int not null,
  modify column sum int not null,
  modify column aver float not null;


- 입력 테스트

insert into test1(no,name,age,kor,eng,math,sum,aver)
  values(1,'aaa',20,100,100,100,300,100);

insert into test1(no,name,age,kor,eng,math,sum,aver)
  values(2,'bbb',21,100,100,100,300,100);

/* 다음은 name과 age의 값이 중복되기 때문에 입력 거절된다.*/
insert into test1(no,name,age,kor,eng,math,sum,aver)
  values(3,'bbb',21,100,100,100,300,100);


### 컬럼 값 자동 증가
- 숫자 타입의 PK 컬럼 또는 Unique 컬럼인 경우 값을 1씩 자동 증가시킬 수 있다.
- 즉 데이터를 입력할 때 해당 컬럼의 값을 넣지 않아도 자동으로 증가된다.
- 단 삭제를 통해 중간에 비어있는 번호는 다시 채우지 않는다.
  즉 증가된 번호는 계속 앞으로 증가할 뿐이다.

- 테이블 생성

create table test1(
  no int not null,
  name varchar(20) not null
);


- 특정 컬럼의 값을 자동으로 증가하게 선언한다.
- 단 반드시 key(primary key 나 unique)여야 한다.

alter table test1
  modify column no int not null auto_increment; /* 아직 no가 pk가 아니기 때문에 오류*/

alter table test1
  add constraint primary key (no); /* 일단 no를 pk로 지정한다.*/

alter table test1
  add constraint unique (no); /* no를 unique로 지정해도 한다.*/

alter table test1
  modify column no int not null auto_increment; /* 그런 후 auto_increment를 지정한다.*/


- 입력 테스트

/* auto-increment 컬럼의 값을 직접 지정할 수 있다.*/
insert into test1(no, name) values(1, 'xxx');

/* auto-increment 컬럼의 값을 생략하면 마지막 값을 증가시켜서 입력한다.*/
insert into test1(name) values('aaa');

insert into test1(no, name) values(100, 'yyy');

insert into test1(name) values('bbb'); /* no는 101이 입력된다.*/


insert into test1(name) values('ccc'); /* no=102 */
insert into test1(name) values('ddd'); /* no=103 */

/* 값을 삭제하더라도 auto-increment는 계속 앞으로 증가한다.*/
delete from test1 where no=103;

insert into test1(name) values('eee'); /* no=104 */

insert into test1(name) values('123456789012345678901234');

/* 다른 DBMS의 경우 입력 오류가 발생하더라도 번호는 자동 증가하기 때문에
 * 다음 값을 입력할 때는 증가된 값이 들어간다.
 * 그러나 MySQL(MariaDB)는 증가되지 않는다.
 */
insert into test1(name) values('fff'); /* no=? */


## 뷰(view)
- 조회 결과를 테이블처럼 사용하는 문법
- select 문장이 복잡할 때 뷰로 정의해 놓고 사용하면 편리하다.


create table test1 (
  no int primary key auto_increment,
  name varchar(20) not null,
  class varchar(10) not null,
  working char(1) not null,
  tel varchar(20)
);

insert into test1(name,class,working) values('aaa','java100','Y');
insert into test1(name,class,working) values('bbb','java100','N');
insert into test1(name,class,working) values('ccc','java100','Y');
insert into test1(name,class,working) values('ddd','java100','N');
insert into test1(name,class,working) values('eee','java100','Y');
insert into test1(name,class,working) values('kkk','java101','N');
insert into test1(name,class,working) values('lll','java101','Y');
insert into test1(name,class,working) values('mmm','java101','N');
insert into test1(name,class,working) values('nnn','java101','Y');
insert into test1(name,class,working) values('ooo','java101','N');


- 직장인만 조회

select no, name, class from test1 where working = 'Y';


- 직장인만 조회한 결과를 가상 테이블로 만들기

create view worker
  as select no, name, class from test1 where working = 'Y';


- view가 참조하는 테이블에 데이터를 입력한 후 view를 조회하면?
  => 새로 추가된 컬럼이 함께 조회된다.
- 뷰를 조회할 때 마다 매번 select 문장을 실행한다.
  => 미리 결과를 만들어 놓는 것이 아니다.
- 일종의 조회 함수 역할을 한다.
- 목적은 복잡한 조회를 가상의 테이블로 표현할 수 있어 SQL문이 간결해진다.

insert into test1(name,class,working) values('ppp','java101','Y');
select * from worker;


### 뷰 삭제

drop view worker;



## 제약 조건 조회

1) 테이블의 제약 조건 조회

select table_name, constraint_name, constraint_type
from table_constraints;


2) 테이블의 키 컬럼 정보 조회

select table_name, column_name, constraint_name
from key_column_usage;


3) 테이블과 컬럼의 키 제약 조건 조회

select
  t2.table_name,
  t2.column_name,
  t2.constraint_name,
  t1.constraint_type
from table_constraints t1
  inner join key_column_usage t2 on t2.constraint_name=t1.constraint_name;
```