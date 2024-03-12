---
title: (Day	81) Spring IoC Container - 애너테이션을 사용한 설정
author: 김준회
date: 2024-03-12 17:00:00 +0900
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
어제는 IoC 컨테이너에 객체를 만들도록 설정하는 방법을 알아봤다. 정확히는 그 중에서 xml을 사용하는 방법에 대해서 알아봤다. (java-lang ex01~07) Spring IoC Container를 관통하는 핵심 시각화는 컨베이어 벨트와 작업자 객체였다.

![](data:image/jpeg;base64,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)

IoC 컨테이너에서 의존 객체들을 세팅하는데, 필요한 작업들을 하는 컨베이어 벨트의 작업자가 있다면 그 작업을 하고 (IoC 컨테이너에 해당 처리를 하는 객체가 있다면 처리를 하고) 최종 결과물로 객체가 만들어진다.

# @Component
xml이 아닌 애너테이션을 사용하여, IoC 컨테이너에게 객체를 생성하도록 알려줄 수 있다.

@Component
=> 스프링 IoC 컨테이너는 이 애노테이션이 붙은 클래스에 대해 객체를 자동 생성한다.

문법:  
* @Component(value="객체이름")  
* @Component("객체이름")  

만약 다음과 같이 객체의 이름을 생략하면 클래스 이름을 객체 이름으로 사용한다.

예) bitcamp.java106.step09.Car => "car"

즉 클래스 이름에서 첫 알파벳을 소문자로 한 이름을
객체 이름으로 사용한다.


## 의존성 주입의 방법
constructor를 이용하거나, setter를 이용하거나, field에 직접 주입하거나...

아주 잘 정리한 [글](https://www.linkedin.com/advice/1/how-do-you-choose-between-constructor-setter-field)이 있었다.

어떤 방법을 선택하는지는, 나중에 DI(dependency injection) 가능하도록 세터를 사용하여 유연성을 살릴 수도 있고, 의존하는만큼 생성시부터 반드시 필요할테니 생성자에 의존하는 방법도 좋다. 

> **Field Injection**
> 
> Field injection is the least recommended type of DI in Java. It involves using annotations, such as @Autowired or @Inject, to mark the fields that need to be injected with dependencies. This way, you can avoid writing constructors or setters for the dependencies, and let a framework or a container handle the injection for you. Field injection can make your code concise and simple, as you only need to declare the fields and annotate them.
> 
> However, field injection also has many drawbacks. It can make your class tightly coupled to the framework or the container that does the injection, making it harder to test and reuse. Field injection also makes your class opaque and brittle, as you cannot see or control what dependencies a class has and where they come from. Furthermore, field injection does not support final fields, optional or dynamic dependencies, or constructor validation, which can cause many problems and bugs.
>
> 필드 주입은 Java에서 가장 권장되지 않는 DI 유형입니다. 여기에는 종속성을 주입해야 하는 필드를 표시하기 위해 @Autowired 또는 @Inject와 같은 주석을 사용하는 작업이 포함됩니다. 이렇게 하면 종속성에 대한 생성자나 설정자를 작성하지 않고 프레임워크나 컨테이너가 주입을 처리하도록 할 수 있습니다. 필드 주입을 사용하면 필드를 선언하고 주석만 달면 되므로 코드를 간결하고 단순하게 만들 수 있습니다. 
> 
> 그러나 필드 주입에는 많은 단점도 있습니다. 클래스가 주입을 수행하는 프레임워크나 컨테이너와 긴밀하게 결합되어 테스트 및 재사용이 더 어려워질 수 있습니다. 또한 필드 주입은 클래스의 종속성이 무엇인지, 클래스의 출처를 확인하거나 제어할 수 없기 때문에 클래스를 불투명하고 불안정하게 만듭니다. 또한 필드 주입은 최종 필드, 선택적 또는 동적 종속성, 생성자 유효성 검사를 지원하지 않으므로 많은 문제와 버그가 발생할 수 있습니다.

필드 주입은 위와 같이 Spring 프레임워크에 강한 결합이 발생하기 때문에, 이상적으로 생각했을 때는 권장되지 않는 방법이기는 하다. 인스턴스 변수에 직접 의존 객체를 주입하는 건 외부에서 직접 인스턴스 변수에 접근하는 것을 막는 캡슐화를 깨기에 좋은 방법이 아니다. 객체지향을 파괴하는 방식이라는 비난을 받기도 한다. 단위테스트가 제대로 진행되지 않고, 유지보수보단 기능의 구현이 급한 경우에 많이 보이게 된다.

왜?
* java를 사용한 프로젝트들에 Spring이 아닌 다른 프레임워크를 쓸 일이 없다고 예상되기 때문에
* 짧고 간결하게 작성할 수 있기 때문에

## @Autowired
[동작원리🔥]   
1) 스프링 IoC 컨테이너는 객체를 만든다.  
2) 프로퍼티 값을 설정한다.  
3) 객체 생성 후 IoC 컨테이너에 등록된  
   리스너(BeanPostProcessor)에게 통보한다.  
