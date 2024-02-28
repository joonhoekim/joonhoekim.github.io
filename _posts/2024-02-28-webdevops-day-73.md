---
title: (Day	73)
author: 김준회
date: 2024-02-28 17:00:00 +0900
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
# REVIEW
* 세션ID는 쿠키 테이블에 저장된다. 
* 쿠키는 웹서버가 클라이언트의 웹 브라우저에 맡겨놓은 데이터다.
* 해커가 쿠키 내의 세션 ID를 탈취하여 서버에게 제시하면, 서버는 그 유효성을 확인할 수 없다.
* 쿠키는 이름/값/만료일/사용범위 데이터를 갖는다.
  * 사용범위는 URL내에 어디에서 사용되는지를 정의한다.
  * 클라이언트가 요청할 때, 사용범위 내의 요청이면 HTTP 요청 헤더에 해당 쿠키를 추가하여 요청한다. 매번 보낸다. 이것은 브라우저 내부적으로 구현된 기능이다.
* 요청/응답에 따른 세션 라이프사이클

---
# URL 인코딩/디코딩
URL에 사용할 수 있는 문자가 제한되어 있다. 

| 문자 유형             | 사용 가능한 문자                                              |
| --------------------- | ------------------------------------------------------------- |
| 알파벳 소문자         | a-z                                                           |
| 알파벳 대문자         | A-Z                                                           |
| 숫자                  | 0-9                                                           |
| 특수 문자             | -  _  .  ~                                                    |
| 예약된 문자           | !  *  '  (  )                                                 |
| 미리 정의된 문자 집합 | $  &  +  ,  ;  =  :  @                                        |
| 부호화된 문자         | % 문자를 나타내기 위한 16진수 표현 (%20은 공백 문자를 나타냄) |
| 사용자 정의 문자      | 일반적으로 % 다음의 16진수로 표현됨 (예: %20은 공백 문자)     |

그래서 한글이나 % 같은 문자들을 사용하려면, 사용가능한 문자들을 이용해서 표현하도록 `변환`해야 한다. 그것을 URL 인코딩이라고 한다. 반대로 그렇게 변환된 문자를 원래의 문자로 바꿔주는 것을 URL 디코딩이라고 한다. URL 인코딩은 `%OO` 형태인데, 1바이트를 16진수 두자리로 표현하고 그 구분을 %로 하는 방식이다. 

Java에서는 java.net 패키지에서 URLEncoer 클래스를 통해 encode(), decode() 메서드로 지원된다.

# 필터 적용하기
Request 의 모든 character set 은 UTF-8 을 사용함.
필터는 모든 서블릿의 실행 전에 조건에 따라 (URL 사용 범위에 따라) 실행되므로 이곳에서 설정

```java
package bitcamp.myapp.filter;

import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;


public class CharacterEncodingFilter implements Filter {

  private String encoding;

  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
    this.encoding = filterConfig.getInitParameter("encoding");

    if (encoding == null) {
      encoding = "UTF-8";
    }
  }

  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
      throws IOException, ServletException {

    if (encoding != null) {
      encoding = "UTF-8";
      request.setCharacterEncoding(encoding);
      //response.setCharacterEncoding();
    }

    chain.doFilter(request, response);
  }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  metadata-complete="false"
  version="4.0"
  xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
  <!-- 이것을 DD File 이라고 한다.-->
  <!--  metadata으로 완성된다. Annotation은 무시한다.-->

  <!--  context-param 은 모든 서블릿이 접근할 수 있다! -->
  <context-param>
    <param-name>jdbc.driver</param-name>
    <param-value>com.mysql.jdbc.Driver</param-value>
  </context-param>
  <context-param>
    <param-name>jdbc.url</param-name>
    <param-value>jdbc:mysql://localhost:3306/studydb</param-value>
  </context-param>


  <context-param>
    <param-name>jdbc.username</param-name>
    <param-value>study</param-value>
  </context-param>

  <context-param>
    <param-name>jdbc.password</param-name>
    <param-value>1111</param-value>
  </context-param>

  <description>
    애플리케이션 소개
  </description>

  <display-name>과제 관리 시스템</display-name>

  <filter>
    <filter-class>bitcamp.myapp.filter.CharacterEncodingFilter</filter-class>
    <filter-name>CharacterEncodingFilter</filter-name>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>
```

