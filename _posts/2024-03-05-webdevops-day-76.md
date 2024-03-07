---
title: (Day	76) JSTL에 대하여
author: 김준회
date: 2024-03-05 17:00:00 +0900
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

# JSP 주요 태그들
* directive element: <%@ %> : JSP 엔진에게 알려주는 정보로, 어떤 언어를 위한 JSP인지 (java 밖에 안쓰지만) 어떤 character set을 사용했는지, 이 페이지가 에러페이지인지 등등을 알려준다.
* declaration element: <%! %> : 멤버를 선언하는 문장을 넣는다. 이 부분은 클래스 블럭에 들어가게 된다. 만약 이 부분에서 HTML로 뭔가를 출력하고 싶다면 빌트인변수 out을 이용해서 write() 또는 print()를 직접 해줘야 한다.
* scriptlet: <% [java statement] %> : 자바 코드가 service() 메서드 안에 순서에 따라 그대로 들어간다고 보면 된다.
* expression element: <%=[java expression] %> out으로 출력할 파라미터를 리턴하는 expression을 넣는다.
* jsp action tag: <jsp:[ ] [ ] /> : JSP를 위한 라이브러라고 보면 된다. 결국 이걸 써서 JSP에서 java 코드들(Scriptlet)을 빼내기 시작했다. 2000년대 초중반, 퍼블리셔와의 협업을 편하게 할 수 있게 되었다. 동적인 문서를 만들기 위한 JSP문서에서 HTML, CSS를 편하게 사용가능하도록 했기 때문에.. (액션 태그를 만드는 건 JSP 개발자)
* template data: 태그 없이 그냥 작성하면 되고, out.print() write() 매번 하는게 힘들어서 JSP가 시작된 이유다. 그래서 JSP는 템플릿 언어라는 구성에 포함된다.
* comment: <%– [ ] –>: HTML 주석과 닮았다.

# JSP Expression Language
EL(Expression Language)은 콤마(.)와 대괄호([]) 등을 사용하여 객체의 프로퍼티나, 리스트, 셋, 맵 객체의 값을 쉽게 꺼내고 설정하게 도와주는 문법이다. 특히 값을 꺼낼 때는 `OGNL 표기법`을 사용한다.

### OGNL(Object Graph Navigation Language)?
객체의 프로퍼티 값을 가리킬 때 사용하는 문법이다. 파일의 경로처럼 객체에 포함된 객체를 탐색하여 값을 쉽게 조회할 수 있다. (aaa.bbb.ccc.ddd.A 같이 타고타고 올라가는 그래프 형태의 문법)

아래와 같이 표기한다.
```jsp
${ 객체명.프로퍼티명.프로퍼티명.프로퍼티명 }
${ 객체명["프로퍼티명"]["프로퍼티명"]["프로퍼티명"] }
```

## EL 에서 사용할 수 있는 객체
```jsp
    pageContext 
      - JSP의 PageContext 객체
    servletContext 
      - ${ pageContext.servletContext.프로퍼티명 }
        자바코드 => pageContext.getServletContext().get프로퍼티()
    session 
      - ${ pageContext.session.프로퍼티명 }
        예) $ { pageContext.session.name }
        => pageContext.getSession().getName();
                    
    request 
      - ${ pageContext.request.프로퍼티명 }
    response
    
    param 
      - ${ param.파라미터명 }
        => request.getParameter("파라미터명");
    paramValues 
      - ${ paramValues.파라미터명 }
        => request.getParameterValues("파라미터명");
    header 
      - ${ header.헤더명 }
        => request.getHeader("헤더명");
    headerValues 
      - ${ headerValues.헤더명 }
        => request.getHeaders("헤더명");
    cookie 
      - ${ cookie.쿠키명 }
      - ${ cookie.쿠키명.name }
      - ${ cookie.쿠키명.value }
    initParam 
      - ${ initParam.파라미터명 }
    
    => 보관소에서 값을 꺼내는 문법
    pageScope 
      - ${ pageScope.객체이름 }
        => pageContext.getAttribute("객체이름");
    requestScope 
      - ${ requestScope.객체이름 }
        => request.getAttribute("객체이름");
    sessionScope 
      - ${ sessionScope.객체이름 }
        => session.getAttribute("객체이름");
        예) ${ sessionScope.name }
        => session.getAttribute("name");
    applicationScope 
      - ${ applicationScope.객체이름 }
        => application.getAttribute("객체이름");

    => 문법을 사용하는 예시
    PageContext 보관소 : ${pageScope.name}<br> (JspPage)
    PageContext 보관소 : <%=pageContext.getAttribute("name")%><br>

    ServletRequest 보관소 : ${requestScope.name}<br>
    ServletRequest 보관소 : <%=request.getAttribute("name")%><br>

    HttpSession  보관소 : ${sessionScope.name}<br>
    HttpSession 보관소 : <%=session.getAttribute("name")%><br>

    ServletContext 보관소 : ${applicationScope.name}<br>
    ServletContext 보관소 : <%=application.getAttribute("name")%><br>
```

