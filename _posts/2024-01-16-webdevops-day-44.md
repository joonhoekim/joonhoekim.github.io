---
title: (Day	44) 프록시 패턴, Stateful VS Stateless, 동적 DAO 생성 (자동객체생성)
author: 김준회
date: 2024-01-16 23:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt:
---
# Review
## Proxy Pattern (GoF)
`Proxy`는 라틴어 단어 "procurator"에서 파생된 단어이며, "Procurator"는 "대리인"이나 "대리인으로서 행동하는 사람"을 의미함. 한국어로 번역하면 `대리인` 정도가 될 수 있음.

>`Proxy`는 컴퓨터 과학 분야에서 주로 사용되며, 네트워크 통신, 보안, 캐싱 등 다양한 컨텍스트에서 중간 매개자 역할을 하는 컴퓨터 시스템이나 서비스를 나타낸다. 예를 들면, 프록시 서버는 클라이언트와 서버 간의 통신을 중계하고, 클라이언트나 서버의 요청을 대신 수행할 수 있는 중간 서버를 의미한다. 트래픽 관리, 보안에도 사용된다.

보통 리모트에 있는 다른 서비스객체를 사용하기 위해, 그 서비스 객체와 사용방법(메서드)가 똑같은 대리인 객체를 로컬에 둔다. 이렇게 사용방법이 똑같은 대리인 객체를 두는 방법이 Proxy Pattern이다. 대리인 객체는 서비스 객체처럼 진짜 일을 할 수는 없고, 그 처리를 요청하고, 응답을 받아 다시 반환하는 역할을 한다.

### Stub / Skeleton / ORB
Stub은 대리인 역할을 하는 객체를 말한다.
Skeleton은 

### Service Object / 
Service Object는

## 프록시 패턴의 발견 과정
처음에는 캡슐화로 시작한다. 서비스 객체의 메서드 호출 방법이 복잡하므로 캡슐화가 진행된다.
나중에 서비스 객체를 더 쉽게 사용하기 위해 사용 방법이 똑같은 객체를 만든다. (I/F 문법이 활용된다)
그것이 Stub이다. Stub - Skeleton - Service Object 형태가 만들어진다.
프록시 패턴이 발견된다.
Proxy 패턴을 적용한 써드파티 서비스 사업자가 Stub을 배포하게 된다.
다른 개발자들은 배포된 Stub을 설치하면, 써드파티 서비스의 메서드를 동일한 사용방법으로 호출할 수 있게 된다. (ex. Payment Gateway)




## 용어를 다시 생각하기
### 인스턴스란 무엇인가?
`인스턴스는 클래스라는 설계도를 실체화한 것이다` 라는 답변이 많다. 틀린 말이 아니고 잘 와닿는 표현이지만 아쉬운 점이 있는 표현이다. 
1. 실체화란 무엇인가?
2. 클래스가 설계도인가?
3. Heap, Method Area 등 다음 용어로 확장하지 못한다. (이건 면접을 주도하는 기술..)
	* Serialize, Marshaling, Encoding (Deserialize, Unmarshaling, decoding)
4. `객체`, `레퍼런스 변수`, `메모리 주소` 같은 개념으로 확장되지 못한다.

### `객체를 사용한다`는 표현
객체와 인스턴스 두 용어는 아주 많이 혼용된다. 객체를 사용한다는 것은, 인스턴스의 데이터(메서드와 필드)를 사용(호출)한다는 것이다. 그리고 대부분은 ***그 객체의 메서드를 호출하는 것을 그 객체를 사용한다고 말한다.*** 인스턴스의 MA 및 Heap 영역에 접근하여 그 코드블럭(메서드)를 쓴다는 것이다.

일반적으로 코드를 가리키며 객체라고 할 때는 인스턴스 주소를 가진 레퍼런스 변수를 가리킨다. 그러나 의식이 확장하는 방향에 따라 사람들은 객체와 인스턴스, 레퍼런스 변수를 큰 구분없이 사용하게 되었다.

레퍼런스 변수라고 말하면 정확히 레퍼런스 변수로서의 해당 대상을 가리키는 경우가 많지만 인스턴스, 혹은 객체라고 말할 때는 **편하니까** 혼용하게 됐다.




## meta
### 분산컴퓨팅에 대한 요구가 생긴 이유

초기 C/S App을 썼을 때는 모든 기능을 1대의 컴퓨터에서 처리했겠지만, 나중에 여러 클라이언트가 해당 서버에 접속하다보니 성능, 안정성을 이유로 여러 컴퓨터로 서비스를 분산하고자 하는 요구가 생긴다.

