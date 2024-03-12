---
title: (Day	70) 서블릿과 HTTP
author: 김준회
date: 2024-02-23 17:00:00 +0900
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
## 서블릿에서 한글 깨짐? (학습한 내용에서 그렇다는거고 이게 최선의 방법이란 이야기는 아님)
* 서블릿에서 텍스트를 클라이언트(웹 브라우저)로 보내는 방법?
* 서블릿에서 바이너리를 클라이언트(웹 브라우저)로 보내는 방법?
  * FileInputStream 으로 서버나 다른 서버에 위치한 정적 자원을 Read해서 보낸다.
  * HTTP 프로토콜
* 클라이언트에게 텍스트 보낼 때 깨지는 것?
  * ServletResponse의 기본 charset 이 ISO-8859-1 이고 여기에 한글이 없음.
  * Response를 charset=utf-8 설정해주면 됨 (단 한번이라도 보냈으면 이후 메서드 호출해서 변경해도 적용 안됨)

* POST 방식으로 한글 받을 때 깨지는 것?
  * ServletRequest 의 기본 charset 이 ISO-8859-1 이고 여기에 한글이 없음.
  * Request를 charset=utf-8 설정해주면 됨 (단 한번이라도 getParameter 했다면 charset 변경 메서드 호출해도 적용 안됨)

## HTTP 프로토콜
* HTTP 프로토콜에 대해서 간단히 설명해보기
* HTTP 메서드에 대해서 간단히 설명해보기

### GET / POST 메서드간 차이
* 쿼리스트링에 붙이냐, message-body에 붙이냐.
* URL에 드러나냐, 드러나지 않냐
* URL에 해당 데이터가 있어야 의미가 있는 것이라면 GET으로 해야. 

### 파일 업로드의 구현
Multipart 인 Content-Type
* 아파치의 fileupload 혹은 fileupload2를 구현하는 방법 (예전 방법)
* 서블릿3.0부터 내장된 기능을 이용하는 방법




# HTML
## 자주 헷갈리는 HTMl
* input type="text"는 값을 넣지 않으면 빈 문자열이 간다. 그래서 getParameter 해도 null 이 아니다.
* input type="checkbox"는 체크하지 않으면 값이 안 간다. 그래서 getParameter 하면 null 이 반환된다.

# 사실상 HTTP 만 쓰기 때문에...
Generic 서블릿으로 다양한 프로토콜에 대응하여 서블릿을 만드는 일이 거의 없다. 그래서 HTTP 에 대응하는 서블릿을 만드는데 매번 똑같은 코드를 작성하기는 비효율적이라서, HttpServlet 클래스가 생겼다.

HttpServlet 추상 클래스는 GenericServlet을 상속받아 매번 형변환하는 것이 비효율적이라 만들어진 것이다. 서블릿의 service() 가 호출되면 형변환하여 오버로딩한 service()에 인자로 넘겨 호출하는 역할을 하게 만든 것. **서블릿 컨테이너**는 원래 service()를 호출할 것이고, 원래 service()는 형변환만 해서 오버로딩한 메서드를 호출하며 형변환한 인자를 넘겨줄 것이다.

```java
    HttpServletRequest request = (HttpServletRequest) req;
    HttpServletResponse response = (HttpServletResponse) res;

    // => 오버로딩한 service()를 호출한다.
    this.service(request, response);
```

서블릿 컨테이너는 service(ServletRequest, ServletResponse) 를 호출할텐데, HttpServlet 상속한 서블릿들은 해당 메서드가 없을 것이다. 그러면 슈퍼클래스로 올라가서 찾아본다. 슈퍼클래스에서 있었다면 그 메서드를 실행한다. **메서드 안에 다른 메서드를 호출한다면 원래 호출했던 this 기준으로 찾아보고 없으면 다시 슈퍼클래스로 올라가서 찾아본다.** 객체지향 프로그래밍의 한 면이다.

## 실체(?)
사실 HttpServlet 클래스는 저렇게 형변환만 해주는 건 아니다. HTTP라는 프로토콜이 그렇게 간단하지가 않다. 메서드도 GET POST HEAD 등 다양하다. 스펙(명세)를 보면 된다.

그래서 요청한 메서드에 따라 서블릿을 컨트롤하는 등 많은 코드들이 HttpServlet 추상 클래스에 구현되어있다.


### Access Modifier Review

| Modifier    | Class | Package | Subclass | World |
| ----------- | ----- | ------- | -------- | ----- |
| `private`   | ✔️     | ❌       | ❌        | ❌     |
| `default`   | ✔️     | ✔️       | ❌        | ❌     |
| `protected` | ✔️     | ✔️       | ✔️        | ❌     |
| `public`    | ✔️     | ✔️       | ✔️        | ✔️     |

Here's what each access modifier means:

1. `private`: Accessible only within the same class.
2. `default` (no modifier): Accessible within the same package but not outside of it.
3. `protected`: Accessible within the same package and to subclasses (even if they are in different packages).
4. `public`: Accessible from anywhere.

## 서블릿 컨테이너가 실행되면 자동으로 서블릿 초기화(init()) 하도록 하기