## JSP 보관소 우선순위
* *보관소의 이름을 지정하지 않으면 다음 순서로 값을 찾는다.*

`pageScope ==> requestScope ==> sessionScope ==> applicationScope`

* 보관소에 저장된 값을 찾지 못하면 빈 문자열을 리턴한다. (null 이면 별도 처리가 필요한데, ""을 리턴하면 예외처리를 따로 안해도 되니 편하게 해준 것이다.)

## EL 에서 문자열이나 숫자를 표현하는 방법
* 문자열: ${"홍길동"}
* 문자열: ${'홍길동'}
* 정수: ${100}
* 부동소수점: ${3.14}
* 논리값: ${true}
* null: ${null}

## EL은 오직 보관소에 저장된 값만 꺼낼 수 있다.
로컬 변수를 사용할 수 없다! 그러한 형태의 사용이 필요하면 보관소에 저장하고 꺼내는 방식으로 써야 한다. 값을 넣지 말고 객체를 보관소에 넣어서 쓰자. 아래 코드가 예시이다.

```jsp
<%
Member member = new Member();
member.setNo(100);
member.setName("홍길동");
member.setEmail("hong@test.com");
member.setTel("1111-2222");

pageContext.setAttribute("member", member);
%>

${member.getNo()}<br>
${member.no}<br>
${member["no"]}<br>
${member['no']}<br>
```




### 배열과 리스트, 맵에서 값 꺼내기
객체가 아니라 꼭 값을 보관소에 넣어서 쓰겠다면 아래와 같이 할 수 있다.

```jsp
<%
pageContext.setAttribute("names", new String[]{"AAA","BBB","CCC"});
%>

${names[0]}<br>
${names[1]}<br>
${names[2]}<br>
${names[3]}<br>
```
java였다면 outOfIndex 예외가 발생했겠지만, EL에서는 무심하게(?) 빈문자열이 반환된다.


리스트 객체에서 값 꺼내기
```jsp
<%
ArrayList<String> nameList = new ArrayList<>();
nameList.add("김구");
nameList.add("안중근");
nameList.add("윤봉길");

pageContext.setAttribute("names", nameList);
%>
${names[0]}<br>
${names[1]}<br>
${names[2]}<br>
${names[3]}<br>

<%-- 보관소가 아닌 로컬 변수는 EL에서 사용할 수 없다. --%>
${nameList[0]}<br>
```

맵 객체에서 값 꺼내기
```jsp
<%
HashMap<String,Object> map = new HashMap<>();
map.put("s01", "김구");
map.put("s02", "안중근");
map.put("s03", "윤봉길");
map.put("s04 ^^", "오호라");

pageContext.setAttribute("map", map);
%>

${map["s01"]}<br>
${map['s01']}<br>
${map.s01}<br>

<%-- 프로퍼티 이름이 변수 이름 짓는 규칙에 맞지 않다면
     다음과 같이 OGNL 표기법을 쓸 수 없다. --%>
<%-- 
${map.s04 ^^}<br> 
--%>

${map["s04 ^^"]}<br>
${map['s04 ^^']}<br>
```