### RMI 발전과정
RPC(절차적언어)-RMI(OOP)-EJB(RMI for java)-CORBA-WebService-RESTful
* RPC: remote procedure call
* RMI: remote method invocation (Proxy Pattern)
* EJB: enterprise java bean (RMI for java)
* CORBA: common object request broker architecture
* WebService: 서버에서 WSDL 받아와서 Local의 IDE가 Stub을 자동으로 생성
* RESTful: HTTP를 통한 클라이언트와 웹서버간의 요청/응답(웹 서버는 서비스 객체에 처리를 위임)

## Managing
### Gradle tutorial: Subproject


# Study
### 초점
객체지향은 언어의 문법 하나하나보다 `구조`에 초점을 맞춰야 한다. 그것이 중요하다.

### Client/Server
1. Local/Remote
2. 메서드를 사용하는 클래/그 처리를 하는 클래스

## 프록시 객체를 자동생성하기
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class Main {
  public static void main(String[] args) {

    Class<?> clazz = Exam0120.class;
    ClassLoader classLoader = Exam0120.class.getClassLoader();
    Class<?>[] interfaceTypes = new Class<?>[] {A.class, B.class, C.class};

    Class<?> aType = A.class;
    System.out.println(aType); // A의 타입 정보
    Class<?> bType = B.class;
    Class<?> cType = C.class;
    Class<?>[] interfaceTypes2 = new Class<?>[] {aType, bType, cType};

    // Reflect API에서 제공하는 클래스
    InvocationHandler invocationHandler = new InvocationHandler() {

      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // TODO Auto-generated method stub
        System.out.println(method.getName());
        return null;
      }
    };

    // newProxyInstance가 리턴하는 객체는 A, B, C 인터페이스를 모두 구현한 클래스의 객체다.
    // 줄임말: newProxyInstance는 A, B, C인터페이스 모두 구현한 객체를 리턴한다.
    Object obj = Proxy.newProxyInstance(classLoader, interfaceTypes2, invocationHandler);

    ((A) obj).m1();
    ((B) obj).m2();
    ((C) obj).m3();

    // ((String) obj).replaceFirst(null, null); //콤파일러는 속일지 몰라도 제붐이는 못속이

    System.out.println(clazz);
    System.out.println(classLoader);
    System.out.println(interfaceTypes);
  }
}
```

## Reflection API를 추가로 배웠음
java-basic에서 reflection 확인!!

>Java에서 Reflection API와 Proxy 클래스를 함께 사용하면, 동적으로 인터페이스를 구현하는 프록시(Proxy) 클래스를 생성할 수 있습니다. 이는 특히 런타임에 동적으로 객체를 생성하거나 메소드를 호출해야 하는 경우에 유용합니다. Proxy 패턴을 사용하면 런타임에 객체의 행동을 변경하거나 제어할 수 있습니다.

>Proxy 클래스는 대상 인터페이스를 구현하며, 메소드 호출에 대한 처리를 추가하거나 변경합니다. Java에서는 `Proxy` 클래스와 `InvocationHandler` 인터페이스를 사용하여 이러한 동적 프록시를 생성할 수 있습니다.
>
### Reflection API를 이용하여 동적으로 DAO 객체 작성하기
app-api/src/main/java.bitcamp.myapp.dao.DaoProxyGenerator
```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.lang.reflect.Proxy;
import java.lang.reflect.Type;

public class DaoProxyGenerator {

  private DataInputStream in;
  private DataOutputStream out;
  private Gson gson;

  public DaoProxyGenerator(DataInputStream in, DataOutputStream out) {
    this.in = in;
    this.out = out;
    gson = new GsonBuilder().setDateFormat("yyyy-MM-dd").create();
  }

