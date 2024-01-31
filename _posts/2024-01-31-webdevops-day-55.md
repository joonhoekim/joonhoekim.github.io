---
title: (Day	55) SQL - Transaction, Normal Form, Foreign Key
author: 김준회
date: 2024-01-31 17:00:00 +0900
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

벌써 1월의 마지막 날이다.

# Review
## SQL
* 테이블의 생성, 변경, 삭제
* null 값 허용/비허용
* Column type (자료형)
* key
  * candidate
  * primary
  * unique
  * alternate
  * artificial (surrogate)
* CONSTRAINTS
* AUTO_INCREMENT
* How to set `INDEX COLUMN`
* VIEW
  * 생성 및 삭제는?
  * 뷰가 동작하는 원리는?

## EXERD
DBA건 백엔드 개발자건간에 전부 

# TIL
# SQL
## TRANSACTION (트랜젝션)
한 단위로 묶은 작업을 의미한다. 하나의 업무가 여러 개의 작업으로 이뤄진 경우, 그것을 한 단위로 다룰 수 있도록 여러 작업을 묶어둔 것이라고 생각하면 된다. 

예를 들면, 온라인으로 물건을 주문받는 서비스를 생각해보자. 이러한 서비스는 크게 3가지 정도의 단계가 있을 것이다.

1. 주문정보를 등록한다. (어떤 고객이 어떤 물건을 주문했는지)
2. 결제정보를 등록한다. (어떤 결제수단으로 얼마를 결제했는지)
3. 배송정보를 등록한다. (어떤 배송지에 어떻게 배송할지)

이 중에서 하나라도 실패하면 물건 주문이 실패한 것이다. 


클라이언트는 서버와 하나의 스레드를 통해서 연결된다. 여러 클라이언트가 하나의 서버에 동시에 접속되려면 각각 스레드를 받아서 연결된다. (이러한 스레드들을 관리하는 기법은 Pooling 기법 => Thread pool) DB도 멀티스레드를 지원하는 경우 스레드풀과 같은 기법이 적용되어 있다.


### AUTOCOMMIT
중요! AUTOCOMMIT은 **스레드별**로 설정된다. 

아래와 같이 AUTOCOMMIT = false 인 경우
```sql
SET AUTOCOMMIT = false;
```
클라이언트와 연결된 스레드는 `임시 DB`에 **작업**을 저장한다.
`COMMIT` 명령을 받으면 `임시 DB`의 작업이 `실제 DB`에 반영된다.

SELECT를 하는 경우, **실제 테이블의 데이터에 임시DB에 저장된 작업들을 반영한 결과를 임시로 만들어서** 그것에 SELECT를 한 결과를 반환한다.

`ROLLBACK` 명령어는 임시DB에 저장한 작업들을 전부 없애는 명령어다.

AUTO_INCREMENT 옵션이 들어가 있는 경우, 다른 스레드에서 커밋되지 않은 상태라고 해도 반영된다! (그래야 PK로 사용할 때 중복되는 키가 발생되지 않을 것이다.) 커밋하지 않은 상태에서 ROLLBACK으로 임시 작업들을 지워버려도, AUTO_INCREMENT는 최신 값에서 계속 증가한다.

### AUTOCOMMIT = FALSE ➡️ TRANSACTION
AUTOCOMMIT이 비활성화 되어 있으면 작업이 바로 실행되지 않고, 임시로 저장된다.
이 **작업 묶음**을 실제 테이블에 반영할 것인지, 버릴 것인지는 COMMIT, ROLLBACK 명령어로 결정할 수 있다. 작업을 묶어서 한 단위의 트랙잭션으로 만들 수 있게 된 것이다.

### 클라이언트의 커넥션 공유
아주 큰 문제가 된다!
클라이언트간의 작업 결과가 서로 간섭되면 안된다.
매번 커넥션을 클라이언트마다 만들어주는 경우는 이런 커넥션 공유 문제가 생기지 않지만, 웹 서비스를 만들다보면 생길 수 있는 문제이다.

## SELECT
SELECT는 Projection, Selection으로 나뉜다.
아래 그림으로 기억하면 된다!

