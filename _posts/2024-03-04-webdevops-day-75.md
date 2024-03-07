---
title: (Day	75) JSP 기본 지식
author: 김준회
date: 2024-03-04 17:00:00 +0900
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

# JSP
## JSP의 구동 원리
* 웹서버가 jsp에 대한 요청을 HTTP 프로토콜을 따라 받는다. (java ee 기술이 다 구현된 웹서버거나, 서버쪽 기술만 들어간 서블릿 컨테이너만 존재하는 경우나 모두 같다)
* JSP로 만든 서블릿 객체를 찾아본다. 여기서 JSP로 만든 서블릿 객체란, 임시 폴더에 생성된 클래스 파일을 말한다. 
  * 만약 생성되어 있는 클래스가 없다면 `JSP Engine`이 해당 java 소스를 생성하고, `java compiler`가 해당 소스를 컴파일하여 바이트코드가 담긴 클래스 파일을 만든다.
* 클래스 파일을 찾았음에도 jsp파일의 변경이 감지된 경우 재생성/재컴파일한다.
* 서블릿 컨테이너가 클래스 파일을 실행하여 인스턴스를 생성한다. (`init()`을 호출한다.)

JSP는 인터프리터가 라인별로 실행해 줄 것 같지만 그렇지 않다. java 소스로 컴파일하고, java compiler가 다시 class 파일로 컴파일하여 실행되는 방식이다.

## JSP의 태그
JSP 태그는 다섯 종류가 있다. 그리고 태그 없이 쓰는 template data가 있다.
1. directive element: <%@ %>
2. declaration element: <%! %>
3. scriptlet: <% [java statement] %>
4. expression element: <%=[java expression] %> expression: 값을 리턴하는 statement
5. jsp action tag: <jsp:[ ] [ ] />
6. template data: 태그 없이 그냥 작성하면 됨
7. comment: <%-- [ ] -->: HTML 주석과 닮았다.

안에 들어가는 statement 들은 출력문(out.print(), out.write()) 안에 들어가는 parameter들이 될 것이다. 어떻게 변환되는지를 아는 것이 중요하다.

##  JSP 파일
- 자바 서블릿 클래스를 만드는 재료로 사용된다.
- 그래서 서블릿 클래스를 만드는 "틀"이라 해서 "템플릿(template)"이라 부른다.
- JSP를 템플릿 기술이라 부르기도 한다.
- JSP 파일을 가지고 서블릿을 만들 때 HttpJspPage를 구현해야 한다.
```
- 클래스 계층도
  Servlet
    - init(ServletConfig):void
    - destroy():void
    - service(ServletRequest, ServletResponse):void
    - getServletInfo():String
    - getServletConfig():ServletConfig
    +---> JspPage
      - jspInit():void
      - jspDestroy():void
      +---> HttpJspPage
        - _jspService(HttpServletRequest, HttpServletResponse):void
```

## JSP 에서 HTML 주석을 작성하면
다른 템플릿 데이터와 다를 것이 없다. 그대로 HTML로 보내서 HTML 주석이 만들어진다. JSP 주석으로 작성하면 보내지 않는다.

## JSP의 태그가 어떻게 변환되는가
JSP 태그는 다섯 종류가 있다. 그리고 태그 없이 쓰는 template data가 있다.
1. directive element: <%@ %>
이 태그 안의 내용은 java code 형태로 바로 바뀌는 것이 아니라, JSP Engine 에게 알려주는 정보이다.
2. declaration element: <%! %>
멤버(필드나 메서드)를 선언하는 문장을 넣는다. **들어가는 위치는 class 블럭이다.** HTML로 출력하는 경우라면 out.write() 혹은 out.print()를 사용해야 한다. out.print() 혹은 out.write() 안에 파라미터로 들어간다.
3. scriptlet: <% [java statement] %>
_jspService 메서드 안에 코드 그대로 들어간다.
4. expression element: <%=[java expression] %> expression: 값을 리턴하는 statement
출력문으로 바뀌는데, 출력문 내의 파라미터가 위 java expression으로 바뀐다.
5. jsp action tag: <jsp:[ ] [ ] />
6. template data: 태그 없이 그냥 작성하면 됨
7. comment: <%-- [ ] -->: HTML 주석과 닮았다.

