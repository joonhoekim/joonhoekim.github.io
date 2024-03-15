---
title: (Day	84) 스프링 프레임워크는 요청에 따라 다른 메서드를 호출한다.
author: 김준회
date: 2024-03-15 17:00:00 +0900
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

[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8A%A4%ED%94%84%EB%A7%81%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC1.pdf)

이전에는 코드트리 풀면서 문제를 정리해야겠다는 생각이 크게 들지 않았다. 근데 요새는 해야겠다고 느낀다. 시뮬레이션, 완전탐색을 배우면서부터는 문제를 분류하고, 풀어내는 아이디어가 재미있으면서도 아주 중요하다고 느꼈기 때문이다. 그런 아이디어들을 글로 작성하고, 이미지로 시각화하면서 문서화하는 작업이 이제 필요한 시점이 됐다. 그리고, 이런 러프한 글 말고 시각화 위주의 컨텐츠를 만들어 낼 수 있는 시점이 왔다. 봄이 다가온다...

# Review
- 학습 점검 목록
  - ServletContainerInitializer의 구동 원리를 설명할 수 있는가?
  - SpringServletContainerInitializer 클래스의 구동 원리를 설명할 수 있는가?
  - WebApplicationInitializer의 구동 원리를 설명할 수 있는가?
  - WebApplicationIntializer를 활용하여 ContextLoaderListener와 DispatcherServlet을 설정할 수 있는가?
    - 맨 마지막 상속된 추상클래스를 쓰거나 인터페이스를 처음부터 구현하거나! 

# URL, 메서드, 파라미터에 따라 호출될 메서드를 분리할 수 있다 (src-14)
이것들은 서블릿과 java를 써서 만들어진 스프링에서 지원하는 것이다. 이런 기능들을 바닥부터 매번 다시 쌓아올리려면 엄청나게 어려울 것이다.

## 사전 세팅
configuration
```java
package bitcamp.config;

import org.springframework.context.annotation.ComponentScan;

@ComponentScan("bitcamp.app1")
public class App1Config {
}
```

Initializer
```java
package bitcamp.config;

import java.io.File;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class App1WebApplicationInitializer
    extends AbstractAnnotationConfigDispatcherServletInitializer {

  String uploadTmpDir;

  public App1WebApplicationInitializer() {
    uploadTmpDir = new File(System.getProperty("java.io.tmpdir")).getAbsolutePath();
    System.out.println("업로드 임시 폴더: " + uploadTmpDir);
  }

  @Override
  protected Class<?>[] getRootConfigClasses() {
    return null;
  }

  @Override
  protected Class<?>[] getServletConfigClasses() {
    return new Class<?>[] {App1Config.class};
  }

  @Override
  protected String[] getServletMappings() {
    return new String[] {"/app1/*"};
  }

  @Override
  protected String getServletName() {
    return "app1";
  }
}
```

## 애너테이션 사용의 기본 방법
```java
// 페이지 컨트롤러 만드는 방법
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller // 이 애노테이션을 붙인다.
@RequestMapping("/c01_1") // 컨트롤러에 URL을 매핑한다.
public class Controller01_1 {

  //@RequestMapping // 이 애노테이션을 붙여서 요청이 들어왔을 때 호출될 메서드임을 표시한다.
  @ResponseBody // 메서드의 리턴 값이 클라이언트에게 출력할 내용임을 표시한다.
  public String handler() {
    return "c01_1 -> handler()";
  }

  // URL 한 개 당 한 개의 핸들러만 연결할 수 있다.
  // 같은 URL에 대해 다른 메서드를 또 정의하면 실행 오류가 발생한다.
  @RequestMapping
  @ResponseBody
  public String handler2() {
    return "c01_1 -> handler2()";
  }
}
```

