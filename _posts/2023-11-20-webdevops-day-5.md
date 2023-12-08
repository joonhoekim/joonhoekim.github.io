---
title: (Day5) - 제목입력
author: 김준회
date: 2023-11-20 23:00:00 +0900
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

> 이 글은 제가 교육을 수강하며 기록하고 추가한 내용입니다.
> **강사님과 무관하게** 잘못된 내용이 있을 수 있습니다.
{: .prompt-warning}

---

# 클라우드 기반 웹 데브옵스 프로젝트 개발자 교육 과정 (5기)

* 비트캠프 엄진영 강사님 (https://github.com/eomjinyoung/)
* 훈련기관 : 네이버클라우드주식회사
* 기간: 2023-11-14 ~ 2024-5-22
* 남은 일자 : ***124*** 일 ( 5/129 )

# 5일(2023-11-20,월)

# 학습


## 자바 컴파일러 및 JVM, 바이트코드간 관계
JVM 상위버전은 하위버전의 바이트코드를 실행이 가능하다. 그러나 하위 버전의 JVM이 상위 버전의 컴파일러에서 만든 바이트코드를 실행하는 것은 불가하다. 요약하면 JVM은 바이트코드에 대한 하위호환성만 보장한다.

## 인터프리트, 컴파일, 하이브리드 방식은 무엇인가? 
이것들은 애플리케이션의 개발 및 실행 방식을 분류한 것이다. 컴파일 방식은 소스코드를 컴파일하여 실행 파일을 기계어로 만든 이후에 실행이 가능한 것이다. 전체를 미리 컴파일해야 하며, 실행파일을 생성하면 그것만 있으면 된다. 단점은 OS별로, CPU의 ISA별로 컴파일을 해야 한다는 것이다. 하이브리드 방식은 인터프리트 방식과 컴파일 방식의 특징을 둘 다 가졌는데, 자바의 경우는 컴파일-javac으로 바이트코드를 컴파일해야 실행 가능하다. 인터프리트-바이트코드를 라인별로 실행한다.

## JIT, AOT
JIT, AOT는 인터프리트 및 하이브리드 방식의 성능 손실을 극복하기 위한 기법이다. 일부만 먼저 기계어로 바꿔서 캐싱해두고 쓰는 것이 JIT이며, 전체를 기계어로(네이티브 코드로) 변환한 다음에 네이티브 코드를 실행하는 방식이 AOT이다. Ahead of time: 실행 전 미리 컴파일, Just in time

기술에 대한 경험기반 이야기: 기술의 발전은 매번 다 엎어놓고 새로 만드는 게 아니고, 문제를 조금씩이라도 해결하며 발전시키는 것이다.

AOT 컴파일러
특정 버전 이후의 안드로이드 OS 설치된 폰은 AOT 컴파일러가 안드로이드 기기마다 설치되어 있다.

 ### 구글의 전략구글은 안드로이드에 성능 향상보다 일단 시장을 먼저 먹자는 전략으로 갔다. 그래서 저렴한 안드로이드 폰에 개발자 생태계를 불리고, 스마트폰에 준비되지 않은 OS, 리눅스 배포판인 안드로이드에 기능을 붙이는 것에 집중한 것이다. 전화도, 메세지도, 카메라도 제대로 된 기능을 먼저 추가해주는게 중요했던 것이다.

 # 빌드 및 빌드 도구에 대하여

## 빌드의 개념소스코드 작성 이후에 사용자에게 배포하기 위한 소프트웨어를 만들기까지의 과정을 빌드라고 한다. 건물을 짓는 데 많은 과정이 순서에 따라 있는 것처럼 소프트웨어도 소스코드에서 배포판으로 만들어가기까지 여러 과정이 있다.

## 빌드 도구의 개념빌드할 때는 디버거 등 다양한 개발자용 프로그램들을 사용한다. 문서도 생성해야하고, war jar 등의 컴파일 결과를 패킹하며, 이런 패킹된 결과를 서버에 업로드하는 등의 과정이 많다. 빌드 과정이 많다는 것이다. 이런 과정들을 도와주는 툴이 빌드 도구다. 자바 빌드 도구는 앤트, 메이븐, 그레이들이 있다.

## 빌드 도구별 차이다른 리포를 빌드할 때 어떤 빌드도구를 써야 하나?: 빌드 스크립트 파일을 보고 확인한다. Ant: build.xml, Maven: pom.xml (project object model), Gradle: build.gradle

ant는 external dependencies 관리가 불가해서 등장한 게 Maven 이다. 외부 라이브러리 의존성을 설정해주면, 그게 없는 경우 빌드 시 자동으로 다운로드 받아준다. 훨씬 편하기 때문에 Ant를 안 쓰게 된 것이다. 외부의 자원을 사용하지 않으면 살아남지를 못하는 것이다.  어떤 일이던간에 타인의 지식을 이용하고 받아들일 수 없다면 그건 경쟁력이 생길 수가 없는 거고, 경쟁력이 없다면 죽는 거다. 외부 코드를 사용하기 위한 파일이 라이브러리다. 참고로 ant는 나중에 의존성관리 기능을 추가했는데 그 때는 이미 메이븐으로 대다수의 사용자가 이주한 상태였다.

Gradle은 어떻게 주류가 되었나? 빌드 과정을 좀 더 정밀하게 제어할 수 있기 때문이다. 안드로이드의 기본 빌드 도구는 그레이들이다. 왜 그레이들은 빌드 과정을 좀더 정밀하게 제어하고, 어떻게 그렇게 했는가? 그루비라는 프로그래밍 언어를 통해서 빌드 스크립트 파일을 제어하기 때문에 가능한 것이고, 그래야 하는 이유는 컴파일러가 어떤게 있는지 등 빌드 과정에 변수가 많기에 그렇다. 

### 리포트 작성
`gradle test` 명령어는 빌드 결과에 대한 리포트도 만들어준다.
Java를  한다면  그레이들은 10년 20년도  쓰게  될  수  있는  도구다.

## 고객사에 배포하는 것?

build 하고 나면 dist 폴더가 생긴다. 이렇게 패킹된 배포판을 통해 고객사에 배포한다.
그러면 고객사는 해당 배포판을 깃허브에 올리고, 리눅스 서버는 배포판을 깃허브에서 가져올 것이다. 여기서 한 단계 더 가면 도커와 쿠버네티스, 그리고 젠킨스가 등장하는 것이다.

젠킨스가 등장하면 gradle build 같은 명령어를 직접 터미널에 입력하지 않고 젠킨스가 서버를 통제하여, gradle build 를 자동화하여 수행하고, 도커 허브에 업로드 및 해당 이미지 다운로드 및 스타트가 되게 한다.

**IDE에서 저장하고 런&디버깅 하는 건 실무에서는 거의 없다.**

  

## build 폴더를 정리해주는 clean 명령어

gradle clean 하면 빌드 폴더를 삭제한다.

  

## gradle run 에 대하여

이것은 자바 플러그인이 아니라 어플리케이션 플러그인에 포함된 명령어이다.

-   compileJava
-   processResources
-   classes
-   run : build script file에 설정된 메인 클래스를 실행하는 명령이다.

### 태스크에 대한 간단한 요약
컴파일만 하고 싶다 -> classes
jar 패킹이 필요하다 -> jar
test 보고서가 필요하다 -> test
위에 내용이 다 필요하다 -> build

---
> 이 아래의 내용은 실습간 강의 내용을 모두 타이핑 할 수는 없었기에 ChatGPT에게 많은 도움을 받았다.

## Java Project Directory
자바 프로젝트 디렉토리(Project Directory)는 **일반적인 java project의 디렉토리 구조**를 말한다.
이는 프로젝트의 특정 요구 사항과 **사용되는 빌드 도구**에 따라 다를 수 있습니다. 여기에서는 일반적인 Java 프로젝트의 디렉토리 구조를 정의합니다. 

아래 구조는 Maven 또는 Gradle과 같은 빌드 도구를 사용하는 경우에 일반적으로 적용됩니다.

```plaintext
my-java-project/
│
├── src/
│   ├── main/
│   │   ├── java/           # 주요 소스 코드 디렉토리
│   │   └── resources/      # 자원 파일(구성 파일, 이미지 등) 디렉토리
│   │
│   └── test/
│       ├── java/           # 테스트 소스 코드 디렉토리
│       └── resources/      # 테스트 자원 파일 디렉토리
│
├── target/                # 빌드된 클래스 파일 및 JAR 파일이 생성되는 디렉토리 (빌드 도구에 따라 다름)
│
├── pom.xml                # Maven 프로젝트의 메타 정보 및 의존성을 정의하는 파일
│
├── build.gradle           # Gradle 프로젝트의 빌드 스크립트 파일
│
├── settings.gradle        # Gradle 멀티 프로젝트에서 각 프로젝트의 설정을 정의하는 파일
│
├── .gitignore             # Git 저장소에서 무시해야 할 파일 및 디렉토리 목록을 정의하는 파일
│
├── README.md              # 프로젝트에 대한 간략한 설명, 빌드 및 실행 방법 등을 기술한 문서
│
└── .idea/                 # IntelliJ IDEA 프로젝트 설정 파일들 (IDE에 따라 다를 수 있음)
```

## 컴파일과 실행
`javac`와 `java` 명령어는 Java 소스 코드의 컴파일과 실행을 위해 사용됩니다. 각 명령어의 가장 중요한 내용을 간략하게 설명하겠습니다.

1. **`javac` 명령어:**
   - **목적:** Java 소스 코드를 컴파일하여 바이트코드(.class 파일)를 생성합니다.
   - **사용법:** `javac [options] [source files]`
   - **주요 옵션:**
     - `-d <directory>`: 컴파일된 클래스 파일을 지정된 디렉토리에 저장합니다.
     - `-classpath` 또는 `-cp`: 컴파일 시에 참조할 클래스 경로를 설정합니다.
     - `-source`와 `-target`: 컴파일할 소스 코드와 대상 JVM 버전을 설정합니다.

   ```bash
   # 예시: MyClass.java를 컴파일하여 output 디렉토리에 클래스 파일을 생성
   javac -d output MyClass.java
   ```

2. **`java` 명령어:**
   - **목적:** 컴파일된 Java 프로그램(바이트코드)을 실행합니다.
   - **사용법:** `java [options] <main class> [args]`
   - **주요 옵션:**
     - `-classpath` 또는 `-cp`: 실행 시에 참조할 클래스 경로를 설정합니다.
     - `-version`: 현재 설치된 Java 버전을 출력합니다.
     - `-Xmx` 및 `-Xms`: Java 가상 머신의 최대 및 최소 힙 크기를 설정합니다.

   ```bash
   # 예시: MyClass 클래스를 실행 (MyClass에는 main 메서드가 있어야 함)
   java -cp output MyClass
   ```

이 명령어들은 Java 개발자가 소스 코드를 컴파일하고 실행하는 데 필수적입니다. 컴파일은 소스 코드를 바이트코드로 변환하며, 실행은 해당 바이트코드를 JVM(Java Virtual Machine)에서 실행하여 프로그램을 시작합니다. 클래스 패스 설정은 프로그램이 필요로 하는 다른 클래스 파일이나 라이브러리에 접근하는 데 중요한 역할을 합니다.

# Gradle 에 관하여
## Gradle 빌드 도구를 사용하여 자바 프로젝트 폴더 준비
gradle init 명령어를 통해서 프로젝트 디렉토리 준비를 자동화할 수 있다.
## Gradle 빌드 도구 설치
[공식 가이드](https://gradle.org/install/)를 참조하여 설치를 진행하면 된다. 방법은 두 가지가 있다.
1. 인스톨러를 사용하여 설치. https://gradle.org/releases/ 
2. 패키지 관리 도구를 통해서 설치
	3. Windows - SDKMAN!  `sdk install gradle 8.5`
	4. macOS - Homebrew `brew install gradle`
## Gradle 빌드 도구를 이용하여 자바 프로젝트 디렉토리를 자동 구성하기
터미널(powershell, zsh 등)에서 원하는 경로로 이동한다. (이 문장이 이해되지 않을 경우 CLI 환경에 대해서 스터디해보면 된다.) 그 후 `gradle init` 명령어를 통해서 프로젝트 디렉토리를 원하는 상황에 따라 자동 구성할 수 있다. (일단은 이렇게만 알아두자!)

## 자바 프로젝트의 폴더 구조 및 파일의 역할 이해

```plaintext
my-java-project/
│
├── src/
│   ├── main/
│   │   ├── java/           # 주요 소스 코드 디렉토리
│   │   └── resources/      # 자원 파일(구성 파일, 이미지 등) 디렉토리
│   │
│   └── test/
│       ├── java/           # 테스트 소스 코드 디렉토리
│       └── resources/      # 테스트 자원 파일 디렉토리
```

## Gradle 빌드 스크립트 파일 구조(1)
Gradle의 빌드 스크립트 파일은 `build.gradle`이라는 이름을 가집니다. 이 파일은 Groovy 또는 Kotlin 스크립트로 작성되며, 프로젝트의 빌드 및 설정을 정의합니다. 아래는 간단한 Gradle 빌드 스크립트의 주요 구조를 설명합니다.

```groovy
// build.gradle (Groovy 스크립트 예시)

// 1. 프로젝트의 기본 설정
group 'com.example'          // 프로젝트 그룹
version '1.0'                // 프로젝트 버전

// 2. 플러그인 적용
apply plugin: 'java'         // Java 플러그인 적용 (Java 프로젝트일 경우)

// 3. 의존성 설정
dependencies {
    implementation 'org.apache.commons:commons-lang3:3.12.0'  // 외부 라이브러리 의존성
    testImplementation 'junit:junit:4.12'                     // 테스트 의존성
}

// 4. 소스 및 리소스 디렉토리 설정
sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']      // 주요 소스 코드 디렉토리
        }
        resources {
            srcDirs = ['src/main/resources'] // 주요 리소스 디렉토리
        }
    }
    test {
        java {
            srcDirs = ['src/test/java']      // 테스트 소스 코드 디렉토리
        }
        resources {
            srcDirs = ['src/test/resources'] // 테스트 리소스 디렉토리
        }
    }
}

// 5. 빌드 태스크 설정
tasks.build {
    // 빌드 태스크의 추가적인 설정
}

// 6. 사용자 정의 태스크 추가
task myCustomTask {
    doLast {
        // 태스크가 실행될 때 수행할 동작
        println "Hello from my custom task!"
    }
}

// 추가적인 설정 및 기능 구현
// ...
```

주요 구성 요소:

1. **프로젝트의 기본 설정:** 프로젝트의 그룹 및 버전을 설정합니다.

2. **플러그인 적용:** Gradle 플러그인을 적용하여 빌드 프로세스에 필요한 기능을 확장합니다. 예를 들어, `java` 플러그인은 Java 프로젝트를 위한 기본 빌드 기능을 제공합니다.

3. **의존성 설정:** 프로젝트가 사용하는 외부 라이브러리 또는 다른 모듈에 대한 의존성을 정의합니다.

4. **소스 및 리소스 디렉토리 설정:** 소스 코드 및 리소스 파일의 위치를 지정합니다.

5. **빌드 태스크 설정:** 빌드 과정을 세부적으로 조정하는 데 사용되는 설정.

6. **사용자 정의 태스크 추가:** 사용자가 원하는 특정 작업을 정의하고 구현할 수 있는 사용자 정의 태스크 추가.

Gradle 빌드 스크립트는 Groovy나 Kotlin과 같은 DSL(Domain-Specific Language)을 사용하여 작성되며, 이를 통해 강력하고 유연한 빌드 설정을 가능하게 합니다.

## tasks 명령
`gradle task` 명령어는 gradle이 현재 설정상 할 수 있는 작업들을 출력해준다.
`gradle task -all` 옵션을 주어서 모든 태스크를 표시할 수도 있다.

**아래 java 플러그인 사용법부터 task에 관한 내용이 많다. `태스크(task)`는 java를 빌드하는 여러 과정들을 자동화한 단위다.  이러한 태스크를 알아보는 건 빌드라는게 실제로 어떻게 이뤄지는지 파악하려는 노력이다.**

![javaPluginTasks](https://docs.gradle.org/current/userguide/img/javaPluginTasks.png)
https://docs.gradle.org/current/userguide/java_plugin.html#java_plugin


## 'java' 플러그인 사용법
Gradle에서 `java` 플러그인은 Java 프로젝트를 위한 빌드 기능을 제공하는 플러그인입니다. 이 플러그인을 적용하면 기본적인 Java 프로젝트 구조 및 빌드 라이프사이클을 설정할 수 있습니다. 여러 가지 기능을 포함하고 있으며, 주요 기능은 다음과 같습니다:

1. **프로젝트 구조 자동 설정:**
   - `src/main/java`: 주요 소스 코드 디렉토리.
   - `src/main/resources`: 주요 리소스 디렉토리.
   - `src/test/java`: 테스트 소스 코드 디렉토리.
   - `src/test/resources`: 테스트 리소스 디렉토리.

2. **기본적인 빌드 태스크:**
   - `assemble`: 프로젝트를 빌드하여 JAR 파일을 생성합니다.
   - `check`: 프로젝트의 테스트를 실행하고 정적 분석을 수행합니다.

3. **의존성 관리:**
   - `compile` 구성: 프로젝트의 컴파일에 필요한 라이브러리 의존성을 지정할 수 있습니다.
   - `testCompile` 구성: 테스트 시에 필요한 라이브러리 의존성을 지정할 수 있습니다.

4. **실행 환경 설정:**
   - `run`: 프로젝트를 실행합니다.
   - `application` 플러그인을 사용하면 실행 가능한 JAR 파일을 생성할 수 있습니다.

5. **프로젝트 설정:**
   - `group`, `version`: 프로젝트의 그룹과 버전을 설정합니다.
   - `sourceCompatibility`, `targetCompatibility`: 소스 및 대상 JVM 버전을 설정합니다.

6. **IDE 통합:**
   - IntelliJ IDEA나 Eclipse와 같은 통합 개발 환경에서 프로젝트를 잘 구성하도록 도와줍니다.

7. **기타 옵션:**
   - `clean`: 빌드된 파일 및 디렉토리를 제거합니다.
   - `build`: `assemble`와 `check` 태스크를 실행하여 프로젝트를 빌드합니다.

`java` 플러그인을 사용하면 프로젝트의 기본 빌드 설정을 간편하게 구성할 수 있으며, 추가적인 설정이나 기능을 필요로 하는 경우에는 빌드 스크립트를 통해 사용자 정의 설정을 추가할 수 있습니다.

## compileJava, processResources, classes
`compileJava`, `processResources`, 그리고 `classes`는 Gradle에서 Java 프로젝트를 빌드할 때 사용되는 중요한 빌드 태스크(Task)입니다. 이들은 각각 컴파일, 리소스 처리, 클래스 생성과 관련된 작업을 수행합니다.

1. **`compileJava`:**
   - **목적:** Java 소스 코드를 컴파일하여 바이트코드로 변환합니다.
   - **활용:** 주로 `src/main/java` 디렉토리에 있는 소스 코드를 컴파일합니다.
   - **예시:**
     ```bash
     gradle compileJava
     ```

2. **`processResources`:**
   - **목적:** 리소스 파일(구성 파일, 이미지 등)을 처리하여 빌드된 클래스 파일과 함께 출력 디렉토리에 배치합니다.
   - **활용:** 주로 `src/main/resources` 디렉토리에 있는 리소스 파일들을 처리합니다.
   - **예시:**
     ```bash
     gradle processResources
     ```

3. **`classes`:**
   - **목적:** `compileJava` 및 `processResources` 태스크를 통해 생성된 클래스 파일들을 빌드하여 출력 디렉토리에 배치합니다.
   - **활용:** `compileJava`와 `processResources` 이후에 이 태스크를 실행하여 최종적인 클래스 파일을 생성합니다.
   - **예시:**
     ```bash
     gradle classes
     ```

이들은 기본적인 빌드 태스크로서, Java 소스 코드를 컴파일하고 리소스 파일을 처리하여 클래스 파일을 생성하는 중요한 단계를 수행합니다. 이 태스크들은 빌드 프로세스에서 자동으로 실행되지만, 필요에 따라 직접 명시적으로 실행할 수도 있습니다. Gradle은 이와 같은 태스크들을 조합하여 빌드 라이프사이클을 정의하고, 사용자는 필요에 따라 추가적인 동작을 정의할 수 있습니다.
## compileTestJava, processTestResources, testClasses
`compileTestJava`, `processTestResources`, 그리고 `testClasses`는 Gradle에서 테스트 관련 작업을 수행하기 위한 빌드 태스크(Task)입니다. 이들은 테스트용 Java 코드의 컴파일, 테스트 리소스 처리, 테스트 클래스 생성과 관련된 작업을 수행합니다.

1. **`compileTestJava`:**
   - **목적:** 테스트용 Java 소스 코드를 컴파일하여 바이트코드로 변환합니다.
   - **활용:** 주로 `src/test/java` 디렉토리에 있는 테스트용 소스 코드를 컴파일합니다.
   - **예시:**
     ```bash
     gradle compileTestJava
     ```

2. **`processTestResources`:**
   - **목적:** 테스트 리소스 파일(구성 파일, 이미지 등)을 처리하여 테스트 클래스와 함께 출력 디렉토리에 배치합니다.
   - **활용:** 주로 `src/test/resources` 디렉토리에 있는 테스트 리소스 파일들을 처리합니다.
   - **예시:**
     ```bash
     gradle processTestResources
     ```

3. **`testClasses`:**
   - **목적:** `compileTestJava` 및 `processTestResources` 태스크를 통해 생성된 테스트 클래스 파일들을 빌드하여 출력 디렉토리에 배치합니다.
   - **활용:** `compileTestJava`와 `processTestResources` 이후에 이 태스크를 실행하여 최종적인 테스트 클래스 파일을 생성합니다.
   - **예시:**
     ```bash
     gradle testClasses
     ```

이들은 주로 테스트 관련 작업을 수행하는 데 사용되며, 테스트 코드를 컴파일하고 테스트 리소스를 처리하여 테스트 클래스를 생성합니다. 테스트 수행에 필요한 클래스들을 준비하는 과정으로, 테스트를 실행하기 전에 반드시 이러한 작업이 수행되어야 합니다. Gradle은 이러한 테스트 관련 빌드 태스크를 제공하여 효율적이고 일관된 테스트 빌드 프로세스를 구성할 수 있도록 도와줍니다.
## test, jar, build, clean
1. **`test`:**
   - **목적:** 프로젝트의 테스트를 실행합니다.
   - **활용:** 테스트 코드를 실행하여 코드의 정확성을 검증합니다. `test` 태스크는 테스트용 클래스를 컴파일하고 JUnit 또는 TestNG와 같은 테스트 프레임워크를 사용하여 테스트를 수행합니다.
   - **예시:**
     ```bash
     gradle test
     ```

2. **`jar`:**
   - **목적:** JAR 파일을 빌드합니다.
   - **활용:** 컴파일된 클래스 파일과 리소스 파일을 패키징하여 실행 가능한 JAR 파일을 생성합니다. 이 파일은 프로젝트의 빌드된 소스 코드 및 리소스를 포함하고 있습니다.
   - **예시:**
     ```bash
     gradle jar
     ```

3. **`build`:**
   - **목적:** 프로젝트를 전체적으로 빌드합니다.
   - **활용:** `assemble`, `check`, `test` 등의 빌드 태스크를 차례로 실행하여 프로젝트를 완전한 형태로 빌드합니다. 주로 릴리스용 빌드를 수행할 때 활용됩니다.
   - **예시:**
     ```bash
     gradle build
     ```

4. **`clean`:**
   - **목적:** 빌드된 파일과 디렉토리를 제거합니다.
   - **활용:** 이전 빌드에서 생성된 빌드된 파일들을 제거하여 프로젝트를 초기 상태로 되돌립니다. `clean` 태스크를 실행한 후에는 다시 빌드해야 합니다.
   - **예시:**
     ```bash
     gradle clean
     ```

이들은 Gradle에서 자주 사용되는 기본적인 빌드 태스크들입니다. `test` 태스크는 테스트를 수행하고, `jar` 태스크는 JAR 파일을 생성하며, `build` 태스크는 전체 프로젝트를 빌드하며, `clean` 태스크는 이전 빌드에서 생성된 파일들을 제거합니다. 이들을 활용하여 프로젝트를 효과적으로 빌드하고 관리할 수 있습니다.

## 'application' 플러그인 사용법
Gradle에서 `application` 플러그인은 실행 가능한 어플리케이션을 빌드하기 위한 플러그인입니다. 이 플러그인을 사용하면 손쉽게 Java 애플리케이션을 빌드하고 실행 가능한 JAR 파일을 생성할 수 있습니다. 아래는 `application` 플러그인을 사용하는 간단한 예시입니다.

1. **build.gradle 파일에 플러그인 적용:**
   
   ```groovy
   plugins {
       id 'application'
   }

   application {
       // 실행 가능한 JAR 파일의 메인 클래스를 설정
       mainClassName = 'com.example.MyMainClass'
   }
   ```

   위 코드에서 `'com.example.MyMainClass'`는 실행 가능한 JAR 파일을 시작할 때 호출될 메인 클래스의 경로입니다. 이 경로는 프로젝트의 패키지 및 클래스 이름을 포함하여 설정합니다.

2. **빌드 스크립트에서 실행 가능한 JAR 파일 빌드:**

   ```bash
   gradle build
   ```

   위 명령을 실행하면 `build/libs` 디렉토리에 실행 가능한 JAR 파일이 생성됩니다.

3. **실행:**

   ```bash
   java -jar build/libs/your-application.jar
   ```

   위 명령으로 생성된 JAR 파일을 실행할 수 있습니다.

위 예시에서 `application` 플러그인은 실행 가능한 JAR 파일을 생성하도록 빌드 스크립트를 구성합니다. 메인 클래스는 `mainClassName` 속성을 통해 설정하며, `gradle build` 명령을 사용하여 빌드하고 `java -jar` 명령을 사용하여 생성된 JAR 파일을 실행할 수 있습니다.

더 복잡한 설정이나 사용자 정의 옵션은 `application` 플러그인의 문서를 참고하여 구성할 수 있습니다.

## run

Gradle의 `run` 태스크는 Gradle 플러그인을 사용하여 Java 애플리케이션을 직접 실행하는 데 사용됩니다. 이 태스크를 사용하면 빌드하지 않고도 프로젝트를 실행할 수 있습니다.

`run` 태스크를 사용하려면 `application` 플러그인을 사용하여 빌드 스크립트에 애플리케이션과 관련된 설정을 추가해야 합니다. 아래는 간단한 사용 예시입니다:

```groovy
plugins {
    id 'application'
}

application {
    // 실행 가능한 JAR 파일의 메인 클래스를 설정
    mainClassName = 'com.example.MyMainClass'
}
```

위 설정에서 `'com.example.MyMainClass'`는 실행 가능한 JAR 파일을 시작할 때 호출될 메인 클래스의 경로입니다.

그리고 나서 터미널에서 다음과 같이 `run` 태스크를 실행할 수 있습니다:

```bash
gradle run
```

위 명령은 `application` 플러그인에 의해 설정된 메인 클래스를 실행합니다. 이는 빌드 없이도 Gradle이 자동으로 클래스패스를 설정하고 애플리케이션을 실행할 수 있게 해줍니다.

주의: `run` 태스크는 `application` 플러그인에 의존하므로 빌드 스크립트에 플러그인이 적용되어 있어야 합니다. 또한 `run` 태스크에 대한 설정이 필요하며, `mainClassName` 등의 설정을 통해 실행될 메인 클래스를 지정합니다.

## 운영체제의 로케일 설정
###  명령어 'intl.cpl' 실행 : 시스템 로케일을 UTF-8을 인식하도록 설정
cpl은 제어판(control panel)의 기능을 가리키고 intl 은 internation이다. 해당 제어판 설정에서 시스템 로케일 설정에서 베타 기능인 `UTF-8을 기본으로 사용` 을 활성화 할 수 있다. 아닌 경우 `MS949` 문자 집합을 기본으로 사용하는 경우가 있는데, 이 경우 UTF-8과 호환이 되지 않으므로 문자 출력이 깨지게 된다.
 
## Java 문법(0:20)
## 클래스 블록과 .class 파일
1. **클래스 블록 (Class Block):**
   
   클래스 블록은 Java 프로그래밍에서 클래스의 정의를 포함하는 블록입니다. 클래스는 객체 지향 프로그래밍에서 사용되며, 클래스 블록은 해당 클래스의 멤버 변수, 메서드, 생성자 등을 중괄호 `{}`로 둘러싸인 영역에 포함합니다. 클래스 블록은 클래스의 구성을 정의하고 해당 클래스로부터 객체를 생성할 때 이 구성을 기반으로 객체를 생성합니다.

   예시:
   ```java
   public class MyClass /%클래스블록 시작%/{
       // 멤버 변수
       int myVariable;

       // 메서드
       void myMethod() {
           // 메서드 내용
       }

       // 생성자
       public MyClass() {
           // 생성자 내용
       }
   }/%클래스블록 끝%/
   ```

2. **.class 파일:**

  ***중요! `.class 파일`은 `.java 파일`과 관계 없이, 그 안에 작성된 클래스 각각에 대해서 생성된다.***
  
   .class 파일은 Java 소스 코드를 컴파일한 결과물로, 바이트코드를 포함하고 있는 이진 파일입니다. Java 컴파일러는 소스 코드를 컴파일하여 .class 파일을 생성하며, 이 파일은 Java 가상 머신 (JVM)에서 실행됩니다. .class 파일은 특정 클래스의 메타데이터, 상수 풀(Constant Pool), 메서드 코드 등을 포함하고 있습니다.

   예를 들어, 위에서 언급한 `MyClass`의 .class 파일은 다음과 같이 생성될 것입니다:

   ```
   MyClass.class
   ```

   .class 파일은 Java 프로그램이 컴파일된 이후에 생성되며, 해당 클래스를 실행하기 위해 JVM에서 읽혀집니다. Java는 플랫폼 독립적인 언어이므로, 컴파일된 .class 파일은 어떤 플랫폼에서든 JVM에 의해 실행될 수 있습니다.

```
1. **클래스 파일 생성:**
   - 각각의 자바 소스 파일은 하나 이상의 클래스를 포함할 수 있습니다. 그러나 클래스 파일은 주로 해당 소스 파일 내에 정의된 각 클래스 당 하나씩 생성됩니다.
   - 예를 들어, `MyClass.java` 소스 파일에 `MyClass`라는 클래스가 정의되어 있다면, `MyClass.class` 파일이 생성됩니다.
   - 하지만 여러 소스 파일에 걸쳐 하나의 패키지에 속한 클래스들이 정의되어 있다면, 각 클래스에 해당하는 별도의 클래스 파일이 생성됩니다.

2. **클래스 블록의 한계:**
   - 클래스 블록 내에서 정의되는 내부 클래스, 익명 클래스 등의 특수한 경우가 있을 수 있습니다. 이러한 경우에는 클래스 블록 내에서 더 많은 클래스 파일이 생성될 수 있습니다.
   - 내부 클래스는 외부 클래스 내에서 정의되는 클래스로, 별도의 .class 파일로 생성됩니다. 예를 들어, `OuterClass` 내에 정의된 `InnerClass`는 `OuterClass$InnerClass.class`와 같은 이름의 .class 파일로 생성됩니다.

3. **디렉토리 구조:**
   - 패키지가 사용된 경우 클래스 파일은 패키지의 디렉토리 구조를 따르게 됩니다. 예를 들어, `com/example/MyClass.class`와 같은 구조가 될 수 있습니다.
   - 이는 소스 파일 내에 정의된 클래스의 패키지 선언과 일치합니다.

4. **완전한 일치:**
   - 클래스 파일 이름은 소스 파일 이름과 완전히 일치하지 않을 수 있습니다. 예를 들어, 소스 파일 이름이 `MyClass.java`이더라도 클래스 파일은 `com/example/MyClass.class`와 같이 패키지 구조를 반영하는 경우가 많습니다.

따라서 클래스 파일은 자바 소스 파일과 일치하는 구조로 생성되지만, 일부 특수한 경우에는 여러 파일로 분할될 수 있고, 패키지 및 내부 클래스의 사용에 따라 디렉토리 구조가 달라질 수 있습니다.
```

# 학습 점검 목록

## Maven 표준 자바 프로젝트 디렉토리 구조를 설명할 수 있는가? 

```plaintext
src
	ㄴmain
		ㄴjava
			#주요 소스코드 저장
		ㄴresources		# 소스코드 이외 자원 파일(이미지, 구성 등)
	ㄴtest
		ㄴjava			# 테스트용 소스코드 저장
		ㄴresources		# 테스트용 자원 파일 저장
target					# 빌드 결과인 .class, .jar파일 저장
```

## Gradle 빌드 도구를 사용해서 Maven 표준 자바 프로젝트 디렉토리를 구성할 수 있는가? 

원하는 경로로 이동한 뒤 `gradle init` 명령어 실행

## Gradle 관련 디렉토리 및 파일의 역할을 설명할 수 있는가? 

## Gradle 빌드 스크립트 파일 구조에 대해 설명할 수 있는가? 

Groovy 스크립트로 설명하면, 아래와 같은 파일 구조가 있다.

1. 플러그인 설정
plugins{ ... }
2. task 각각에 대한 설정
3. 각각의 플러그인에 대한 설정
4. 외부 저장소(리포지토리)에 대한 설정
5. 의존성에 대한 설정
6. 
1. 프로젝트 기본 설정(그룹 및 버전)
2. 플러그인 적용
3. 의존성 설정
4. 빌드 태스크 설정
5. 사용자 정의 ㅌ
```groovy
/*

* This file was generated by the Gradle 'init' task.

*

* This generated file contains a sample Java application project to get you started.

* For more details on building Java & JVM projects, please refer to https://docs.gradle.org/8.4/userguide/building_java_projects.html in the Gradle documentation.

*/

  

plugins {

// Apply the application plugin to add support for building a CLI application in Java.

//실행할 수 있는 java application을 생성하는 도구이다. 이 플러그인을 추가하면 자바 플러그인이 자동으로 추가된다.

//따라서 따로 java plugin을 추가할 필요가 없다.

  

//application 플러그인 주석처리한 이유는 저 플러그인이 메인 메서드 하나 있는 단일 어플리케이션을 대상으로 한 플러그인이기 때문이다.

id 'application'

  

//이클립스 IDE 관련 작업을 수행할 수 있는 플러그인

id 'eclipse'

}

  

// 자바 소스를 컴파일 할 때 적용할 옵션

tasks.withType(JavaCompile) {

// 프로젝트의 소스 파일 인코딩을 gradle에게 알려준다.

// $javac -encoding UTF-8 ..

options.encoding =  'UTF-8'

  

// 소스 파일을 작성할 때 사용할 자바 버전

sourceCompatibility =  '17'

  

// 자바 클래스를 실행시킬 JVM의 최소 버전

targetCompatibility =  '17'

}

  

// eclipse 프로젝트 이름을 설정하기

eclipse {

project {

name =  "myapp"

}

jdt {

sourceCompatibility =  17

targetCompatibility =  17

javaRuntimeName =  "JavaSE-17"

}

}

  
  

//외부 라이브러리를 다운로드 받을 저장소에 대한 정보를 알려준다.

repositories {

// Use Maven Central for resolving dependencies.

mavenCentral()

}

  
  

dependencies {

// Use JUnit Jupiter for testing.

testImplementation 'org.junit.jupiter:junit-jupiter:5.9.3'

  

testRuntimeOnly 'org.junit.platform:junit-platform-launcher'

  

// This dependency is used by the application.

implementation 'com.google.guava:guava:32.1.1-jre'

}

  

// Apply a specific Java toolchain to ease working on different environments.

// 자바 그레이들 플러그인

java {

toolchain {

languageVersion =  JavaLanguageVersion.of(17)

}

}

  

application {

// Define the main class for the application.

mainClass =  'bitcamp.myapp.App'

}

  

//단위테스트를 어떤 도구로 실행할지 설정

tasks.named('test') {

// Use JUnit Platform for unit tests.

useJUnitPlatform()

}

  

//gradle에서 표준 입력 스트림이 키보드가 아니다. 그래서 추가한 설정이다.

run {

standardInput =  System.in

}
```


## Gradle java 플러그인이 제공하는 주요 task를 수행할 수 있는가? 
`gradle [task] [--option]`


## Gradle run 태스크를 통해 애플리케이션을 실행할 때 실행시킬 클래스를 설정할 수 있는가?

 `application` 플러그인을 사용하여 `mainClassName` 속성을 설정하면 됨.

예를 들어, 아래는 `application` 플러그인을 사용하여 `run` 태스크에서 실행될 클래스를 설정하는 예제입니다:

```groovy
plugins {
    id 'application'
}

application {
    // 실행 가능한 JAR 파일의 메인 클래스를 설정
    mainClassName = 'com.example.MyMainClass'
}

// 다른 설정 및 의존성은 여기에 추가

```

위 예제에서 `com.example.MyMainClass`는 실행 가능한 JAR 파일을 시작할 때 호출될 메인 클래스의 경로를 나타냅니다. `mainClassName`을 설정하면 `run` 태스크에서 해당 클래스를 실행할 수 있습니다.

`run` 태스크를 실행하려면 터미널에서 다음과 같이 명령어를 사용합니다:

```bash
gradle run
```

또는 Gradle 래퍼를 사용하는 경우:

```bash
./gradlew run
```

위의 설정을 사용하면 `run` 태스크가 `mainClassName`에 지정된 클래스를 실행합니다. 이 클래스는 메인 애플리케이션 클래스일 것이며, Gradle은 이를 실행하여 애플리케이션을 시작합니다.

