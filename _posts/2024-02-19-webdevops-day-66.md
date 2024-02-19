---
title: (Day	63)
author: 김준회
date: 2024-02-14 17:00:00 +0900
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
주말간 리눅스 커맨드 라인을 공부했다.

# Review
## 아키텍처의 변화
이전의 클라이언트/서버 아키텍처는 웹 앱/서버 아키텍처로 변경됐다.
서버 앱이 톰캣 서버로 대체되었다. 톰캣 서버는 웹 서버와 서블릿 컨테이너로 구성된다.

**클라이언트 앱은 웹 브라우저로 대체되었다.** HTTP 프로토콜로 통신한다.

톰캣 서버는 네트워킹, HTTP(프로토콜), 멀티스레딩 관리를 할 수 있다. 그것을 직접 구현했던 이전과는 달리 이제 다른 사람들이 작성한 것을 쓰게 된 것이다.

DBMS와의 통신은 JDBC를 통해 하는 이전 방식과 동일하다. java App이 서버에서 돌아가면서 JDBC를 통해 DBMS와 통신한다.

### 다르게 표현하면
웹 기술을 기반으로 Application Server 아키텍처를 구축한 것이다.
어플리케이션은 서버에서 구동된다. 
물론 받은 HTTP 문서를 해석하여 렌더링하고 사용자에게 보여주는 것은 브라우저가 하는 일이다.

### 지금 중요한 것은
톰캣 서버가 어떻게 동작하는지보다, 어떻게 웹/서버 아키텍처가 구성되는지다.
톰캣 서버는 Networking, HTTP, MultiThreading을 처리해주는 도구다. 제이보스, 웹스피어, TOW, 제우스 등등 다른 것으로 대체될 수 있는 도구다. 그래서 이것이 어떻게 동작하는지를 자세히 알아보는 것은 지금 중요도가 떨어진다.

어떤 솔루션을 쓰건간에 대부분 웹서버와 서블릿 컨테이너로 구성된다.
웹서버가 스태틱 리소스를 서비스해준다. 다이나믹하게 뭔가를 바꿔줄 필요가 없는 리소스를 요청하면 웹서버가 서비스해준다.
모든 요청은 웹서버가 받는다. 웹서버가 동적 요청을 받는 경우는 서블릿 컨테이너에 그 요청에 대한 처리를 위임한다.

서블릿 컨테이너는 자바 앱을 관리한다.
서블릿 컨테이너는 서블릿(자바 클래스)를 호출하여, 위임받은 요청을 처리한다.

