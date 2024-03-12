---
title: (Day	65) 아파치 톰캣, 자원의 표현과 요청/응답 프로토콜
author: 김준회
date: 2024-02-16 17:00:00 +0900
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

인간은 망각의 동물이다.
원수도, 분노도, 서러움도 망각해버리니
계속해서 의식을 띄우며 기억을 덧씌워야 한다. 옅어지니까.

![](https://mblogthumb-phinf.pstatic.net/MjAxNjExMDFfMTc2/MDAxNDc3OTk0MjgwNTQw.Jg0Eda1JkbrfjEXV2_fm9BbjRbN0BZ1TOkTcbJBJkvcg.iAMykrs5s-Fe0TOilm9XqyeCtZy_YD1DQq9Cn870VGkg.JPEG.freebird07/608110743181454.jpg?type=w800)


# TIL

## 톰캣
### 오너 변경
오라클이 9까지는 했음
10은 이클립스 재단이 관리했음
오라클/구글간 소송을 보고 이클립스재단이 패키지명을 java->jakarta로 패키지 이름을 바꿨음
시장에서 대부분의 업체들이 패키지를 java. 으로 쓰다보니 9 버전으로 훈련 진행.
### 톰캣 임베디드와 그냥 톰캣(?)의 차이점

https://tomcat.apache.org/download-90.cgi 같은 곳에서 다운로드 받으면, jar, 외부 jar, 임시폴더, HTML, 예제들을 모아서 받는다.

임베디드는 필수적인 것만 다운받는다. 
`C:\Users\Username\server\apache-tomcat-9.0.85\lib` 에 들어있는 `tomcat-xxx.jar` 같은 파일들이다.

### 서블릿 컨테이너가 관리하는 객체
서블릿 컨테이너가 관리하는 객체는 `컴포넌트`라고 부른다.
서블릿 컨테이너는 컴포넌트들을 관리한다.

아래는 컴포넌트들의 예시이다.
1. 서블릿: 클라이언트 요청을 처리하는 처리한다.
2. 필터: 서블릿 실행 전후에 보조 작업을 수행한다.
3. 리스너: 특정 상태(Event)에 놓일 때 작업을 수행한다.

이벤트의 예시
   - 웹 앱이 시작되거나 종료됐을 때
   - 요청을 받거나, 응답을 완료했을 때
   - 세션이 시작되거나 종료됐을 때


### 서블릿 컨테이너가 저장하는 객체
1. ServletContext: 
   1. 웹 애플리케이션당 1개
   2.  서블릿, 필터, 리스너가 공유하는 저장소
   3.  DB Connection, DAO, TransactionManager 등의 객체들이 사용
2. HTTPSession: 
   1. 클라이언트당 1개
   2. 서블릿, 필터, 리스너가 공유하는 저장소 
   3. 
3. ServletRequest
   1. 요청당 1개
   2. 서블릿, 필터, 리스너가 공유하는 저장소

## 서블릿 컨테이너
서블릿의 service()의 파라미터는 다양한 프로토콜에 대응할 수 있도록 선언되어 있다.
`public void service(ServletRequest servletRequest, ServletResponse servletResponse)`
그래서 항상 어떤 프로토콜을 위해 사용하는지 explicit casting을 알려주도록 되어 있다. (위 파라미터들 들어가보면 인터페이스로 선언된 타입들이다. 그래서 그대로 쓸 수가 없게 되어있다.)
**이것은 1990년대 중반에 서블릿 컨테이너를 개발한 사람들의 큰 계획이었다. 다양한 프로토콜을 받아들일 수 있도록 만든 것이다.**

그러나 2000년대 들어오면서 HTTP 프로토콜 말고 다른 프로토콜을 사용하는 경우가 사실상 없다시피하다.
웹스피어, 웹로직, Jboss 같은 다른 서블릿 컨테이너들도 HTTP 프로토콜을 쓴다.





## HTML
a 태그에서 a 는 anchor를 의미한다.
 


## 버튼
http://localhost:8888/auth/form.html?
액션 지정된 게 없어서, ? 뒤에 아무것도 없다.

```html
<form>
  <div>
    이메일: <input type="text">
  </div>

  <div>
    암호: <input type="password">
  </div>

  <button>로그인</button>

</form>
```

## 유니폼 리소스 로케이터
### 정적자원, 동적자원
정적자원은 `html`, `css`, `js`, `pdf`, `jpg` 같은 실행결과가 고정된 자원을 말한다.
동적자원은 서블릿이나 JSP등 프로그램을 실행한 결과로, 조건에 따라 실행결과가 달라지는 자원을 말한다.

## URL Encoding
percent encoding
URL을 작성할 때 특별한 의미를 지니는 문자의 경우, 일반문자로 사용할 수 없다.
그런 문자의 예를 들면, / : ? # & = @ 등이 있다.

| 이스케이프 문자 | 설명                                                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------------------ |
| %xx             | ASCII 문자를 나타내는데 사용됨. 여기서 "xx"는 해당 ASCII 문자의 16진수 코드임. 예를 들어, 공백은 `%20`으로 이스케이프됨. |
| +               | 공백을 나타냄. 일반적으로 HTML 폼에서 공백을 전달할 때 사용됨.                                                           |
| &               | 쿼리 문자열에서 여러 매개변수를 구분하는데 사용됨.                                                                       |
| =               | 쿼리 문자열에서 매개변수 이름과 값 사이를 구분하는데 사용됨.                                                             |
| ?               | URL에서 쿼리 문자열을 시작하는데 사용됨.                                                                                 |
| #               | URL에서 fragment identifier를 시작하는데 사용됨. fragment identifier는 리소스 내에서 특정 부분을 가리키는 데 사용됨.     |
| /               | 경로 구분자로 사용됨.                                                                                                    |

### 퍼센트 인코딩
https://developer.mozilla.org/ko/docs/Glossary/Percent-encoding

https://ko.wikipedia.org/wiki/%ED%8D%BC%EC%84%BC%ED%8A%B8_%EC%9D%B8%EC%BD%94%EB%94%A9

https://en.wikipedia.org/wiki/Percent-encoding

https://www.w3schools.com/tags/ref_urlencode.ASP

한글이 유니폼 리소스 로케이터에서는 %EA%B0%80 처럼 퍼센트 문자들로 표현되는 것을 이해하려면, 퍼센트 인코딩을 이해하면 된다. UTF-8 에서 한글 문자가 갖는 3바이트를 표현한 것이 저 퍼센트 인코딩이다.
영문자나 숫자, 그리고 - _ . 같은 문자들은 별도로 인코딩하지 않아도 URL에서 사용할 수 있다.

### 퍼센트 인코딩 책임
URL에 인코딩되지 않은 문자열이 들어가면, **웹브라우저**에서 인코딩한다. 그리고 그렇게 인코딩 된 URL 을 서버에 보내면, 웹 서버는 (톰캣 등) 그것을 디코딩하는 기능을 가지고 있다. 다시말하면, URL의 인코딩과 디코딩은 브라우저(웹 클라이언트)와 웹 서버가 처리하니 웹서비스 개발자가 직접 인코딩/디코딩을 구현할 필요가 없다는 것이다. (브라우저와 웹서버를 개발한 사람들이 그것들을 만들어줬으니까...) 

### 클라이언트란
서블릿 컨테이너에 있어서 클라이언트란 무엇인가? 세션을 만드는 단위가 클라이언트인데, 무엇이 기준이 되어서 구분이 되는가?
* 브라우저가 달라지면 클라이언트가 구분된다.
   * 브라우저가 같고, 탭이나 창을 여러개를 띄우는 경우 하나의 클라이언트로 처리된다.
* 시크릿 창 등 세션을 공유하지 않는, 같은 브라우저의 새 창을 띄우는 경우 다른 클라이언트로 처리된다.

### Servlet Container, Web Application, ServletContext
인사관리시스템에 입사 퇴사 경력증명서 같은 서블릿들이 있을 것이다.
결재 시스템 안에는 상신, 승인과 같은 서블릿이 있을 것이다.

![](https://miro.medium.com/v2/resize:fit:1400/1*Cyyy59ISyWUq5jkt9GvZrA.png)

ServletRequest 객체는 요청이 있을 때마다 생성된다. 요청의 개수만큼 가비지가 생긴다.
근데 지금도 쓰고 있다는 건, 살아남았다는 건 이보다 더 나은 방법을 찾지 못했다는 것이다.
다만, 꼭 모든 요청에 대해 객체가 생성되는 것은 아니다. 자세한 내용은 chatGPT의 답변을 참조해보자.

> **ServletRequest 객체는 클라이언트 요청마다 생성되는 것이 일반적입니다.** 그러나 Servlet 3.0 스펙 이후로는 ServletRequest 객체가 더 이상 매 요청마다 새로 생성되지는 않습니다. 대신, 이전 요청의 정보를 재사용하고, 새로운 요청에 대한 메타데이터를 업데이트하여 다시 사용합니다. 이렇게 함으로써 서버 부하를 줄이고 성능을 향상시킬 수 있습니다.
>
> 또한, Servlet 3.0 이후에는 HttpServletRequestWrapper 클래스를 사용하여 HttpServletRequest 객체를 래핑하고 기존의 요청 객체를 확장하거나 수정하는 방법을 제공합니다. 이를 통해 개발자는 필요한 요청 처리를 추가하거나 변경할 수 있습니다.
>
> **그러나 ServletRequest 객체가 매 요청마다 생성되는 것을 완전히 회피할 수 있는 방법은 아직까지는 없습니다. 요청마다 고유한 정보를 유지해야 하기 때문에 최소한의 메모리 사용과 빠른 응답 시간을 보장하기 위해 Servlet 컨테이너는 매 요청마다 객체를 인스턴스화해야 합니다.**

Servlet Container에서 관리하는 3개의 저장소에서 어디에 무엇을 저장할지를 잘 결정해야 한다.
로그인 정보같은 경우는 Session에 저장해야 할 것이다.

## HttpServlet
서블릿이 다양한 프로토콜에 대응할 수 있도록 만들었다. (GenericServlet)
그래서 명시적으로 타입캐스팅을 해주면서 다양한 프로토콜에 대응하는 서블릿을 만들 수 있는데,
사실상 HTTP 프로토콜만이 사용되다보니, GenericServlet이 아닌 HttpServlet 클래스를 이용한다.
```java
package bitcamp.myapp.servlet;

import java.io.IOException;
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public abstract class HttpServlet extends GenericServlet {

  @Override
  public void service(ServletRequest servletRequest, ServletResponse servletResponse)
      throws ServletException, IOException {
    HttpServletRequest request = (HttpServletRequest) servletRequest;
    HttpServletResponse response = (HttpServletResponse) servletResponse;

    //this에 저장된 인스턴스의 클래스부터 service() 메서드를 찾아 올라간다.
    //현재 클래스(HttpServlet)을 가리키는 것이 아님. 호출한 인스턴스가 될 것임.
    //호출한 인스턴스에 service()가 있으면 그걸 호출할 것임
    //이전 강의에서 이 내용을 java-basic 통해서 강의했던 때가 있었음!!
    this.service(request, response);
  }
}
```

이것은 자바에서 제공하는 java.servlet.HttpServlet 클래스와 거의 유사하다.
```java
  public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    HttpServletRequest request;
    HttpServletResponse response;
    try {
      request = (HttpServletRequest)req;
      response = (HttpServletResponse)res;
    } catch (ClassCastException var6) {
      throw new ServletException(lStrings.getString("http.non_http"));
    }

    this.service(request, response);
  }
```

> 서블릿을 만드는 방법은 뭐냐고 물어볼 수 있다.
서블릿을 만드는 방법은 서블릿을 만드는 인터페이스 사용 규칙에 따라서 만들면 된다.
서블릿을 만드려면 서블릿의 규칙에 따라 클래스를 만들어야 한다.
이게 좋은 답변이고, 'HttpServlet을 상속하면 된다' 라는 답변은 조금 아쉬운 답변이다.
HTTP만을 사용하는 경우가 많고 이 경우 HttpServlet 을 상속하는 것이 좋은 방법이다.

