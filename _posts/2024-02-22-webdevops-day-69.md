---
title: (Day	69)
author: 김준회
date: 2024-02-22 17:00:00 +0900
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
* 서블릿 배치 방법?
  * web.xml 에서 명시
  * 서블릿 클래스에 @WebServlet() 에 value로 주면 된다. 한개 줘도 되고 여러개 줘도 된다. `value={ "/.../...", "/.../*" }`
    * value인 경우 값이 1개 뿐인 경우는 중괄호 생략 가능
* HttpServlet
  * 이것을 상속받는다는 이야기는
    * 1. 형변환을 굳이 안하겠다는 것
    * 2. 요청의 종류에 따라 상세하게 서블릿을 제작한다는 것
      * Http 요청이 Get이 될 수도 있고, Post 요청이 될 수도 있다.
      * 그래서 doGet() doPost() 등등등 요청에 따른 메서드를 오버라이드해서 사용한다.
* 필터
  * URL에 따라 서블릿을 실행하는 전/후로 기능을 추가/변경하려고 할 때 쓴다.
  * 필터의 생명 주기에 따라 호출되는 메서드 설명
    * 일단 웹 앱이 시작할 때 필터 객체가 생성됨
    * 객체 생성시 init() 호출됨
    * 필터 배치의 방법: @WebFilter() 혹은 web.xml 배치
    * 필터 동작 과정: Chain of Resource (GoF) 디자인 패턴이 적용됨 
* 리스너
  * 이벤트(특정상황)에서 호출됨
  * 옵저버 패턴(GoF)이 적용됨
  * XxxListener 클래스들이 있어 이벤트에 적절한 클래스를 사용하는 방식
  * 생명 주기
    * 웹앱 -> 리스너 이니셜라이즈드 호출/리턴 -> 필터 -> 서블릿 -> 필터 -> 웹앱 -> 리스너 디스트로이 호출/리턴
* Design Pattern
  * Observer : Listener
  * Chain Of Responsibility : Filter

스프링 프레임워크도 결국 서블릿으로 돌아감

# PrintWriter 한글꺠짐
서버에서 PrintWriter를 통해 ServletResponse를 보낼 때, 클라이언트 브라우저에서 한글이 ?로 나오는 이유가 무엇일까?

이건 브라우저 잘못이 아니다. (클라이언트쪽 잘못이 아니다.) 서버의 response에서 문자집합 설정을 잘못했기 때문이다.

PrintWriter에서 ServletResponse로 출력하면, 별도 설정이 없으면 character set(문자집합)을 ISO-8859-1 문자집합을 사용한다. 여기엔 256자의 영문 대소문자 및 일부 특수문자만 정의되어 있다. 존재하지 않는 문자들은 퀘스천 마크인 ?(십진수 63)으로 변환되어 출력된다.

순서도 중요하다. ServletResponse 객체가 getWriter()으로 PrintWriter 객체를 얻기 전에 먼저 설정해야 한다.

`res.setContentType("text/plain;charset=UTF-8");`

```java
// 클라이언트로 출력하기 - 한글 깨짐 현상 처리
package com.eomcs.web.ex03;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;

@WebServlet("/ex03/s2")
public class Servlet02 extends GenericServlet {

  private static final long serialVersionUID = 1L;

  @Override
  public void service(ServletRequest req, ServletResponse res)
      throws ServletException, IOException {

    // 한글 깨짐 처리하기
    // => 출력 스트림을 꺼내기 전에
    //    출력 스트림이 사용할 문자표(charset)를 지정하라.
    // => 반드시 출력 스트림을 얻기 전에 설정해야 한다.
    //      res.setContentType("MIME Type;charset=문자표이름");
    //
    res.setContentType("text/plain;charset=UTF-8"); // UCS2(UTF-16) ==> UTF-8
    PrintWriter out = res.getWriter();

    out.println("Hello!");

    // 한글이나 아랍문자, 중국문자, 일본문자는
    // UTF-8 문자표에 정의되어 있기 때문에
    // UTF-8 문자로 변환할 수 있다.
    out.println("안녕하세요!");
    out.println("こんにちは");
    out.println("您好");
    out.println("مع السلامة؛ إلى اللقاء!");

    // MIME Type : Multi-purpose Internet Mail Extension
    // => 콘텐트의 형식을 표현
    // => 콘텐트타입/상세타입
    // => 예) text/plain, text/css, text/html 등
    // => 웹 브라우저는 콘텐트를 출력할 때 서버가 알려준 MIME 타입을 보고
    //    어떤 방식으로 출력할 지 결정한다.
  }
}
```