## EL의 연산자
### 산술연산자
직관적으로 쓸 수 있다.

* 100 + 200 = ${100 + 200}
* 100 - 200 = ${100 - 200}
* 100 * 200 = ${100 * 200}
* 100 / 200 = ${100 / 200}
* 100 div 200 = ${100 div 200}
* 100 % 200 = ${100 % 200}
* 100 mod 200 = ${100 mod 200}

```
100 + 200 = 300
100 - 200 = -100
100 * 200 = 20000
100 / 200 = 0.5
100 div 200 = 0.5
100 % 200 = 100
100 mod 200 = 100
```

### 논리연산자
*true && false = ${true && false}
*true and false = ${true and false}
*true || false = ${true || false}
*true or false = ${true or false}
*!true = ${!true}
*not true = ${not true}
```
true && false = false
true and false = false
true || false = true
true or false = true
!true = false
not true = false
```
### 관계연산자
* 100 == 200 = ${100 == 200}
* 100 eq 200 = ${100 eq 200}
* 100 != 200 = ${100 != 200}
* 100 ne 200 = ${100 ne 200}
* 100 > 200 = ${100 > 200}
* 100 gt 200 = ${100 gt 200}
* 100 >= 200 = ${100 >= 200}
* 100 ge 200 = ${100 ge 200}
* 100 &lt; 200 = ${100 < 200}
* 100 lt 200 = ${100 lt 200}
* 100 &lt;= 200 = ${100 <= 200}
* 100 le 200 = ${100 le 200}
```
100 == 200 = false
100 eq 200 = false
100 != 200 = true
100 ne 200 = true
100 > 200 = false
100 gt 200 = false
100 >= 200 = false
100 ge 200 = false
100 < 200 = true
100 lt 200 = true
100 <= 200 = true
100 le 200 = true
```
### empty
보관소에 해당 객체가 없는지 검사한다. 없으면 true, 있으면 false.
```
<%
pageContext.setAttribute("name", new String("홍길동"));
%>

name 값이 없는가? ${empty name} => false
name2 값이 없는가? ${empty name2} => true
```
### 조건연산자
조건 연산자 - 조건 ? 식1 : 식2 

```
name == "홍길동" : ${name == "홍길동" ? "맞다!" : "아니다!"}
```

(주소가 아닌 값을 비교하게 되므로 equals()를 사용하지 않는다.)
```
<%
String a = "홍길동";
String b = new String("홍길동");
if (a == b) { // 인스턴스의 주소를 비교!
    out.println("== : 같다!<br>");
} else {
    out.println("== : 다르다!<br>");
}

if (a.equals(b)) { // 인스턴스의 값을 비교!
    out.println("equals() : 같다!<br>");
} else {
    out.println("equals() : 다르다!<br>");
}
```

위 두 코드의 OUTPUT
```
name == "홍길동" : 맞다!
== : 다르다!
equals() : 같다!
``` 


# JSTL (JSP Standard Tag Library)

**JSTL은 EL이다.**

- JSP 확장 태그이다.
  - 기본으로 제공하지 않는다.
  - JSTL API를 구현한 외부 라이브러리를 가져와서 사용해야 한다.
- JSTL 라이브러리 가져오기
  - mvnrepository.com 에서 JSTL 검색하여 라이브러리를 정보를 알아낸다.
  - build.gradle 파일의 dependencies {} 블록에 추가한다.
  - 'gradle eclipse' 실행하여 이클립스 설정 파일을 갱신한다.
  - 이클립스 프로젝트를 리프래시 한다.