### directive element
예시.
```jsp
<%@ page 
    language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.net.Socket"
    import="java.net.ServerSocket"
    import="java.util.List,java.util.Map,java.util.Set"
    trimDirectiveWhitespaces="true"
    buffer="8kb"
    autoFlush="false"%>
<%@ page import="java.sql.Connection"%>
<%@ page import="java.sql.Statement"%>
```
지시문
1) page
  - 서블릿 실행과 관련하여 특정 기능을 설정한다.
2) include
  - 다른 파일의 내용을 복사해온다.
3) taglib
  - JSTL 등 외부에서 정의한 태그 정보를 가져온다.

### page 지시문
1) language="java"
   - JSP 페이지에서 코드를 작성할 때 사용할 언어를 지정한다.
   - 즉 <% 코드 %>, <%= 표현식 %>, <%! 코드 %> 태그에 코드를 작성할 때 사용할 언어이다.
   - 원래는 다양한 언어를 사용할 경우를 고려해 설계되었지만,
     현재는 java 언어만 사용 가능하다.
   - 이 속성은 생략해도 된다.

2) contentType="text/html; charset=UTF-8"
   - 다음 자바 코드를 생성한다.
       response.setContentType("text/html; charset=UTF-8");  

3) pageEncoding="UTF-8"
   - JSP 파일의 인코딩을 설정한다.
   - JSP 파일을 저장할 때 UTF-8로 저장한다면 위와 같이 선언하라.
   - 생략한다면 에디터에 설정된 문자집합으로 인코딩할 것이다.

4) import="java.net.Socket"
   - 자바의 import 문을 생성한다.
   - 사용법
     import="java.net.Socket"
       => 자바 코드: 
          import java.net.Socket;
     import="java.net.Socket,java.net.ServerSocket,java.util.List"
       => 자바 코드:
          import java.net.Socket;
          import java.net.Serversocket;
          import java.util.List;
   - 한 개의 page 지시문에 여러 개의 import를 작성할 수 있다.
   - 여러 개의 page 지시문을 작성할 수 있다.

5) trimDirectiveWhitespaces="true"
   - 지시문 끝에 줄바꿈 코드를 무시하고 싶을 때 사용한다.
   
6) buffer="8kb"
   - 출력 버퍼의 크기를 변경할 때 사용한다. 
   - 지정하지 않으면 기본이 8kb 이다.
   - 출력 내용이 버퍼의 크기를 넘으면 예외가 발생한다.
     서블릿에서는 자동으로 출력하였다. 
     그러나 JSP는 예외가 발생한다.

7) autoFlush="true"
   - 출력 버퍼가 찼을 때 자동으로 출력한다.
   - 기본은 true 이다.
   - false로 설정하면 출력 버퍼가 찼을 때 예외가 발생한다.

8) errorPage="URL"
   - JSP를 실행하는 중에 오류가 발생했을 때 포워딩할 URL을 지정한다.

9) isErrorPage="true|false"
   - JSP 페이지가 예외를 처리하는 페이지인지 지정한다.
   - true로 설정하면, 포워딩할 때 받은 예외 객체를 사용할 수 있도록 
     Throwable 타입의 exception 빌트인 객체가 추가된다.

### include 지시문
```jsp
<%@ include file="./ex08_header.txt"%>
<p>테스트</p>
<%@ include file="./ex08_footer.txt" %>
```
1) file="JSP에 포함시킬 파일 경로"
   - 지정한 파일을 JSP로 포함시킨 후에 자바 서블릿 클래스를 생성한다.
     자동 생성된 자바 서블릿 클래스의 소스를 확인해보라!
   - 따라서 일반 텍스트 파일이면 된다. JSP 파일일 필요가 없다.
   - RequestDispatcher의 include()와 다르다.
   - 비록 JSP 파일이 아니더라도 다음을 선언하여 해당 파일의 문자집합을 지정해야 한다. 
       <%@ page pageEncoding="UTF-8"%>
     JSP 엔진이 해당 파일의 내용을 가져올 때 pageEncoding에 지정된 문자집합으로
     내용을 인식한다.
     또한 JSP 엔진은 <%@ page ...%>는 참고만 할 뿐 가져오지는 않는다. 