# JSP
Java Server Pages (Jakarta Server Pages)
서블릿에서 매번 out.print(HTML 코드) 같은 방식으로, 라인별로 클라이언트에게 보내줄 HTMl 문서를 작성하는 것은 아주아주 고통스러운 일이다. 그래서 좀더 자연스러운 방식으로, 동적인 HTML 문서를 만드는 방법이 등장했다. 그것이 JSP이다.

* JSP 문법에 따라 jsp 파일을 작성한다. (좀 더 간결하다.)
* JSP 엔진이 jsp 파일을 java 소스 파일로 컴파일한다.
  * jsp 파일에 작성된대로 동적 HTML 문서를 만드는 `서블릿`으로 컴파일한다.
* ![](https://media.geeksforgeeks.org/wp-content/uploads/20210702122047/m5.png)

참고로 JSP는 1999년에 처음 등장했다. 동적 웹을 만드는데 JSP 의 편의성이 큰 인기를 끌면서 java의 인기를 이끌었다. 지금은 Spring MVC, JavaServer Faces, AngularJS, React 등의 후발주자 기술들에 밀려서 우선순위가 낮은 하나의 선택지로 남아있지만...

모든 HTML 문서를 보내는 것보다 필요한 데이터만 API로 받는 것이 더 가볍기도 하고..

JSP의 상속구조
* HttpJspBase

JSP의 예시. 아래처럼 작성하면, 다음 코드블럭의 java 서블릿으로 변환된다.
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>
<html lang='en'>
<head>
  <meta charset='UTF-8'>
  <title>비트캠프 데브옵스 5기</title>
</head>
...
```

변환된 java 서블릿의 예시
```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.apache.jsp.member;

import java.io.IOException;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;
import javax.el.ExpressionFactory;
import javax.servlet.DispatcherType;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.jsp.JspFactory;
import javax.servlet.jsp.JspWriter;
import javax.servlet.jsp.PageContext;
import javax.servlet.jsp.SkipPageException;
import org.apache.jasper.runtime.HttpJspBase;
import org.apache.jasper.runtime.InstanceManagerFactory;
import org.apache.jasper.runtime.JspRuntimeLibrary;
import org.apache.jasper.runtime.JspSourceDependent;
import org.apache.jasper.runtime.JspSourceImports;
import org.apache.tomcat.InstanceManager;

public final class form_jsp extends HttpJspBase implements JspSourceDependent, JspSourceImports {
  private static final JspFactory _jspxFactory = JspFactory.getDefaultFactory();
  private static Map<String, Long> _jspx_dependants;
  private static final Set<String> _jspx_imports_packages = new HashSet();
  private static final Set<String> _jspx_imports_classes;
  private volatile ExpressionFactory _el_expressionfactory;
  private volatile InstanceManager _jsp_instancemanager;

  static {
    _jspx_imports_packages.add("javax.servlet");
    _jspx_imports_packages.add("javax.servlet.http");
    _jspx_imports_packages.add("javax.servlet.jsp");
    _jspx_imports_classes = null;
  }

  public form_jsp() {
  }

  public Map<String, Long> getDependants() {
    return _jspx_dependants;
  }

  public Set<String> getPackageImports() {
    return _jspx_imports_packages;
  }

  public Set<String> getClassImports() {
    return _jspx_imports_classes;
  }

  public ExpressionFactory _jsp_getExpressionFactory() {
    if (this._el_expressionfactory == null) {
      synchronized(this) {
        if (this._el_expressionfactory == null) {
          this._el_expressionfactory = _jspxFactory.getJspApplicationContext(this.getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }

    return this._el_expressionfactory;
  }

  public InstanceManager _jsp_getInstanceManager() {
    if (this._jsp_instancemanager == null) {
      synchronized(this) {
        if (this._jsp_instancemanager == null) {
          this._jsp_instancemanager = InstanceManagerFactory.getInstanceManager(this.getServletConfig());
        }
      }
    }

    return this._jsp_instancemanager;
  }

  public void _jspInit() {
  }

  public void _jspDestroy() {
  }

  public void _jspService(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    if (!DispatcherType.ERROR.equals(request.getDispatcherType())) {
      String _jspx_method = request.getMethod();
      if ("OPTIONS".equals(_jspx_method)) {
        response.setHeader("Allow", "GET, HEAD, POST, OPTIONS");
        return;
      }

      if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)) {
        response.setHeader("Allow", "GET, HEAD, POST, OPTIONS");
        response.sendError(405, "JSPs only permit GET, POST or HEAD. Jasper also permits OPTIONS");
        return;
      }
    }

    JspWriter out = null;
    JspWriter _jspx_out = null;
    PageContext _jspx_page_context = null;

    try {
      response.setContentType("text/html; charset=UTF-8");
      PageContext pageContext = _jspxFactory.getPageContext(this, request, response, (String)null, true, 8192, true);
      _jspx_page_context = pageContext;
      pageContext.getServletContext();
      pageContext.getServletConfig();
      pageContext.getSession();
      out = pageContext.getOut();
      out.write("\r\n");
      out.write("\r\n");
      out.write("\"<!DOCTYPE html>\"\r\n");
      out.write("\"<html lang='en'>\"\r\n");
      out.write("\"<head>\"\r\n");

...

```

## MVC 1
이렇게 기존 서블릿을 jsp로 변환하면 MVC 버전 1 이다. (주의! 학습을 위해 구분한 것이지 공식적인 내용은 아니다.) MVC는 왜 등장했나? 출력은 편하게 하는데 java코드는 도리어 작성하기 번거로워진다. 뭐가 주체가 되느냐의 차이가 된 것이다. 그래서 서블릿은 서블릿 그대로 사용하고 출력이 중요한 부분은 jsp로 작성하는 형태로 분리된다.

## MVC 2
실행흐름을 제어(Controller)하는 것은 서블릿이 담당한다. UI를 생성(View)하는 것은 JSP파일이 담당한다. 데이터를 처리(Model)하는 것은 도메인과 DAO를 통해서 DBMS와 통신하며 담당한다.

핵심: 모든 요청은 서블릿이 받는다. 

타임리프쓰거나 ajax 로 HTML..

## JSP안에 codelet 언어
사실 PHP, C#, C++ 같은 언어도 코드릿으로 넣을 수 있다. 근데 java 말고는 인듀싱 포인트가 없었다.. php는 그냥 쓰면 되고, 다른 기술들도 더 나은 대안들이 있었다. 초기에는 실험적으로 다른 언어를 JSP 안에 코드릿에 사용해보기도 했으나 얼마 유지되지 못했다. 그 흔적이 JSP에서 <%@language="java ...> 하는 부분이 남아있다. java 넣지 않으면 안되는데 남아있는 이유는, 이전 다른 언어를 코드릿 언어로 채택한 JSP에 대한 호환성 지원을 위한 것이다...

처음부터 원대한 목적(?)을 가진 경우보다, 작은 목적으로 시작했는데 위대한 일을 하고 있는 경우가 많은 것 같다. 단순히 이런 사례를 보고 난 이후의 확증편향일 수도 있지만.

## JSP와 Servlet의 역할
서블릿이 하면 좋은 일
* 입력데이터를 작업하기 적합하게 가공
* 모델 객체를 통해 요청한 작업을 처리
* JSP가 출력하기 적합하게 작업한 데이터를 가공
* 클라이언트로 보낼 화면을 만든 JSP를 실행

JSP가 하면 좋은 일
* 서블릿이 준비해준 데이터를 가지고 화면을 만든다. (HTML/CSS/JS)
## JSP 원리
익스프레션은 out.write(); 에 라인별로 파라미터로 다 넣어주고 코드릿은 out.print();에 라인별로 파라미터로 다 넣어준다. 그래서 세미콜론을 붙이는 일이 없다. 코드릿에는 Expression이 와야 한다. <%   %>





# 취업관련
* 자기소개서나 포트폴리오/이력서가 면접 기회를 만들고 면접의 방향을 결정한다.
* 스프링 IOC 컨테이너 직접 만들었다 등 수업에서 진행한 내용에서 현업의 관심이 들만한 내용으로 면접 유도하는 것을 권장함 (리플렉션/맵)
* 수업 끝나고 실습내용 반복해볼 것! 새로울 것임.