- JSTL 라이브러리 모듈
  - Core(c) : http://java.sun.com/jsp/jstl/core
  - XML(x) : http://java.sun.com/jsp/jstl/xml
  - I18N(fmt) : http://java.sun.com/jsp/jstl/fmt
  - Database(sql) : http://java.sun.com/jsp/jstl/sql
  - Functions(fn) : http://java.sun.com/jsp/jstl/functions
- JSP 페이지에서 JSTL 라이브러리의 모듈 사용하기
  - JSTL 모듈의 네임스페이스를 가져온다.
      <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
  - JSTL 태그 사용
      <접두어명:태그명 속성="값" 속성="값"/> 

* I18N(Internationalization 의 약자)
  * 프로그램을 짤 때 여러 언어를 고려해서 코딩하는 것을 말한다.
  * 특히 화면에서 버튼에 제목이나 라벨을 출력할 때 특정 언어로 고정된 값을 출력하지 않고, 외부 파일(예: label_ko_KR.properties)에서 읽어 온 값을 출력하도록 프로그래밍 하는 것.
   
* L10N(Localization 의 약자)
  * 특정 언어에 대해 라벨 텍스트를 담은 프로퍼티 파일(예: label_ko_KR.properties)을    작성하는 것을 말한다.

* 출력의 4가지 방법
```jsp
<%
out.println("<h2>오호라!!!</h2>");
%>

<%="<h2>오호라!!!</h2>"%>

${"<h2>오호라!!!</h2>"}

<c:out value="<h2>오호라!!!<h2>"/>
```

가장 간단한 3번쨰 방법을 쓴다.

## JSTL - 출력문 & 기본값 지정
```jsp
<%
pageContext.setAttribute("name", "유관순");
%>

<c:out value="임꺽정"/><br>
<c:out value="${null}" default="홍길동"/><br>
<c:out value="${null}">홍길동</c:out><br>
<c:out value="${'임꺽정'}" default="홍길동"/><br>
<c:out value="${name}" default="홍길동"/><br>
<c:out value="${name2}" default="홍길동"/><br>
```
OUTPUT
```
임꺽정
홍길동
홍길동
임꺽정
유관순
홍길동
```

## JSTL - 보관소에 값 넣기
남아있는 scriptlet들을 상당부분 제거할 수 있다.

JSP
```jsp
<c:set scope="request" var="name1" value="홍길동2"/>
1: ${requestScope.name1}<br>
2: ${pageScope.name1}<br>
3: ${name1}<br> 
```
⬇️
```
1: 홍길동2
2:
3: 홍길동2
```
> 그런데 여기서 일관성이 꺠지는 것을 목격할 수 있다. pageContext 라는 이름에서 pageScope 등으로 네이밍의 일관성이 깨진다...

---

scope을 생략하면 기본이 PageContext 이다.
```jsp
<c:set var="name2" value="임꺽정"/>
1: ${requestScope.name2}<br>
2: ${pageScope.name2}<br>
3: ${name2}<br>
```
⬇️
```
1:
2: 임꺽정
3: 임꺽정
```

---

시작 태그와 끝 태그 사이(content)에 값을 넣을 수 있다.
```jsp
<c:set var="name3">유관순</c:set>
1: ${requestScope.name3}<br>
2: ${pageScope.name3}<br>
3: ${name3}<br>
```
⬇️
```
1:
2: 유관순
3: 유관순
```

```jsp
<h2>객체의 프로퍼티 값 설정하기</h2>
<jsp:useBean id="m1" class="com.eomcs.web.vo.Member"/>
<%--
Member m1 = (Member) pageContext.getAttribute("m1");
if (m1 == null) {
  m1 = new Member();
  pageContext.setAttribute("m1", m1);
}
 --%>
<jsp:setProperty name="m1" property="no" value="100"/>
<%-- 
m1.setNo(100);
--%>

<c:set target="${pageScope.m1}" property="email" value="hong@test.com"/>
<%--
Object obj = pageContext.getAttribute("m1");
Method m = obj.getClass().getMethod("setEmail", String.class);
m.invoke(obj, "hong@test.com");

=> m1.setEmail("hong@test.com");
--%>

${pageScope.m1.no}<br>
${pageScope.m1.email}<br>
```