<jsp:include [ ]/> 와 달리 삽입 후 컴파일할 뿐이다. RequestDispatcher가 사용되지 않는다.


### tablib
```groovy
dependencies {
    // JSTL 라이브러리
    implementation 'javax.servlet:jstl:1.2'
    ...
}
```

```jsp
...
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
...
<c:forEach items="홍길동,임꺽정,유관순,안중근,윤봉길,김구,김원봉" var="n">
이름=<%=pageContext.getAttribute("n")%>, ${n}<br>
</c:forEach>
...
```
items는 한 개의 문자열을 추출하여 보관함(PageContext, JspContext를 상속함)에 넣는다. 이 보관함은 라이프사이클이 개별 JSP service() 호출 시작/끝과 동일하다.
최근에는 JSP를 쓰는 일은 많지 않기에 JSP를 깊게 배우지는 않는다... (타임리프를 쓰거나, vue, react가 대세가 됐다.)

tablib 지시문
=> 외부에 따로 정의된 JSP 확장 태그를 가져올 때 사용한다.
=> JSP 확장 태그
   1) JSTL(JSP Standard Tag Library)
      - JSP 명세에 추가로 정의된 태그이다.
      - Servlet/JSP API에서는 JSTL 구현체를 제공하지 않는다.
      - 따로 구현된 라이브러리를 다운로드 받아야 한다.
        mvnrepository.com에서 JSTL 라이브러리를 검색하여 프로젝트에 추가하라.
      - 보통 apache.org 사이트에서 구현한 것을 사용한다.
   2) 개발자가 정의한 태그
      - 개발자가 따로 태그를 정의할 수 있다.
      - 그러나 실무에서는 유지보수를 일관성을 위해 JSTL과 같은 표준 API를 사용한다.
      - 즉 개발자가 자신의 회사에서만 사용할 태그를 따로 정의하지 않는다.

=> 사용법
      <%@ taglib 
          uri="확장 태그를 정의할 때 부여한 확장 태그 URI" 
          prefix="확장태그를 사용할 때 붙이는 접두사"%> (통상적으로 사용하는 접두사가 있으므로 따르도록 하자)
   JSP 페이지에서 사용하기
      <확장_태그_접두사:사용자_태그명 속성명="값" .../>

### jsp 에서 확장 태그가 등장의 유래
클라이언트가 디자이너/퍼블리셔에게 변경 요청 -> 퍼블리셔가 HTML/CSS 수정하고 Frontend Developer에게 수정 요청 -> FE Dev가 변경

퍼블리셔가 JS/JSP 모르는 경우에도 JSP 편집하도록, 업무 효율성을 높이려고 했던 것이 jsp 등장 유래라고 한다. 요즘은 퍼블리셔가 흔치 않지만..

## JSP 빌트인 객체
- JSP를 가지고 서블릿 클래스를 만들 때 _jspService() 메서드에서 기본으로 준비하는 객체
- JSP 엔진은 반드시 다음과 같은 이름으로 레퍼런스를 선언해야 한다.
  즉 서블릿 컨테이너(ex: 톰캣, jetty, resin 등)에 상관없이 이름이 같다.

1) request - HttpServletRequest => _jspService() 파라미터이다.
2) response - HttpServletResponse => _jspService() 파라미터이다.
3) pageContext - PageContext => _jspService()의 로컬 변수이다.
4) session - HttpSession => _jspService()의 로컬 변수이다.
5) application - ServletContext => _jspService()의 로컬 변수이다.
6) config - ServletConfig => _jspService()의 로컬 변수이다.
7) out - JspWriter => _jspService()의 로컬 변수이다.
8) page - 서블릿 객체를 가리킨다. 즉 this 이다. => _jspService()의 로컬 변수이다.
9) exception - Throwable => _jspService()의 로컬 변수이다.
   - 이 변수는 JSP 페이지가 <%@ page isErrorPage="true"%>로 설정되었을 때만 존재한다.
   - 주로 오류가 발생되었을 때 실행되는 JSP 페이지인 경우 위 설정을 붙인다. 

