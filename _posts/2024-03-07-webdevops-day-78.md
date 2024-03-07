---
title: (Day	78) 미니 스프링 프레임워크
author: 김준회
date: 2024-03-07 17:00:00 +0900
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


아.. 오늘 입실 입력을 까먹어서 지각처리가 됐다 😭😭😭
요새는 복습위주로 많이 하면서, 코드트리는 계속 꾸준히 하는 중.

오늘 목표는 제네릭 복습하기, 코드트리 100XP 이상, 서블릿 동작구조 복습, 프로젝트 프로토타이핑 진행이다.
> 완료~ 코드트리에서 어려움 문제 (시뮬레이션2 레이저)가 어려웠다. 막상 풀어보니, 문제가 복잡해도 하나하나의 구현이 심하게 어려운 건 아니었다. 다만 구현해야 하는 부분이 많고, 거기서 실수가 나지 않게 하는 게 어려웠다. 능숙함의 차이인 것 같다. 
> 
> 요즘 느끼는건데, 8시 30분부터 23시까지 원하는 공부를 하는 거 자체는 어려운 게 아닌 것 같다. 집중을 놓지 않는 게 어렵고, 효율적으로 쓰는 게 어렵다.

강사님 조언.

장기적인 목표는
* myapp 1~99번 까지 다시 해보기
* 스터디 혼자 하지 말고 여럿이서 하기

과정 끝나고 절대 지켜야 하는 목표는
* 집에 있지 않기 (ㅋㅋ)
  * 집->기다림->게임->후달림->게임도 노잼, 정신도 피폐
* 공유오피스 가장 추천. 같이 할 사람 모으길 바람


# Mini Spring Framework
미니 스프링 프레임워크는
* 프론트 컨트롤러 디자인패턴 적용 (DispatcherServlet)
* 애너테이션(Component, RequestMapping, RequestParam)을 통한 자유도 확보
* Listener (ContextLoaderListener) 
  * 필요한 객체 생성 후 HashMap에 저장하고 ServletContext을 통해 공유
* 페이지 컨트롤러 자동 생성(DispatcherServlet)
  * 생성자에서 요구하는 Arguments 를 



# ContextLoaderListener 개선하기
Map 객체에 필요한 객체를 세팅해서 넣고, 자동으로 생성자에서 필요한 객체를 가져가도록 해보자.

이렇게 하면 좋은 점은? 각각의 컴포넌트에서 필요한 객체들을 각각 주는 코드를 중복하지 않아도 된다.

방식은 이렇다.. 리스너에서 객체를 세팅해서 ServletContext에 넣어둔다.
```java
    // 공유 객체를 보관할 맵 객체 준비
    Map<String, Object> beanMap = new HashMap<>();
    beanMap.put("connectionPool", connectionPool);
    beanMap.put("assignmentDao", new AssignmentDaoImpl(connectionPool));
    beanMap.put("memberDao", new MemberDaoImpl(connectionPool));
    beanMap.put("boardDao", new BoardDaoImpl(connectionPool));
    beanMap.put("attachedFileDao", new AttachedFileDaoImpl(connectionPool));
    beanMap.put("txManager", new TransactionManager(connectionPool));

    // 서블릿에서 사용할 수 있도록 웹애플리케이션 저장소에 보관한다.
    ServletContext 웹애플리케이션저장소 = sce.getServletContext();
    웹애플리케이션저장소.setAttribute("beanMap", beanMap);
```