4) AutowiredAnnotationBeanPostProcessor 리스너가 있다면,  
   @Autowired 애노테이션을 처리한다.  


의존 객체를 자동 주입하는 기능을 쓰고 싶어요!  
=> 그 일을 할 객체를 등록하세요.  
어떤 객체인가요?  
=> AutowiredAnnotationBeanPostProcessor 입니다.  
이 객체는 어떻게 사용하나요?  
=> 셋터 메서드 또는 필드에 @Autowired를 붙이면 됩니다.  
  
@Autowired 애노테이션을 셋터 메서드에 붙였다고 해서   
의존 객체가 자동 주입되는 것이 아니다.  
@Autowired 애노테이션이 붙은 셋터에 대해  
프로퍼티 값을 자동으로 주입하는 일을 할 객체를 등록해야 한다.  
  
@Autowired 애노테이션 도우미 등록방법:  
다음 클래스의 객체를 등록하면 된다.  
org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor  

```xml
    <bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>

    <!--  이제 이렇게 의존객체를 ref로 알려주지 않아도 된다.
    <property name="engine" ref="e1"/>
    AutowiredAnnotationBeanPostProcessor 가 컨베이어 벨트에 들어왔기 때문이다.
    -->
```
  
동작원리:  
=> Spring IoC 컨테이너가 설정 파일에 적혀있는 대로 객체를 생성한다.  
=> 객체 생성 후에 BeanPostProcessor에게 보고한다.  
=> AutowiredAnnotationBeanPostProcessor는 생성된 객체에 대해   
   @Autowired 애노테이션을 검사하여   
   이 애노테이션이 붙은 프로퍼티 값을 자동 주입하는 일을 한다.  
=> 이 객체를 스프링 IoC 컨테이너에 등록하지 않으면,  
   @Autowired 애노테이션은 처리되지 않는다.   
  
객체 생성 후 작업을 수행하는 역할자를 정의하는 방법:  
=> BeanPostProcessor 규칙에 따라 클래스를 정의한 후 객체를 등록하면 된다.   
  
BeanPostProcessor 인터페이스:  
=> 스프링 IoC 컨테이너는 객체 중에 이 인터페이스를 구현한 객체가 있다면,  
   설정 파일에 적혀있는 객체를 생성한 후에  
   이 구현체의 postProcess....() 메서드를 호출한다.   
=> 즉 빈 생성 이후의 마무리 작업을 진행시킨다.  
=> 그래서 이 인터페이스의 이름이   
   BeanPostProcessor(객체 생성 후 처리기) 인 것이다.  

# BeanPostProcessor
빈 전처리 인터페이스에는 두 메서드가 있다.
```java
	@Nullable
	default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}

    	@Nullable
	default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}
```

```
BeanPostProcessor 인터페이스 구현체 만들고 등록하기
BeanPostProcessor 인터페이스:
    => 스프링 IoC 컨테이너는 객체 중에 이 인터페이스를 구현한 객체가 있다면,
    설정 파일에 적혀있는 객체를 생성한 후에
    이 구현체의 postProcess....() 메서드를 호출한다. 
    => 즉 빈 생성 이후의 마무리 작업을 진행시킨다.
    => 그래서 이 인터페이스의 이름이 
    BeanPostProcessor(객체 생성 후 처리기) 인 것이다.

<bean class="com.eomcs.spring.ioc.ex08.c1.MyBeanPostProcessor"/>
<!-- 위치가 밑으로 내려와도 전처리 클래스가 항상 먼저 생성되도록 Spring Framework 는 섬세하다. 
        init-method="init" 라는 속성이 태그에 들어가 있음을 확인하자.
        해당 메서드가 호출되기 전, 호출이 끝난 이후에 PostProcessor의 메서드들이 실행된다.
        -->
```
@Autowired 애노테이션을 필드(인스턴스 변수)에 붙여도 된다.  
=> 그러면 의존 객체를 직접 변수에 주입한다.  
=> 셋터를 호출하지 않는다. 즉 셋터가 없어도 된다.  
=> 인스턴스 변수에 직접 의존 객체를 주입한다는 것은  
캡슐화(즉 외부에서 직접 인스턴스 변수에 접근하는 것을 막는 기법)를  
위배하는 측면이 있기 때문에  
이 방식은 "객체지향을 파괴하는 방식"이라는 비난을 받는다.  