JSP가 자바로 컴파일된 결과를 확인해보면,
```java
final javax.servlet.jsp.PageContext pageContext;
javax.servlet.http.HttpSession session = null;
final javax.servlet.ServletContext application;
final javax.servlet.ServletConfig config;
javax.servlet.jsp.JspWriter out = null;
final java.lang.Object page = this;
javax.servlet.jsp.JspWriter _jspx_out = null;
javax.servlet.jsp.PageContext _jspx_page_context = null;


try {
  response.setContentType("text/html; charset=UTF-8");
  pageContext = _jspxFactory.getPageContext(this, request, response,
        null, true, 8192, true);
  _jspx_page_context = pageContext;
  application = pageContext.getServletContext();
  config = pageContext.getServletConfig();
  session = pageContext.getSession();
  out = pageContext.getOut();
  _jspx_out = out;
...
```



## 경로: 서버 루트와 웹 애플리케이션 루트
### 서버루트와 웹앱루트가 다른 경우
* http://localhost:8888`/`myapp`/`board/list
  * `서버:포트` 뒤에 `/`는 서버루트.
  * myapp 뒤에 `/`는 웹애플리케이션 루트. Contect Root 라고도 한다.
  * 서버 설정에 따라 Context Path를 변경할 수 있다.
  * 루트의 절대경로 또한 서버 설정에 따라 변경할 수 있다.
```java
// 톰캣 서버에 배포할 웹 애플리케이션의 환경 정보 준비
        StandardContext ctx = (StandardContext) tomcat.addWebapp(
            "/", // 컨텍스트 경로(웹 애플리케이션 경로)
            new File("src/main/webapp").getAbsolutePath() // 웹 애플리케이션 파일이 있는 실제 경로
        );
        ctx.setReloadable(true);
```

### JSP 액션 태그
=> JSP에서 기본으로 제공하는 JSP 전용 태그
=> 따로 taglib를 사용하여 라이브러리를 선언할 필요가 없다.
=> JSP에서 기본으로 제공하기 때문에 그대로 사용하면 된다.
=> 네임스페이스 이름은 jsp 이다.
   <jsp:태그명 ..../>

```jsp
<h1>JSP 액션 태그 - jsp:useBean, jsp:setProperty</h1>
<%-- bitcamp.vo.Board 객체 생성하기 --%>
<jsp:useBean id="b1" class="com.eomcs.web.vo.Board" scope="page"/>
<%-- 
위 태그의 자바 코드:
com.eomcs.web.vo.Board b1 = (com.eomcs.web.vo.Board) pageContext.getAttribute("b1");
if (b1 == null) {
  b1 = new com.eomcs.web.vo.Board();
  pageContext.setAttribute("b1", b1);
}
--%>

<%-- scope을 생략하면 기본이 page(PageContext)이다. --%>
<jsp:useBean id="b2" class="com.eomcs.web.vo.Board"/>

<jsp:useBean id="b3" class="com.eomcs.web.vo.Board"/>

<%-- 객체의 setter 메서드를 호출하기 --%>
<jsp:setProperty name="b3" property="no" value="100"/>
<jsp:setProperty name="b3" property="contents" value="내용입니다."/>
<jsp:setProperty name="b3" property="viewCount" value="88"/>
<%-- 단, 자바 객체의 프로퍼티 타입이 자바 원시 타입과 문자열인 경우 가능하다.
     다른 타입이라면 따로 처리해야 한다. --%>
<%-- 
<jsp:setProperty name="b3" property="createdDate" value="2019-4-8"/>
--%>

<%=b1%> => out.print(b1);<br>  
<%=b2%> => out.print(b2);<br>
<%=b3%> => out.print(b3);<br> 
<%=b3.toString()%> => out.print(b3.toString());<br>

</body>
</html>
```