## 디렉토리 방식도 지원됨
```java
// 페이지 컨트롤러 만드는 방법 - 한 개의 페이지 컨트롤러에 여러 개의 요청 핸들러 두기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class Controller01_2 {

  // 요청이 들어 왔을 때 호출되는 메서드를 "request handler" 라 부른다.

  @RequestMapping("/c01_2_h1") // 핸들러에서 URL을 지정한다.
  @ResponseBody
  public String handler() {
    return "c01_2_h1";
  }

  @RequestMapping("/c01_2_h2") // 핸들러에서 URL을 지정한다.
  @ResponseBody
  public String handler2() {
    return "c01_2_h2";
  }

  @RequestMapping("/c01_2/h3") // URL을 지정할 때 디렉토리 형식으로 지정할 수 있다.
  @ResponseBody
  public String handler3() {
    return "/c01_2/h3";
  }

  @RequestMapping("/c01_2/h4") // URL을 지정할 때 디렉토리 형식으로 지정할 수 있다.
  @ResponseBody
  public String handler4() {

  // 한 개의 request handler에 여러 개의 URL을 매핑할 수 있다.
  @RequestMapping({"/c01_2/h5", "/c01_2/h6", "/c01_2/h7"})
  @ResponseBody
  public String handler5() {
    return "/c01_2/h5,h6,h7";
  }
}
```
## 기본 URL과 상세 URL 분리가 가능함
```java
// 페이지 컨트롤러 만드는 방법 - 기본 URL과 상세 URL을 분리하여 설정하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c01_3") // 핸들러에 적용될 기본 URL을 지정한다.
public class Controller01_3 {

  @RequestMapping("h1") // 기본 URL에 뒤에 붙는 상세 URL. 예) /c01_3/h1
  @ResponseBody
  public String handler() {
    return "h1";
  }

  @RequestMapping("/h2") // 앞에 /를 붙여도 되고 생략해도 된다. 예) /c01_3/h2
  @ResponseBody
  public String handler2() {
    return "h2";
  }

  @RequestMapping("h3")
  @ResponseBody
  public String handler3() {
    return "h3";
  }

  @RequestMapping("h4")
  @ResponseBody
  public String handler4() {
    return "h4";
  }

  @RequestMapping({"h5", "h6", "h7"})
  @ResponseBody
  public String handler5() {
    return "h5,h6,h7";
  }
}
```

## 메서드에 따라 구분하여 호출할 메서드를 분리할 수 있음
```java
// GET, POST 구분하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c02_1")
public class Controller02_1 {

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/html/app1/c02_1.html

  @RequestMapping(method = RequestMethod.GET) // GET 요청일 때만 호출된다.
  @ResponseBody
  public String handler1() {
    return "get";
  }

  @RequestMapping(method = RequestMethod.POST) // POST 요청일 때만 호출된다.
  @ResponseBody
  public String handler2() {
    return "post";
  }
}
```
이 방식으로 애너테이션을 붙여도 된다.
```java
// GET, POST 구분하기 II
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c02_2")
public class Controller02_2 {

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/html/app1/c02_2.html

  @GetMapping // GET 요청일 때만 호출된다.
  @ResponseBody
  public String handler1() {
    return "get";
  }

  @PostMapping // POST 요청일 때만 호출된다.
  @ResponseBody
  public String handler2() {
    return "post";
  }
}
```


## GET요청에서 파라미터에 따라 호출될 메서드 구분하기
```java
// request handler를 구분하는 방법 - 파라미터 이름으로 구분하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c03_1")
public class Controller03_1 {

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/app1/c03_1?name=kim
  @GetMapping(params = "name")
  @ResponseBody
  public String handler1() {
    return "handler1";
  }

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/app1/c03_1?age=20
  @GetMapping(params = "age")
  @ResponseBody
  public String handler2() {
    return "handler2";
  }

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/app1/c03_1?name=kim&age=20
  @GetMapping(params = {"age", "name"})
  @ResponseBody
  public String handler3() {
    return "handler3";
  }

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/app1/c03_1
  @GetMapping
  @ResponseBody
  public String handler4() {
    return "handler4";
  }
}
```