MIME Type에 메일이라는 단어가 있는데, 이게 원래는 컨텐츠 종류를 메일을 위해 알려주려는 것이었다고 한다 ㅎㅎㅎㅎ 그러나 이제 그 외에도 표준으로 쓰인다. 관련된 정보는 [위키](https://en.wikipedia.org/wiki/Media_type)에서 한번 훑어보면 좋다.

## 컨텐츠 타입도 중요하다
문자집합을 잘 맞췄다고 해도, HTML 데이터를 그대로 출력하는 경우도 있다. 컨텐츠 타입을 HTML로 알려주지 않을 때 그럴 수 있다.

`res.setContentType("text/HTML;charset=UTF-8"); // UCS2(UTF-16) ==> UTF-8`

## 바이너리 데이터 보내고 받기
1. 웹서버에서 정적 자원을 direct로 직접 보내주는 방법이 있다.
2. 서블릿을 통해서 보내는 방법이 있다.

웹서버에서 정적 자원을 직접 주는게 더 효율적이기는 하다. 문제는 제어를 할 수 없다는 것이다. 권한이나 조건에 따라 정적 자원을 보낼지 말지를 결정할 수도 없고, 어떠한 가공도 불가능하다.


```java
// 클라이언트로 출력하기 - 바이너리 데이터 출력하기
package com.eomcs.web.ex03;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.OutputStream;
import javax.servlet.GenericServlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;

@WebServlet("/ex03/s4")
public class Servlet04 extends GenericServlet {

  private static final long serialVersionUID = 1L;

  @Override
  public void service(ServletRequest req, ServletResponse res)
      throws ServletException, IOException {

    // photo.jpeg 파일의 실제 경로 알아내기
    // 1) 서블릿의 환경 정보를 다루는 객체를 먼저 얻는다.
    ServletContext ctx = req.getServletContext();

    // 2) ServletContext를 통해 웹 자원의 실제 경로를 알아낸다.
    // => getRealPath(현재 웹 애플리케이션의 파일 경로) : 실제 전체 경로를 리턴한다.
    String path = ctx.getRealPath("/photo.jpeg");
    System.out.println(path);

    FileInputStream in = new FileInputStream(path);

    // 바이너리를 출력할 때 MIME 타입을 지정해야 웹 브라우저가 제대로 출력할 수 있다.
    // => 웹 브라우저가 모르는 형식을 지정하면 웹 브라우저는 처리하지 못하기 때문에
    //    그냥 다운로드 대화상자를 띄운다.
    res.setContentType("image/jpeg");
    
    //res.setContentType("plain/text");


    OutputStream out = res.getOutputStream();

    int b;
    while ((b = in.read()) != -1) {
      out.write(b);
    }

    out.flush(); // 버퍼 데코레이터에 보관된 데이터를 클라이언트로 방출한다.
    out.close();
    in.close();
  }
}
```

# HTTP
HTTP는 웹 브라우저와 웹 서버 사이에 데이터를 주고받는 규약으로 쓰인다. `규약이름://주소` 형태를 많이 봐 왔을 것이다. 

HTTP의 [스펙(명세)](https://datatracker.ietf.org/doc/html/rfc2616#section-5)을 보자.


## Request (요청)

```
...

5 Request

   A request message from a client to a server includes, within the
   first line of that message, the method to be applied to the resource,
   the identifier of the resource, and the protocol version in use.

        Request       = Request-Line              ; Section 5.1
                        *(( general-header        ; Section 4.5
                         | request-header         ; Section 5.3
                         | entity-header ) CRLF)  ; Section 7.1
                        CRLF
                        [ message-body ]          ; Section 4.3

5.1 Request-Line

   The Request-Line begins with a method token, followed by the
   Request-URI and the protocol version, and ending with CRLF. The
   elements are separated by SP characters. No CR or LF is allowed
   except in the final CRLF sequence.

        Request-Line   = Method SP Request-URI SP HTTP-Version CRLF

5.1.1 Method

   The Method  token indicates the method to be performed on the
   resource identified by the Request-URI. The method is case-sensitive.

       Method         = "OPTIONS"                ; Section 9.2
                      | "GET"                    ; Section 9.3
                      | "HEAD"                   ; Section 9.4
                      | "POST"                   ; Section 9.5
                      | "PUT"                    ; Section 9.6
                      | "DELETE"                 ; Section 9.7
                      | "TRACE"                  ; Section 9.8
                      | "CONNECT"                ; Section 9.9
                      | extension-method
       extension-method = token

   The list of methods allowed by a resource can be specified in an
   Allow header field (section 14.7). The return code of the response
   always notifies the client whether a method is currently allowed on a
   resource, since the set of allowed methods can change dynamically. An
   origin server SHOULD return the status code 405 (Method Not Allowed)
   if the method is known by the origin server but not allowed for the
   requested resource, and 501 (Not Implemented) if the method is
   unrecognized or not implemented by the origin server. The methods GET
   and HEAD MUST be supported by all general-purpose servers. All other
   methods are OPTIONAL; however, if the above methods are implemented,
   they MUST be implemented with the same semantics as those specified
   in section 9.

5.1.2 Request-URI

   The Request-URI is a Uniform Resource Identifier (section 3.2) and
   identifies the resource upon which to apply the request.

       Request-URI    = "*" | absoluteURI | abs_path | authority

   The four options for Request-URI are dependent on the nature of the
   request. The asterisk "*" means that the request does not apply to a
   particular resource, but to the server itself, and is only allowed
   when the method used does not necessarily apply to a resource. One
   example would be

       OPTIONS * HTTP/1.1

   The absoluteURI form is REQUIRED when the request is being made to a
   proxy. The proxy is requested to forward the request or service it
   from a valid cache, and return the response. Note that the proxy MAY
   forward the request on to another proxy or directly to the server

...

```

중요한 건 요청 형태가 이렇다는 것이다. ( 참고로 SP = 스페이스(공백) 이다.)

```
    Request       = Request-Line              ; Section 5.1
                    *(( general-header        ; Section 4.5
                      | request-header         ; Section 5.3
                      | entity-header ) CRLF)  ; Section 7.1
                    CRLF
                    [ message-body ]          ; Section 4.3

---------------------------------------------------------------

    Request-Line   = Method SP Request-URI SP HTTP-Version CRLF
```

1. general-header: 요청 및 응답에 모두 사용한다. date, connection and etc..
2. request-header: 요청에만 사용된다. Accept, Host, User-Agent ...
   - 
   - User-Agent를 보면 복잡하게 되어있는데, 마이크로소프트가 넷스케이프 잡아먹으려고 User-Agent를 정직하지 않게 사용하기 시작했기 때문.
3. entity-header: 컨텐츠 타입 및 컨텐츠 길이 등...
4. CRLF: 빈줄
5. message-body: 서버에 보내는 데이터


## Response (응답)
```
6 Response

   After receiving and interpreting a request message, a server responds
   with an HTTP response message.

       Response      = Status-Line               ; Section 6.1
                       *(( general-header        ; Section 4.5
                        | response-header        ; Section 6.2
                        | entity-header ) CRLF)  ; Section 7.1
                       CRLF
                       [ message-body ]          ; Section 7.2

6.1 Status-Line

   The first line of a Response message is the Status-Line, consisting
   of the protocol version followed by a numeric status code and its
   associated textual phrase, with each element separated by SP
   characters. No CR or LF is allowed except in the final CRLF sequence.

       Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF

6.1.1 Status Code and Reason Phrase

   The Status-Code element is a 3-digit integer result code of the
   attempt to understand and satisfy the request. These codes are fully
   defined in section 10. The Reason-Phrase is intended to give a short
   textual description of the Status-Code. The Status-Code is intended
   for use by automata and the Reason-Phrase is intended for the human
   user. The client is not required to examine or display the Reason-
   Phrase.

   The first digit of the Status-Code defines the class of response. The
   last two digits do not have any categorization role. There are 5
   values for the first digit:

      - 1xx: Informational - Request received, continuing process

      - 2xx: Success - The action was successfully received,
        understood, and accepted

      - 3xx: Redirection - Further action must be taken in order to
        complete the request

      - 4xx: Client Error - The request contains bad syntax or cannot
        be fulfilled

      - 5xx: Server Error - The server failed to fulfill an apparently
        valid request

   The individual values of the numeric status codes defined for
   HTTP/1.1, and an example set of corresponding Reason-Phrase's, are
   presented below. The reason phrases listed here are only
   recommendations -- they MAY be replaced by local equivalents without
   affecting the protocol.

      Status-Code    =
            "100"  ; Section 10.1.1: Continue
          | "101"  ; Section 10.1.2: Switching Protocols
          | "200"  ; Section 10.2.1: OK
          | "201"  ; Section 10.2.2: Created
          | "202"  ; Section 10.2.3: Accepted
          | "203"  ; Section 10.2.4: Non-Authoritative Information
          | "204"  ; Section 10.2.5: No Content
          | "205"  ; Section 10.2.6: Reset Content
          | "206"  ; Section 10.2.7: Partial Content
          | "300"  ; Section 10.3.1: Multiple Choices
          | "301"  ; Section 10.3.2: Moved Permanently
          | "302"  ; Section 10.3.3: Found
          | "303"  ; Section 10.3.4: See Other
          | "304"  ; Section 10.3.5: Not Modified
          | "305"  ; Section 10.3.6: Use Proxy
          | "307"  ; Section 10.3.8: Temporary Redirect
          | "400"  ; Section 10.4.1: Bad Request
          | "401"  ; Section 10.4.2: Unauthorized
          | "402"  ; Section 10.4.3: Payment Required
          | "403"  ; Section 10.4.4: Forbidden
          | "404"  ; Section 10.4.5: Not Found
          | "405"  ; Section 10.4.6: Method Not Allowed
          | "406"  ; Section 10.4.7: Not Acceptable
          | "407"  ; Section 10.4.8: Proxy Authentication Required
          | "408"  ; Section 10.4.9: Request Time-out
          | "409"  ; Section 10.4.10: Conflict
          | "410"  ; Section 10.4.11: Gone
          | "411"  ; Section 10.4.12: Length Required
          | "412"  ; Section 10.4.13: Precondition Failed
          | "413"  ; Section 10.4.14: Request Entity Too Large
          | "414"  ; Section 10.4.15: Request-URI Too Large
          | "415"  ; Section 10.4.16: Unsupported Media Type
          | "416"  ; Section 10.4.17: Requested range not satisfiable
          | "417"  ; Section 10.4.18: Expectation Failed
          | "500"  ; Section 10.5.1: Internal Server Error
          | "501"  ; Section 10.5.2: Not Implemented
          | "502"  ; Section 10.5.3: Bad Gateway
          | "503"  ; Section 10.5.4: Service Unavailable
          | "504"  ; Section 10.5.5: Gateway Time-out
          | "505"  ; Section 10.5.6: HTTP Version not supported
          | extension-code

      extension-code = 3DIGIT
      Reason-Phrase  = *<TEXT, excluding CR, LF>

   HTTP status codes are extensible. HTTP applications are not required
   to understand the meaning of all registered status codes, though such
   understanding is obviously desirable. However, applications MUST
   understand the class of any status code, as indicated by the first
   digit, and treat any unrecognized response as being equivalent to the
   x00 status code of that class, with the exception that an
   unrecognized response MUST NOT be cached. For example, if an
   unrecognized status code of 431 is received by the client, it can
   safely assume that there was something wrong with its request and
   treat the response as if it had received a 400 status code. In such
   cases, user agents SHOULD present to the user the entity returned
   with the response, since that entity is likely to include human-
   readable information which will explain the unusual status.

6.2 Response Header Fields

   The response-header fields allow the server to pass additional
   information about the response which cannot be placed in the Status-
   Line. These header fields give information about the server and about
   further access to the resource identified by the Request-URI.

       response-header = Accept-Ranges           ; Section 14.5
                       | Age                     ; Section 14.6
                       | ETag                    ; Section 14.19
                       | Location                ; Section 14.30
                       | Proxy-Authenticate      ; Section 14.33
                       | Retry-After             ; Section 14.37
                       | Server                  ; Section 14.38
                       | Vary                    ; Section 14.44
                       | WWW-Authenticate        ; Section 14.47

   Response-header field names can be extended reliably only in
   combination with a change in the protocol version. However, new or
   experimental header fields MAY be given the semantics of response-
   header fields if all parties in the communication recognize them to
   be response-header fields. Unrecognized header fields are treated as
   entity-header fields.
```

익숙한(?) 것이 하나 보인다. 요청에 대한 응답 코드에 대한 명세다. 404 혹은 500은 웹을 돌아다니다보면 다들 가끔씩 만나본 적이 있을 것이다.

거칠게 말하면 200번대는 성공, 400번대는 클라측 오류, 500번대는 서버측 오류로 보면 된다.
```
Status-Line               
*(( general-header       :
| response-header        :
| entity-header ) CRLF)  :
CRLF
[ message-body ]         :
```


* 참고: 프론트엔드/백엔드 관련해서 HTTP 혹은 SQL 관련해서 깊은 질문들이 나올 확률이 높다. 프레임워크에 대한 깊은 질문보다도...

## HTTP 분석도구
요새 가장 많이 쓰이는 건 fiddler라고 알고 있는데 강의에서는 찰스로 진행됐다. 피들러나 찰스 둘 다 유료인 것 같다. 무료 사용기간을 제공한다. 오픈소스를 선호한다면 wireshark.

https://www.charlesproxy.com/ 
> Charles is an HTTP proxy / HTTP monitor / Reverse Proxy that enables a developer to view all of the HTTP and SSL / HTTPS traffic between their machine and the Internet. This includes requests, responses and the HTTP headers (which contain the cookies and caching information).

charles같은 프록시 서버들은 아래 사진같은 방식으로 동작한다.

![](https://www.bounteous.com/sites/default/files/luna-migrate/charles-0-howitworks-550x309.png)

찰스를 설치하면 이 프로그램이 운영체제 설정을 바꾼다.
이걸 확인하는 방법은,
- MacOS
  - 설정에서 네트워크 들어가서, 연결된 이더넷/와이파이에서 세부사항을 보면,
  - 프록시에 웹 프록시가 활성화되어있으며 서버 127.0.0.1로 잡혀있고 포트가 (기본값 8888) 잡혀 있는 것으로 확인할 수 있다.
  - 프로그램이 자동으로 잡긴 하는데 권한 문제 등으로 자동으로 못잡으면 수동으로 추가해주면 된다.
- Windows
  - 마찬가지로 자동으로 잡히고,  설정  > 네트워크 및 인터넷  > 프록시 에서 확인될 것이다.
  - 포트 충돌 등으로 자동으로 못잡은 경우 수동으로 프록시 서버를 추가해줄 수 있다.
  - 네트워크 & 인터넷 > 프록시 > Manual Proxy Setting (영문 OS라 한글메뉴명을 모르겠다) 에서 직접 편집해주면 된다.
  - 127.0.0.1 추가하고 포트번호는 charles 등 프로그램에서 설정된 포트번호로 잡아주면 된다.
  
1. 프록시 서버를 만들어서 웹브라우저와 우베서버 사이에 주고받는 데이터를 감시한다.
2. 서버에서 받은 컨텐츠를 캐싱한다.



참고로 많은 기업에서 프록시서버가 일정기간의 데이터를 캐싱한다.

### 캐싱을 하는 이유
1. 트래픽 줄이기
   - 여러 클라이언트가 동일한 큰 파일을 다운받는다고 하면, 프록시 서버가 캐시해 둔 것을 제공해 트래픽을 줄인다.
     - 매번 기가바이트 단위의 IDE, SDK를 각각의 클라이언트가 인터넷을 통해 받는다면 얼마나 느려질까...
     - HTTP로 어떤 요청을 했는지를 저장하고, 동일한 요청이 들어왔을 경우
     - 인터넷에서 해당 응답이 변경되었을 수도 있기 때문에, HEAD 요청으로 가볍게 확인한다.
     - 확인해보니 같은 경우 캐시에 저장된 걸 클라이언트에게 줌으로서 트래픽을 줄인다.
2. 메일 주고받은 것, 검색하거나 다운로드 받은 것들이 저장되어 필요한 경우 증거로 활용된다.

## GET 요청과 POST 요청
### 기본 개념
HTTP 메서드 중 GET 요청은 ?이름=값&이름&값&... 형태로 URL 뒤에 쿼리스트링을 물음표 찍고 붙인다. POST 요청은 메세지 바디가 따로 있어서, URL에 데이터를 따로 표시하지 않는다.

참고로 메세지 바디는 엔티티라고도 부른다. message body is called entity.
POST요청을 날리는 `<form action="s2" method="post">` 을 만들어서 POST 요청을 날려보면, 이런 식의 요청이 발생한다.

```
POST /ex04/s2 HTTP/1.1
Host: 192.168.0.28:8888
Content-Length: 32
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://192.168.0.28:8888
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.0.28:8888/ex04/test02.html
Accept-Encoding: gzip, deflate
Accept-Language: ko,en-US;q=0.9,en;q=0.8,ko-KR;q=0.7,ja;q=0.6
Connection: keep-alive

name=AB%EA%B0%80%EA%B0%81&age=40
```

보내는 데이터 타입에 따라 이 헤더가 달라진다. `Content-Type: application/x-www-form-urlencoded` csv, json, image, text 등등등...

요약하면, 
1. GET 요청은 Request-URI에 Query String으로 값을 붙여서 보낸다.
2. POST 요청은 message-body에 데이터를 실어서 보낸다.

근데 이 차이로 인해 한글이 꺠질 수 있으니...

### 한글 깨짐 (또?)
브라우저가 GET 요청을 만들 때, utf-8으로 변환하고 퍼센트인코딩을 한다. 그렇게 한 다음 요청을 보내면, 웹서버가 getParameter() 할 때 utf-8에서 utf-16 be 로 변환된다. 이건 문제가 없다.

브라우저가 POST 요청을 만들어서 보내는 경우는 동일하다. 근데 문제는 서버측에서 POST 요청을 getParameter() 메서드에서 ISO-8859-1 이라고 간주한다. 그래서 한글이 깨진다. (해당 문자집합이 유럽어+영어를 위한 것)

request에서 들어오는 문자열이 어떤 문자집합인지를 미리 알려주면 해결가능하다. 

`req.setCharacterEncoding("UTF-8");` 

getParameter()를 호출하기 전에 setCharacterEncoding()을 해야 한다. 한번 getParameter()하고 나면 캐릭터셋을 변경하는 것이 불가능하다.

### 한글 깨짐 결론
- res는 setCharacterEncoding() 필수, utf-8로 바꿔야 PrintWriter 정상 사용 가능
- req는 GET은 상관 없는데 POST 요청을 해석하려면 setCharacterEncoding() 필수

### 차이 요약 (아주 중요함)
1. 데이터 전송: 
   1. GET 은 URL에 쿼리스트링으로 넣음.
   2. POST는 message-body에 넣음.
2. Binary 데이터 전송: 
   1. GET은 BASE64 같이 문자로 변환하지 않으면 불가능. 문자 변환 해도 URL에 넣을 수 있는 글자수가 많지 않음. 2048 바이트 정도라고 보면 됨.
   2. POST는 message-body에 이진데이터 넣어서 보낼 수 있음
      1. mutlpart/form-data 포맷으로 인코딩하여 가능
3. 사이즈 리밋
   1. RFC에서는 제한하지 않더라도 웹서버에서 제한을 둔다.
      1. GET은 웹서버에 설정된 크기로 전송사이즈가 제한된다. 보통 64KB 정도.
      2. POST 사이즈는 서버에서는 제한이 없고 서블릿에서 필요하면 제한한다.
4. 보안
   1. GET은 URL에서 데이터가 노출된다. 브라우저는 사용했던 URL들을 캐싱해둔다.
5. 용도
   1. GET: URL에 데이터를 포함해야 하는 경우 (게시글 조회 등) 라면 GET이 맞다. 그래야 URL을 공유하면서 필요한 데이터도 같이 공유할 수 있으니까. 
   2. POST: 대량의 데이터를 전송해야 하거나, 데이터를 노출하지 말아야 하는(암호 등) 경우라면 POST요청을 해야 한다.


# 바이너리 보내보기
POST 방식으로 보내자. 보낼 때 Content-Type이 중요하다.
```
<form action="s3" enctype="multipart/form-data" method="post">
```