어노테이션 방식
 클라이언트가 실행을 요청하지 않아도 서블릿을 미리 생성하고 싶다면,
 loadOnStartup 프로퍼티 값을 지정하라.

     loadOnStartup=실행순서

 미리 생성할 서블릿이 여러 개 있다면, loadOnStartup에 지정한 순서대로 생성한다.
 서블릿을 미리 생성하는 경우?
 
 => 서블릿이 작업할 때 사용할 자원을 준비하는데 시간이 오래 걸리는 경우
    웹 애플리케이션을 시작할 때 미리 서블릿 객체를 준비하는 것이 좋다.
    
    예) DB 연결, 소켓 연결, 필요한 환경 변수 로딩, 스프링 IoC 컨테이너 준비 등

DD 파일 사용하는 방식
```xml
  <servlet>
    <servlet-name>Servlet02</servlet-name>
    <description>서블릿 설명</description>
    <servlet-class>com.eomcs.web.ex06.Servlet02</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
```

# 하드코딩
소스코드에 값을 직접 입력하는 것. 문제는 값 변경하면 소스코드 다시 컴파일해야한다는 것. 그래서 변할 수 있는 값을 직접 적는 것은 바람직하지 않음. 

DB주소나 드라이버를 나타내는 문자열을 서블릿에 알려주고 싶다면, DD 파일을 쓰는 것이 권장된다.

## DD File
ChatGPT:

서블릿 컨테이너를 위한 Data Descriptor 파일은 웹 애플리케이션에서 사용되는 서블릿 및 관련 데이터 구조를 설명하는 메타데이터를 포함하고 있다. 이러한 파일은 일반적으로 서블릿 컨테이너가 서블릿을 로드하고 실행하는 데 필요한 정보를 포함하고 있으며, 서블릿을 효율적으로 관리하고 실행할 수 있도록 돕는다.

Data Descriptor 파일에는 다음과 같은 정보가 포함될 수 있다:

1. **서블릿 구조**: 각 서블릿의 이름, URL 매핑, 초기화 매개변수 등의 정보를 포함한다.
2. **서블릿 속성**: 서블릿에 대한 속성 정보를 정의한다. 예를 들어, 로그 레벨, 세션 관리 방법 등이 될 수 있다.
3. **리소스 및 경로**: 서블릿이 사용할 수 있는 리소스 및 경로 정보를 제공한다. 이는 CSS, JavaScript, 이미지 등의 정적 파일에 대한 참조를 포함할 수 있다.
4. **세션 및 쿠키 관리**: 서블릿 컨테이너가 세션 및 쿠키를 관리하는 방법을 정의한다.
5. **인증 및 권한 부여**: 서블릿에 대한 인증 및 권한 부여 정책을 정의한다.

이러한 Data Descriptor 파일은 서블릿 컨테이너가 웹 애플리케이션을 효율적으로 관리하고 실행할 수 있도록 돕는다. 또한 서블릿의 구조와 동작에 대한 이해를 증진시키고, 웹 애플리케이션의 유지 보수 및 관리를 용이하게 한다.

# 포워딩(Forwarding)
서블릿에서 포워딩이란, 서블릿이 다른 서블릿에게 처리를 위임하는 것을 말한다.

**중요한 점: 포워딩 하기 전에 버퍼에 저장되어 있는, 출력한 모든 내용을 삭제한다.** 버퍼에 있는 상태고 서블릿 종료시 클라이언트에게 보내므로 클라이언트는 포워딩 전에 서버가 무엇을 보내려고 했는지 알 수 없다.

포워딩은 RequestDispatcher 클래스를 통해서 가능하다.

`RequestDispatcher 요청배달자 = request.getRequestDispatcher("/ex07/s2");`

배달원을 요청해서 받고,

`요청배달자.forward(request, response);`

요청을 배달(포워딩)한다. **포위딩하면서 출력했던 내용은 다 지운다.** 이게 명시적으로 코드에 보이지는 않으니 주의. 그래서 포워드하면 return 해주는 게 좋다.

무시 안하고 전부 다 출력하고 싶다면? 포워딩 말고 인클루딩이 있다.

# 인클루딩(Including)

# 리프레시(HTTP)
Servlet
`response.setHeader("Refresh", "3;url=s100");`

HTML
`<meta http-equiv='Refresh' content='3;url=s100'>`
equivalent : 동등한 => HTTP 헤더와 동등한 동작을 하도록 함

n초 뒤에 특정 URL 로 이동시킴.

# 리다이렉트
서블릿 메서드. 즉시요청. 버퍼에 뭐가 있어도 보내지 않고 (무시하고) 리다이렉트 경로로 보냄. 

응답 코드는 302임. [모질라 재단 설명](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302)
The HyperText Transfer Protocol (HTTP) 302 Found redirect status response code indicates that the resource requested has been temporarily moved to the URL given by the Location header.

주의 : 버퍼 다 차서 응답해버린 경우 sendRedirect 동작하지 않음!



# 궁금증
1. service()와 doGet doPost 등 메서드별 처리는?

=> 각 메서드별로 처리해주는 메서드가 있고, service()는 들어온 메서드 모두를 받게 됨. 그래서 메서드별로 나눠서 처리하려면 doPost, doGet 과 같은 메서드를 오버라이드하게 된다.