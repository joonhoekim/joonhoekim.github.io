---
title: (Day	77) 페이지 컨트롤러 자동생성 - 프론트 컨트롤러를 개선하기 위한 애너테이션의 사용
author: 김준회
date: 2024-02-29 17:00:00 +0900
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

# Review
어제는 PageController 인터페이스를 삭제하고, RequestMapping을 해주는 애너테이션을 추가했다. 이에 따라 DispatcherServlet 클래스와 페이지 컨트롤러들을 수정했다. 핵심은 프론트 컨트롤러에서 페이지 컨트롤러를 다룰 때 `인터페이스를 사용`하는 방법에서 `애너테이션을 사용`하는 방법으로 변화했다는 것이다. 이렇게 되면 페이지 컨트롤러를 개발할 때는 좀 더 쉬워지고, 자유로워진다. 반대로 DispatcherServlet를 개발하는 것은 불편해지고 어려워진다. 사실 이 방향으로 변화하는 것은 꽤나 자연스러운 일이다. DispatcherServlet과 같은 스프링 컨테이너에 가까운 측은 도구처럼 사용자들(페이지 컨트롤러 개발자)을 편하게 해주는 쪽으로 진화하는 것이 자연스럽기 때문이다.
```java
package bitcamp.myapp.servlet;

import bitcamp.myapp.controller.AssignmentController;
import bitcamp.myapp.controller.AuthController;
import bitcamp.myapp.controller.BoardController;
import bitcamp.myapp.controller.HomeController;
import bitcamp.myapp.controller.MemberController;
import bitcamp.myapp.controller.RequestMapping;
import bitcamp.myapp.controller.RequestParam;
import bitcamp.myapp.dao.AssignmentDao;
import bitcamp.myapp.dao.AttachedFileDao;
import bitcamp.myapp.dao.BoardDao;
import bitcamp.myapp.dao.MemberDao;
import bitcamp.util.TransactionManager;
import java.io.IOException;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.lang.reflect.Parameter;
import java.sql.Date;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@MultipartConfig(maxFileSize = 1024 * 1024 * 10)
@WebServlet("/app/*")
public class DispatcherServlet extends HttpServlet {

  private Map<String, RequestHandler> requestHandlerMap = new HashMap<>();
  private List<Object> controllers = new ArrayList<>();


  @Override
  public void init() throws ServletException {
    ServletContext ctx = this.getServletContext();
    TransactionManager txManager = (TransactionManager) ctx.getAttribute("txManager");
    BoardDao boardDao = (BoardDao) ctx.getAttribute("boardDao");
    MemberDao memberDao = (MemberDao) ctx.getAttribute("memberDao");
    AssignmentDao assignmentDao = (AssignmentDao) ctx.getAttribute("assignmentDao");
    AttachedFileDao attachedFileDao = (AttachedFileDao) ctx.getAttribute("attachedFileDao");

    controllers.add(new HomeController());
    controllers.add(new AssignmentController(assignmentDao));
    controllers.add(new AuthController(memberDao));

    String boardUploadDir = this.getServletContext().getRealPath("/upload/board");
    controllers.add(new BoardController(txManager, boardDao, attachedFileDao, boardUploadDir));

    String memberUploadDir = this.getServletContext().getRealPath("/upload");
    controllers.add(new MemberController(memberDao, memberUploadDir));

    prepareRequestHandlers(controllers);
  }

  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {

    try {
      // URL 요청을 처리할 request handler를 찾는다.
      RequestHandler requestHandler = requestHandlerMap.get(request.getPathInfo());
      if (requestHandler == null) {
        throw new Exception(request.getPathInfo() + " 요청 페이지를 찾을 수 없습니다.");
      }

      Object[] args = prepareRequestHandlerArguments(requestHandler.handler, request, response);

      String viewUrl = (String) requestHandler.handler.invoke(requestHandler.controller, args);
      // 페이지 컨트롤러가 알려준 JSP로 포워딩 한다.
      if (viewUrl.startsWith("redirect:")) {
        response.sendRedirect(viewUrl.substring(9));
      } else {
        request.getRequestDispatcher(viewUrl).forward(request, response);
      }

    } catch (Exception e) {
      // 페이지 컨트롤러에서 오류가 발생했으면 오류페이지로 포워딩한다.
      request.setAttribute("message", request.getPathInfo() + " 실행 오류!");

      StringWriter stringWriter = new StringWriter();
      PrintWriter printWriter = new PrintWriter(stringWriter);
      e.printStackTrace(printWriter);
      request.setAttribute("detail", stringWriter.toString());

      request.getRequestDispatcher("/error.jsp").forward(request, response);
    }
  }

  private void prepareRequestHandlers(List<Object> controllers) {
    for (Object controller : controllers) {
      Method[] methods = controller.getClass().getDeclaredMethods();
      for (Method m : methods) {
        RequestMapping requestMapping = m.getAnnotation(RequestMapping.class);
        if (requestMapping != null) {
          requestHandlerMap.put(requestMapping.value(), new RequestHandler(controller, m));
        }
      }
    }
  }

  private Object[] prepareRequestHandlerArguments(
      Method handler,
      HttpServletRequest request,
      HttpServletResponse response) throws Exception {
    // 요청 핸들러의 파라미터 정보를 알아낸다.
    Parameter[] methodParams = handler.getParameters();

    // 파라미터에 전달할 값을 담을 배열을 준비한다.
    Object[] args = new Object[methodParams.length];

    // 파라미터 확인해 각 파라미터에 맞는 값을 배열에 담는다.
    for (int i = 0; i < args.length; i++) {
      Parameter methodParam = methodParams[i];
      if (methodParam.getType() == HttpServletRequest.class
          || methodParam.getType() == ServletRequest.class) {
        args[i] = request;
      } else if (methodParam.getType() == HttpServletResponse.class
          || methodParam.getType() == ServletResponse.class) {
        args[i] = response;
      } else { //이런 작업은 백 컨트롤러 말고 프론트 컨트롤러가 전부 처리하도록 하자
        System.out.printf("%s() have parameter > %s\n", handler.getName(), methodParam.getName());
        RequestParam requestParam = methodParam.getAnnotation(RequestParam.class);
        if (requestParam != null) {
          String requestParameterName = requestParam.value();
          String requestParameterValue = request.getParameter(requestParameterName);
          args[i] = valueOf(requestParameterValue, methodParam.getType());
        } else {
          //파라미터 타입이 도메인 클래스일 경우 해당 클래스의 객체를 준비하기
          args[i] = createValueObject(methodParam.getType(), request);
        }
      }
    }
    return args;
  }

  private Object valueOf(String strValue, Class<?> type) {
    if (type == int.class) {
      return Integer.parseInt(strValue);
    } else if (type == byte.class) {
      return Byte.parseByte((strValue));
    } else if (type == short.class) {
      return Byte.parseByte((strValue));
    } else if (type == long.class) {
      return Long.parseLong((strValue));
    } else if (type == float.class) {
      return Float.parseFloat((strValue));
    } else if (type == double.class) {
      return Double.parseDouble((strValue));
    } else if (type == boolean.class) {
      return Boolean.parseBoolean(strValue);
    } else if (type == char.class) {
      return strValue.charAt(0);
    } else if (type == Date.class) {
      return Date.valueOf(strValue);
    } else if (type == String.class) {
      return strValue;
    }
    return null;
  }

  //request handler의 파라미터 타입이 도메인 클래스일 때, 해당 클래스의 객체를 생성하고 요청파라미터 값을 담아서 리턴한다.
  private Object createValueObject(Class<?> type, HttpServletRequest request) throws Exception {
    //requestHandler가 원하는 값이 Domain 객체라면,
    // - 도메인 객체를 생성한 후 도메인 객체의 프로퍼티 이름과 일치하는 요청 파라미터 값을 담아서 주기

    // 1) 도메인 클래스의 생성자를 Reflection API 통해 알아내기
    Constructor constructor = type.getConstructor();
    // 2) 알아낸 생성자로 도메인 객체를 인스턴스로 생성
    Object obj = constructor.newInstance();
    // 3) 도메인 클래스의 메서드 목록을 가져오기
    Method[] methods = type.getClass().getDeclaredMethods();
    // 4) 메서드 중에서 세터 메서드를 알아내기
    for (Method setter : methods) {
      if (!setter.getName().startsWith("set")) {
        continue;
      }
      // 5) 세터 메서드의 이름에서 프로퍼티를 추출하기
      // ex) setName => name을 추출 (시작을 소문자로 바꿔주기)
      String propName =
          Character.toLowerCase(setter.getName().charAt(3)) + setter.getName().substring(4);

      // 6) 프로퍼티 이름으로 넘어온 요청 파라미터의 값을 꺼낸다.
      String requestParamValue = request.getParameter(propName);

      // 7) 요청 파라미터 값이 도메인 객체의 프로퍼티 이름과 일치하는 요청 파라미터 값이 있다면 객체에 저장한다.
      if (requestParamValue != null) {
        //세터 메서드의 파라미터 타입을 알아낸다.
        Class<?> setterParameterType = setter.getParameters()[0].getType();
        //세터를 호출한다.
        setter.invoke(obj, valueOf(requestParamValue, setterParameterType));
      }
    }
    return obj;
  }


}

``` 