```java
  // 필드에 @Autowired를 붙인 경우,
  // 셋터를 통해 값을 넣는 것이 아니라,
  // 인스턴스 필드에 직접 값을 넣는다.
  // private 이라도 상관없다.
  // 따라서 셋터를 정의하지 않아도 된다.
  @Autowired
  private Engine engine;
```

```java
// 의존 객체 주입 자동화하기 - private 필드에 값을 넣는 방법
package com.eomcs.spring.ioc.ex08.d;

import java.lang.reflect.Field;

public class Exam02 {

  public static void main(String[] args) throws Exception {
    // private 멤버는 직접 접근 불가!
    Car c2 = new Car();
    //    c2.model = "비트비트"; // private 멤버이기 때문에 접근 불가! 컴파일 오류!
    c2.setModel("비트비트"); // 그래서 public 멤버인 셋터를 이용해서 값을 넣는 것이다.
    System.out.println(c2);


    // 그런데 Reflection API 사용하면 private 멤버도 접근할 수 있다.
    // 정말?
    //
    Field f = Car.class.getDeclaredField("model");
    f.setAccessible(true); // private 멤버이지만 난 접근할래!!!
    f.set(c2, "오호라2");
    System.out.println(c2);

  }

}
```

**Reflection API는 프로그램이 자신의 구조를 조사하고 수정할 수 있는 강력하고 유연한 기능이지만 아래와 같은 문제들이 발생할 수 있다.**

성능 저하: Reflection은 일반적으로 정적으로 컴파일된 코드보다 실행 시간에 추가적인 오버헤드를 발생시킵니다. Reflection을 사용하여 객체를 동적으로 생성하거나 메서드를 호출하는 등의 작업은 정적인 코드보다 느릴 수 있습니다.

보안 취약점: Reflection을 사용하면 접근 지정자(접근 제한자)를 무시하고 private 필드 또는 메서드에 접근할 수 있습니다. 이는 보안 취약점을 초래할 수 있으며, 악의적인 공격에 취약해질 수 있습니다.

코드의 가독성 저하: Reflection을 사용하면 코드가 동적으로 작동하기 때문에 코드의 동작을 추론하기 어려울 수 있습니다. 특히 Reflection을 과도하게 사용하면 코드의 가독성이 저하될 수 있습니다.

컴파일 타임 에러 감지의 어려움: Reflection을 사용하면 컴파일 타임에 발생할 수 있는 오류를 런타임에야 확인할 수 있습니다. 이는 디버깅 및 오류 해결을 어렵게 만들 수 있습니다.

## 생성자로 의존객체 주입 [권장]
```
생성자를 통해 의존 객체 주입하기
    => @Autowired 나 @Resource를 사용할 필요가 없다.
    => 스프링 전문가들 사이에서는 이 방식을 권고하기도 한다.
        왜?
        생성자의 파라미터로 선언하면 해당 의존 객체가 필수 항목이 된다.
        즉 그 의존 객체없이 생성자를 호출할 수  없기 때문이다.
    => 파라미터를 받는 다른 생성자를 호출하여 의존 객체를 자동주입하려면 
        다음 객체를 등록해야 한다.
        AutowiredAnnotationBeanPostProcessor
```

## @autowired - required 값
@Autowired의 required 값은 기본이 true이다.  
=> 즉 의존객체 주입이 필수사항이다.  
   해당하는 의존 객체가 없으면 예외가 발생한다.  
=> 선택사항으로 바꾸고 싶으면 false로 설정하라!  
=> required를 false로 설정하면 해당 객체가 없더라도 오류가 발생하지 않는다.  

# 여러 개의 의존 객체가 있는 경우
여러 개의 의존 객체가 있는 경우, @Autowired 사용할 때 어떤 의존객체를 주입해야 하는지 알 수 없는 문제가 생긴다. 그래서 이 때 NoUniqueBeanDefinition 이라는 예외를 던진다.