DispatcherServlet
```java
  ...
  private Map<String, Object> beanMap;

  ... init() {
    ...
    리스너에서 서블릿 컨텍스트에 넣어둔 맵 객체를 가져온다.
        beanMap = (Map<String, Object>) this.getServletContext().getAttribute("beanMap");
    ...
  }

  ...
    ㉠: 컴포넌트를 가져오고
    private void findComponents(File dir, String packageName) throws Exception {
    File[] files = dir.listFiles(file ->
        file.isDirectory() || (file.isFile()
            && !file.getName().contains("$")
            && file.getName().endsWith(".class")));

    if (packageName.length() > 0) {
      packageName += ".";
    }
    for (File file : files) {
      if (file.isFile()) {
        Class<?> clazz = Class.forName(packageName + file.getName().replace(".class", ""));
        Component compAnno = clazz.getAnnotation(Component.class);
        if (compAnno != null) {
          Constructor<?> constructor = clazz.getConstructors()[0];
          Parameter[] params = constructor.getParameters();
          Object[] args = getArguments(params);
          controllers.add(constructor.newInstance(args));
          System.out.println(clazz.getName() + " 객체 생성!");
        }
      } else {
        findComponents(file, packageName + file.getName());
      }
    }
  }
  ㉡: 생성자를 가져오고 요구하는 아규먼트들을 findBean() 메서드에게 찾게 한다.
  private Object[] getArguments(Parameter[] params) {
    Object[] args = new Object[params.length];
    for (int i = 0; i < params.length; i++) {
      args[i] = findBean(params[i].getType());
    }
    return args;
  }

  ㉡: 맵에 해당 아규먼트가 요구하는 타입으로 만들어진 객체가 있다면 찾아준다.
  private Object findBean(Class<?> type) {
    Collection<Object> objs = beanMap.values();
    for (Object obj : objs) {
      if (type.isInstance(obj)) {
        //System.out.printf("%s ==> %s\n", type.getName(), obj.getClass().getName());
        return obj;
      }
    }
    return null;
  }
```

스프링 컨테이너도 이런 방식으로 동작한다. (물론 훨씬 복잡하다.) 결국 웹 개발자들은 DispatcherServlet, ContextLoaderListener 등 서블릿을 다루는 핵심 클래스들에 대해서는 사용방법들만 알아도 개발이 가능해진다. 그래서 스프링 프레임워크의 사용법과, 페이지 컨트롤러를 작성하는 방법만 배우면 피상적인 배움이 된다... 프레임워크 개발이 저렴하기 힘든 이유가 따로 있는 것이 아니네..



# (미니) IoC 컨테이너 만들어보기 (myapp 73)
## IoC 가 뭔데?
1. Inversion Of Control의 약자다.
2. 제어의 역전으로 번역될 수 있는데, 이것은 아래와 같은 의미들을 담는다.
  * Dependency Injection (의존성 주입)
    * 필요한 객체들을 그 객체가 아닌 다른 객체가 주입해준다.
    * 위에서 작성한, BeanMap 안에 들어있는 객체들을 주입해주는 것이 예시다.
  * 어떤 사건이 발생했을 때 (이벤트), Control(제어)하는 주체가 역전된다.
  
DI Container = IoC Container

### ChatGPT에게 물어봤다
```
IoC(제어의 역전) 컨테이너는 소프트웨어 개발에서 사용되는 디자인 패턴 중 하나입니다. 
이 패턴은 프로그램의 흐름 제어 방식을 변경하여 프로그램의 결합도를 낮추고 유연성을 향상시킵니다.

일반적으로 IoC 컨테이너는 객체의 생명주기를 관리하고 객체 간의 의존성을 해결합니다. 
프로그래머는 객체를 직접 생성하고 관리하는 대신 IoC 컨테이너에 객체 생성 및 의존성 주입을 위임합니다. 
이러한 방식으로 IoC 컨테이너는 객체 간의 결합도를 줄이고 테스트 용이성을 개선합니다.

Spring 프레임워크와 같은 많은 프레임워크와 라이브러리는 IoC 컨테이너를 제공합니다. 
이러한 프레임워크에서는 XML 또는 어노테이션과 같은 방법을 사용하여 객체의 구성을 정의하고 IoC 컨테이너에 의해 관리됩니다. 
이는 개발자가 비즈니스 로직에 집중할 수 있도록 하며 객체 생성 및 관리에 대한 부분적인 책임을 프레임워크에게 위임함으로써 코드의 유지보수성을 향상시킵니다.
```

### 왜 그러시는 거에요??
직접 객체를 만드는 코드가 나중에 엄청 복잡해지기 때문에 그렇다.
애너테이션이 붙은 객체들은 IoC 컨테이너에서 자동으로 생성하고, 관리하도록 하고 싶은 것이다.

**그렇지 않으면, 의존성 객체를 추가하거나 삭제했을 때마다 listener 혹은 servlet에서 의존객체를 가져오는 코드를 개발자가 작성해야 할 것이다.**