## 컨트롤러의 서블릿 의존도 낮추기
서블릿에서만 사용하는 request / response 에 대한 의존도를 페이지 컨트롤러에서 낮춰보자.

```java
서블릿에서 코드를 아래처럼 바꾼다..
  @RequestMapping("/assignment/list")
  public String list(HttpServletRequest request) throws Exception {
    request.setAttribute("list", assignmentDao.findAll());
    return "/assignment/list.jsp";
  }
위 코드를 아래로 바꾼다.
  @RequestMapping("/assignment/list")
  public String list(Map<String,Object> map) throws Exception { //의존도 낮추기
    map.put("list", assignmentDao.findAll());
    return "/assignment/list.jsp";
  }

서블릿 디스패처 클래스에서는 이를 처리해줘야 한다.
  Map<String, Object> map = new HashMap<>();
  Object[] args = prepareRequestHandlerArguments(requestHandler.handler, request, response, map);
  ...
  for (Entry<String, Object> entry : map.entrySet()) {
    request.setAttribute(entry.getKey(), entry.getValue());
  }

```

스프링에서는 이 맵의 역할을 모델이 한다. list.jsp 등 리턴하는 뷰에서 이런 모델의 정보를 알 수 있는 이유는 페이지 컨트롤러가 해시맵 등의 객체에 값을 담는 행위를 하고, 그 객체에서 값을 꺼내서 쓰기 때문이다.