```
Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: 

No qualifying bean of type 'com.eomcs.spring.ioc.ex08.g.Engine' 
available: expected single matching bean but found 2: e1,e2
```

이것을 해결하는 방법은 단 하나의 의존객체만 두거나, XML에서 어떤 id의 객체를 쓰는지 알려주거나, **@Qualifier("id")** 애너테이션을 통해서 어떤 id의 객체를 쓸 것인지 알려주는 것이다.

# 필요한 컨베이어 벨트 작업자를 자동으로 등록하기

이전에는 하나씩 필요한 IoC 컨테이너의 컨베이어 벨트에서 객체 생성을 위해 필요한 작업자들을 등록했다. 이렇게 **하나 하나** 말이다.

```xml
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>
````

```xml
<context:annotation-config/>
```

위 태그 하나면 애너테이션들을 다룰 객체들을 자동으로 등록할 수 있다. (모두 등록하는 것이 아니라 필요한 것만 IoC 컨테이너에 들어간다.) `<context:annotation-config/>` 태그라면, 애너테이션들을 미리 검사하고, 검사 결과에 따라 필요한 컨베이어 벨트의 작업자 객체들을 IoC Container에 등록한다. (Annotation들을 처리하는 객체들)

# 객체 자동생성하기
객체를 생성하기 위해 bean 태그를 사용하지 않고도 객체를 자동 생성할 수 있다. 그 방법은 클래스 선언에 @Component 애노테이션을 붙이는 것이다. 단 이 애노테이션을 처리할 객체를 등록해야 한다. (클래스에 @Component 애너테이션을 붙여줘야 한다.)

@Component  
=> 스프링 IoC 컨테이너는 이 애노테이션이 붙은 클래스에 대해 객체를 자동 생성한다.  
문법:  
     @Component(value="객체이름")  
     @Component("객체이름")  
만약 다음과 같이 객체의 이름을 생략하면  
클래스 이름을 객체 이름으로 사용한다.  
     예) bitcamp.java106.step09.Car => "car"  
즉 클래스 이름에서 첫 알파벳을 소문자로 한 이름을  
객체 이름으로 사용한다.  


다른 애너테이션들도 함께 알아보자.
## 클래스별 애너테이션 분류
```xml
<!-- 애노테이션을 처리할 도우미 객체를 등록한다. -->
    <context:annotation-config/>

<!-- 객체를 생성하기 위해 bean 태그를 사용하지 않고도
        객체를 자동 생성할 수 있다.
        방법은?
        클래스 선언에 @Component 애노테이션을 붙이는 것이다.
        단 이 애노테이션을 처리할 객체를 등록해야 한다.
-->
<!-- component-scan 태그는 지정된 패키지에서 
        @Component, @Service, @Repository, @Controller 애노테이션이 붙은 클래스를 찾아서 
        객체를 자동 생성하도록 명령을 내리는 태그이다.
    => base-package 속성 
        어느 패키지의 있는 클래스를 찾아서 등록할 것인지 지정하는 속성이다.
    => @Component : 일반 클래스에 대해 붙인다.
    => @Repository : DAO 역할을 수행하는 클래스에 대해 붙인다.
    => @Service : 비즈니스 로직을 수행하는 클래스에 대해 붙인다.
    => @Controller : MVC 구조에서 컨트롤러 역할을 하는 클래스에 대해 붙인다.
    => @RestController : MVC 구조에서 REST API 컨트롤러 역할을 하는 클래스에 대해 붙인다.
    이렇게 역할에 따라 애노테이션으로 클래스를 분류해두면 나중에 통제하기가 편하다. 
-->
    <context:component-scan base-package="com.eomcs.spring.ioc.ex09"/>
   

</beans>
```


```java
@Component
public class Car {
  String model;
  String maker;
  int cc;
  boolean auto;
  Date createdDate;
  Engine engine;

// 의존 객체는 생성자에서 주입하는 것이 좋다.
// 의존 객체라는 말에서 이미 그 객체없이는 작업을 수행할 수 없다는 의미이기 때문에
// 보통 필수 객체이다.
// 따라서 생성자에서 필수 객체를 받게 하는 것이 유지보수에 좋다.
// 즉 의존 객체없이 해당 객체를 생성하는 일을 방지할 수 있기 때문이다.
  public Car(Engine engine) {
    this.engine = engine;
  }

