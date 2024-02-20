---
title: (Day	67)
author: 김준회
date: 2024-02-20 17:00:00 +0900
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

# Review
## ServletContextListener
이벤트 발생시 처리하기 위한 클래스
## ServletContext
서블릿의 상태를 저장하기 위한 클래스. 서블릿간 공유의 필요성이 있는 객체를 공유할 수 있음.

TIL
---
# Web Application의 등장 배경과 활용
## HTTP
한번쯤은 들어봤을 HTTP 의 역사.

> The term hypertext was coined by Ted Nelson in 1965 in the Xanadu Project, which was in turn inspired by Vannevar Bush's 1930s vision of the microfilm-based information retrieval and management "memex" system described in his 1945 essay "As We May Think". Tim Berners-Lee and his team at CERN are credited with inventing the original HTTP, along with HTML and the associated technology for a web server and a client user interface called web browser. Berners-Lee designed HTTP in order to help with the adoption of his other idea: the "WorldWideWeb" project, which was first proposed in 1989, now known as the World Wide Web.
>
> 하이퍼텍스트라는 용어는 1965년 Ted Nelson이 Xanadu 프로젝트에서 처음으로 만들어졌으며, 이는 다시 Vannevar Bush의 1930년대 미크로 필름 기반 정보 검색 및 관리 "메멕스" 시스템에 영감을 받은 것으로, 이는 그가 1945년의 에세이 "우리가 생각할 수 있는대로"에서 설명한 것입니다. 팀 버너스-리와 그의 CERN 팀은 원래의 HTTP를 발명한 것으로 인정받으며, HTML 및 관련 웹 서버 및 클라이언트 사용자 인터페이스인 웹 브라우저와 함께합니다. 버너스-리는 HTTP를 설계함으로써 그의 다른 아이디어인 1989년에 처음 제안된 "WorldWideWeb" 프로젝트의 채택을 돕기 위해 만들었습니다. 이 프로젝트는 지금은 월드 와이드 웹으로 알려져 있습니다.

HTTP가 등장한 배경은 사실 논문을 쉽게 공유하기 위한 것이었다고 한다. 