  public <T> T create(Class<T> clazz, String dataName) {
    return (T) Proxy.newProxyInstance(clazz.getClassLoader(),
        new Class<?>[]{clazz},
        (proxy, method, args) -> { //이 파라미터를 쓰는 functional interface가 특정되므로 가능한 람다문법...
          try {
            out.writeUTF(dataName);
            out.writeUTF(method.getName());
            if (args == null) {
              out.writeUTF("");
            } else {
              out.writeUTF(gson.toJson(args[0]));
            }

            String statusCode = in.readUTF();
            String entity = in.readUTF();

            if (!statusCode.equals("200")) {
              throw new Exception(entity);
            }

            //리턴타입 확인하기
            Type returnType = method.getGenericReturnType();

            if (returnType == void.class) {

              return null;

            } else {

              return gson.fromJson(entity, returnType);

            }
          } catch (Exception e) {
            e.printStackTrace();
          }

          return null;
        });
  }

}
```

## 네트워크 사례분석(java-basic)
### Stateless VS Stateful
![Stateful and Stateless Applications and its Best Practices](https://www.xenonstack.com/hubfs/xenonstack-stateful-vs-stateless.png)

![Stateful vs Stateless Firewalls - You NEED to know the difference - YouTube](https://i.ytimg.com/vi/rL4-vbsN35w/maxresdefault.jpg)

Stateful VS Stateless...
State(상태)를 유지하느냐, 유지하지 않느냐가 차이다.

Stateful, 즉 상태를 유지하는 경우는 이런 장단점이 생긴다.
* 장점: 한번 연결하면 여러번의 요청/응답을 수행할 수 있다.
* 단점: 요청/응답을 수행하지 않는 순간에도 연결 정보를 계속 유지하므로 대량의 클라이언트 연결을 지원하지 못한다.

Stateless, 즉 상태를 유지하지 않는 경우는 반대의 장단점이 생긴다.
* 장점: 요청/응답을 수행하지 않는 경우에는 연결 정보를 유지하지 않으므로 더 많은 클라이언트의 요청을 처리할 수 있다.
* 단점: 요청이 발생하는 경우 매번 연결도 수행해야 하므로 요청의 처리시간이 길어진다. (연결에 필요한 시간이 요청 처리하는 시간보다도 더 크다.)

대부분은 Stateless방식을 쓴다. 연결에 걸리는 시간이 있더라도, 클라이언트를 더 많이 회전시키는 것이 중요하기 때문이다.

서버 - 클라1, 클라2, 클라3, 클라4 ... (단일쓰레드)
* Stateful 의 경우, 먼저 연결된 클라가 연결을 끊을 때 까지 다음 클라이언트는 기다려야 한다.
* Stateless 의 경우, **한번 요청/응답이 완수되면 연결을 바로 끊는다!** 그 다음에는 기다리던 다른 클라이언트의 요청에 응답한다.

### 그럼 무조건 Stateless가 더 좋은 거 아닌가요?
연결이 끊기지 않고, 하나가 끝날 때 까지 무조건 기다려야 하는 경우가 있다. 전화 문의같은 경우가 그렇다.
그리고 각 쓰레드에서 끝까지 기다리는 경우도 그렇다. 웹서비스 로그인 세션, 게임 로그인 세션 등...
이건 우열을 가릴 것이 아니라, 상황에 적합한 처리방법이 있는 것이다.

## 네트워크 프로그래밍
### java 소켓 객체가 하는 일
```java
ServerSocket serverSocket = new ServerSocket(8888);
```
https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/ServerSocket.html
* 랜카드와 연결할 정보를 생성한다.
* 대기열을 준비시킨다.

```java

```

## 포트번호
서버쪽 포트 번호는 사전에 정해진다. [0~49151] 사이에서 개발자가 결정한다.
클라이언트의 포트번호는 OS가 자동으로 발급해준다. 개발자가 직접 정하지 않는다.
[49152~65535] 사이에서 운영체제가 결정해준다.



# Others
### 해외 직장 문화
외국에서 (특히 미국) 개발자는 워라밸이 좋다?
하나밖에 모르고 하는 소리다.
외국에서는 연봉이 성과에 따라 아주 많이 변한다.

한국 회사는 연봉이 근속연수에 따라 변하는데
외국에선 본인 성과에 따라 연봉이 변한다.

한국에선 성과에 큰 관계 없이 똑같은 돈을 받고 일하니
누가 야근을 얼마나 더 했니 그런 얘기가 나온다.

외국에선 야근을 얼마나 했는지는 관심 없다.
성과를 얼마나 냈는지가 중요하다.
근무 시간에 상관 없이 성과를 많이 냈으면 좋은 평가를 받는거고...
근무 시간이 길어도 성과를 못내면 나쁜 평가를 받는다.
한국도 스타트업은 이런 평가가 많이 적용되고 있다.

외국 개발자는 워라밸이 좋다? 아니다.
적은 시간에도 성과를 잘 내면 워라밸 좋게 다닐 수 있는거고,
긴 시간에도 성과를 못 내면 정치력이 있거나 뭔가 다른 부분에서 성과를 낼 수 있어야 하는 거다...

그래서 성과를 레버리지 할 수 있는 라이브러리 개발자 같이, 딥한 부분을 다루는 개발자들이 성과증명하기가 쉬워서 연봉 상승이 더 빠르다.