## 페이지 컨트롤러의 자동생성
디스패처 서블릿에서 @Component 애너테이션이 붙은 클래스를 찾아 객체를 자동생성하도록 한다.

아래처럼 컨트롤러를 추가하지 않아도 되도록 하자는 것이다.

```java
controllers.add(new HomeController());
controllers.add(new AssignmentController(assignmentDao));
controllers.add(new AuthController(memberDao));
controllers.add(new BoardController(txManager, boardDao, attachedFileDao));
controllers.add(new MemberController(memberDao));
```

그럼 어떻게 컨트롤러 클래스가 등록될까? 마찬가지로 애너테이션을 이용한다.

```java
package bitcamp.util;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Component {

}
```

이것을 어떻게 이용할까? 페이지 컨트롤러로 등록할 클래스에 애너테이션을 붙인다.

그럼 프론트 컨트롤러인 DispatcherServlet 클래스가 이를 처리하여 controllers 객체에 자동으로 추가한다.

어떻게 자동으로 추가하나?.. classpath 하위 경로에 있는 `.class`로 끝나는 파일들의 경로들을 가져와서, java.lang.Class 클래스에서 지원하는 기능을 사용해서 만든다. 그 기능들은, 경로를 아는 클래스파일을 로드하고, 로드된 클래스들이 Component라는 애노테이션을 가진 경우에 생성자를 getConstructor()로 얻고, controllers 객체에 constructor.newInstance()로 인스턴스를 만들어서 넣는 것이다.

위에서 설명된 행동을 하는 코드는 이렇다.
```java
private void findComponents(File dir, String packageName) throws Exception {
      File[] files = dir.listFiles(
          (File file) -> file.isDirectory() || (file.isFile() && !file.getName().contains("$")
              && file.getName().endsWith(".class")));
      for (File file : files) {
        if (file.isFile()) {
          System.out.println(packageName + file.getName().replace(".class", ""));
          Class<?> clazz = Class.forName(packageName + file.getName().replace(".class", ""));
          Component compAnno = clazz.getAnnotation(Component.class);
          if (compAnno != null) {
            Constructor<?> constructor = clazz.getConstructor();
            controllers.add(constructor.newInstance());
            System.out.println(clazz.getName() + " 인스턴스가 생성되었습니다.");
          }
        } else {
          if (packageName.length() > 0) {
            packageName += ".";
          }
          findComponents(file, packageName + file.getName());
        }
      }
    }
```

스프링도 이런 식으로 동작한다. 훨씬 훨씬 훨씬 복잡하지만... 서블릿이 어떻게 동작하는지 알기 어렵게 할 정도로 편리하지만, 그 근간은 서블릿이다.