### jsp:useBean
=> JSP에서 사용할 객체를 생성할 때 사용할 수 있다.
=> 또는 보관소(ServletContext, HttpSession, ServletRequest, PageContext)에 
   저장된 객체를 꺼낼 때도 사용한다.
=> 사용법
     <jsp:useBean scope="보관소명" id="객체명" class="클래스명"/>   
=> 주요 속성
   scope
     - 객체를 꺼내거나 생성된 객체를 저장할 보관소 이름
     - 다음 4개의 값 중 한 개를 지정할 수 있다. 값을 지정하지 않으면 기본이 "page" 이다.
       application(ServletContext), session(HttpSession),
       request(ServletRequest), page(PageContext)
   id 
     - 객체를 꺼내거나 저장할 때 사용할 이름
   class
     - 보관소에서 객체를 찾을 수 없을 때 생성할 객체의 클래스명
     - 반드시 패키지 이름을 포함해 클래스명(fully-qualified name; FQName)을 지정해야 한다.
       <%@ page import="..."%> 를 선언해도 소용없다.
     - 객체를 꺼내는 경우 레퍼런스의 타입으로도 사용된다.
     - 객체를 생성할 때도 사용할 수 있기 때문에 반드시 콘크리트(concrete) 클래스명이어야 한다.
       추상 클래스와 인터페이스는 객체를 생성할 수 없기 때문에 안된다.


pageContext ~~~Scope... 일관성이 없게 작명되었기에 그냥 pageContext부터 찾으니 pageContext에 넣고 저장소를 명시하지 않고 변수를 사용하는 것이 낫다.

* type 속성 대신에 class 속성을 사용하면 
* id로 지정한 객체를 찾지 못했을 때 해당 객체를 만들고,
* 그 id로 보관소에 저장한다. 
* 단 class 속성에는 generic 문법을 사용할 수 없다.
* 또한 보관소에 객체가 없을 때 생성해야 하기 때문에 
* class 속성에는 인터페이스를 설정할 수 없다.

## errorPage
=> JSP를 실행하는 중에 오류가 발생했을 때 실행할 JSP를 지정할 수 있다.

=> 어떻게?
     <%@ page errorPage="URL"%>

=> 이 속성에 URL을 지정하지 않으면 오류가 발생했을 때 
   서블릿 컨테이너의 기본 오류 출력 페이지가 실행된다.
     
isErrorPage
=> 오류가 발생했을 때 실행되는 JSP 쪽에서
   그 오류 내용을 받을 때 사용한다. 

```jsp
<%@ page 
    ...
    errorPage="ex20_error.jsp"%>
```

오류가 발생했을 때 실행되는 JSP 페이지는 exception이라는 변수를 통해 오류 내용을 받을 수 있다.

단, isErrorPage 속성이 true이어야 해당 변수가 준비된다.

```jsp
<%@ page 
    ...
    isErrorPage="true"%>
...
<%=exception.getMessage()%>
```

exception 변수는 JspRuntimeLibrary 클래스를 통해서 공유된다.
```java
  public void _jspService(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    Throwable exception = JspRuntimeLibrary.getThrowable(request);
    if (exception != null) {
      response.setStatus(500);
    }
    ...
  }
```

JspRuntimeLibrary는 ServletRequest 객체를 통해서 exception을 받는다.
```java
public class JspRuntimeLibrary {

  ...

  public static Throwable getThrowable(ServletRequest request) {
    Throwable error = (Throwable)request.getAttribute("javax.servlet.error.exception");
    if (error == null) {
      error = (Throwable)request.getAttribute("javax.servlet.jsp.jspException");
      if (error != null) {
        request.setAttribute("javax.servlet.error.exception", error);
      }
    }
    ...
```
JspContext는 서로 다른 jsp간 공유가 불가능하므로 ServletRequest 에 저장될 것이 예상될 수 있다.