![HTTP](https://i.ytimg.com/vi/d8Ib8a3TeN0/maxresdefault.jpg)

논문은 서로를 참조하는 형태다. 하이퍼텍스트란 개념이 아주 필요한 분야다. HTTP 가 등장하기 전엔 FTP 가 주로 쓰였는데, FTP는 여러 논문들을 오가며 보기가 불편했다.
> 참고로 CERN에서 Xanadu project를 진행한 팀 버너스 리는 연구소에 핵물리학을 연구하러 갔던 사람은 아닌걸로 보인다. 그의 부모님도 컴퓨터과학/수학 전문가였으며 팀 버너스리도 물리학자이면서 컴퓨터공학을 전공한 사람이었다. CERN에서 일하기 전에는 통신 회사에서 엔지니어로 일했다고 한다.

File Transfer Protocol 
![](https://www.investopedia.com/thmb/UFV80PFEYghBSjHmciitTVgsxOo=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/terms_f_ftp-file-transfer-protocol_FINAL-d9b2ee7e0fe34859b6be843034e500eb.jpg)

FTP는 불편했다. 논문에 언급된 다른 논문을 보기 위해서는 그 논문이 업로드된 다른 서버에 접속하여 그 논문을 다운로드 받아야 했다.
논문에 언급된 다른 논문을 보기 위해선, 그 다른 논문이 업로드 되어 있는 서버에 FTP로 접속하여 해당 경로의 파일을 다운로드 받아야했다...

> I just had to take the hypertext idea and connect it to the TCP and DNS ideas and—ta-da!—the World Wide Web.
— Tim Berners-Lee

> Creating the web was really an act of desperation, because the situation without it was very difficult when I was working at CERN later. Most of the technology involved in the web, like the hypertext, like the Internet, multifont text objects, had all been designed already. I just had to put them together. It was a step of generalising, going to a higher level of abstraction, thinking about all the documentation systems out there as being possibly part of a larger imaginary documentation system.
— Tim Berners-Lee

## HTTP란 무엇인가?
하이퍼텍스트(고수준텍스트)를 교환하는 프로토콜이다. 하이퍼? 표, 서식이나 링크같은 고차원 개념이 없던 이전 텍스트랑 다르다는 것을 강조하기 위한 것이다. 텍스트가 아닌 다른 매체들 (그림 등) 또한 포함될 수 있다. 그래서 다른 인용된 논문의 주소를 저장할 수 있고, 링크로 쉽게 다운로드할 수 있다.

물론 이런 고차원의, 하이퍼울트라슈퍼짱 텍스트를 만드려면 이전의 방식대로 텍스트만 입력하는 것은 안된다. 데이터를 제어하기 위한, 컴퓨터와 대화하는 언어가 함께 쓰어야 한다. 그것이 마크업 언어다. 이것도 TimBL이 만들었다. 하이퍼텍스트 마크업 랭기지. HTML이 그것이다. 왜 마크업, mark up 이라는 단어가 붙었을까? HTML을 보면, 보통 아래와 같은 모양이다.

```HTML
<데이터제어명령> 텍스트 텍스트 텍스트 텍스트 </데이터제어명령끝>
```
`< ... >` 이 부분이 마크업(표식), 즉 태그(메타데이터) 처럼 보여서 그런 이름을 팀 버너스리가 그런 이름을 붙였다. 아래 사진의 BODY, P, A, SPAN 같은 것들이 텍스트를 하이퍼하게 만들어주는 마크업들이다.

![](https://ils.unc.edu/courses/2014_fall/inls161_001/images/composer.markup.png)

한국 논문 제목처럼 변경하면, 고수준 텍스트를 만들기 위한 표식기반 언어... 정도가 될 것이다.

북한에서는 [초본문표식달기언어, 하이퍼본문표식달기언어](https://ko.wikipedia.org/wiki/HTML) 라고 부른다고 한다.

## 정리하면,
FTP로 텍스트를 공유하는데, 연결된 문서들이 하도 많다보니 이게 너무 불편했다. 그래서 CERN에서 논문 공유를 편하게 하기 위해 TimBL과 함께 일했다. 여기서 HTML과 이것을 쉽게 주고받기 위한 규약인 HTTP가 개발되었다. 그리고 이것이 세상의 주된 통신 규약이 됐다. 

FTP는 사용 방법에 대한 진입장벽이 있었으나, 그 진입장벽이 HTML과 HTTP로 무너졌다. (1991년 공개) 웹브라우저가 대중화되고 (1993 웹의 폭발), 여기에 검색엔진이 대중화되자 (구글은 1996년부터 시작해서 98년 말부터 급격히 성장) [소프트웨어가 세상을 먹어치우기 시작했다](https://a16z.com/why-software-is-eating-the-world/)

병렬처리 하드웨어의 발전과 인공신경망 개념의 발전, Attension is all.
AGI의 등장이 머지 않은 것 같다.

# 시대와 목적
FTP 이후부터 본다.
* 처음 시작은 논문과 그 인용된 논문들을 어떻게하면 더 편하게 볼 수 있을까 하는 것에서 시작했다.
* HTML과 HTTP가 발전하면서, 상업적인 목적으로 활용되기 시작한다.
  * 회사, 제품의 홍보 목적으로 사용된다.
* 프로그램이 붙는다. (정적 자원에서 동적 자원도 활용하기 시작한다.)
  * 방명록과 게시판과 같은, 웹 프로그램이 등장한다.
  * 브라우저 - 웹서버 - 방명록/게시판 등 프로그램(CGI 프로그램)
    * CGI (Common Gateway Interface): 웹 서버와 앱 사이에 데이터를 주고받는 규칙
    * **CGI에 따라 웹서버와 데이터를 주고받도록 작성된 프로그램이 웹 애플리케이션의 시초다.**
    * 이 때 작성되었던 CGI Program들은 주로 C/C++ 로 작성되었다.
* 주문 서비스가 등장한다. 
  * 웹이 대폭발한다. IT버블(dotcom bubble)이 일어난다. [1995-2000]
  * ![](../assets/img/2024-02-20-10-54-54.png)
  * 컴파일 방식에서 인터프리터 방식을 사용하는 경우가 많아진다. 텍스트 처리가 쉽기 때문이다.
  * 대표적인 인터프리터 언어. Perl, PHP, ASP 같은 스크립트 파일들이 등장한다. 이런 스크립트를 실행하는 엔진들은 각각의 언어에 맞는 것들이 있다. (펄과 PHP는 인터프리터로 부르는데 ASP는 엔진이라고 부른다.) 라인별로 해석하고 실행하니까 컴파일 결과가 되는 이진코드들을 따로 생성하지 않는다.
* 웹 어플리케이션이 등장한다.
  * C/S 아키텍처에서 Web App 아키텍처로 변경된다.
    * 기존 C/S 아키텍처의 단점
      * Client가 App을 통해 직접 DBMS 통신하던 C/S 아키텍처는 계정 탈취되면 DB에 치명적이었다.
        * DBMS에서 정교한 계정 및 권한 서비스를 제공하는 것은 어색한 일이 아니다...
      * 기능이 변경될 때마다 클라이언트 앱은 재배포되어야 했다.
    * 웹앱 아키텍처로 변경 후 장점
      * 클라이언트는 브라우저만 있으면 된다. 뭔가 더 설치하지 않아도 된다.
      * 프로그램의 변경 및 배포가 용이하다. 서버측 웹앱만 변경하면 된다.
      * DBMS는 클라이언트와 격리되므로 보안에서도 더 유리하다.
        * 서버를 제외하고는 IP/포트를 전부 닫아버려도 된다.
    * 이러한 장점으로 인해 C/S 아키텍처는 사실상 **대체**된다.
* 한 앱에서 전체를 다 처리하던 방식에서, 기능별로 서비스를 분리해야 한다는 설득이 진행된다.
  * Monolithic to **MicroService** Architecture
  * **모놀리식: 하나의 서비스나 웹 애플리케이션에 모든 기능을 넣고 그것만 쓰는 방식**
    * 각 기능이 서로를 호출할 수 있어 연동이 쉽다. (다른 기능의 메서드를 직접 호출 가능)
    * 그러나 기능별로 따로 따로 제어하기가 어렵다.
      * 기능 변경시 전체 서비스를 재시작해야 하는 단점이 있다.
      * 일부 기능만 동작시키거나 멈추는 것이 불가능하다.
      * 특정 기능에 더 많은 리소스를 할당하는 등의 제어가 불가능하다.
      * 기능간 강결합되어있어 상호기능이 매우 의존적임.
  * **마이크로서비스: 기능별로 서비스를 분리하는 방식이다.**
    * 예시를 들면, 각 기능별로 별도의 DBMS를 붙이는 것이다.
      * 회원관리-DBMS
      * 과제관리-DBMS
      * 강사관리-DBMS
      * 게시글관리-DBMS
      * 사용자인증-DBMS
    * 기능별 제어가 쉬워지는 장점이 있다. 모놀리식의 단점들이 해소된다.
      * 기능별로 켜거나, 끌 수 있다. 특정 기능 변경해도 나머지 기능들을 동시에 변경하진 않아도 된다.
      * 기능별로 리소스 할당을 제어할 수 있다.
    * 기능간 연동이 어려워진다. 서로간 독립적인 서비스다보니 네트워크를 통해 서로 통신해야 한다. 기능간 실행 오버헤드가 발생한다. 달리말하면 모놀리식에선 필요없던 부가적인 작업들이 발생한다.
      * 세션의 유지 및 관리가 모놀리식보다 어려워진다. (구현이 어려워지고, 오버헤드도 발생한다.)
    * 마이크로서비스로 전환하면, 오버헤드가 발생하며 세션 등 데이터 중복이 발생하며 FK 관리가 어려워진다. DB무결성 관리가 어려워진다.
    * 마이크로서비스로 전환한다는 것이 무조건 좋은 것은 아니다. 그리고 모든 기능을 분리해야 하는 것도 아니다. 특정 시간대에 클라이언트가 아주 많이 몰리는 기능이 있을 때, 그 기능(서비스)를 분리하는 것이 필요한 것이다. 그런 기능은 리소스를 줬다가 잘 안쓸 때는 해제함에 있어 의미가 있을 것이다. 대표적으로는 수강신청같이 특정 일자에 사람들이 몰리는 기능이다.
    * 특히 FK를 걸 필요가 없는 경우 마이크로서비스로 분리하면 아주 좋다. 대표적으로 페이팔의 결제다. 페이팔은 node.js를 썼다고한다. 페이팔의 결제는 페이팔의 다른 기능하고 연동시킬 필요가 없었기 때문에.
    * DBMS를 공유해서 쓰면 완전한 마이크로서비스 아키텍처라고 부르기 어렵다. 기능은 별도의 톰캣 서버로 분리하고, DBMS는 공유하는 경우가 있을 수는 있다. 모놀리식도 아니고 마이크로서비스 아키텍처도 아닌 상태다.
    * 마이크로서비스 아키텍처에서 모놀리식으로 회귀하는 경우도 있다고 한다. 마이크로서비스 아키텍처의 오버헤드가 상당하기에 모놀리식으로 회귀후 클라우드 비용이 감소한 경우도 있다고 한다.
* 자바 웹 어플리케이션의 등장
  * Java EE(Enterprise Edition): 기업에서 사용할 앱을 만드는데 필요한 기술과 도구를 제공하는 에디션
  * 기업용과 개인용의 가장 큰 차이: `혼자 쓰냐`, `여러 명이 동시에 쓰냐`가 가장 큰 차이
    * 기업용은 동시 접속을 제어하는 기술이 필요해요: 네트워킹과 멀티스레딩을 다뤄야 함
    * 기업용은 사용자 관리가 필요해요: 사용자 인증, 권한 제어를 할 수 있어야 함
    * 기업용은 리소스 관리가 필요해요: 리소스를 제어할 수 있어야 함
    * 기업용은 분산처리가 필요해요: 사용자 많아지면 분산처리는 선택이 아님
    * 그래서 javaEE가 위 기능들을 제공해요
      * 웹 서비스
        * Servlet
        * JSP
        * JSF
        * EL
        * JSTL
      * 분산 컴포넌트
        * EJB
        * JPA
        * JTA
      * 분산 서비스
        * WebService
        * JAX-RPC
      
### 모놀리식
모놀리식(Monolithic)은 모놀리式 이 아니다. 
> "모놀리식"이라는 용어는 영어에서 유래했습니다. 이 용어는 "하나의 돌"이나 "하나의 조각"을 의미하는 **그리스어** "모노" (mono)와 "돌"을 의미하는 "릴리스" (lithos)의 조합에서 비롯되었습니다. 프랑스어와는 직접적인 관련이 없습니다. "모놀리식"은 주로 소프트웨어 엔지니어링에서 사용되는 용어로, 하나의 큰 애플리케이션에 모든 기능이 포함되어 있는 아키텍처를 가리킵니다.


### java EE 버전에 따른 기술들 버전

[공식 홈페이지](https://tomcat.apache.org/whichversion.html) 에서 확인하자.
톰캣 각 버전별로 지원하는 java EE 버전을 확인할 수 있다.



## 서블릿과 EJB
일반적인 상황
* 웹브라우저가 HTTP 요청을 넣으면 Java EE 서버가 받는다. 그러면 요청에 따라 서블릿을 호출하고, 서블릿은 실행되어 요청을 처리한 뒤 반환한다. 그러면 서버가 응답한다. HTML, XML, json 등으로...

EJB가 사용된 경우 : 프록시패턴을 기억해야 한다. (43, 44일차 강의)
* 앱에서 Stub에 메서드를 호출한다. (Stub is proxy!)
* Stub이 메서드 호출을 요청한다. (RMI-IIOP) 
* Java EE Server가 메서드를 호출한다. (Skeleton)
* EJB가 호출된다. (Remote Object) 그리고 리턴한다. (java 데이터타입으로 리턴)
* java EE Server가 메서드의 리턴값을 응답한다.
* Stub이 해당 리턴값을 받아 앱에 리턴한다.

요약: EJB는 원격에 있는 코드(메서드)를 마치 내 메서드처럼 사용하는 기술이다. 프록시패턴이 적용되고 다듬어진 기술이다.

초창기 금융권 시스템이 EJB로 개발됐었다. 근데 요샌 거의 안쓴다고 한다. RESTful이 지배적이라서.

## 서블릿과 RESTful
RESTful 은 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.
대표적으로 자원은 URL로 나타낸다.

앱에서 HTTP 요청을 이용해 원격의 서블릿을 실행하는 방법이다.
* 앱이 HTTP로 java EE 서버에 서비스 실행을 요청하면,
* java EE 서버가 서블릿을 호출하고, 서블릿이 실행되어 값을 리턴한다.
* java EE 서버는 리턴받은 값을 통해 응답한다. (HTML, XML, JSON 등, 보통 json)

**왜 그렇게 Restful Restful 소리가 나올까?**

EJB 방식은 stub 객체가 자바 객체이기 때문에 java app에서만 사용할 수 있다.
HTTP 요청은 거의 모든 프로그래밍 언어가 처리할 수 있기 때문에 플랫폼이나 기술에 종속되지 않고 독립적이다. 그래서 HTTP 요청으로 처리하자는 것이다.

그래서 App을 만드는 언어가 Java여도 되고, C#, Python, JavaScript 써도 된다...
**결국 RESTful은 EJB를 고사시키고, 대세가 됐다.**

### 이왕 작성한 김에 RMI 히스토리도..

1. 로컬 메서드를 사용하는 것으로 시작
2. RPC: 원격에 있는 함수 호출
3. CORBA: 이기종 언어간 메서드 호출
4. RMI: Proxy pattern >> stub & skeleton
5. EJB: Proxy pattern >> stub & skeleton 개선판
6. Web Service: HTTP 요청
7. RESTful 등장: HTTP 요청 기반으로 원격 함수 실행


## 필요해서 배워야 하는 것
Servlet, JSP, EL, JSTL 은 배워야 한다.

근데 JSF, EJB, JPA,JTA, WebService, JAX-RPC는 잘 사용되지 않는다...
JSF는 JSF 태그를 사용해서 HTML, CSS, JS 코드를 자동으로 생성해주는 것이다.
서블릿에서 페이지를 동적으로 만들어보는 실습을 해보니, 매번 라인별로 만들어주기가 상당히 번거로운 걸 느꼈었다. 그런 번거로움을 해결해주겠다고 만들어준 것이 JSF였다.

근데 왜 안배우나? 망했기 때문이다. JavaScript 라이브러리가 훌륭한 것들이 많이 등장하고 JS 기술이 발전하면서, JS코드를 작성하는 것에 큰 부담을 느끼지 않게 되었고 JSF에서 자동생성 해주는 것보다 직접 작성하는 것이 더 나은 경우가 많았기 때문이다. jQuery, three.js, Konva, Two.js 등등등등.....

RMI 에 있어서 EJB도 RESTful에게 밀린 것처럼, 기술과 기술 사이의 경쟁도 아주 치열하다.

---

아래는 ChatGPT에게 물어본 결과다.

Remote Method Invocation (RMI)는 Java 플랫폼에서 분산 객체 통신을 위한 기술이며, Java의 일부로 제공됩니다. RMI는 객체 지향적인 방식으로 다른 Java 가상 머신(VM)에서 실행 중인 객체 간에 통신할 수 있게 해줍니다. 이 기술은 분산 시스템에서 각 객체가 로컬 객체처럼 동작하도록 해주는 것이 주요 목표입니다. 이를 위해 객체 간 메소드 호출 및 데이터 전달을 지원합니다.

RMI의 발전은 Java의 발전과 밀접한 관련이 있습니다. 아래는 RMI의 주요 발전 과정입니다:

1. **초기 개발**: Remote Method Invocation은 초기에 Sun Microsystems(현재 오라클)에서 Java의 일부로 개발되었습니다. 이 초기 버전은 비교적 단순한 형태의 분산 객체 통신을 제공했습니다.

2. **Java 2 플랫폼의 도입**: Java 2 플랫폼은 RMI를 개선하고 보강하는데 중요한 역할을 했습니다. 이 단계에서 RMI는 보안, 성능, 확장성 등의 측면에서 개선되었습니다.

3. **RMI-IIOP(Internet Inter-ORB Protocol) 통합**: 이 단계에서 RMI는 CORBA(Common Object Request Broker Architecture)와 통합되었습니다. 이것은 Java 객체와 다른 언어로 작성된 객체 간의 통신을 가능하게 했습니다. RMI-IIOP는 Java 객체가 CORBA 객체와 통신할 수 있도록 했고, 이를 통해 기존 시스템과의 상호 운용성을 강화했습니다.

4. **Java RMI의 지속적인 발전**: Java 플랫폼이 지속적으로 발전함에 따라 RMI도 새로운 기능과 개선 사항이 추가되었습니다. 이는 Java의 버전 업데이트와 함께 이루어졌습니다.

5. **웹 서비스 및 RESTful API의 등장**: RMI는 여전히 유용하지만, 웹 서비스와 RESTful API와 같은 더 간단하고 유연한 방식이 등장함에 따라 사용빈도가 감소했습니다. 특히, 웹 기반의 서비스가 보다 널리 사용되면서 이러한 기술들이 주목을 받았습니다.

6. **마이크로서비스 아키텍처의 부상**: 현재는 마이크로서비스 아키텍처가 인기를 얻으면서, 분산 시스템에서 RMI와 같은 기술보다는 경량화된 통신 프로토콜이 선호되는 경향이 있습니다.

요약하자면, RMI는 Java에서 분산 객체 통신을 가능하게 하기 위해 개발되었으며, Java의 발전과 함께 계속 발전해왔습니다. 그러나 더 간단하고 유연한 기술의 등장과 마이크로서비스 아키텍처의 부상으로 인해 그 중요성은 줄어들고 있습니다.

---

## javaEE & Server
Java EE 기술명세에 따라 동작하도록 구현한 서버를 `Java EE Implementation Server`라고 한다. WAS(Web Application Server) 라고 불릴때도 있다.

### 서블릿 컨테이너
서블릿 컨테이너는 JavaEE 기술중에서 웹 기술(Servlet, JSP, JSTL, EL 등)만을 구현한 것이다. WAS(Web Application Server) 라고 불릴때도 있다.

###  java EE Server와 Servlet Container를 정리한 표

---
주의: 아래 표는 ChatGPT를 통해 얻은 것이라, 부정확한 정보가 포함되었을 가능성이 있습니다. 

Java Enterprise Edition Server
---
| 이름                             | 벤더                 | 특징                                                               | 출시일      | 시장 점유율 |
| -------------------------------- | -------------------- | ------------------------------------------------------------------ | ----------- | ----------- |
| WebLogic Server                  | Oracle               | 고성능, 확장 가능한 애플리케이션 서버, 엔터프라이즈 환경에 적합    | 1996년 12월 | ★★★★☆       |
| IBM WebSphere Application Server | IBM                  | 엔터프라이즈급 Java EE 애플리케이션 서버, 확장성 및 보안 기능 제공 | 1998년 12월 | ★★★☆☆       |
| GlassFish                        | Eclipse Foundation   | 오픈 소스 자바 EE 구현 서버, 기업 및 개발자들에게 인기             | 2006년 5월  | ★★★★☆       |
| JBoss EAP                        | Red Hat              | 기업용 Java EE 애플리케이션 플랫폼, 고성능 및 관리 기능 제공       | 2006년 10월 | ★★★☆☆       |
| Payara Server                    | Payara Services Ltd. | GlassFish를 기반으로 한 상용 및 오픈 소스 자바 EE 서버             | 2014년 3월  | ★★☆☆☆       |
| WildFly (이전 JBoss)             | Red Hat              | 자바 EE 구현 서버, 모듈식 아키텍처, 고성능, 관리 기능 제공         | 2014년 11월 | ★★★☆☆       |
| Liberty                          | IBM                  | 경량 및 모듈식 애플리케이션 서버, 마이크로서비스 아키텍처 지원     | 2015년 6월  | ★★☆☆☆       |

Servlet Container
---
| 이름                             | 벤더               | 특징                                                               | 출시일      | 시장 점유율 |
| -------------------------------- | ------------------ | ------------------------------------------------------------------ | ----------- | ----------- |
| Apache Tomcat                    | Apache             | 경량 웹 애플리케이션 서버, Java Servlet 및 JSP를 지원              | 1999년 12월 | ★★★★★       |
| Jetty                            | Eclipse Foundation | 경량 서블릿 컨테이너, 내장형 모드로 사용 가능                      | 1995년 9월  | ★★★★☆       |
| Undertow                         | Red Hat            | 경량이면서도 고성능의 서블릿 컨테이너, WildFly에 내장              | 2012년 9월  | ★★★☆☆       |
| GlassFish                        | Eclipse Foundation | 오픈 소스 자바 EE 구현 서버, 기업 및 개발자들에게 인기             | 2006년 5월  | ★★★★☆       |
| Resin                            | Caucho Technology  | 경량 및 빠른 서블릿 컨테이너, 클러스터링 및 로드 밸런싱 지원       | 1998년 1월  | ★★☆☆☆       |
| WebLogic Server                  | Oracle             | 고성능, 확장 가능한 애플리케이션 서버, 엔터프라이즈 환경에 적합    | 1996년 12월 | ★★★★☆       |
| IBM WebSphere Application Server | IBM                | 엔터프라이즈급 Java EE 애플리케이션 서버, 확장성 및 보안 기능 제공 | 1998년 12월 | ★★★☆☆       |
| JBoss EAP                        | Red Hat            | 기업용 Java EE 애플리케이션 플랫폼, 고성능 및 관리 기능 제공       | 2006년 10월 | ★★★☆☆       |
| Liberty                          | IBM                | 경량 및 모듈식 애플리케이션 서버, 마이크로서비스 아키텍처 지원     | 2015년 6월  | ★★☆☆☆       |

---

## 버전도 중요하다
Tomcat은 Servlet, JSP로 구성되는데, Tomcat 버전에 따라 사용할 수 있는 Servlet, JSP의 버전이 제한된다.
(하위 버전은 호환되는데 상위 버전에 추가된 메서드들을 쓸 순 없음)
고객사나 클라우드의 톰캣 서버의 버전에 따라 사용할 수 있는 Java EE 기술버전(JSP, Servlet)이 정해진다. 톰캣이 아닌 다른 Servlet Container도 마찬가지로 Java EE 기술을 어느 버전까지 지원하는지 문서에 명시되어 있을 것이다. Java EE Server 또한 마찬가지로, 버전에 따라 지원하는 Java EE 기술의 버전이 달라진다.

## 패키지명 jakarta
[상세한 글](https://www.samsungsds.com/kr/insights/java_jakarta.html)

Java EE 8 까지는 오라클이 유지보수를 했다. 그러다가 이클립스 소프트웨어 재단이 유지보수를 이전받는다. 이 때가 Java EE 8 때다. java 라는 상표권에 대한 분쟁이 우려된 결과, 패키지명이 java에서 jakarta 로 변경된다. javax. 에서 Jakarta. 로 패키지명이 바뀌었다. Java EE 9 부터는 .javax 패키지명을 지원하지 않고, Jakarta. 패키지만을 지원한다. Tomcat 9sms Java EE 8 인데 Tomcat 10 부터는 Jakarta9를, Tomcat 10.1은 Jakarta10 을 지원한다.

Tomcat 8은 javax. 패키지도 (오라클) 있고, 패키지명만 Jararat. 로 바뀐 (이클립스 재단) 것도 있다.
Tomcat 9, 10은 Jakarta. 패키지만이 존재한다. 

# 실습
## Gradle init...
(복습이다.)
자바 프로젝트 디렉토리(Project Directory)는 일반적인 java project의 디렉토리 구조를 말한다. 이는 프로젝트의 특정 요구 사항과 사용되는 빌드 도구에 따라 다를 수 있다.

그레이들이나 메이븐, 앤트같이 어떤 빌드 스크립트 도구를 쓰느냐에 따라 달라질 수 있다. 그러나 원리는 동일하다. 

```
java -classpath [루트패키지를 두는 폴더의 경로 | .jar 파일의 경로] [FQName (Fully Qualified class name)]
```

여기서 FQName 은 패키지명+클래스명 전부를 말한다.
만약 루트패키지를 둔 폴더가 src/main/java/ 이고, 실행하려는 자바 소스파일이 Hello.java 이며 com.mystudy패키지에 있다
`java -cp src/main/java com.mystudy.Hello` 라고 해줘야 한다.

## 웹 프로젝트 표준 디렉토리 구조
이전 자바 프로젝트 표준 디렉토리 구조에 이어서, 웹 프로젝트의 표준 디렉토리 구조를 알아보자.

* app
  * src
    * main
      * java
      자바 소스 폴더이며 `.java` 파일들을 둔다.
      * resources
      자바 소스 폴더이며 `.xml`, `properties` 같은 파일들을 둔다.
      * webapp
      webapp이라는 이름도 그대로 쭉 쓴다.
        * WEB-INF
          * web.xml - 배치기술서(Deployment Descriptor File (DD File))
          컴포넌트들의 배치 정보를 기술해 놓은 파일
          * 기타 웹 애플리케이션 관련 설정파일
        * HTML/CSS/JavaScript/images ... etc
        Static Resource들을 여기에 둔다.