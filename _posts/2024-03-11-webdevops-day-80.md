---
title: (Day	78) Spring IoC Container
author: 김준회
date: 2024-03-11 17:00:00 +0900
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

# IoC Container
IoC Container = Inversion of Control => Dependency Object Injection

필요한 객체들 (의존하고 있는 객체들)을 본인이 생성하는 것이 아니라, 다른 것에서 생성해서 주입해준다는 것. 그래서 제어가 역전된다는 것.

예시를 들면 아래 코드에서 `assignmentDao` 객체가 주입받는 의존성이다.

```java
  @RequestMapping("/assignment/add")
  public String add(Assignment assignment) throws Exception {
    System.out.println(assignment);
    assignmentDao.add(assignment);
    return "redirect:list";
  }
```

## How to set IoC Container?
1. ClassPathXmlApplicationContext

2. FileSystemXmlApplicationContext

3. AnnotationConfigApplicationContext

## Working principle of Spring IoC Container
.class 바이트코드를 기반으로 해서 객체를 만든다. *그 객체는 쓰지만 맛있다(?)*

.class가 객체로 만들어질 때까지, 마치 컨베이어벨트를 지나듯 여러 여러 작업이 수행된다. @ComponentScan 도 처리하고, @Component 도 처리하고...

각 작업들을 수행하는 객체들이 또 따로 있다. 이건 마치 컨베이어 벨트에 각 작업을 수행하는 사람들이 대기하는 것처럼 보인다.

![](data:image/jpeg;base64,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)

![](https://media.sciencephoto.com/image/t9300109/800wm/T9300109.jpg)

IoC 컨테이너가 구동할 때, 작업을 처리하는 객체들이 모두 있어야 하는 것은 아니다. 작업을 처리할 객체가 존재하는 경우 IoC 컨테이너가 그 객체를 사용해 작업을 수행한다. 없다면 그 작업은 수행되지 않는다. 

이것이 IoC 컨테이너가 구동되는 기본적인 흐름이다. 이것이 먼저 이해되어야 한다. 그리고, 어떤 작업자들은 (IoC 컨테이너에서 작업하는 객체들은) 따로 생성하지 않아도 존재한다. 따로 유저가 생성하지 않아도 생성되는 객체가 있다는 것이다. 그런 객체가 무엇인지 알아야 한다.

예를 들면 `AnnotationConfigApplicationContext`에서는 아래 4개의 객체가 기본적으로 필요한 객체이다.
```
org.springframework.context.annotation.internalConfigurationAnnotationProcessor = org.springframework.context.annotation.ConfigurationClassPostProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor = org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor
org.springframework.context.event.internalEventListenerProcessor = org.springframework.context.event.EventListenerMethodProcessor
org.springframework.context.event.internalEventListenerFactory = org.springframework.context.event.DefaultEventListenerFactory
```

internalConfigurationAnnotationProcessor: 기본적인 애너테이션을 확인해서 작업을 처리한다.
XML을 쓰는 클래스에서는 필요가 없지만 `AnnotationConfigApplicationContext`은 필요로 한다.

오늘은 XML로 IoC Container에 객체를 추가하는 방법을 배운다. 다만, 이것을 배우는 것은 XML로 Spring을 설정해주는 것이 대세라는 의미는 아니다. 대세는 Annotation으로 설정하는 것인데, 이전에 개발된 시스템을 관리할 때 필요할수도 있어 한번 보고 가는 것이다.

## 익명 객체의 별명 (ex02/d)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 빈의 이름을 지정하지 않을 경우 
         => FQName과 인덱스 번호가 객체의 이름으로 사용된다.
         => FQName#인덱스번호
         => 예) com.eomcs.spring.ioc.ex02.Car#0
         => 익명 객체의 수만큼 인덱스 번호가 증가한다.
    -->
    
    <!-- 
      특히 0번 익명 객체의 별명은 클래스명과 같다.
      즉 com.eomcs.spring.ioc.ex02.Car#0 이름을 가진 익명 객체의 별명은 
         com.eomcs.spring.ioc.ex02.Car 이다.
      그외 익명 객체는 별명이 붙지 않는다.  -->
    <bean class="com.eomcs.spring.ioc.ex02.Car"/>
    <bean class="com.eomcs.spring.ioc.ex02.Car"/>
    <bean class="com.eomcs.spring.ioc.ex02.Car"/>
    <bean class="com.eomcs.spring.ioc.ex02.Car"/>
    
    <!-- 인덱스 번호는 클래스마다 0부터 시작한다. -->
    <bean class="com.eomcs.spring.ioc.ex02.Engine"/>
    <bean class="com.eomcs.spring.ioc.ex02.Engine"/>
    <bean class="com.eomcs.spring.ioc.ex02.Engine"/>