```
객체의 프로퍼티 값 설정하기
100
hong@test.com
```

## c:remove
- 보관소에 저장된 값을 제거한다.
- <c:remove var="name" scope="page"/>
- <c:remove var="name" scope="request"/>
- <c:remove var="name" scope="session"/>
- <c:remove var="name" scope="application"/>

## c:if
```jsp
<c:set var="name" value="홍길동"/>
<c:set var="age" value="16"/>
<c:set var="gender" value="man"/>

<c:if test="${not empty name}">
    <p>${name}님 환영합니다!
</c:if>
<c:if test="${age < 19}">
    <p>미성년입니다.</p>
</c:if>
<c:if test="${age >= 19}">
    <p>성년입니다.</p>
</c:if>

조건문의 결과를 보관소에 저장하기

var 속성으로 변수이름을 설정하면, 
조건문의 테스트 결과는 지정된 이름으로 보관소에 저장된다.

<c:if test="${gender == 'woman'}" var="r1"/>
${r1}<br>
${pageScope.r1 ? "여성" : "남성"}<br>
```

```

홍길동님 환영합니다!

미성년입니다.

false
남성
```

## 끝 태그의 생략
시작 태그와 끝 태그 사이에 들어가는 컨텐츠가 없다면, 끝 태그를 생략할 수 있다. 단, 생략한 경우 시작 태그의 끝에 /를 추가해줘야 한다.

위 JSTL 중 `<c:if test="${gender == 'woman'}" var="r1"/>` 가 예시이다.

## switch-case랑 비슷한, choose-when
```jsp
...
<h1>JSTL - c:choose</h1>
<pre>
- 다중 조건문을 만든다.
- 자바의 switch와 유사하다.
</pre>

<c:set var="name" value="홍길동"/>
<c:set var="age" value="26"/>
<%--
pageContext.setAttribute("name", "홍길동");
pageContext.setAttribute("age", "16");
--%>

<c:choose>
    <c:when test="${age < 19}">
        <p>미성년입니다.</p>
    </c:when>
    <c:when test="${age >= 19 and age < 65}">
        <p>성년입니다.</p>
    </c:when>
    <c:otherwise>
        <p>노인입니다.</p>
    </c:otherwise>
</c:choose>
...
```
⬇️
```
JSTL - c:choose
- 다중 조건문을 만든다.
- 자바의 switch와 유사하다.
성년입니다.
```

## forEach (java에선 enhanced for라는 이름으로 부르는..)
반복문을 편하게 만든다.