```java
//이제 이것은 IoC 컨테이너가 해준다.
      preparePageControllers();
      prepareRequestHandlers(controllers);

      ↓

      prepareRequestHandlers(applicationContext.getBeans());

```

IoC Container
```java
package bitcamp.context;

import bitcamp.util.Component;
import java.io.File;
import java.lang.reflect.Constructor;
import java.lang.reflect.Parameter;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

public class ApplicationContext {

  private ApplicationContext parent;
  private String basePackage;
  private Map<String, Object> beans = new HashMap<>();

  public ApplicationContext(Map<String, Object> beanMap, String basePackage) throws Exception {
    beans.putAll(beanMap);
    this.basePackage = basePackage;
    findComponents(new File("./build/classes/java/main"), "");
  }

  public ApplicationContext(ApplicationContext parent, String basePackage) throws Exception {
    this.parent = parent;
    this.basePackage = basePackage;
    findComponents(new File("./build/classes/java/main"), "");
  }

  private void findComponents(File dir, String packageName) throws Exception {

    File[] files = dir.listFiles(file ->
        file.isDirectory() || (file.isFile()
            && !file.getName().contains("$")
            && file.getName().endsWith(".class")));

    if (packageName.length() > 0) {
      packageName += ".";
    }
    for (File file : files) {
      if (packageName.startsWith(basePackage) && file.isFile()) {
        Class<?> clazz = Class.forName(packageName + file.getName().replace(".class", ""));
        Component compAnno = clazz.getAnnotation(Component.class);
        if (compAnno == null) {
          continue; //이렇게 하면 인덴트 복잡도가 줄어들어 가독성이 높아진다.
        }
        Constructor<?> constructor = clazz.getConstructors()[0];
        Parameter[] params = constructor.getParameters();
        Object[] args = getArguments(params);
        beans.put(
            compAnno.value().length() == 0 ? clazz.getName() : compAnno.value(),
            constructor.newInstance(args));
        System.out.println(clazz.getName() + " 객체 생성!");
      } else {
        findComponents(file, packageName + file.getName());
      }
    }

  }

  private Object[] getArguments(Parameter[] params) {
    Object[] args = new Object[params.length];
    for (int i = 0; i < params.length; i++) {
      args[i] = getBean(params[i].getType());
    }
    return args;
  }

  private Object getBean(Class<?> type) {
    //현재 IoC 컨테이너에서 찾는다.
    Collection<Object> objs = beans.values();
    for (Object obj : objs) {
      if (type.isInstance(obj)) {
        //System.out.printf("%s ==> %s\n", type.getName(), obj.getClass().getName());
        return obj;
      }
    }

    //현재 IoC 객체 안에 원하는 객체가 없으면 부모 IoC 컨테이너에서 찾는다. (recursive call)
    if (parent != null) {
      return parent.getBean(type);
    }

    //parent 없는 경우 null 반환
    return null;
  }

  public Collection<Object> getBeans() {
    return beans.values();
  }

}
```

이 변화를 클래스 다이어그램으로 보면 (PDF) 왜 편해지는 것을 볼 수 있다.

# Sprint IoC Container 도입
jakarta로 패키지명 변경되기 전인 5.3.32 버전을 적용했다.


```java

```

```xml

```

# package & namespace
패키지와 네임스페이스는 무엇인가? 자바는 클래스가 소속된 그룹을 패키지라고 부른다. **java 외 다른 프로그래밍 언어에서는 namespace라고 한다.** 즉 네임스페이스가 더 대세(?)인 용어이다. 그래서 아래에선 namespace라는 용어로 통일하여 기록한다.

### GPT의 설명
네임스페이스는 프로그래밍에서 중요한 개념 중 하나입니다. 그것은 변수, 함수, 클래스 등과 같은 요소들을 구분하고 식별하기 위한 방법을 제공합니다. 네임스페이스는 어떤 이름이 특정한 컨텍스트 안에서 유일하게 식별되도록 보장합니다.

간단하게 말하면, 네임스페이스는 이름의 충돌을 방지하고 코드를 구조화하는 데 도움이 되는 일종의 "이름 공간"입니다. 이를 통해 코드가 보다 모듈화되고 이해하기 쉬워집니다.