### 정보 시각화
![](https://www.programcreek.com/wp-content/uploads/2013/04/web-server-servlet-container.jpg)
![](https://miro.medium.com/v2/resize:fit:1121/1*yw7D8njef49dBsR15wMY9g.png)
![](https://docs.oracle.com/javaee/5/tutorial/doc/figures/overview-serverAndContainers.gif)
![](https://miro.medium.com/v2/resize:fit:1400/1*RoDdyWhZxiZ5ODWoK9XnGw.png)

## Servlet, 서블릿
### 서블릿이라는 용어의 유래
-let 은 작은 것을 지칭할 때 쓰이는 접미어
server Application + let = servlet ... 작은 서버앱을 서블릿이라고 부른다.

### 서블릿은 서버앱을 구성하는 요소
서블릿은 서버 앱의 작은 기능을 의미하므로
서블릿은 서버 앱을 구성하는 요소다.
서버 앱은 최소 하나, 아니면 여러 개의 서블릿으로 구성된다.

## 서블릿 컨테이너
서블릿 컨테이너는 메서드를 호출한다.
물론 서블릿 컨테이너가 호출할 수 있는 메서드는 클래스부터 규칙에 따라 만들어야 한다.

### 서블릿으로 사용하기 위한 자바 클래스 규칙
* 서블릿 클래스 구현
  * 메서드 구현
    * init : 객체 생성후 호출되는 초기화용 메서드
    * service : 요청이 들어올 때마다 실행될 메서드
    * destory : 서블릿 종료시 호출될 자원해제 등 목적을 가진 메서드
    * getServletInfo
    * getServletConfig
  * 이닛, 서비스, 디스트로이는 서블릿 생명주기 메서드라고 부른다. 
* 배치 정보를 설정해야 한다.
  * 어떤 경로(URL)에서 서블릿이 호출될 것인지 애노테이션으로 정의해줘야 한다.
    * 이 애노테이션은 서블릿 컨테이너에게 알려주는 것이다.

### 서블릿 객체를 만들고, 배치하고, 사용하는 방법?
인터뷰인 경우, 후속 질문이 자연스럽게 흘러가도록

### Generic Servlet
추상클래스인 제네릭서블릿은 Servlet 인터페이스를 service()를 제외하고 나머지를 구현해둔 것이다.
그래서 서블릿 인터페이스를 구현하는 것보다 제네릭서블릿을 상속하는 것이 보통 더 편한다.
* 참고로 서블릿은 HTTP 말고 다른 프로토콜에도 대응할 수 있도록 만들어졌다. 그래서 리퀘스트와 리스폰스를 항상 명시적 캐스팅을 해줘야 한다. 그래서, GenericServlet을 상속하여 HttpServletRequest라는 것이 만들어져 있다. 현재는 HTTP가 거의 100% 지배한 상황이라 HttpServletRequest를 상속해서 쓰는 경우가 대부분이다.

## 웹 브라우저의 역할
이전의 CLI 클라이언트를 웹브라우저가 대체했다.
웹브라우저는 서버가 보내준 것을 사용자에게 출력하는 역할이 핵심이다.
HTML 마크업 언어는 이 출력되는 것을 단순한 텍스트에서 서식과 하이퍼링크를 가진 더 나은 출력이 가능하게 한다.

### 서블릿 컨테이너가 관리하는 객체
* 저장과 관련된 객체
  * ServletContext: 웹 애플리케이션 저장소
    * 웹 애플리케이션당 1개: DB 커넥션, DAO 객체, 트랜잭션 관리 객체
  * HttpSession: 세션 저장소
    * 클라이언트당 1개: 로그인 사용자 정보, 페이지 작업 정보 등
  * ServletRequest: 요청 저장소
    * 요청당 1개: 요청을 처리하는 동안 사용할 정보
* 실행과 관련된 객체
  * Servlet: 클라이언트 요청을 처리
  * Filter: 서블릿 실행 전후에 보조 작업을 처리
  * Listner: 특정 상태에 놓일 때 작업을 수행 (이벤트 발생시 처리)

### 서버에서 클라이언트와 세션을 관리하는 기법
* 브라우저당 1 세션 (시크릿창 같은 건 별도의 세션으로 잡힘)
* 탭이 여러 개인 경우도 세션은 한개
> TODO 여기는 좀 더 넣어줘야 할 것 같다.. 브라우저와 서버가 세션을 어떻게 알아내고 저장하는지


### 포함관계
서블릿 컨테이너, 웹 어플리케이션, 서블릿의 포함관계?
서블릿 컨테이너가 하나 혹은 여러개의 웹 어플리케이션을 가질 수 있다.
웹 어플리케이션은 하나 혹은 여러개의 서블릿을 가질 수 있다.

### 어플리케이션 서버를 위한 솔루션
아주 많다.... [24년 랭크 링크](https://www.g2.com/categories/application-server)
1. Tomcat
2. Glassfish
3. Jboss
4. Zeus
5. WebLogic
6. WebSphere
7. Geronimo ⬅️ 제로니모라는 단어에 대해 궁금하신 분은... [1](https://namu.wiki/w/%ED%9E%88%EC%97%90%EB%A1%9C%EB%8B%88%EB%AC%B4%EC%8A%A4?from=%EC%98%88%EB%A1%9C%EB%8B%88%EB%AA%A8) [2](https://m.blog.naver.com/kye507/221492473533) [3](https://m.blog.naver.com/min157482/222080022718)

### URL 인코딩, 디코딩
퍼센트 인코딩 개념이 들어간다.
URL에 `%[16진수]` 를 많이 보아왔다. 이것을 이해하려면 URL 인코딩을 알아야 한다!
https://developer.mozilla.org/ko/docs/Glossary/Percent-encoding
https://ko.wikipedia.org/wiki/%ED%8D%BC%EC%84%BC%ED%8A%B8_%EC%9D%B8%EC%BD%94%EB%94%A9

### request and call `service()`
요청이 들어오면 서블릿 컨테이너가 그것을 HttpServletRequest를 생성하여 그것에 요청을 담는다.
서블릿에서 `service()` 메서드를 실행한다. 

# TIL
## URL을 다루는 코딩 컨벤션
java에서는 카멜 케이스를 쓴다.
sql에서는 전부 소문자에 스네이크 케이스를 쓴다.
url에서는 케밥 케이스를 쓴다.

왜?
* 우선 URL에서 사용할 수 있는 특수문자가 제한되어 있음. 적절한 단어구분 기호가 대시였음.
* 이런 제한 사이에서 검색엔진에 URL을 최적화해야 하는데, 대세에서 벗어나야 할 이유가 없음.

## 웹 어플리케이션과 컨텍스트
웹 어플리케이션 = 컨텍스트 = 서블릿 컨텍스트
용어들이 섞여 쓰인다. 혼용된다.

서블릿 컨텍스트의 라이프사이클에 변화가 발생했을 때 그 이벤트에 대한 동작을 처리할 수 있다. 이 방법은 인터페이스를 구현하여 가능한데, 구현해야 하는 인터페이스는 ServletContextListner 이다.

> Interface for receiving notification events about ServletContext lifecycle changes.
In order to receive these notification events, the implementation class must be either declared in the deployment descriptor of the web application, annotated with WebListener, or registered via one of the addListener methods defined on ServletContext.
>
> Implementations of this interface are invoked at their contextInitialized(javax.servlet.ServletContextEvent) method in the order in which they have been declared, and at their contextDestroyed(javax.servlet.ServletContextEvent) method in reverse order.
> [출처](https://javaee.github.io/javaee-spec/javadocs/)

ServletContextListner를 구현한 리스너를 통해, 웹 애플리케이션이 시작되거나 종료될 때 메서드를 호출할 수 있다. *웹 애플리케이션의 생명주기가 변화했을 때* **서블릿컨테이너가** 리스너를 호출한다.

이벤트가 발생했음을 감지하고(트리거) 리스너를 호출하는 것은 **서블릿 컨테이너**다!
물론 이런 호출은 규칙에 따라 진행된다.

1. 웹 애플리케이션이 시작된 직후: `contextInitialized()`를 호출한다.
   * 주로 하는 일: 웹 애플리케이션이 실행되는 동안, 사용할 자원을 준비한다.
2. 웹 애플리케이션이 종료되기 직전: `contextDestroyed()`를 호출한다.
   * 주로 하는 일: 웹 애플리케이션이 종료되기 전에 사용한 자원을 해제한다.

자원? 무슨 자원?
➡️ DB Connection, DAO, Service Object, Transaction ... 등을 관리하기 위한 자원들.

### 리스너 사용방법
```java
package bitcamp.myapp.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class ContextLoaderListener implements ServletContextListener {
  //웹 애플리케이션이 사용할 자원들을 로드한다.


  @Override
  public void contextInitialized(ServletContextEvent sce) {
    System.out.println("웹 애플리케이션 자원 준비");
  }

  @Override
  public void contextDestroyed(ServletContextEvent sce) {
    System.out.println("웹 애플리케이션 자원 해제");
  }
}
```

OUTPUT
```
1:00:38 PM: Executing 'run'...

> Task :app:compileJava
> Task :app:processResources UP-TO-DATE
> Task :app:classes

> Task :app:run
과제관리 시스템 서버 실행!
웹 애플리케이션 자원 준비
```


---

### 리스너 사용 설명
ServletContextListenr
* 웹 애플리케이션간 DB Connection, DAO, Transaction Manager를 공유할 수 있다.

### init()
```java
  @Override
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }

    /**
     * A convenience method which can be overridden so that there's no need to call <code>super.init(config)</code>.
     * <p>
     * Instead of overriding {@link #init(ServletConfig)}, simply override this method and it will be called by
     * <code>GenericServlet.init(ServletConfig config)</code>. The <code>ServletConfig</code> object can still be
     * retrieved via {@link #getServletConfig}.
     *
     * @exception ServletException if an exception occurs that interrupts the servlet's normal operation
     */
    public void init() throws ServletException {
        // NOOP by default
    }
```

소스코드 보면, 기본생성자는 init() 에서 필수적인 ServletConfig를 따로 호출 안해도 괜찮게 되어 있다. 아마 원래는 `init(ServletConfig config)` 만 있었을지도 모른다. 그런데 그러면 config를 저장하는 부분이 항상 필요한데, 그 불편을 줄이기 위해 init()을 파라미터 없는 것으로 만들어준 것 같다. 서블릿 컨테이너에서는 `init(ServletConfig config)`를 실행한다.

개발자는 init()만 재정의해주면 된다.

이렇게 Dao를 세션에서 공유해서 사용하기 시작할 때부터, DB커넥션 Pool이 제대로 동작한다.
### 시사점
ServletContextListener: 웹 애플리케이션을 시작하거나 종료할 때 작업을 수행시킬 수 있다.
**모든 서블릿이 공유하는 자원을 준비하기에 적절한 위치이다.**

ServletContext의 활용: 웹 애플리케이션당 1개가 생성되는 객체 저장소다. 웹 애플리케이션에서 공유할 객체를 보관하기에 적절하다. 그러한 객체의 예시는 DB 커넥션, DAO, Transaction Manager 같은 것들이다.


# 조언
최소 현재까지 진행된 서버 앱을 변형하여 원하는 앱을 만들 수 있는 정도는 되어야 함.