```jsp
생략
<h1>JSTL - c:forEach</h1>
<pre>
- 반복문을 만든다.
</pre>

<h2>배열</h2>
<%
pageContext.setAttribute("names", new String[]{"홍길동", "임꺽정", "유관순"});
%>
<hr>
<ul>
<% 
String[] names = (String[]) pageContext.getAttribute("names");
for (String n : names) {%>
  <li><%=n%></li>
<%} %>
</ul>

<hr>
<ul>
<c:forEach items="${names}" var="n">
    <li>${n}</li>
</c:forEach>
</ul>

<h2>Collection 객체</h2>
<%
List<String> names2 = new ArrayList<>();
names2.add("홍길동");
names2.add("임꺽정");
names2.add("유관순");
pageContext.setAttribute("names2", names2);
%>

<ul>
<c:forEach items="${names2}" var="n">
    <li>${n}</li>
</c:forEach>
</ul>

<h2>Map 객체</h2>
<%
Map<String,Object> names3 = new HashMap<>();
names3.put("s01", "홍길동");
names3.put("s02", "임꺽정");
names3.put("s03", "유관순");
pageContext.setAttribute("names3", names3);
%>

<ul>
<%-- Map 객체에 대해 반복문을 돌리면 var로 저장되는 것은 
     key와 value를 갖고 있는 Entry 객체이다. --%>
<c:forEach items="${names3}" var="n">
    <li>${n.getKey()} : ${n.getValue()} => ${n.key} : ${n.value}</li>   
</c:forEach>
</ul>

<h2>CVS(comma separated value) 문자열</h2>
<%
pageContext.setAttribute("names4", "홍길동,임꺽정,유관순,김구");
%>

<ul>
<c:forEach items="${names4}" var="n">
    <li>${n}</li>
</c:forEach>
</ul>
생략
```
⬇️
```HTML
생략
<h1>JSTL - c:forEach</h1>
<pre>
- 반복문을 만든다.
</pre>

<h2>배열</h2>
<hr>
<ul>
<li>홍길동</li>
<li>임꺽정</li>
<li>유관순</li>
</ul>

<hr>
<ul>
<li>홍길동</li>
<li>임꺽정</li>
<li>유관순</li>
</ul>

<h2>Collection 객체</h2>
<ul>
<li>홍길동</li>
<li>임꺽정</li>
<li>유관순</li>
</ul>

<h2>Map 객체</h2>
<ul>
<li>s02 : 임꺽정 => s02 : 임꺽정</li>   
<li>s01 : 홍길동 => s01 : 홍길동</li>   
<li>s03 : 유관순 => s03 : 유관순</li>   
</ul>

<h2>CVS(comma separated value) 문자열</h2>
<ul>
<li>홍길동</li>
<li>임꺽정</li>
<li>유관순</li>
<li>김구</li>
</ul>
생략
```

## forToken
문자열만을 대상으로 조금 더 정밀한 제어를 할 수 있다.

```jsp
<%@ page language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - c:forTokens</h1>
<pre>
- 반복문을 만든다.
</pre>

<h2>CVS 문자열</h2>
<%
pageContext.setAttribute("names1", "홍길동,임꺽정,유관순,김구");

/*
String str = (String) pageContext.getAttribute("names1");
String[] values = str.split(",");
for (String n : values) {
  out.println("<li>" + n + "</li>");
}
*/
%>

<ul>
<c:forTokens items="${names1}" var="n" delims=",">
    <li>${n}</li>
</c:forTokens>
</ul>


<h2>Query String 문자열</h2>
<%
pageContext.setAttribute("qs", "name=홍길동&age=20&tel=1111-2222");
%>

<ul>
<c:forTokens items="${pageScope.qs}" var="n" delims="&">
    <li>${n}</li>
</c:forTokens>
</ul>



</body>
</html>
```

```html

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - c:forTokens</h1>
<pre>
- 반복문을 만든다.
</pre>

<h2>CVS 문자열</h2>
<ul>
<li>홍길동</li>
<li>임꺽정</li>
<li>유관순</li>
<li>김구</li>
</ul>


<h2>Query String 문자열</h2>
<ul>
<li>name=홍길동</li>
<li>age=20</li>
<li>tel=1111-2222</li>
</ul>



</body>
</html>
```

## JSTL:url
```jsp
<%@ page language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - c:url</h1>
<pre>
- 복잡한 형식의 URL을 만들 수 있다.
</pre>

<h2>네이버 검색 URL 만들기</h2>
<pre>
https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=%ED%99%8D%EA%B8%B8%EB%8F%99
</pre>

<c:url value="https://search.naver.com/search.naver" var="naverUrl">
    <c:param name="where" value="nexearch"/>
    <c:param name="sm" value="top_hty"/>
    <c:param name="fbm" value="1"/>
    <c:param name="ie" value="utf8"/>
    <c:param name="query" value="홍길동"/>
</c:url>

<pre>${naverUrl}</pre>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - c:url</h1>
<pre>
- 복잡한 형식의 URL을 만들 수 있다.
</pre>

<h2>네이버 검색 URL 만들기</h2>
<pre>
https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=%ED%99%8D%EA%B8%B8%EB%8F%99
</pre>

<pre>https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=%ed%99%8d%ea%b8%b8%eb%8f%99</pre>

</body>
</html>
```