## HTTP 프로토콜의 헤더 이름에 따라 호출될 메서드 정의도 가능함
```java
// request handler를 구분하는 방법 - 요청 헤더 이름으로 구분하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c03_2")
public class Controller03_2 {

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/html/app1/c03_2.html
  // => 요청 헤더 중에서 특정 이름을 갖는 헤더가 있을 때 호출될 메서드를 지정할 수 있다.
  // => 웹 페이지에서 링크를 클릭하거나 입력 폼에 값을 넣고 등록 버튼을 누르는
  //    일반적인 상황에서는 요청헤더에 임의의 헤더를 추가할 수 없다.
  // => 자바스크립트 등의 프로그래밍으로 임의의 HTTP 요청을 할 때
  //    HTTP 프로토콜에 표준이 아닌 헤더를 추가할 수 있다.
  //    그 헤더를 처리하는 메서드를 정의할 때 사용한다.
  // => 보통 Open API를 개발하는 서비스 회사에서 많이 사용한다.

  // @GetMapping(headers="name")
  @RequestMapping(method = RequestMethod.GET, headers = "name")
  @ResponseBody
  public String handler1() {
    return "handler1";
  }

  // @GetMapping(headers="age")
  @RequestMapping(method = RequestMethod.GET, headers = "age")
  @ResponseBody
  public String handler2() {
    return "handler2";
  }

  @GetMapping(headers = {"age", "name"})
  @ResponseBody
  public String handler3() {
    return "handler3";
  }

  @GetMapping
  @ResponseBody
  public String handler4() {
    return "handler4";
  }
}
```

## 헤더의 Accept 값에 따라서도 호출할 메서드 변경이 가능하다.

HTTP Request, Response의 Header에 대해 이전에 배운 적이 있었다. 서블릿에서 한글 깨짐은 Response Header에서 Content-Type에서 text/html;charset=UTF-8 이 아닌 경우였다.

Request Header를 확인해보면 Accept라는 헤더 필드(Header Field)가 있다.
이런 식이다. 

```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
```

이 헤더 필드에 따라서 (클라이언트가 원하는 응답의 형태에 따라서) 다른 메서드를 호출할 수 있다. 아래와 같은 형태로 애너테이션으로 설정할 수 있다.

**produces는 메서드가 어떤 형식의 데이터를 생산하는지를 아주 직관적으로 표현한 동사다.**

```java
// request handler를 구분하는 방법 - Accept 요청 헤더의 값에 따라 구분하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c03_3")
public class Controller03_3 {

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/html/app1/c03_3.html
  // => 요청 헤더 중에서 Accept의 값에 따라 구분할 때 사용한다.
  //
  // Accept 헤더?
  // => HTTP 클라이언트(웹 브라우저)에서 서버에 요청할 때
  //    받고자 하는 콘텐트의 타입을 알려준다.

  @GetMapping(produces = "text/plain")
  @ResponseBody
  public String handler1() {
    return "text";
  }

  @GetMapping(produces = "text/html")
  @ResponseBody
  public String handler2() {
    return "<html><body><h1>header</h1></body></html>";
  }

  @GetMapping(produces = "application/json")
  @ResponseBody
  public String handler3() {
    return "{\"title\":\"text\"}";
  }

  @GetMapping(produces = "text/csv")
  @ResponseBody
  public String handler4() {
    return "john,39,male";
  }

  @GetMapping
  @ResponseBody
  public String handler5() {
    return "others...";
  }
}
```

프론트엔드, 백엔드가 분리된 경우에, HTTP 요청 헤더는 굉장히 유용하다. 어떤 자료를 원하는지를, HTTP 요청 헤더의 본연의 규칙에 따라 알 수 있다.


REST API가 제대로 개발되기 전에는, 헤더를 구분하지 않고 URL로만 구분한 적도 있었다. RESTful이 확산되면서, Accept 요청 헤더 뿐만 아니라 메서드 또한 본연의 설계 목적대로 사용되기 시작한다. GET, POST 메서드만 쓰지 않고, 호출될 메서드가 수행할 작업을 고려한다.

## 클라이언트의 POST요청에서 보내는 데이터 형식에 따라 다른 메서드를 호출하기