## 그래서?

### java
java에서는 패키지명을 명시하지 않아도 쓸 수 있게 해주는, 네임스페이스에 클래스를 추가하는 import를 해줄 수 있다. java에서는 `java.lang`이 기본네임스페이스이다. 기본네임스페이스에 등록된 클래스들이나 메서드들, 변수 등은 따로 패키지명을 명시하지 않아도 바로 사용할 수 있다. 

### xml
XML에서는 네임스페이스에 항목을 추가하는 것이 java처럼 그렇게 간단하진 않다.

```xml
...
<beans   xmlns="http://www.springframework.org/schema/beans" 
         ↑↑↑ 이렇게 기본네임스페이스를 설정할 수 있다.
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         ↑↑↑ xsi라고 이름을 붙인 추가 네임스페이스를 설정할 수 있다.
         ↑↑↑ 추가 네임스페이스는 이름을 반드시 붙여야 한다.
         xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd"
         ↑↑↑ 추가 네임스페이스의 설계 도면을 알려줘야 한다.
         ↑↑↑ xsd 는 xml schema documentation
>
```
## 어떻게 하는데?
일단 

## 오늘 한 것과 내일 할 일은?
오늘은..
* IoC Container로 객체를 자동생성하는 원리를 배웠다.
* DO(Dependency Object)를 자동으로 주입하는 원리를 배웠다.
* Spring IoC Container를 적용했다.

내일은..
* Spring web MVC를 적용한다.
* Spring의 DispatcherServlet으로 교체한다.
  * 작성해봤던 애너테이션들 제거하고 교체한다.
* Spring을 설정한다.
  * 문제는, Spring을 설정하는 방법이 너무 다양하다는 것이다.
  * 이전 구버전의 설정 방법부터 최신 버전의 설정 방법까지 다양한 방법들이 존재한다.
  * 그래서 하나의 문제더라도 여러 상황에 따른 여러 답안이 있을 수 있다.
  * 유지보수 입장에서 최신 버전에 대한 방법만 아는 건 위험하므로.. 다양하게 배운다.

컨트롤러와 DAO만 남는건데, MyBatis를 적용하면 MySQL DAO도 교체된다. JDBC에서 제공하는 DBConnectionPool을 적용하면 그 클래스도 교체된다. 결국.. DAO 구현체, 컨트롤러만 남게 된다.

잠시 정리하면..

프론트 컨트롤러 적용하면서 미니 스프링 프레임워크를 만들어봤던 과정은 스프링 프레임워크의 동작 원리를 이해하는데 많은 도움을 준다. 지금까지 배웠던 내용 중에서 아주 중요한 파트이다. 물론 진짜 스프링 프레임워크보다는 너무너무너무 * 100 간단하지만 아주 중요한 부분들이다. 

물론 원래 스프링보다 간단할 수밖에 없다. 2002년 로드 존슨의 저서에서 24년 지금까지 발전해 온, `Expert One-on-One J2EE Design and Development` 에서 지금껏 발전했으니 그럴 수 밖에..


참고로 스프링이 대세가 된 2006년여 전에는 아파치에서도 프레임워크를 제공했었고 많이 쓰였는데, 스프링이 다 먹었다. nodejs에서 여러 프레임워크가 있고, 파이썬에서는 장고 말고 다른 것도 있는데 java 세계는 사실상 Spring이 천상천하 유아독존이다.

이 교육과정에서 핵심적이라고 할 수 있는 프로그래밍 방법은 이제 거의 교육이 끝났다. 남은 부분은 미니 jQuery를 만들어보는 부분 정도이다. 나머지는 이제 핵심 프로그래밍이 아닌 Spring 사용 방법, 설정 방법 등이다.




# log4j
어떤 인스턴스가 생성되었는지, 어떤 메서드가 호출되었는지 이런 걸 자동으로 해준다. 생성자나 메서드에 `System.out.println(get.class()+" 가 호출됨")` 같은 코드들 작성할 필요가 없게 만들어준다. 이건 나중에 적용! 지금을 일단 필요한 경우 기본스트림으로 정보를 출력하도록 한다.