  @Override
  public String toString() {
    return "Car [model=" + model + ", maker=" + maker + ", cc=" + cc + ", auto=" + auto + ", createdDate="
        + createdDate + ", engine=" + engine + "]";
  }
  /*
   * 의존 객체는 작업하는데 사용하라고 생성자를 호출할 때 외부에서 주입하는 객체이기 때문에
   * 셋터나 겟터를 정의할 필요가 없다.
    public Engine getEngine() {
        return engine;
    }
    public void setEngine(Engine engine) {
        this.engine = engine;
    }
   */
  ```

## 특정 패키지의 특정 애너테이션을 대상으로 제외할 수도 있다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    
    <context:component-scan base-package="com.eomcs.spring.ioc.ex09">
        <!-- 다음 패키지의 클래스 중에서 @Component,@Service,@Controller,@Repository
             애노테이션이 붙은 것은 객체를 생성한다. -->
        <context:include-filter type="regex" 
            expression="com.eomcs.spring.ioc.ex09.p2.Service2"/>
        
        <!-- 특정 패키지의 특정 클래스를 객체 생성 대상에서 제외하기  -->
        <context:exclude-filter type="regex" 
            expression="com.eomcs.spring.ioc.ex09.p2.Service1"/>
        
        <!-- 특정 애노테이션이 붙은 클래스는 객체 생성에서 제외시킨다. -->
        <context:exclude-filter type="annotation" 
            expression="org.springframework.stereotype.Controller"/>
            
        <!-- 특정 패키지만 제외하기 -->
        <context:exclude-filter type="regex" 
            expression="com.eomcs.spring.ioc.ex09.p4.*"/>
            
        <!-- 특정 패키지에서 지정된 패턴의 이름을 가진 클래스를 제외하기 -->
        <context:exclude-filter type="regex" 
            expression="com.eomcs.spring.ioc.ex09.p5.*Truck"/>
    </context:component-scan>
    
</beans>
```

include, exclude를 regex(정규표현식)에 따라서 표현할 수가 있다... 강력하다.

## @Bean
XML은 거의 안 쓰고, 애너테이션 위주로 사용된다. 다만 애너테이션을 사용할 수 없을 때 수동으로 객체를 생성하는 방법이 있다.

```java
package com.eomcs.spring.ioc.ex10.a;

import org.springframework.context.annotation.Bean;
import com.eomcs.spring.ioc.ex10.Car;

public class AppConfig {

  // @Component와 같은 애노테이션을 사용할 수 없는 경우
  // Java Config 에서 수동으로 객체를 생성할 수 있다.
  // 방법:
  // 1) 객체를 생성하여 리턴하는 메서드를 정의한다.
  // 2) 그리고 그 메서드에 @Bean 애노테이션을 붙인다.
  //
  // @Bean 애노테이션을 붙이면,
  // 스프링 IoC 컨테이너(AnnotationConfigApplicationContext)는
  // 해당 메서드를 호출하고,
  // 그 메서드가 리턴한 객체를 컨테이너에 보관한다.
  // 컨테이너에 보관할 때 사용할 객체 이름은
  // @Bean(객체이름) 애노테이션에 설정된 이름을 사용한다.
  // 만약 @Bean 애노테이션에 이름이 없으면,
  // 메서드 이름을 객체 이름으로 사용한다.
  //
  @Bean("car") // 애노케이션에 지정한 이름으로 리턴 값을 보관한다.
  public Car getCar2() {
    Car c = new Car(null);
    c.setMaker("비트자동차");
    c.setModel("티코");
    c.setCc(890);
    c.setAuto(true);
    return c;
  }

  @Bean // 이름을 지정하지 않으면 메서드 이름을 사용하여 저장한다.
  public Car getCar() {
    Car c = new Car(null);
    c.setMaker("비트자동차");
    c.setModel("티코");
    c.setCc(890);
    c.setAuto(true);
    return c;
  }

  // 실무에서는 스프링 설정용으로 사용하는 클래스에서
  // 객체를 리턴하는 메서드를 만들 때
  // 그 메서드의 이름을 객체 이름과 같게 짓는다.
  // => 보통 어떤 값을 리턴할 때는 getXxx()라는 이름으로 메서드를 만드는데,
  // 이처럼 객체이름으로 사용할 수 있도록 메서드를 만드는 경우도 있으니
  // 당황하지 말라!
  @Bean
  public Car car2() {
    Car c = new Car(null);
    c.setMaker("비트자동차");
    c.setModel("티코");
    c.setCc(890);
    c.setAuto(true);
    return c;
  }
}
```


# @Configuration
`@Component`와 `@Configuration`은 무엇이 다를까?

같은 appConfig 클래스에 @Component, @Configuration을 붙이고, Bean의 개수, 별명, 이름 리스트를 출력해보자.

@Configuration
```
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
--------------------------------
빈 개수: 9
org.springframework.context.annotation.internalConfigurationAnnotationProcessor = org.springframework.context.annotation.ConfigurationClassPostProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor = org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor
org.springframework.context.event.internalEventListenerProcessor = org.springframework.context.event.EventListenerMethodProcessor
org.springframework.context.event.internalEventListenerFactory = org.springframework.context.event.DefaultEventListenerFactory
a = com.eomcs.spring.ioc.ex10.b.A
appConfig = com.eomcs.spring.ioc.ex10.b.AppConfig$$SpringCGLIB$$0
c = com.eomcs.spring.ioc.ex10.b.C
car2 = com.eomcs.spring.ioc.ex10.Car
car3 = com.eomcs.spring.ioc.ex10.Car
--------------------------------
```

@Component
```
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
--------------------------------
빈 개수: 9
org.springframework.context.annotation.internalConfigurationAnnotationProcessor = org.springframework.context.annotation.ConfigurationClassPostProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor = org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor
org.springframework.context.event.internalEventListenerProcessor = org.springframework.context.event.EventListenerMethodProcessor
org.springframework.context.event.internalEventListenerFactory = org.springframework.context.event.DefaultEventListenerFactory
a = com.eomcs.spring.ioc.ex10.b.A
appConfig = com.eomcs.spring.ioc.ex10.b.AppConfig
c = com.eomcs.spring.ioc.ex10.b.C
car2 = com.eomcs.spring.ioc.ex10.Car
car3 = com.eomcs.spring.ioc.ex10.Car
--------------------------------
```

차이가 있는 부분은 아래와 같다.

* @Configuration: `appConfig = com.eomcs.spring.ioc.ex10.b.AppConfig$$SpringCGLIB$$01`
* @Component: `appConfig = com.eomcs.spring.ioc.ex10.b.AppConfig`


## @PropertySource 애노테이션
```java
package com.eomcs.spring.ioc.ex10.c;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.env.Environment;
import com.eomcs.spring.ioc.ex10.Car;

@Configuration

// @PropertySource 애노테이션
// => .properties 파일을 데이터를 메모리에 로딩하는 일을 한다.
// => 파일 경로가 클래스 경로를 가리킨다면,
//    파일 경로 앞에 "classpath:" 접두어를 붙여라.
@PropertySource({
  "classpath:com/eomcs/spring/ioc/ex10/c/jdbc.properties",
  "classpath:com/eomcs/spring/ioc/ex10/c/jdbc2.properties"
})
public class AppConfig {

  // @PropertySource를 통해 로딩된 프로퍼티 값을 사용하고 싶다면
  // Environment 타입의 객체를 주입 받아라!
  @Autowired
  Environment env;  // Spring IoC 컨테이너는 Environment 객체를 주입해 준다.

  // @PropertySource를 통해 로딩된 프로퍼티 값 중에서
  // 특정 값만 필드로 주입 받을 수 있다.
  // => 필드 선언에 @Value 애노테이션을 붙인다.
  // => @Value("${프로퍼티이름}")
  @Value("${jdbc.url}")
  String jdbcUrl;

  @Value("${jdbc2.url}")
  String jdbc2Url;

  // Environment 객체를 통해 메모리에 보관된 .properties 의 값 가져오기
  @Bean
  public Car car1() {
    System.out.println("car1() 호출: ");
    System.out.println("  " + this.env.getProperty("jdbc.username"));
    System.out.println("  " + this.env.getProperty("jdbc2.username"));
    return new Car(null);
  }

  // @Value를 통해 필드로 주입 받은 .properties 값 꺼내기
  @Bean
  public Car car2() {
    System.out.println("car2() 호출: ");
    System.out.println("  " + this.jdbcUrl);
    System.out.println("  " + this.jdbc2Url);
    return new Car(null);
  }

  // @Value를 사용하여 파라미터로 .properties 값 주입 받기
  @Bean
  public Car car3(
      @Value("${jdbc.username}") String username,
      @Value("${jdbc2.username}") String username2) {

    System.out.println("car3() 호출 :");
    System.out.println("  " + username);
    System.out.println("  " + username2);
    return new Car(null);
  }

}

```