</beans>
```
클래스마다 인덱스는 0부터 시작한다. 같은 클래스에 대해 첫 번째 익명 객체 만이 별명을 갖는다. 같은 클래스에 대해 두 번째 익명 객체부터는 별명이 없다.

## ex03
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 호출할 생성자 지정하기 -->
    
    <!-- 생성자의 파라미터 값을 주지 않으면 기본 생성자가 호출된다.  -->
    <bean id="c1" class="com.eomcs.spring.ioc.ex03.Car"/>
    
    <!-- 다른 생성자 호출하기 : 
      => 파라미터 값을 설정하면 그 값에 맞는 생성자가 선택되어 호출된다.  
      => <constructor-arg/> 엘리먼트를 사용하여 호출될 생성자를 지정할 수 있다.
      => 즉 생성자를 호출할 때 넘겨줄 값을 지정하면 
         스프링 IoC 컨테이너는 그 값을 받을 생성자를 찾아 호출한다. 
      => 파라미터의 개수가 같은 생성자가 여러 개 있을 경우 
         스프링 IoC 컨테이너는 내부의 정책에 따라 적절한 생성자를 선택한다.
         보통 String 타입이 우선이다.-->
    <bean id="c2" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg>
            <value>티코</value>
        </constructor-arg>
    </bean>
    
    <!-- 한 개의 파라미터 값을 받는 생성자가 여러 개 있을 경우,
         String 타입의 값을 받는 생성자가 우선하여 선택된다. 
         생성자를 정의한 순서는 상관없다.-->
    <bean id="c3" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg>
            <value>1980</value>
        </constructor-arg>
    </bean>
    
    <!-- 한 개의 파라미터를 가지는 생성자가 여러 개 있을 경우, 
         특정 생성자를 지정하고 싶다면 파라미터의 타입을 지정하라! -->
    <bean id="c4" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg>
            <value type="int">1980</value>
        </constructor-arg>
    </bean>
    
    <!-- 파라미터가 여러 개인 생성자를 호출할 경우 
         IoC 컨테이너가 가장 적합한 생성자를 찾아 호출한다. -->
    <bean id="c5" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg>
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
        <constructor-arg>
            <value type="int">1980</value>
        </constructor-arg>
    </bean>
    <bean id="c6" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg>
            <value type="int">1980</value>
        </constructor-arg>
        <constructor-arg>
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
    </bean>
    
    <!-- 파라미터의 값을 설정할 때 이름을 지정해도 
         개발자가 임의로 특정 생성자를 호출하게 제어할 수 없다.
         IoC 컨테이너가 판단하여 적절한 생성자를 호출한다. -->
    <bean id="c7" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg name="cc">
            <value type="int">1980</value>
        </constructor-arg>
        <constructor-arg name="model">
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
    </bean>
    <bean id="c8" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg name="model">
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
        <constructor-arg name="cc">
            <value type="int">1980</value>
        </constructor-arg>
    </bean>
    
    <!-- index 속성을 사용하여 파라미터 값이 들어가는 순서를 지정할 수 있다.
         즉 개발자가 어떤 생성자를 호출할 지 지정할 수 있다. -->
    <bean id="c9" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg index="0">
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
        <constructor-arg index="1">
            <value type="int">1980</value>
        </constructor-arg>
    </bean>
    <bean id="c10" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg index="1">
            <value type="java.lang.String">소나타</value>
        </constructor-arg>
        <constructor-arg index="0">
            <value type="int">1980</value>
        </constructor-arg>
    </bean>
    
    <!-- 기본 생성자가 없으면 예외 발생! -->
    <!--
    <bean id="e1" class="com.eomcs.spring.ioc.ex03.Engine"/>
    -->
</beans>
```


## XML 객체에서 생성자에 주는 아규먼트 넣기

방법1

방법2
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 호출할 생성자 지정하기 II -->
    
    <!-- 생성자의 파라미터 값을 지정하는 간단한 방법 -->
    <bean id="c1" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg type="java.lang.String" value="티코"/>
    </bean>
    
    <!-- index로 파라미터의 순서를 지정하기 -->
    <bean id="c2" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg index="0" type="java.lang.String" value="티코"/>
        <constructor-arg index="1" type="int" value="890"/>
    </bean>
    
    <!-- value 속성에 지정한 값은 문자열이다.
         생성자를 호출하여 값을 넣을 때 
         IoC 컨테이너는 이 문자열을 파라미터 타입으로 형변환하여 넣는다. 
         단 primitive type에 대해서만 형변환할 수 있다.
         다른 타입은 불가하다 -->
    <bean id="c3" class="com.eomcs.spring.ioc.ex03.Car">
        <constructor-arg index="0" value="티코"/>
        <constructor-arg index="1" value="890"/>
    </bean>
</beans>
```