## c:import
외부 사이트의 컨텐츠를 가져와서 클라이언트에게 보여줄 수 있다.
```jsp
<%@ page language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - c:import</h1>
<pre>
- HTTP 요청을 수행하는 코드를 만든다.
</pre>

<h2>HTTP 요청하기</h2>
<c:url value="ex10_sub.jsp" 
       var="url1">
    <c:param name="name" value="홍길동"/>
    <c:param name="age" value="20"/>
    <c:param name="gender" value="woman"/>
</c:url>
<pre>${url1}</pre>

<%-- 지정된 URL을 요청하고 서버로부터 받은 콘텐트를 contents라는 이름으로 
     PageContext 보관소에 저장한다. --%>
<c:import url="${url1}" var="contents"/>

<textarea cols="120" rows="20">${pageScope.contents}</textarea>

<c:import url="https://www.naver.com" var="contents2"/>
<textarea cols="120" rows="20">${pageScope.contents2}</textarea>
</body>
</html>
```

## JSTL - fmt:date 날짜 다루기
문자열을 날짜 객체로 만들어준다.

M = month, m = minute로 쓰인다. 더 큰 단위를 대문자로 쓰는 것이 자연스럽다.

```jsp
<%@page import="java.util.Date"%>
<%@ page language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - fmt:parseDate</h1>
<pre>
- 문자열로 지정된 날짜 값을 java.util.Date 객체로 만들기
</pre>

<fmt:parseDate value="2020-04-14" pattern="yyyy-MM-dd" var="d1"/>
<fmt:parseDate value="04/14/2020" pattern="MM/dd/yyyy" var="d2"/>

<%
Date date1 = (Date)pageContext.getAttribute("d1");
Date date2 = (Date)pageContext.getAttribute("d2");

out.println(date1.toString() + "<br>");
out.println(date2.toString() + "<br>");

%>

</body>
</html>
```

## JSTL - fmt:formatDate
날짜 객체를 문자열로 바꿔서 보기 편하게 해준다.

```jsp
<%@page import="java.util.Date"%>
<%@ page language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL</title>
</head>
<body>
<h1>JSTL - fmt:formatDate</h1>
<pre>
- java.util.Date 객체의 값을 문자열로 만들기
</pre>

<%
pageContext.setAttribute("today", new Date());
%>

<fmt:formatDate value="${pageScope.today}" 
    pattern="yyyy-MM-dd"/><br>
<fmt:formatDate value="${pageScope.today}" 
    pattern="MM/dd/yyyy"/><br>
<fmt:formatDate value="${pageScope.today}" 
    pattern="yyyy-MM-dd hh:mm:ss"/><br>
<hr>

<fmt:formatDate value="${pageScope.today}" 
    pattern="yyyy-MM-dd"
    var="str1"/>
    
<p>오늘 날짜는 '${pageScope.str1}'입니다.</p>    
        
</body>
</html>
```


# myapp 프로젝트 개선
## Annotation으로 RequestMapping
PageController를 상속하여, execute()를 구현하여 컨트롤러에서 execute를 실행하도록 했던 게 이전 버젼.

아쉬운 점은 execute()로 메서드 이름이 고정된다는 것인데, 이것을 Annotation과 Reflection을 이용하여 더 관리하기 쉽게 바꿀 수 있다.

Annotation 선언하고, @RequestMapping 이라는 Annotation을 가진 메서드를 Reflection으로 알아내어 그것을 호출하도록 DispatcherServlet 클래스를 변경하는 방식이다. 스프링 프레임워크도 이러한 방식을 이용한다.

핵심적인 코드는 아래와 같다.

먼저 애너테이션을 정의한다. 메서드에만 달 수 있는 RequestMapping 애너테이션이다.
```java
package bitcamp.myapp.controller;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface RequestMapping {

}
```