![](https://velog.velcdn.com/images/jhaneul/post/4a97be0f-b5d7-43e9-9ad2-994854165584/image.png)


`SELECT no, name FROM test1;` 이라고 하면?
여기서 결과로 추출할 컬럼인 no, name 컬럼이 PROJECTION으로 만들어진다.
(열 선택)


`SELECT no, name FROM test1 WHERE no=1;` 이라고 하면?
no, name 컬럼에서 `WHERE no=1` 라는 조건에 맞춰 SELECTION으로 만들어진다.
(행 선택)

### SQL Function Call Count
SQL 함수를 호출하는 횟수는 Projection이 아닌 Selection의 개수에 비례한다.

### 함수를 사용한 경우, DBMS 프로그래밍에서 컬럼명은...
함수를 사용해 가상의 컬럼을 만든 경우를 생각해보자.
```sql
SELECT no, concat(name,'(',class,')') from test1;
```

이러면, java App에서 ResultMap을 사용하여 값을 가져오는 경우,
해당 컬럼명이 `concat(name,'(',class,')')` 가 된다.

이 경우 해당 컬럼을 지칭하기 위한 방법이 3가지가 있다.
1. 인덱스 (컬럼 순서대로 붙는 번호)를 이용하는 방법
```java
rs.getString(2);
```

2. 함수 호출하는 것 그대로 이름으로 사용하는 방법
```java
rs.getString(concat(name,'(',class,')'));
```
3. 별명을 짓는 방법
```sql
--sql
CONCAT(name,'(',class,')') AS title
```
```java
rs.getString(title);
```
 

## SQL 쿼리의 실행 순서

### SELECT 
![](https://blog.kakaocdn.net/dn/dOq9i4/btsDQ0mOa2U/Ok1Xmn3BXiQ6ELcFzr7HLk/img.png)
![](https://blog.kakaocdn.net/dn/tm2XI/btsDJXdXdqS/o6xVybnz0zFzFyaSqMj2a1/img.png)
[이미지 출처](https://jaehoney.tistory.com/191)

* SELECT 실행될 때 컬럼을 ***실제로 없애는 것이 아니라***, 출력할 부분을 체크한다. 
* 그래서 SELECT 하지 않은 COLUMN에 대해서도 `ORDER BY`가 가능하다.

## JOIN
![](https://i.stack.imgur.com/rOeAz.jpg)
![](https://cdn.educba.com/academy/wp-content/uploads/2019/11/joins-in-mysql-1.png)
### CARTESIAN JOIN = CROSS JOIN
N개의 튜플을 가진 테이블 A와
M개의 튜플을 가진 테이블 B를
CARTESIAN JOIN (음차하면, 카테전 조인) 하면
M*N개 행의 결과가 나온다.
(CROSS JOIN과 같다)

![Cartesian Join](https://i.stack.imgur.com/EF70Z.jpg)

1. FROM, JOIN 처리
```sql
SELECT 번호, 이름, 제품명, 가격 FROM 고객 JOIN 주문
```

* **M*N 개**의 튜플을 가진 테이블이 결과로 생성된다.

```sql
SELECT 번호, 이름, 제품명, 가격 FROM 고객 JOIN 주문 ON 주문.고객번호
```
`ON 주문.고객번호` 가 동일한 `FROM 고객 JOIN 주문` 행만 남긴다.

해당하지 않는 열들을 삭제하는 것이 아니라, 출력할 컬럼들을 선택한다.

### INNER JOIN
`ON 조건`을 넣게 되면 INNER JOIN이 된다.

![](https://miro.medium.com/v2/resize:fit:1358/1*kGuUJxJwsuiLRa5pL3bsCA.png)


## Storage Service
클라이언트 연결을 받고 웹서비스를 제공하는 서버들이 여러개가 있을 것이다. 이들은 DBMS(하나 또는 복수의 서버)와 연결되어 서비스를 제공할 것이다.

많은 서비스에서 제공되는, 파일 첨부 기능은 어떻게 구현될까?
DBMS에 그 첨부파일들을 직접 보관을 할까?
그렇지는 않다.
어차피 DBMS도 파일시스템에 저장될텐데, 이중으로 저장하는 것은 비효율적이기 때문이다.
첨부파일은 파일시스템을 쓴다. 그리고 파일 전송의 비용이 비싸기 때문에 이러한 파일시스템을 위한 서버를 따로 둔다. 그런 서버를 보통 스토리지 서버라고 부른다.

그래서 DBMS는 첨부된 파일의 링크만을 컬럼으로 가진다.
근데 여기서 고민해 볼 사항이 발생한다.
### 첨부파일 링크관리
하나의 테이블에 게시글 일련번호(넘버)와 제목, 내용, 첨부파일 링크 5개를 위한 컬럼을 가지고 있다면 이런 문제가 생긴다.

1. 파일 첨부하지 않은 게시글은 NULL인 컬럼 5개를 가진다. [공간 낭비]
2. 파일 첨부를 1개만 한 경우 NULL인 컬럼 4개를 가진다. [공간 낭비]
3. 첨부하려는 파일이 5개를 초과할 경우에도 5개만 첨부 가능하다.

매번 5개의 파일을 첨부하는 상황이 아닌 경우라면 공간이 항상 낭비된다. 5개를 넘어가는 파일을 첨부하지 못하니 기능도 유연하지 못하다.

그래서 테이블을 분리한다. OOP에서 코드 블럭을 분리하는 것과 유사하다. 
여기서 테이블을 분리하는 것이 **정규화**다.

## 정규화 (Normal Form)
위에서 언급한 문제를 해결하기 위해서는 가장 기본적인 정규화인 제1정규화가 필요하다.

제1정규화는 한 테이블 안에서 중복되는 컬럼이 있을 경우, 그것을 별도의 테이블로 분리하는 것이다. 파일링크를 생각해보면 사실 같은 종류의 데이터를 다룬다. 그런데 중복해서 컬럼을 여러개 만들었기 때문에 문제가 발생한 것이다.

첨부파일을 다루는 별도의 테이블로 분리하고, 게시글 넘버를 통해 첨부파일 테이블의 튜플들이 유니크한 게시글로 연결되도록 하는 것이 문제 해결의 방법이다.
> 이것을 좀 더 명확하게 말하면, 게시판의 PK(일련번호)를 참조하는 FK를 첨부파일 테이블에 추가한다고 말하면 된다. (PK: Primary Key, FK: Foreign Key)

이렇게 되면, 아래와 같은 장점이 생긴다.
1. 공간 낭비 문제가 없어진다.
2. 첨부파일 개수의 제한이 없어진다.

다만, 첨부파일 링크를 확인할 때 게시글 넘버에 일치하는 튜플들을 찾아야 하는 시간 비용은 발생한다.

### 결함이 발생한 데이터
유효하지 않은 경우. 예를 들면, 존재하지 않는 게시글을 가리키는 첨부파일 테이블의 튜플은 결함이 발생한 데이터다.
다음 두 가지 경우에 대해 발생할 수 있다.

* 아직 존재하지 않는 데이터에 대해 첨부파일 튜플이 추가됨
* 첨부파일 정보가 있는 게시글이 삭제됨

DB에 이런 첨부파일 정보가 있다면, 어떤 게시글에서도 찾아볼 수 없는 첨부파일이 되어버린다.

## FK (Foreign Key)
1. 무효한 데이터가 입력되는 것을 방지한다.
2. 참조하는 데이터가 삭제되어 무효한 데이터가 되는 것을 방지한다.
```sql
alter table 테이블명
    add constraint 제약조건이름 foreign key (컬럼명) references 테이블명(컬럼명);
```

FK가 참조하는(reference) 부모 테이블의 키는 PK 혹은 UK여야 한다. (유일성이 보장되어야 무엇을 참조하는지 알 수 있기 때문이다.)

FK가 참조하는 키로 설정되어 있으면, 자식 테이블이 참조중인 튜플은 삭제가 불가능하다. (constraints fail exception)

# OTHERS
## 클라우드 엔지니어
클라우드 분야로 커리어를 잡으려면, 빅펌에서 대규모 엔지니어링을 해본 경험이 아주 중요함

## 학습
학습 초반에는 끊임없이 물어보고, 모르는 걸 계속 확인하는 시간이 계속 필요.
모르는 것들이 계속 발목을 잡는 건 아주 당연한 것.
마치 늪지대를 가는 것 같은 시간이 반드시 필요함.

## 경력

### 웹으로의 진행
신기술은 끊임없이 나오기 때문에, 이미 시장에서 스택이 고정된 플레이어들은 계속해서 도태된다. (도태라는 말이 좀 많이 부정적으로 들리지만, {코볼, 포트란, 파워빌더, 델파이} 같은 예전 기술들이 퇴출되었던 것들을 생각해보면 된다...)
그래서 유동적으로 기술의 변화를 따라가야 했고, 어느정도 신입이 유리한 형태였다. 

기술을 가졌다는 것만으로 시니어를 밀어냈던 세대가 이전이었다면
이젠 시니어들도 AI를 장착했다면 변화를 더 쉽게 받아들일 수 있는 시대다. 더 깊이가 있는 신입이 되어야 시장에 시니어를 밀어내고 진입할 수 있는 시대가 됐다.

그래서 계속 끊임없이 내공을 쌓아야 한다. 내공이라는 말이 정말 좋은 것 같다...