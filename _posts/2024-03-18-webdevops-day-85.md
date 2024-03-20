---
title: (Day	85) 스프링 프레임워크, PropertyEditorSupport
author: 김준회
date: 2024-03-18 17:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: data:image/png;base64,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
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---

[PDF](https://github.com/eomjinyoung/bitcamp-study/blob/main/docs/%EC%8A%A4%ED%94%84%EB%A7%81%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC1.pdf)

# Controller
## FrontController에게 파라미터에 값 받기
```java
// 요청 핸들러의 아규먼트 - 프론트 컨트롤러로부터 받을 수 있는 파라미터 값
package bitcamp.app1;

import java.io.PrintWriter;
import java.util.Map;
import javax.servlet.ServletContext;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller //컨트롤러로 객체 생성됨
@RequestMapping("/c04_1") //베이스주소
public class Controller04_1 {

  // 프론트 컨트롤러(DispatcherServlet)로부터 받고 싶은 값이 있다면
  // 요청 핸들러를 정의할 때 받고 싶은 타입의 파라미터를 선언하라!
  // 그러면 프론트 컨트롤러가 메서드를 호출할 때 해당 타입의 값을 넘겨줄 것이다.

  // ServletContext는 의존 객체로 주입 받아야 한다.
  // 요청 핸들러에서 파라미터로 받을 수 없다.
  @Autowired
  ServletContext sc;

  // 요청 핸들러에서 받을 수 있는 타입의 아규먼트를 선언해 보자!
  @GetMapping("h1")
  @ResponseBody
  public void handler1(
      // ServletContext sc,
      // ServletContext는 파라미터로 받을 수 없다. 예외 발생!
      // 의존 객체로 주입 받아야 한다.
      ServletRequest request, //HttpServletRequest나 ServletRequest나 같음
      ServletResponse response,
      HttpServletRequest request2,
      HttpServletResponse response2,
      HttpSession session,
      Map<String, Object> map, // JSP에 전달할 값을 담는 임시 보관소
      Model model, // Map과 같다. 둘 중 한 개만 받으면 된다. Model을 더 많이 쓴다. 제네릭보다 간단하기 작성이 간단하고 코드가 간결하여 읽기 쉬워서
      PrintWriter out // 클라이언트에게 콘텐트를 보낼 때 사용할 출력 스트림
  ) {

    out.printf("ServletContext: %b\n", sc != null);
    out.printf("ServletRequest: %b\n", request != null);
    out.printf("ServletResponse: %b\n", response != null);
    out.printf("HttpServletRequest: %b\n", request2 != null);
    out.printf("HttpServletResponse: %b\n", response2 != null);
    out.printf("HttpSession: %b\n", session != null);
    out.printf("Map: %b\n", map != null);
    out.printf("Model: %b\n", model != null);
    out.printf("ServletRequest == HttpServletRequest : %b\n", request == request2);
    out.printf("ServletResponse == HttpServletResponse : %b\n", response == response2);
  }
}
```

## @RequestParam 사용법
1. 생략도 가능하다. 실무는 거의 생략해서 쓴다. 생략하면 동일한 이름의 변수를 프론트컨트롤러가 가지고 있을 때 받아온다.
2. 애너테이션 생략하는 경우, 없으면 그냥 넘어간다. 생략하지 않고 @RequestParam을 명시한 경우, 해당하는 값을 프론트 컨트롤러에게 받지 못했을 때 예외를 발생시킨다.
3. defaultValue 를 지정하여 없을 때 기본값을 넣고 예외를 띄우지 않게 할 수도 있다.
```java
// 요청 핸들러의 아규먼트 - @RequestParam
package bitcamp.app1;

import java.io.PrintWriter;
import javax.servlet.ServletRequest;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller //with @Controller, make Page Controller instance. MVC Container store those instances.
@RequestMapping("/c04_2") //mapping base location with @RequestMapping.
public class Controller04_2 {

  // 클라이언트가 보낸 파라미터 값을 바로 받을 수 있다.

  // => 요청 핸들러의 파라미터로 선언하면 된다.
  //    단 파라미터 앞에 @RequestParam 애노테이션을 붙인다.
  //    그리고 클라이언트가 보낸 파라미터 이름을 지정한다.
  // 테스트:
  // => http://localhost:9999/eomcs-spring-webmvc/app1/c04_2/h1?name=kim
  @GetMapping("h1") //set sub location. /dispatcherServletBaseLocation/PageControllerBase/Sub/..
  @ResponseBody
  public void handler1(
      PrintWriter out,
      ServletRequest request,
      @RequestParam(value = "name") String name1,
      @RequestParam(name = "name") String name2, // value와 name은 같은 일을 한다.
      @RequestParam("name") String name3, // value 이름을 생략할 수 있다.
      /* @RequestParam("name") */ String name // 요청 파라미터 이름과 메서드 파라미터(아규먼트)의 이름이 같다면
      // 애노테이션을 생략해도 된다.
  ) {

    out.printf("name=%s\n", request.getParameter("name"));
    out.printf("name=%s\n", name1);
    out.printf("name=%s\n", name2);
    out.printf("name=%s\n", name3);
    out.printf("name=%s\n", name);
  }

  // 테스트:
  // http://.../app1/c04_2/h2?name1=kim&name2=park
  @GetMapping("h2")
  @ResponseBody
  public void handler2(
      PrintWriter out,

      @RequestParam("name1") String name1, // 애노테이션을 붙이면 필수 항목으로 간주한다.
      // 따라서 파라미터 값이 없으면 예외가 발생한다.

      String name2, // 애노테이션을 붙이지 않으면 선택 항목으로 간주한다.
      // 따라서 파라미터 값이 없으면 null을 받는다.

      @RequestParam(value = "name3", required = false) String name3,
      // required 프로퍼티를 false로 설정하면 선택 항목으로 간주한다.

      @RequestParam(value = "name4", defaultValue = "ohora") String name4
      // 기본 값을 지정하면 파라미터 값이 없어도 된다.
  ) {

    out.printf("name1=%s\n", name1);
    out.printf("name2=%s\n", name2);
    out.printf("name3=%s\n", name3);
    out.printf("name4=%s\n", name4);
  }
}
```

## defaultValue에는 문자열을 넣자
프론트 컨트롤러가 받으려는 파라미터 타입에 따라 값을 변환해서 넣어준다. 단 프론트 컨트롤러에서 타입 변환이 불가능한 경우는 예외가 발생한다.

1. "true" "false" 는 대소문자 구분없이 true, false boolean 값으로 변환해준다.
2. "1", "0"이 아닌 1, 0은 각각 true, false boolean 값으로 변환해준다. 그외의 문자열이 아닌 숫자는 예외가 발생한다.
3. 그러니 문자열만 쓰는 게 적당하다. 특히, 400?BadRequest는 정확히 무엇이 무엇인지 바로 알아채기 어렵다.

객체에 값 넣을 때, OGNL 표기법을 사용할 수 있다. (Object-Graph Navigation Language)
```java
// 요청 핸들러의 아규먼트 - 도메인 객체(값 객체; Value Object)로 요청 파라미터 값 받기
package bitcamp.app1;

import java.io.PrintWriter;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c04_3")
public class Controller04_3 {

  // 클라이언트가 보낸 요청 파라미터 값을 값 객체에 받을 수 있다.

  // => 요청 핸들러의 아규먼트가 값 객체라면,
  //    프론트 컨트롤러는 메서드를 호출할 때 값 객체의 인스턴스를 생성한 후
  //    요청 파라미터와 일치하는 프로퍼티에 대해 값을 저장한다.
  //    그리고 호출할 때 넘겨준다.
  //
  // 테스트:
  // => http://.../c04_3/h1?model=sonata&maker=hyundai&capacity=5&auto=true&engine.model=ok&engine.cc=1980&engine.valve=16
  @GetMapping("h1")
  @ResponseBody
  public void handler1(
      PrintWriter out,

      String model,

      String maker,

      @RequestParam(defaultValue = "100") int capacity, // 프론트 컨트롤러가 String 값을 int로 변환해 준다.
      // 단 변환할 수 없을 경우 예외가 발생한다.

      boolean auto,
      // 프론트 컨트롤러가 String 값을 boolean으로 변환해 준다.
      // 단 변환할 수 없을 경우 예외가 발생한다.
      // "true", "false"는 대소문자 구분없이 true, false로 변환해 준다.
      // 1 ==> true, 0 ==> false 로 변환해 준다. 그 외 숫자는 예외 발생!

      Car car
      // 아규먼트가 값 객체이면 요청 파라미터 중에서 값 객체의 프로퍼티 이름과 일치하는
      // 항목에 대해 값을 넣어준다.
      // 값 객체 안에 또 값 객체가 있을 때는 OGNL 방식으로 요청 파라미터 값을
      // 지정하면 된다.
      // 예) ...&engine.model=ok&engine.cc=1980&engine.valve=16
      ) {

    out.printf("model=%s\n", model);
    out.printf("maker=%s\n", maker);
    out.printf("capacity=%s\n", capacity);
    out.printf("auto=%s\n", auto);
    out.printf("car=%s\n", car);
  }

}
```

## 날짜를 요청 파라미터로 받을 때
그냥은 못받는다. java.util.Date 타입의 요청 파라미터를 받고 싶을 때, 프론트 컨트롤러가 자동변환해주지 않는다.

페이지 컨트롤러의 request handler를 호출할 때, 요청 파라미터의 값을 request handler의 아규먼트로 바꿀때마다 호출되는 메서드가 있다. 이 메서드에서 파라미터 값을 형변환시키는 도구를 설정해주면, java.util.Date와 같은 기본 전략이 존재하지 않는 데이터타입에 대해서도 요청 파라미터를 자동 변환해줄 수 있다.

그럼 그 요청 파라미터의 값을 request handler의 아규먼트로 바꿀떄마다 호출된다는 메서드는 뭔가? `@InitBinder` 애너테이션이 붙은 메서드다.

문자열을 날짜로 바꿔주는 방법은 PropertyEditorClass 클래스를 상속받아 직접 변환기를 만들어서 사용하는 방법이 편하다.

```java
  @InitBinder
  public void ok(WebDataBinder 데이터변환등록기) {
    System.out.println("데이터변환등록기 소환");
    PropertyEditor 파라미터값변환기 = new DatePropertyEditor();
    데이터변환등록기.registerCustomEditor(java.sql.Date.class, //String 값을 어떤 타입으로 바꿀 것인지 지정한다.
        파라미터값변환기 //String 값을 해당 타입으로 변환해줄 변환기를 지정한다.
    );
  }
```

참고로 이러한 데이터 변환 등록을 위한 인스턴스는 쓰레드세이프해야 한다. 그런데 사용자가 많다면, 그냥 클라이언트별로 인스턴스를 만들어주는게 맞다. 공유 하지 말자.

다른 PropertyEditor 사용의 예시를 보면, 쿼리스트링으로 받은 값을 객체에 한번에 넣도록 하는 변환방법을 등록할 수 있다.
```java
package bitcamp.app1;

import java.beans.PropertyEditorSupport;
import java.sql.Date;

public class CarPropertyEditor extends PropertyEditorSupport {

  @Override
  public void setAsText(String text) throws IllegalArgumentException {
    String[] values = text.split(","); //model, maker, capacity, auto, createdDate
    Car car = new Car();
    car.setModel(values[0]);
    car.setMaker(values[1]);
    car.setCapacity(Integer.parseInt(values[2]));
    car.setAuto(Boolean.parseBoolean(values[3]));
    car.setCreatedDate(Date.valueOf(values[4]));
    this.setValue(car);
  }
}
```


```java
// 요청 핸들러의 아규먼트 - 프로퍼티 에디터 사용하기
package bitcamp.app1;

import java.beans.PropertyEditorSupport;
import java.io.PrintWriter;
import java.util.Date;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c04_4")
public class Controller04_4 {

  // 클라이언트가 보낸 요청 파라미터 값(String 타입)을
  // request handler의 아규먼트 타입(String, int, boolean 등)의 값으로 바꿀 때
  // primitive type에 대해서만 자동으로 변환해 준다.
  // 그 외의 타입에 대해서는 프로퍼티 에디터(타입 변환기)가 없으면 예외를 발생시킨다.

  // 테스트:
  // http://.../c04_4/h1?model=sonata&capacity=5&auto=true&createdDate=2019-4-19
  @GetMapping("h1")
  @ResponseBody
  public void handler1(
      PrintWriter out,
      String model,
      @RequestParam(defaultValue = "5") int capacity, // String ===> int : Integer.parseInt(String)
      boolean auto, // String ===> boolean : Boolean.parseBoolean(String)
      Date createdDate // 프로퍼티 에디터를 설정하지 않으면 변환 오류 발생
      ) {

    out.printf("model=%s\n", model);
    out.printf("capacity=%s\n", capacity);
    out.printf("auto=%s\n", auto);
    out.printf("createdDate=%s\n", createdDate);
  }

  // 테스트:
  // http://.../c04_4/h2?car=sonata,5,true,2019-4-19
  @GetMapping("h2")
  @ResponseBody
  public void handler2(PrintWriter out,
      // 콤마(,)로 구분된 문자열을 Car 객체로 변환하기?
      // => String ===> Car 프로퍼티 에디터를 등록하면 된다.
      @RequestParam("car") Car car) {

    out.println(car);
  }

  // 테스트:
  // http://.../c04_4/h3?engine=bitengine,3500,16
  @GetMapping("h3")
  @ResponseBody
  public void handler3(PrintWriter out,
      // 콤마(,)로 구분된 문자열을 Engine 객체로 변환하기?
      // => String ===> Engine 프로퍼티 에디터를 등록하면 된다.
      @RequestParam("engine") Engine engine) {

    out.println(engine);
  }



  // 이 페이지 컨트롤러에서 사용할 프로퍼티 에디터 설정하는 방법
  // => 프론트 컨트롤러는 request handler를 호출하기 전에
  //    그 메서드가 원하는 아규먼트 값을 준비해야 한다.
  //    각 아규먼트 값을 준비할 때
  //    @InitBinder가 표시된 메서드(request handler를 실행할 때 사용할 도구를 준비하는 메서드)
  //    를 호출하여 프로퍼티 에디터(변환기)를 준비시킨다.
  //    그리고 이 준비된 값 변환기(프로퍼티 에디터)를 이용하여 파라미터 값을
  //    request handler의 아규먼트가 원하는 타입의 값을 바꾼다.
  //    request handler의 아규먼트 개수 만큼 이 메서드를 호출한다.
  // => 따라서 프로퍼티 에디터를 적용하기에
  //    @InitBinder가 표시된 메서드가 적절한 지점이다.
  //    즉 이 메서드에 프로퍼티 에디터를 등록하는 코드를 둔다.
  //

  @InitBinder
  // => 메서드 이름은 마음대로.
  // => 작업하는데 필요한 값이 있다면 파라미터로 선언하라.
  public void initBinder(WebDataBinder binder) {
    System.out.println("Controller04_4.initBinder()...");
    // 프로퍼티 에디터를 등록하려면 그 일을 수행할 객체(WebDataBinder)가 필요하다.
    // request handler 처럼 아규먼트를 선언하여
    // 프론트 컨트롤러에게 달라고 요청하라.

    //String ===> java.util.Date 프로퍼티 에디터 준비
    DatePropertyEditor propEditor = new DatePropertyEditor();

    // WebDataBinder에 프로퍼티 에디터 등록하기
    binder.registerCustomEditor(
        java.util.Date.class, // String을 Date 타입으로 바꾸는 에디터임을 지정한다.
        propEditor // 바꿔주는 일을 하는 프로퍼티 에디터를 등록한다.
        );


    // WebDataBinder에 프로퍼티 에디터 등록하기
    binder.registerCustomEditor(
        Car.class, // String을 Car 타입으로 바꾸는 에디터임을 지정한다.
        new CarPropertyEditor() // 바꿔주는 일을 하는 프로퍼티 에디터를 등록한다.
        );

    // WebDataBinder에 프로퍼티 에디터 등록하기
    binder.registerCustomEditor(Engine.class, // String을 Engine 타입으로 바꾸는 에디터임을 지정한다.
        new EnginePropertyEditor() // 바꿔주는 일을 하는 프로퍼티 에디터를 등록한다.
        );
  }

  // PropertyEditor 만들기
  // => 문자열을 특정 타입의 프로퍼터의 값으로 변환시킬 때 사용하는 에디터이다.
  // => java.beans.PropertyEditor 인터페이스를 구현해야 한다.
  // => PropertyEditor를 직접 구현하면 너무 많은 메서드를 오버라이딩 해야 하기 때문에
  //    자바에서는 도우미 클래스인 PropertyEditorSupport 클래스를 제공한다.
  //    이 클래스는 PropertyEditor를 미리 구현하였다.
  //    따라서 이 클래스를 상속 받은 것 더 낫다.
  class DatePropertyEditor extends PropertyEditorSupport {

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      System.out.println("DatePropertyEditor.setAsText()");
      // 프로퍼티 에디터를 사용하는 측(예: 프론트 컨트롤러)에서
      // 문자열을 Date 객체로 바꾸기 위해 이 메서드를 호출할 것이다.
      // 그러면 이 메서드에서 문자열을 프로퍼티가 원하는 타입으로 변환한 후 저장하면 된다.
      try {

        // 1) String ==> java.util.Date
        // SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        // Date date = format.parse(text); // String ===> java.util.Date
        // setValue(date); // 내부에 저장

        // 2) String ==> java.sql.Date
        setValue(java.sql.Date.valueOf(text));

      } catch (Exception e) {
        throw new IllegalArgumentException(e);
      }
    }

    @Override
    public Object getValue() {
      System.out.println("DatePropertyEditor.getValue()");
      // 이 메서드는 프로퍼티 에디터를 사용하는 측(예: 프론트 컨트롤러)에서
      // 변환된 값을 꺼낼 때 호출된다.
      // 이 메서드를 오버라이딩 하는 이유는 이 메서드가 호출된 것을
      // 확인하기 위함이다. 원래는 오버라이딩 해야 할 이유가 없다.
      return super.getValue();
    }
  }

  // String ===> Car 프로퍼티 에디터 만들기
  class CarPropertyEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      String[] values = text.split(",");

      Car car = new Car();
      car.setModel(values[0]);
      car.setCapacity(Integer.parseInt(values[1]));
      car.setAuto(Boolean.parseBoolean(values[2]));
      car.setCreatedDate(java.sql.Date.valueOf(values[3]));

      setValue(car);
    }
  }

  class EnginePropertyEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      String[] values = text.split(",");

      Engine engine = new Engine();
      engine.setModel(values[0]);
      engine.setCc(Integer.parseInt(values[1]));
      engine.setValve(Integer.parseInt(values[2]));

      setValue(engine);
    }
  }
}
```

# 모든 컨트롤러에 적용되는 @InitBinder를 설정할 수 있다.
그 방법은, @ControllerAdvice 라는 애너테이션으로 공통적으로 호출할 클래스를 등록하는 것이다.
```java
package bitcamp.app1;

import java.beans.PropertyEditorSupport;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.InitBinder;

 @ControllerAdvice
// => 이름에 이미 역할에 대한 정보가 담겨있다.
// => 페이지 컨트롤러를 실행할 때 충고하는 역할을 수행한다.
//    즉 프론트 컨트롤러가 페이지 컨트롤러의 request handler를 호출하기 전에
//    이 애노테이션이 붙은 클래스를 참고하여 적절한 메서드를 호출한다.
//
//@ControllerAdvice
public class GlobalControllerAdvice {

  // 이 클래스에 프로퍼티 에디터를 등록하는 @InitBinder 메서드를 정의한다.
  @InitBinder
  public void initBinder(WebDataBinder binder) {

    DatePropertyEditor propEditor = new DatePropertyEditor();
    binder.registerCustomEditor(java.util.Date.class, propEditor);

    binder.registerCustomEditor(Car.class, new CarPropertyEditor());

    binder.registerCustomEditor(Engine.class, new EnginePropertyEditor());
  }

  class DatePropertyEditor extends PropertyEditorSupport {

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      try {
        // String ==> java.util.Date
        // SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        // Date date = format.parse(text);
        // setValue(date);

        // String ==> java.sql.Date
        setValue(java.sql.Date.valueOf(text));

      } catch (Exception e) {
        throw new IllegalArgumentException(e);
      }
    }
  }

  class CarPropertyEditor extends PropertyEditorSupport {

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      String[] values = text.split(",");

      Car car = new Car();
      car.setModel(values[0]);
      car.setCapacity(Integer.parseInt(values[1]));
      car.setAuto(Boolean.parseBoolean(values[2]));
      car.setCreatedDate(java.sql.Date.valueOf(values[3]));

      setValue(car);
    }
  }

  class EnginePropertyEditor extends PropertyEditorSupport {

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
      String[] values = text.split(",");

      Engine engine = new Engine();
      engine.setModel(values[0]);
      engine.setCc(Integer.parseInt(values[1]));
      engine.setValve(Integer.parseInt(values[1]));

      setValue(engine);
    }
  }
}
```
이렇게 등록해두면 된다.

```java
// 요청 핸들러의 아규먼트 - 글로벌 프로퍼티 에디터 적용하기
package bitcamp.app1;

import java.io.PrintWriter;
import java.util.Date;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller 
@RequestMapping("/c04_5")
public class Controller04_5 {

  // 다른 페이지 컨트롤러에서 등록한 프로퍼티 에디터는 사용할 수 없다.
  // 각 페이지 컨트롤러 마다 자신이 사용할 프로퍼티 에디터를 등록해야 한다.
  // 따라서 만약 여러 페이지 컨트롤러에서 공통으로 사용하는 프로퍼티 에디터라면 
  // 글로벌 프로퍼티 에디터로 등록하는 것이 편하다.
  
  // 글로벌 프로퍼티 에디터 등록하기
  // @ControllerAdvice 애노테이션이 붙은 클래스에서 
  // @InitBinder 메서드를 정의하면 된다.
  // => GlobalControllerAdvice 클래스를 참고하라!
  
  // 테스트:
  //    http://.../c04_5/h1?model=sonata&capacity=5&auto=true&createdDate=2019-4-19
  @GetMapping("h1") 
  @ResponseBody 
  public void handler1(
      PrintWriter out,
      String model,
      int capacity, 
      boolean auto, 
      Date createdDate
      ) {
    
    out.printf("model=%s\n", model);
    out.printf("capacity=%s\n", capacity);
    out.printf("auto=%s\n", auto);
    out.printf("createdDate=%s\n", createdDate);
  }
  
  //테스트:
  //    http://.../c04_5/h2?car=sonata,5,true,2019-4-19
  @GetMapping("h2") 
  @ResponseBody 
  public void handler2(
      PrintWriter out,
      @RequestParam("car") Car car) {
    
    out.println(car);
  }
  
  //테스트:
  //    http://.../c04_5/h3?engine=bitengine,3500,16
  @GetMapping("h3") 
  @ResponseBody 
  public void handler3(
      PrintWriter out,
      @RequestParam("engine") Engine engine) {
    
    out.println(engine);
  }

}
```

## 요청 헤더를 받을 수 있다.
브라우저를 구별하는 일 등을 할 수 있다.
```java
// 요청 핸들러의 아규먼트 - @RequestHeader
package bitcamp.app1;

import java.io.PrintWriter;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c04_6")
public class Controller04_6 {

  // 클라이언트의 HTTP 요청 헤더를 받고 싶으면
  // request handler의 아규먼트 앞에 @RequestHeader(헤더명) 애노테이션을 붙여라!

  // 테스트:
  // http://.../c04_6/h1
  @GetMapping("h1")
  @ResponseBody
  public void handler1(
      PrintWriter out,
      @RequestHeader("Accept") String accept,
      @RequestHeader("User-Agent") String userAgent) {

    out.printf("Accept=%s\n", accept);
    out.printf("User-Agent=%s\n", userAgent);

    if (userAgent.matches(".*Edg.*")) {
      out.println("Edge");
    } else if (userAgent.matches(".*Chrome.*")) {
      out.println("chrome");
    } else if (userAgent.matches(".*Safari.*")) {
      out.println("safari");
    } else if (userAgent.matches(".*Firefox.*")) {
      out.println("firefox");
    } else {
      out.println("etc");
    }
  }

  public static void main(String[] args) {
    String str =
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36";
    // String str = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/605.1.15 (KHTML,
    // like Gecko) Version/12.1 Safari/605.1.15";
    // String str = "AA BB Aa Ba $12,000";

    // 정규 표현식으로 패턴을 정의한다.
    // String regex = "Chrome";
    // String regex = "Chrome.*Safari";
    String regex = "[^(Chrome.*)]Safari";
    Pattern pattern = Pattern.compile(regex);

    // 주어진 문자열에서 패턴과 일치하는 정보를 찾는다.
    Matcher matcher = pattern.matcher(str);

    // 일치 여부를 확인한다.
    if (matcher.find()) {
      System.out.println("OK!");
      // for (int i = 1; i < matcher.groupCount(); i++) {
      // System.out.println(matcher.group(1));
      // }
    } else {
      System.out.println("NO!");
    }

  }

}
```

참고로 UserAgent라는 메타데이터는 상당히 더럽다. 왜 그럴까? 재밌게 정리한 문서가 있다. https://wormwlrm.github.io/2021/10/11/Why-User-Agent-string-is-so-complex.html 워낙 재미있는(?) 이야기라서 정리한 문서가 엄청 많다.

## 쿠키를 활용하기 & Charset 문제 (한글깨짐) 해결하기

```java
// 요청 핸들러의 아규먼트 - @Cookie
package bitcamp.app1;

import java.io.PrintWriter;
import java.net.URLEncoder;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.CookieValue;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c04_7")
public class Controller04_7 {

  // 클라이언트가 보낸 쿠키 꺼내기
  // => @CookieValue(쿠키명) 애노테이션을 request handler의 아규먼트 앞에 붙인다.

  // 테스트:
  // http://.../c04_7/h1
  @GetMapping("h1")
  @ResponseBody
  public void handler1(
      PrintWriter out,
      HttpServletResponse response
      ) {
    // 이 메서드에서 쿠키를 클라이언트로 보낸다.
    try {
      // 쿠키의 값이 ASCII가 아니라면 URL 인코딩 해야만 데이터가 깨지지 않는다.
      // URL 인코딩을 하지 않으면 ? 문자로 변환된다.
      response.addCookie(new Cookie("name1", "AB가각"));
      response.addCookie(new Cookie("name2", URLEncoder.encode("AB가각", "UTF-8")));
      response.addCookie(new Cookie("name3", "HongKildong"));
      response.addCookie(new Cookie("age", "30"));

    } catch (Exception e) {
      e.printStackTrace();
    }

    out.println("send cookie!");
  }

  // 테스트:
  // http://.../c04_7/h2
  @GetMapping(value = "h2", produces = "text/plain;charset=UTF-8")
  @ResponseBody
  public String handler2(
      @CookieValue(value = "name1", required = false) String name1,
      @CookieValue(value = "name2", defaultValue = "") String name2,
      @CookieValue(value = "name3", defaultValue = "홍길동") String name3,
      @CookieValue(value = "age", defaultValue = "0") int age // String ===> int 자동 변환
      ) throws Exception {

    //
    // 1) URLEncoder.encode("AB가각", "UTF-8")
    // ==> JVM 문자열은 UTF-16 바이트 배열이다.
    //     0041 0042 ac00 ac01
    // ==> UTF-8 바이트로 변환한다.
    //     41 42 ea b0 80 ea b0 81
    // ==> 8비트 데이터가 짤리지 않도록 URL 인코딩으로 7비트화 시킨다.
    //     "AB%EA%B0%80%EA%B0%81"
    //     41 42 25 45 41 25 42 30 25 38 30 25 45 41 25 42 30 25 38 31
    // ==> 웹 브라우저에서는 받은 값을 그대로 저장
    //
    // 2) 쿠키를 다시 서버로 보내기
    // ==> 웹 브라우저는 저장된 값을 그대로 전송
    //     "AB%EA%B0%80%EA%B0%81"
    //     41 42 25 45 41 25 42 30 25 38 30 25 45 41 25 42 30 25 38 31
    // ==> 프론트 컨트롤러가 쿠키 값을 꺼낼 때 자동으로 URL 디코딩을 수행한다.
    //     즉 7비트 문자화된 코드를 값을 원래의 8비트 코드로 복원한다.
    //     41 42 ea b0 80 ea b0 81
    // ==> 디코딩 하여 나온 바이트 배열을 UTF-16으로 만든다.
    //     문제는 바이트 배열을 ISO-8859-1로 간주한다는 것이다.
    //     그래서 UTF-16으로 만들 때 무조건 앞에 00 1바이트를 붙인다.
    //     0041 0042 00ea 00b0 0080 00ea 00b0 0081
    //     그래서 한글이 깨진 것이다.
    //
    // 해결책:
    // => UTF-16을 ISO-8859-1 바이트 배열로 변경한다.
    //    41 42 ea b0 80 ea b0 81
    byte[] originBytes = name2.getBytes("ISO-8859-1");

    // => 다시 바이트 배열을 UTF-16으로 바꾼다.
    //    이때 바이트 배열이 UTF-8로 인코딩된 값임을 알려줘야 한다.
    //    0041 0042 ac00 ac01
    String namex = new String(originBytes, "UTF-8");

    return String.format(//
        "name1=%s\n name2=%s\n name2=%s\n name3=%s\n age=%d\n", //
        name1, name2, namex, name3, age);
  }


}
```

## multipart 처리하기
* multipart는 javax.servlet.http.Part 클래스를 기반으로 한다.
* Part 클래스를 사용하지 않고, spring 의 multipart를 사용할 수도 있다.
사용법은 동일하다. (org.springframework.web.multipart.MultipartFile)1

```java
// 요청 핸들러의 아규먼트 - multipart/form-data 형식의 파라미터 값 받기
package bitcamp.app1;

import java.io.File;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.util.UUID;
import javax.servlet.ServletContext;
import javax.servlet.http.Part;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller
@RequestMapping("/c04_8")
public class Controller04_8 {

  // ServletContext는 메서드의 아규먼트로 받을 수 없다.
  // 의존 객체로 주입 받아야 한다.
  @Autowired
  ServletContext sc;

  // 클라이언트가 멀티파트 형식으로 전송한 데이터를 꺼내기
  // => Servlet API에서 제공하는 Part를 사용하거나
  //    또는 Spring에서 제공하는 MultipartFile 타입의 아규먼트를 선언하면 된다.
  //
  // 주의!
  // => DispatcherServlet을 web.xml을 통해 배치했다면,
  //    <multipart-config/> 태그를 추가해야 한다.
  // => WebApplicationInitializer를 통해 DispatcherServlet을 배치했다면,
  //    App1WebApplicationInitializer 클래스를 참고하라!
  //

  // 테스트:
  // http://.../html/app1/c04_8.html
  @PostMapping(value = "h1", produces = "text/html;charset=UTF-8")
  @ResponseBody
  public String handler1(
      String name,
      int age,
      Part photo // Servlet API의 객체
  ) throws Exception {

    String filename = null;
    if (photo != null && photo.getSize() > 0) {
      filename = UUID.randomUUID().toString();
      // String path = sc.getRealPath("/html/app1/" + filename);
      String path = sc.getRealPath("/upload/" + filename);
      photo.write(path);
    }

    return "<html><head><title>c04_8/h1</title></head><body>" + "<h1>업로드 결과</h1>" + "<p>이름:" + name
        + "</p>" + "<p>나이:" + age + "</p>" +
        // 현재 URL이 다음과 같기 때문에 업로드 이미지의 URL을 이 경로를 기준으로 계산해야 한다.
        // http://localhost:8080/java-spring-webmvc/app1/c04_8/h1
        //
//        (filename != null ? "<p><img src='../../html/app1/" + filename + "'></p>" : "")
        (filename != null ? "<p><img src='../../upload/" + filename + "'></p>" : "")
        + "</body></html>";
  }

  // MultipartFile로 멀티파트 데이터를 받으려면,
  // Spring WebMVC 설정에서 MultipartResolver 객체를 등록해야 한다.
  //
  // 테스트:
  // http://.../html/app1/c04_8.html
  @PostMapping(value = "h2", produces = "text/html;charset=UTF-8")
  @ResponseBody
  public String handler2(//
      String name, //
      @RequestParam(defaultValue = "0") int age, //
      MultipartFile photo // Spring API의 객체
  ) throws Exception {

    String filename = null;
    if (!photo.isEmpty()) {
      filename = UUID.randomUUID().toString();
      String path = sc.getRealPath("/html/app1/" + filename);
      photo.transferTo(new File(path));
    }

    return "<html><head><title>c04_8/h2</title></head><body>" + "<h1>업로드 결과</h1>" + "<p>이름:" + name
        + "</p>" + "<p>나이:" + age + "</p>" +
        // 현재 URL이 다음과 같기 때문에 업로드 이미지의 URL을 이 경로를 기준으로 계산해야 한다.
        // http://localhost:8080/java-spring-webmvc/app1/c04_8/h2
        //
        (filename != null ? "<p><img src='../../html/app1/" + filename + "'></p>" : "")
        + "</body></html>";
  }

  // 테스트:
  // http://.../html/app1/c04_8.html
  @PostMapping(value = "h3", produces = "text/html;charset=UTF-8")
  @ResponseBody
  public String handler3(//
      String name, //
      int age, //
      // 같은 이름으로 전송된 여러 개의 파일은 배열로 받으면 된다.
      MultipartFile[] photo //
  ) throws Exception {

    StringWriter out0 = new StringWriter();
    PrintWriter out = new PrintWriter(out0);
    out.println("<html><head><title>c04_8/h3</title></head><body>");
    out.println("<h1>업로드 결과</h1>");
    out.printf("<p>이름:%s</p>\n", name);
    out.printf("<p>나이:%s</p>\n", age);

    for (MultipartFile f : photo) {
      if (!f.isEmpty()) {
        String filename = UUID.randomUUID().toString();
        String path = sc.getRealPath("/html/app1/" + filename);
        f.transferTo(new File(path));
        out.printf("<p><img src='../../html/app1/%s'></p>\n", filename);
      }
    }
    out.println("</body></html>");

    return out0.toString();
  }

}
```

## 한글 깨짐 관련해서 두번째
응답 헤더에 charset 문제일 확률이 아주아주 높다.
아래 컨트롤러 중 handler1에 대해 요청을 postman으로 보내서 확인해보면...
그러면 ISO-8859-1 이라는 문자집합으로 응답을 받았다는 것을 알 수 있고 (헤더), 그렇게 받은 값은 `abc???`으로 보인다.

그리고, 브라우저에서 요청할 때랑 Postman - Body - none 으로 보내는 것과 응답 헤더가 다르다. 브라우저에 응답할 때는 컨텐츠 타입을 text/html 로 주고, Postman에서는 text/plain으로 준다. **요청 헤더의 Accept가 다르기 때문이다.**

이것의 해결 방법은 여러가지가 있다. 애너테이션에 produces 이름과 값을 줘서 Content-Type을 명시할 수 있다. HttpEntity에 HttpHeaders를 만들어서 Content-Type을 설정하고 headers 객체를 함께 넘기는 방법도 있다. 애너테이션을 활용하는 방법으로 통일하여 진행되는 것이 가장 권장된다.


첨언: HttpEntity 클래스를 리턴하는 방식으로 값을 담을수도 있지만 일관성을 해치게 되므로 추천되지 않는다.


```java
// 요청 핸들러의 리턴 값 - 콘텐트를 직접 리턴하기
package bitcamp.app1;

import javax.servlet.http.HttpServletResponse;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/c05_1")
public class Controller05_1 {

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h1
  @GetMapping("h1")
  @ResponseBody
  public String handler1() {
    // 리턴 값이 클라이언트에게 보내는 콘텐트라면
    // 메서드 선언부에 @ResponseBody를 붙인다.
    // => 붙이지 않으면 프론트 컨트롤러는 view URL로 인식한다.
    // => 출력 콘텐트는 브라우저에서 기본으로 HTML로 간주한다.
    //    단 한글은 ISO-8859-1 문자표에 정의된 코드가 아니기 때문에
    //    클라이언트로 보낼 때 '?' 문자로 바꿔 보낸다.
    return "<html><body><h1>abc가각간</h1></body></html>";
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h2
  // => 리턴되는 콘텐트의 MIME 타입과 charset을 지정하고 싶다면
  //    애노테이션의 produces 프로퍼티에 설정하라.
  @GetMapping(value = "h2", produces = "text/html;charset=UTF-8")
  @ResponseBody
  public String handler2() {
    return "<html><body><h1>abc가각간<h1></body></html>";
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h3
  @GetMapping("h3")
  @ResponseBody
  public String handler3(HttpServletResponse response) {

    // HttpServletResponse에 대해 다음과 같이 콘텐트 타입을 설정해봐야 소용없다.
    response.setContentType("text/html;charset=UTF-8");

    return "<html><body><h1>abc가각간<h1></body></html>";
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h4
  @GetMapping("h4")
  public HttpEntity<String> handler4(HttpServletResponse response) {
    // HttpEntity 객체에 콘텐트를 담아 리턴할 수 있다.
    // 이 경우에는 리턴 타입으로 콘텐트임을 알 수 있기 때문에
    // @ResponseBody 애노테이션을 붙이지 않아도 된다.

    HttpEntity<String> entity = new HttpEntity<>(
        "<html><body><h1>abc가각간<h1></body></html>");

    // 이 경우에는 출력할 때 ISO-8859-1 문자표의 코드로 변환하여 출력한다.
    // 그래서 한글은 ? 문자로 변환된다.

    return entity;
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h5
  @GetMapping(value = "h5", produces = "text/html;charset=UTF-8")
  public HttpEntity<String> handler5(HttpServletResponse response) {
    // 한글을 제대로 출력하고 싶으면 위 애노테이션의 produces 속성에 콘텐트 타입을 지정한다.
    HttpEntity<String> entity = new HttpEntity<>(
        "<html><body><h1>abc가각간<h1></body></html>");

    return entity;
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h6
  @GetMapping("h6")
  public HttpEntity<String> handler6(HttpServletResponse response) {
    // 한글을 제대로 출력하고 싶으면,
    // 응답 헤더에 직접 Content-Type을 설정할 수 있다.

    HttpHeaders headers = new HttpHeaders();
    headers.add("Content-Type", "text/html;charset=UTF-8");

    HttpEntity<String> entity = new HttpEntity<>(
        "<html><body><h1>abc가각간<h1></body></html>",
        headers);

    return entity;
  }

  // 테스트:
  // http://localhost:9999/eomcs-spring-webmvc/app1/c05_1/h7
  @GetMapping("h7")
  public ResponseEntity<String> handler7(HttpServletResponse response) {
    // HttpEntity 대신에 ResponseEntity 객체를 리턴 할 수 있다.
    // 이 클래스의 경우 응답 상태 코드를 추가하기 편하다.

    HttpHeaders headers = new HttpHeaders();
    headers.add("Content-Type", "text/html;charset=UTF-8");

    // 이렇게 응답 헤더를 따로 설정하는 방법이 존재하는 이유는
    // 다음과 같이 임의의 응답 헤더를 추가하는 경우가 있기 때문이다.
    headers.add("BIT-OK", "ohora");

    ResponseEntity<String> entity = new ResponseEntity<>(
        "<html><body><h1>abc가각간<h1></body></html>",
        headers,
        HttpStatus.OK // 응답 상태 코드를 설정할 수 있다.
        );

    return entity;
  }
}
```

## 접근제한
jsp는 접근하지 못하게 WEB-INF 경로 아래로 두고, 정적 자원들(실행X)은 WEB-INF 밖에 두어 직접 접근 가능하게 할 것.