DispatcherServlet에서는 아래와 같은 메서드를 이용하여 컨트롤러 객체에서 애너테이션이 달린 메서드를 찾는다.
```java
  private Method findRequestHandler(Object controller) {
    Method[] methods = controller.getClass().getDeclaredMethods();
    for (Method m : methods) {
      RequestMapping requestMapping = m.getAnnotation(RequestMapping.class);
      if (requestMapping != null) {
        return m; //찾으면 메서드를 반환
      }
    }
    return null; //못찾으면 null 반환
  }
```

사용할 때는 아래 코드와 같이 사용할 수 있다.
```
  Method requestHandler = findRequestHandler(controller);
      String viewUrl = (String) requestHandler.invoke(controller, request, response);
```

이젠 컨트롤러 클래스들은 PageController를 구현할 것이 아니라, 실행할 메서드에 `@RequestMapping` 애너테이션을 필수적으로 붙여야 한다. 메서드의 이름에는 제약이 없다.

## 컨트롤러 묶기
현재 VO 별로 컨트롤러가 나뉘어 있다. Add, List, View, Update, Delete ...

관련된 기능이 한 클래스에 모여있으면 다루기 더 쉬워지니 그렇게 묶어보자. 

컨트롤러 인터페이스를 구현하는 방식에서 애너테이션을 사용하는 방식으로 바뀌어서, execute가 아닌 해당 메서드가 수행하는 작업에 따라 메서드 이름을 네이밍할 수 있게 되었다.

```java
package bitcamp.myapp.controller;

import bitcamp.myapp.dao.AssignmentDao;
import bitcamp.myapp.vo.Assignment;
import java.sql.Date;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AssignmentController {

  private AssignmentDao assignmentDao;

  public AssignmentController(AssignmentDao assignmentDao) {
    this.assignmentDao = assignmentDao;
  }

  @RequestMapping("/assignment/add")
  public String add(HttpServletRequest request, HttpServletResponse response) throws Exception {
    if (request.getMethod().equals("GET")) {
      return "/assignment/form.jsp";
    }

    Assignment assignment = new Assignment();
    assignment.setTitle(request.getParameter("title"));
    assignment.setContent(request.getParameter("content"));
    assignment.setDeadline(Date.valueOf(request.getParameter("deadline")));

    assignmentDao.add(assignment);
    return "redirect:list";
  }

  @RequestMapping("/assignment/list")
  public String list(HttpServletRequest request, HttpServletResponse response) throws Exception {
    request.setAttribute("list", assignmentDao.findAll());
    return "/assignment/list.jsp";
  }

  @RequestMapping("/assignment/view")
  public String view(HttpServletRequest request, HttpServletResponse response) throws Exception {
    int no = Integer.parseInt(request.getParameter("no"));
    Assignment assignment = assignmentDao.findBy(no);
    if (assignment == null) {
      throw new Exception("과제 번호가 유효하지 않습니다.");
    }
    request.setAttribute("assignment", assignment);
    return "/assignment/view.jsp";
  }

  @RequestMapping("/assignment/update")
  public String update(HttpServletRequest request, HttpServletResponse response) throws Exception {
    int no = Integer.parseInt(request.getParameter("no"));

    Assignment old = assignmentDao.findBy(no);
    if (old == null) {
      throw new Exception("과제 번호가 유효하지 않습니다.");
    }

    Assignment assignment = new Assignment();
    assignment.setNo(old.getNo());
    assignment.setTitle(request.getParameter("title"));
    assignment.setContent(request.getParameter("content"));
    assignment.setDeadline(Date.valueOf(request.getParameter("deadline")));

    assignmentDao.update(assignment);
    return "redirect:list";
  }

  @RequestMapping("/assignment/delete")
  public String delete(HttpServletRequest request, HttpServletResponse response) throws Exception {
    int no = Integer.parseInt(request.getParameter("no"));
    if (assignmentDao.delete(no) == 0) {
      throw new Exception("과제 번호가 유효하지 않습니다.");
    }
    return "redirect:list";
  }
}
```