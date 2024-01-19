---
title: (Day	38) Reflection API, csv, json(Gson, jackson-databind)
author: 김준회
date: 2024-01-08 23:00:00 +0900
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
# 복습
## Serialize / Deserialize
객체를 SSD, HDD 등의 스토리지에 저장하려면, 저장할 수 있는 형식으로 변경해야 한다. 그래서 일련의 바이트 배열로 변경하는 경우가 있는데, 이것을 Serialization이라고 표현한다. 그렇게 출력하고 저장한 바이트 배열을 읽어들여서 다시 인스턴스를 생성하는 과정은 Deserialization이라고 한다.
 
### serialVersionUID
 자동으로 추가되는 정적 영역의 변수이다. static final long 변수이다, 
 이것은 개발자가 명시하여 변수에 값을 할당할 수도 있고, 컴파일러가 자동으로 추가하도록 할 수도 있다. 클래스의 내용이 바뀌면 SVUID 도 바뀌게 되는데 이는 클래스에 호환되지 않는 객체를 Deserialization 하는 것을 방지하려는 목적이다. 개발자가 판단했을 때, 기존 저장된 데이터를 Deserialization 하는 것에 문제가 없다고 판단되면 값을 직접 할당하여 SVUID의 변경을 막으면 된다.

## 메서드에 제네릭 적용하기