```java
// request handler를 구분하는 방법 - Content-Type 헤더의 값에 따라 구분하기
package bitcamp.app1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c03_4")
public class Controller03_4 {

  // Content-Type 요청 헤더
  // => HTTP 클라이언트가 보내는 데이터의 콘텐트 타입이다.
  // => 프론트 컨트롤러는 보내는 데이터의 타입에 따라 처리를 구분할 수 있다.

  // 테스트 방법:
  // => http://localhost:9999/eomcs-spring-webmvc/html/app1/c03_4.html
  // => 클라이언트가 POST 요청으로 데이터를 보낼 때 기본 형식은 다음과 같다.
  //      application/x-www-form-urlencoded
  // => <form> 태그에서 enctype 속성에 "mulpart/form-data"를 지정하면
  //    해당 형식으로 서버에 값을 보낸다.
  // => 자바스크립트를 사용하여 개발자가 임의의 형식으로 값을 보낼 수 있다.
  //
  // 클라이언트가 POST로 요청할 때 보내는 데이터의 유형에 따라 호출될 메서드를 구분할 때 사용한다.

  // 다음 메서드는 application/x-www-form-urlencoded 형식의 데이터를 소비한다.
  // => 즉 클라이언트의 HTTP 요청에서 Content-Type 헤더의 값이 위와 같을 때
  //    이 메서드를 호출하라는 의미다.
  @PostMapping(consumes = "application/x-www-form-urlencoded")
  @ResponseBody
  public String handler1() {
    return "handler1";
  }

  // 다음 메서드는 multipart/form-data 형식의 데이터를 소비한다.
  @PostMapping(consumes = "multipart/form-data")
  @ResponseBody
  public String handler2() {
    return "handler2";
  }

  // 다음 메서드는 text/csv 형식의 데이터를 소비한다.
  @PostMapping(consumes = "text/csv")
  @ResponseBody
  public String handler3() {
    return "handler3";
  }

  // 다음 메서드는 application/json 형식의 데이터를 소비한다.
  @PostMapping(consumes = "application/json")
  @ResponseBody
  public String handler4() {
    return "handler4";
  }

  // 다음 메서드는 Content-Type 헤더가 없을 때 호출된다.
  @RequestMapping
  @ResponseBody
  public String handler5() {
    return "handler5";
  }
}
```


# SpringBoot 의 예제
SpringBoot 의 예제를 보면 어노테이션이 `@RestController`이다. 서버에서 HTML을 만들어서 클라이언트에게 응답하는 게 아니라, RESTFul 방식으로 데이터만 주고, 나머지 화면을 만드는 건 이제 프론트엔드로 분리된 것이다. 그 방식이 대세가 됐다. 서점이나 강의 플랫폼들을 보면 스프링 프레임워크보다 스프링 부트에 대한 컨텐츠가 더 많다... 백엔드에선 데이터만 주는 것이 대세다. 이제 백엔드 개발자, 프론트엔드 개발자가 무엇인지 더 명확하게 알게 되었다.

# 팀 프로젝트
DB Modeling 리뷰를 받았다. 필요 없이 분할된 테이블이 많았고, fk 조건을 묶어서 줘야 하는 부분이 있다는 것을 알게 되었다.
스터디 모집글에 지원하여, 지원이 수락된 사용자들이 스터디와 연결된 테이블이 있고 그 테이블에 있는 해당 회원만 작성에 할당해야 한다.
이것을 데이터베이스에서 무결성을 지키도록 제약조건을 넣을 수 있는데, (member_no, study_no) 같은 식으로 속성을 두 개 묶어서 fk를 제약조건으로 걸면 DBMS 무결성을 한번 더 검증할 수 있다.


# 계획형 인간
면접에서 들어와서 경력을 어떻게 개발하고 싶은지 장기적인 계획을 물어볼 수 있음. 혹은 어떤 일을 하고 싶은지 물어볼 수 있음. 이에 대한 답변이 준비되어야 함. (물론 이건 이직을 금방 할 거라는 인상을 주라는 것이 아니라, 계획성이 있고 발전가능성이 있는 사람으로 보여야 한다는 것임. 사실 업계에서 평생직장으로 근무할 것이라거나 4-5년 재직할 거라고 기대하지도 않음. 3년 근무하더라도 퍼포먼스가 곧 나올 사람을 원함. 그래서 글로벌 기업으로도 가고싶다 이런 포부는 괜찮음.)

# Postman
웹 서비스를 개발하다보면 브라우저틀 통해서 요청을 보내서 어떤 응답이 들어오는지 확인하는 일을 자주 하게 된다.

이것을 체계적으로 할 수 있는 툴이 Postman이다.
요청의 메서드를 GET, POST 등 무엇으로 할 지를 미리 작성해두거나, 응답을 편하게 본다거나 할 수 있다. 웹 서비스 개발에 필수적인 툴이다.