## Byte Stream / Character Stream
![What is the Difference Between Byte Stream and Character Stream in Java -  Pediaa.Com](https://i0.wp.com/pediaa.com/wp-content/uploads/2018/12/Difference-Between-Byte-Stream-and-Character-Stream-in-Java-Comparison-Summary.jpg?resize=475%2C412)
### Byte Stream
OutputStream, InputStream을 상속받은 클래스들을 말한다.

복잡할 게 없다. 1Byte 를 읽고, 1Byte를 쓴다.

### Character Stream
Reader, Writer을 상속받은 클래스들을 말한다.

JVM이 관리하는 메모리에서 파일로 문자열을 출력한다면,
1. JVM은 항상 UTF-16 BE CharSet을 사용한다.
2. 별도 명시가 없으면 출력시 file.encoding에 설정된 CharSet을 기준으로 **변환한 다음에** 출력한다. 

파일에서 JVM이 관리하는 메모리로 문자열을 가져오려 한다면
1. 별도 명시가 없으면 입력시 file.encoding에 설정된 CharSet에 따라 작성된 것이라고 가정하여 읽어들인다.
2. 그래서 보통 UTF-8 로 작성된 것으로 가정하여 읽어들이고 **UTF-16 BE로 변환**한다. 
 
---
### 변환으로 생길 수 있는 문제
이 변환한다는 것이 중요한 게, UTF-16 BE는 한글/영어 모두 2바이트고 UTF-8은 영어 1바이트, 한글 3바이트다. 그래서 변환하면 잘못 변환한 것이다. 만약 잘못 변환하여 읽은 것을 저장한다면 (특히 덮어씌운다면) 기존 파일에 치명적인 문제를 초래할 수 있다.

---
### 바이너리 파일 VS 텍스트 파일
초반에 다뤘는데, 이진 파일은 '특정 형식으로 저장되었다' 라는 것이 특징이다. 텍스트파일은 어떤 문자집합이라는 형식에 따라 저장되었고, 이진 파일들은 다른 특정 형식으로 저장된 것이다. 


### 백준 문제 중 빠른 A+B...
보통 java.util.Scanner 클래스

## CSV 파일 형식을 다루는 방법
Comma Seperated Value
String.format() 메서드를 사용해서 `,` 로 구분된 문자열을 만들어서 파일로 출력한다.
읽어들일 때는 `String.split()` 메서드에 delmiter를 마찬가지로 `,` 로 주고 읽는다.

## Information Expert 적용 (GRASP 패턴)

## Factory Method 적용 (GoF 디자인패턴)

## Reflection API
어떤 클래스의 메서드 정보를 요청하고, 받은 정보를 통해서 그 메서드를 사용할 수 있다? 그걸 가능하게 해 주는 것이 Reflection API 이다. (참고로 java에서만 이런 것이 가능한 게 아니라 다른 언어도 이와 유사한 기능을 지원한다.)



## CSV의 문제
간단하게 출력가능하고, 원래 데이터에 구분을 위해 추가되는 바이트들도 별로 없다. (메타데이터가 크지 않다.)
1. `,` 는 잘 사용되는 문자인데 이를 delimiter를 쓰는 것이 불편하다.
2. 계층구조(Hierachy)를 표현하기 불편하다. 대부분 계층 구조를 가진 데이터를 다룰텐데 말이다.
계층 구조의 예시는 GPT에게 물어보니 설명을 너무 잘해줘서 아래에 붙인다.

### 그래서 XML이 나왔는데...
마크업으로 표현한 XML 이 나와서, 이것으로 데이터를 출력했다. 계층 구조의 표현을 좋게 하고 가독성이 좋다. 그런데 마크업으로 메타데이터를 표현하다보니 실제 데이터보다 메타데이터 양이 더 커지는 경우가 많은 문제가 생겼다.
`<태그> 데이터 </태그>` 가 항상 들어가야 하다보니...

### 그래서 메타데이터를 줄인 json이 나왔는데...
json은 키:값으로 데이터를 표현한다. 마찬가지로 데이터의 계층 구조의 표현이 가능한데, 마크업이 아니다보니 메타데이터 양이 상당히 감소한다.
참고로 json의 표현방법은 json이 만들어지면서 생긴 것이 아니라, javascript에서 객체를 리터럴을 표현하는 방법을 모방한 것이다. (똑같은 건 아니다.)
`"키": "값(데이터)"`

### GPT가 말하길...

JSON(JavaScript Object Notation)은 데이터를 표현하기 위한 경량의 데이터 교환 형식입니다. JSON은 자바스크립트 객체와 유사한 구조를 가지고 있으며, 사실 자바스크립트에서 객체를 생성하고 사용하는 데에 JSON 형식을 적극적으로 활용할 수 있습니다. 이는 JSON이 자바스크립트의 일부로서 탄생했기 때문입니다.

JSON 데이터 표현 방법과 자바스크립트의 관계를 아래에서 살펴보겠습니다:

### JSON 데이터 표현 방법:

1. **객체 (Object):** 중괄호 `{}`로 둘러싸인 키-값 쌍의 집합으로 표현됩니다. 키와 값은 콜론(`:`)으로 구분됩니다.

    ```json
    {
        "name": "John Doe",
        "age": 30,
        "isStudent": false
    }
    ```

2. **배열 (Array):** 대괄호 `[]`로 둘러싸인 값들의 순서 있는 목록으로 표현됩니다.

    ```json
    [
        "apple",
        "orange",
        "banana"
    ]
    ```

3. **문자열 (String):** 큰따옴표로 둘러싸인 문자열입니다.

    ```json
    "Hello, World!"
    ```

4. **숫자 (Number):** 정수 또는 부동 소수점 숫자입니다.

    ```json
    42
    ```

5. **불리언 (Boolean):** `true` 또는 `false`로 표현됩니다.

    ```json
    true
    ```

6. **null 값:** `null`로 표현됩니다.

    ```json
    null
    ```

### 자바스크립트와의 관계:
JavaScript는 키에 quotation을 `'`  그리고 `"` 중에서 둘 중 아무거나 써도 상관없고, 심지어 안 써도 상관 없다. 값에는 `'` 또는 `"` 하나를 쓰면 된다.
JSON에서는 전부 다 `"` 를 써서 표현해야 한다. 

아래는 GPT 설명. 

JSON 데이터는 자바스크립트의 객체 표현과 유사합니다. 따라서 자바스크립트에서는 JSON 데이터를 다루기 위한 내장 메서드를 제공합니다.

1. **JSON 파싱 (Parsing):**
   - JSON 문자열을 자바스크립트 객체로 변환하는 것을 JSON 파싱이라고 합니다.

    ```javascript
    const jsonString = '{"name": "John Doe", "age": 30, "isStudent": false}';
    const jsonObject = JSON.parse(jsonString);
    console.log(jsonObject.name); // Output: John Doe
    ```

2. **JSON 문자열화 (Stringify):**
   - 자바스크립트 객체를 JSON 문자열로 변환하는 것을 JSON 문자열화라고 합니다.

    ```javascript
    const person = { name: "John Doe", age: 30, isStudent: false };
    const jsonString = JSON.stringify(person);
    console.log(jsonString); // Output: {"name":"John Doe","age":30,"isStudent":false}
    ```

자바스크립트에서는 `JSON.parse()` 메서드로 JSON을 파싱하고, `JSON.stringify()` 메서드로 자바스크립트 객체를 JSON 문자열로 변환할 수 있습니다. 이를 통해 서로 다른 시스템 간에 데이터를 교환하고 처리하는 데 유용하게 사용됩니다.

## JSON의 단점
1. **가독성:**
   - JSON은 가독성이 떨어질 수 있습니다. 중괄호와 콤마의 사용이 많아져 복잡한 데이터 구조일수록 이해하기 어려울 수 있습니다.

2. **불필요한 구두점:**
   - JSON은 데이터를 표현하기 위해 꽤 많은 구두점을 사용합니다. 이는 작은 데이터셋에서는 큰 문제가 되지 않지만, 대규모 데이터 구조에서는 불필요한 복잡성을 가져올 수 있습니다.

3. **중첩 구조의 복잡성:**
   - 중첩된 객체나 배열의 경우, JSON은 좀 더 복잡해지며 가독성이 떨어질 수 있습니다.
   
## 그래서 YAML이 등장했는데...
JSON은 데이터를 효과적으로 표현하는 데 많은 장점이 있지만, 특정 상황에서는 몇 가지 제한이나 불편한 점이 발생할 수 있습니다. YAML(YAML Ain't Markup Language)은 JSON의 한계를 극복하고 사람이 읽고 쓰기에 더 편리한 형식을 제공합니다. 아래는 JSON의 일반적인 문제점과 YAML이 이를 해결하려고 하는 방식에 대한 간략한 설명입니다:

### YAML의 해결책:

1. **인간 친화적인 구조:**
   - YAML은 들여쓰기를 사용하여 계층 구조를 표현하므로 가독성이 향상됩니다. 이는 사람이 데이터를 읽고 작성하기 쉽게 만듭니다.

    ```yaml
    name: John Doe
    age: 30
    isStudent: false
    ```

2. **간결한 표현:**
   - YAML은 더 적은 기호를 사용하여 데이터를 표현하므로 JSON에 비해 문서의 크기가 줄어들 수 있습니다.

3. **중첩 구조의 단순화:**
   - YAML은 중첩된 데이터 구조를 표현하기에 JSON에 비해 간단하고 읽기 쉽습니다.

    ```yaml
    books:
      - title: "Introduction to Algorithms"
        author: "Thomas H. Cormen"
      - title: "Clean Code"
        author: "Robert C. Martin"
    ```

4. **데이터 주석:**
   - YAML은 주석을 사용하여 데이터에 대한 설명을 추가할 수 있습니다. JSON은 주석을 지원하지 않습니다.

    ```yaml
    # This is a YAML document with a comment
    name: John Doe
    age: 30
    ```

YAML은 주로 설정 파일이나 문서의 메타데이터와 같이 사람이 직접 읽고 수정해야 하는 경우에 사용되며, JSON은 주로 데이터 교환 형식으로 사용됩니다. 선택은 사용하려는 컨텍스트와 목적에 따라 달라집니다.

> 물론 이도 완벽한 것은 아니기에 TOML 같은 것도 나오고 YAML의 확장판도 나오고 그런다.
> 대부분 JSON 을 사용한다!

## 계층 구조의 예시
계층 구조 데이터는 데이터 요소 간에 부모-자식 관계를 가지고 있는 데이터를 나타냅니다. 이러한 형태의 데이터는 트리 구조나 그래프와 같은 형태로 표현될 수 있습니다. 여기에 몇 가지 계층 구조 데이터의 예시가 있습니다:

1. **파일 시스템:**
   - 파일 시스템은 계층 구조 데이터의 예시 중 하나입니다. 디렉터리가 폴더의 부모 역할을 하고, 그 안에 포함된 파일들이 자식 역할을 합니다.

   ```
   /
   ├── home
   │   ├── user1
   │   │   ├── document.txt
   │   │   └── image.jpg
   │   └── user2
   │       ├── notes.txt
   │       └── presentation.ppt
   ├── var
   │   ├── log
   │   │   └── system.log
   │   └── tmp
   │       ├── temp_file1.tmp
   │       └── temp_file2.tmp
   └── etc
       ├── config.ini
       └── settings.conf
   ```

2. **조직도(계층적 조직 구조):**
   - 기업이나 조직의 계층적 구조도 계층 구조 데이터의 예시입니다. 각 부서는 상위 부서의 하위 부서로서 계층적으로 구성됩니다.

   ```
   CEO
   ├── CTO
   │   ├── Engineering
   │   │   ├── Development
   │   │   └── QA
   │   └── IT Operations
   └── CFO
       ├── Finance
       └── Accounting
   ```

3. **XML 또는 JSON 데이터:**
   - XML 또는 JSON 형식은 계층 구조 데이터를 표현하기에 매우 적합합니다. 요소 간의 부모-자식 관계가 명시적으로 표현됩니다.
> 아래는 XML 데이터 에시다.
   ```xml
   <bookstore>
       <book category="fiction">
           <title>Harry Potter</title>
           <author>J.K. Rowling</author>
       </book>
       <book category="non-fiction">
           <title>Introduction to Algorithms</title>
           <author>Thomas H. Cormen</author>
       </book>
   </bookstore>
   ```
> 여기서 저자 정보에 이름, 이메일, 소개글 같은 하위 계층을 추가해서 데이터 구조를 변경하는 것도 가능하다!

4. **웹 페이지의 HTML 구조:**
   - 웹 페이지의 HTML은 계층 구조를 가지고 있습니다. 각 HTML 요소는 부모 노드와 자식 노드로 구성되어 있습니다.

   ```html
   <html>
       <head>
           <title>Example Page</title>
       </head>
       <body>
           <header>
               <h1>Welcome to our website</h1>
           </header>
           <main>
               <p>This is the main content.</p>
           </main>
           <footer>
               <p>Contact us at info@example.com</p>
           </footer>
       </body>
   </html>
   ```

이러한 예시들은 계층 구조 데이터의 다양한 형태를 나타냅니다.


# java에서 JSON을 다루려면
1. JSON 형식의 표준을 관리하는 곳은 json.org 이다.
2. 여기서 json을 다루기 위한 라이브러리들을 안내하고 있다.
정말 많은 라이브러리들이 있는데,    [google-gson](https://github.com/google/gson)

## Jackson-Databind / GSON
이 둘이 정말 많이 쓰이는 라이브러리인데, 차이점이 있다.
JSON을 생성할 때 키 이름을 둘다 Reflection API 기반으로 만드는데,
GSON은 필드명을 그대로 가져와서 쓰고
Jackson은 게터 메서드의 이름을 기준으로 만들어서 쓴다.

# Builder (GoF Design Patter)
빌더 패턴은 무엇인가?
객체를 생성하기 전에, 필요한 설정을 Builder를 통해 `필요한 옵션들`을 설정하고 그에 따라 객체를 만드는 패턴이다.

1. 일반적으로 클래스를 인스턴스화 할 때
```java
Gson gson = new Gson();
```
2. Builder 패턴을 적용한 경우
```java
GsonBuilder builder = new GsonBuilder();
builder.setDateFormat("yyyy-MM-dd");
Gson gson = builder.create();
// gson 객체가 DateFormat을 어떻게 사용할지 그 옵션을 Builder를 통해 옵션 설정함
```

Factory Method보다 더 복잡한 형태의 옵션 설정이 가능하다. 팩토리 메서드는 메서드 호출해서 그 호출된 메서드(중괄호 블록) 안에서 객체를 생성한다. 딱 블럭 하나에서 다 처리해야 한다.

여러 객체들을 조립해서 하나의 객체를 만들어야 할 때 적절하다. 객체 여러개를 조립한다는 점에서 `Builder`라는 이름이 직관적이다. 이렇게 Builder 패턴을 사용하게 되면 객체의 복잡한 생성 과정을 외부에 감출 수 있다. 

건축에서도 설계도가 있으면 건물이 뿅 나오지 않는다. 땅 파고 기초치고 철근 배근하고 배관 심고 거푸집 놓고 콘크리트 치고 미장 하고 외장 하고... 아주아주 복잡한 과정이 있다. 이 과정을 건축물을 주문한 사람이 직접 하지 않고, 시공사(빌더)에게 위임하는 것처럼... 

### GSON 에 적용된 Builder Pattern
`JsonSerializer<E>` 클래스에서 GSON에 적용된 빌더 패턴을 볼 수 있다.
GSON을 통해 JSON으로 출력할 때, 적용된 빌더 패턴을 이용하여 **타입별로 어떻게 처리할 것인지에 대한 `옵션`을** 설정할 수 있다.

아래 코드는 빌더 패턴으로 작성된 GSON의 Builder를 사용하는 예시이다.
```java
class GsonDateFormatAdapter implements JsonSerializer<Date> {
	private DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
	@Override
	public JsonElement serialize(Date src, Type typeOfSrc, JsonSerializationContext context) {
	// 객체를 JSON 문자열로 변환할 때 호출된다.
	// 그 중 Date 타입의 프로퍼티 값을 JSON 문자열로 바꿀 때 호출된다.
			System.out.println(src.getTime());
			return  new JsonPrimitive(dateFormat.format(src));
		}
	}
  
// Gson 객체를 만들어 줄 도우미 객체
	GsonBuilder builder = new GsonBuilder();
  
	// Date 타입의 프로퍼티 값을 JSON 형식의 문자열로 바꿔줄 변환기를 등록한다.
	builder.registerTypeAdapter(
		Date.class, // 원래 데이터의 타입
		new GsonDateFormatAdapter() // Date 형식의 데이터를 JSON 문자열로 바꿔줄 변환기
	);
  
	Gson gson = builder.create();
```
참고로 이렇게 별도 어댑터를 빌더에 등록하지 않으면 기본 어댑터가 적용된다.

## 객체를 포함하는 JSON
GSON 에서는 객체는 []로 포장하여 JSON으로 바꾼다.


### Spring Framework 에서 기본 프레임워크는 Jackson이다.
그래서 Jackson도 알아보자~

