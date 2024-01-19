---
title: (Day	37) 버퍼, Serialization, UML, Stream(Byte, Character)
author: 김준회
date: 2024-01-05 23:00:00 +0900
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
## 버퍼 사용의 이유
버퍼를 쓰는 이유가 뭔가? 버퍼를 쓰는 이유는 프로그램을 더 빠르게 만들기 위함이다. 버퍼를 쓰면 왜 빨라지나? 컴퓨터는 cpu가 메모리를 다룰 때는 엄청 빠른데, 외부 저장장치에 접근하여 읽거나 쓸 때는 그에 비해서 엄청 느리다. 그래서 메모리 입출력의 횟수를 줄이기 위해서 버퍼라는 임시 저장공간을 메모리에 만들어서 우선 거기에 모으고, 저장장치에 읽거나 써야 할 때 임시 저장해둔 걸 한번에 처리하는 것이다. 

참고로 위에서는 저장장치라고 했는데, 정확히는 입출력장치라고 보는게 맞다. Scanner 보다 BufferedReader를 쓰라는 이야기도 같은 내용이다. 느린 장치에 대한 사용 횟수를 줄이는 목적이란 점에서 같다.

##  UML: Class Diagram
가장 중요한 5가지 관계.
1. Inheritance
2. Association
3. Aggregation  
4. Composition
5. Dependency

일반적으로 사용되는 UML 클래스 다이어그램 관계 유형에는 다음과 같은 것들이 있습니다:
-   `Inheritance` 또는 `Generalization`: 상속 관계를 나타냅니다.
-   `Association`: 두 클래스 간의 기본적인 연결을 나타냅니다.
-   `Aggregation`: 한 클래스가 다른 클래스를 포함할 수 있는 관계를 나타냅니다.
-   `Composition`: 한 클래스가 다른 클래스를 포함하며, 포함된 클래스의 수명이 포함하는 클래스와 함께하는 경우를 나타냅니다.
-   `Realization`: 클래스가 인터페이스를 실제로 구현하는 관계를 나타냅니다

# 학습 
## Java I/O Stream API
자바의 기본 클래스에서 입출력을 지원하기위한 스트림은 두 종류가 있다.
데이터 싱크 스트림은 가라앉은 (이미 있는) 것에서 하는 것이고
프로세싱 스트림은 데코레이터 역할을 하는, 가공하는 일을 하는 클래스다.
뒤에 리더가 붙으면 문자기반 스트림이며, 스트림이 붙으면 바이트 기반 스트림이다.

GPT3.5에 표 작성을 요청해봤다. 전달력이 아쉽지만 마크다운 표를 이렇게 쑥쑥 뽑아내다니..

**바이트 기반 스트림:**

| 스트림 종류           | 입/출력 | 기반 스트림           | 보조 스트림          | 예제                                                                                     |
| --------------------- | ------- | --------------------- | -------------------- | ---------------------------------------------------------------------------------------- |
| FileInputStream       | 입력    | FileInputStream       | 없음                 | `FileInputStream fis = new FileInputStream("파일경로");`                                 |
| FileOutputStream      | 출력    | FileOutputStream      | 없음                 | `FileOutputStream fos = new FileOutputStream("파일경로");`                               |
| BufferedInputStream   | 입력    | FileInputStream       | BufferedInputStream  | `BufferedInputStream bis = new BufferedInputStream(new FileInputStream("파일경로"));`    |
| BufferedOutputStream  | 출력    | FileOutputStream      | BufferedOutputStream | `BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("파일경로"));` |
| DataInputStream       | 입력    | FileInputStream       | DataInputStream      | `DataInputStream dis = new DataInputStream(new FileInputStream("파일경로"));`            |
| DataOutputStream      | 출력    | FileOutputStream      | DataOutputStream     | `DataOutputStream dos = new DataOutputStream(new FileOutputStream("파일경로"));`         |
| ByteArrayInputStream  | 입력    | ByteArrayInputStream  | 없음                 | `ByteArrayInputStream bais = new ByteArrayInputStream(byteArray);`                       |
| ByteArrayOutputStream | 출력    | ByteArrayOutputStream | 없음                 | `ByteArrayOutputStream baos = new ByteArrayOutputStream();`                              |

**문자 기반 스트림:**

| 스트림 종류     | 입/출력 | 기반 스트림     | 보조 스트림    | 예제                                                                  |
| --------------- | ------- | --------------- | -------------- | --------------------------------------------------------------------- |
| FileReader      | 입력    | FileReader      | 없음           | `FileReader fr = new FileReader("파일경로");`                         |
| FileWriter      | 출력    | FileWriter      | 없음           | `FileWriter fw = new FileWriter("파일경로");`                         |
| BufferedReader  | 입력    | FileReader      | BufferedReader | `BufferedReader br = new BufferedReader(new FileReader("파일경로"));` |
| BufferedWriter  | 출력    | FileWriter      | BufferedWriter | `BufferedWriter bw = new BufferedWriter(new FileWriter("파일경로"));` |
| CharArrayReader | 입력    | CharArrayReader | 없음           | `CharArrayReader car = new CharArrayReader(charArray);`               |
| CharArrayWriter | 출력    | CharArrayWriter | 없음           | `CharArrayWriter caw = new CharArrayWriter();`                        |


### 데코레이터 클래스가 무엇인지 알아야 한다.
위 표에서 보조 스트림이 데코레이터 역할을 하고 있는 것을 볼 수 있다.

## Serialization, Marshalling
Serialization, 잠시 다른 얘기를 하면.. 직렬화라고 번역되었는데, 대부분의 번역된 단어는 직관적이지도 않다. 그냥 기존에 한국어에서 쓰던 단어들의 조합이기에 뭔가 익숙해보일 뿐이다. 점화식, 연역, 귀납, 연결, 연결 리스트(ㅋㅋ리스트는 왜 번역을 안하냐)같은 것들도 그렇고.. 차라리 Interface 같이 그냥 영어를 음독하는게 훨씬 낫다. 

java에서 Serialization이 무엇인가? instance를 byte array로 변환하는 걸 말한다. 그리고 Serialization을 지원하는 기본 객체들은 Interface를 통해서 사용된다. 그 인터페이스가 Serializable Interface이다. 
> AutoClosable에서도 유사한 활용방법을 봤을 것이다. 이런 인터페이스가 없었다면 유사한 기능을 사용할 때 사용방법이 혼재되어 배우기도 어렵고 사용하기도 불편했을 것이다. 유지보수가 어려워지는 건 당연하고.

Instance를 byte[]로 변환했다고 하더라도, byte[] 를 다른 프로그래밍 언어로 읽거나 쓸 수는 없다. Compatibility가 없다. 그래서 다른 프로그래밍 언어로 만들어진 프로그램과 호환이 필요한 경우는 사용할 수 없다. 그래서 실무에서는 자주 사용되지 않는다. (json, xml 혹은 yaml을 사용한다)

### 왜 serialization은 호환성이 없나?
인스턴스를 바이트 배열로 변환하고 다시 deserialization 하려고 한다면 당연히 `format`이 있을 것이다.  그 `format`이 호환되지 않는다.




## 예외처리의 목적
예외 처리의 목적을 가장 중요한 것부터 말하면,
에러가 아닌 예외가 사용자나 개발자에 의해 발생했을 때
**프로그램이 멈추지 않고 계속 돌아가게 하기 위한 것이다.**


## 오류의 예시
초보때는 문법에서 문제가 발생할 수 있지만,
나중에 경력자들은 실행 순서의 문제에서 발생하는 문제가 많다.
그리고 그런 문제가 잡기가 오래 걸린다.


## 취준관련
솔직히 취업 어렵다고 하지만
기존 대기업 준비하던 정도로 준비하면 이 시장에서 취업에 문제가 전혀 없다.
서류(자소서)
기술면접
포트폴리오
코딩테스트

이거 준비하면 된다.
그리고 이 시장은 적절한 경력이 쌓이면 쌓일수록 (물경력은 당연 안되고)
몸값도 오르고 본인이 할 수 있는 기회의 폭도 늘어난다.

그리고 증명은
포폴과 깃 잔디, 블로그다.

## Difference between ByteStream, CharacterStream

CharcaterStream 은 문자 단위로 쓰거나 읽는다.

FileWriter.write()
: 문자 단위로 쓴다.
영어라면, 1바이트 기준으로 쓴다.
한글이라면, 3바이트를 한번에 쓴다.
쓸 때 charset 지정하여 UTF-8로 쓰는게 보통이다.

FileReader.read()
: 문자 단위로 읽는다. 
영어라면, read file by 1 byte 한다.
한글이라면, UTF-8에서 3바이트이므로 3 byte를 통째로 읽어온다. 
**근데 읽어올 때 UTF-8로 읽어오는 게 아니다.**
**JVM 내부에서는 UTF-16(BE)를 사용하므로 그 형식으로 변환된 값이 리턴된다. [UCS2]**

반대로 스트림은!
---
1. 바이트 단위로만 읽는다.
2. 읽어온 바이트에 대한 변환이 없다.

### 코드 확인
```java
// Byte Stream - 바이트 단위로 읽기 II
package com.eomcs.io.ex02;
import java.io.FileReader;

public  class Exam0130 {
	public  static  void main(String[] args) throws Exception {
	FileReader in = new FileReader("sample/utf8.txt"); 
	// 41 42 EA B0 80 EA B0 81
	
	int  ch;
	while ((ch = in.read()) != -1) {
		System.out.printf("%04X ", ch); // x 혹은 X 하면 대소문자로 바꿀 수 있다.
		// 0041 0042 AC00 AC01
		// UTF-16 BE 로 바뀌었다.
	}
		in.close();
	}
}
```

### file.encoding
그런데 위 코드를 보면, UTF-8 데이터를 읽는다고 알려주는 내용이 없다. 기본값이 UTF-8 일까? 정확하게 알아보면, JVM 환경변수인 file.encoding 에 설정된 문자집합이 기본값이 된다. 

### JVM 은 왜 UTF-16 BE를 쓸까?
UTF-8 로 전환이 필요한지에 대한 논의가 있었다고 한다. 영어를 2바이트에서 1바이트만 써도 되는게 장점이 된다. 그리고 대부분의 시스템에서 사용하는 문자집합이기도 하다. 그런데 UTF-16에도 장점이 있다. 일단 이 문자집합의 특성상 문자의 정렬이 쉽다. 그리고 모든 문자를 2바이트로 출력하는 것도 장점이다. UTF-8은 자주 사용되지 않는, (혹은 파워가 약한) 문자들을 3바이트 혹은 4바이트를 써서 표현해야 한다. 그만큼 정렬이나 다른 문자열 기능 구현이 어렵고, 영어를 제외한 경우 성능에도 손실이 있을 수 있다.

이런 내용을 자세히 작성해보신 분이 계신다. https://lordofkangs.tistory.com/86

### BE는 무엇일까?
Big Endian 이다.

## FileReader, FileWriter 클래스 사용하기
문자열 다룰 때 훨씬 편하다!

## 36. 리팩토링 => 제네릭 활용하여 중복코드 제거

각각의 저장소별로 load, save 하는 메서드를 정의했으나 그렇게 하는 것은 중복된 코드를 만들어낸다. 리팩토링 하자!

저장소별로 사용하는 사용자 정의 데이터타입이 다르므로, save, load를 위해서는 제네릭을 활용하는 게 좋다. 

제네릭을 이용하여 호출할 때의 타입을 이용해서 타입을 결정하자.
아래 코드에서, 메서드의 앞에 작성된 <E>는 호출할 때 결정된다. (E=Element)

그리고, 아래 ?는 List에 어떤 타입의 값이 저장되어 있는지 상관 없다는 뜻이다.
아주 직관적이다. 

```java
<E> List<E> loadData(String filepath, Class<E> clazz) {  
    try (ObjectInputStream in = new ObjectInputStream(  
        new BufferedInputStream(new FileInputStream(filepath)))) {  
  
      //키워드라서 class 못써서 clazz clz cla cls c 이런식으로 쓴다.  
  return (List<E>) in.readObject();  
  
  
    } catch (Exception e) {  
      System.out.printf("%s 파일 로딩 중 오류 발생!\n", filepath);  
      e.printStackTrace();  
    }  
    return new ArrayList<E>(); //여기서 ArrayList, LinkedList 고정해버리는 것은 좋지 않다!!  
  }  
  
//  <E> void loadData(String filepath, List<E> dataList) {  
//    try (ObjectInputStream in = new ObjectInputStream(  
//        new BufferedInputStream(new FileInputStream(filepath)))) {  
//  
//      List<E> list = (List<E>) in.readObject();  
//      dataList.addAll(list);  
//  
//    } catch (Exception e) {  
//      System.out.printf("%s 파일 로딩 중 오류 발생!\n", filepath);  
//      e.printStackTrace();  
//    }  
//  }
```

## SerialVersionUID
Serial Version UID 가 무엇일까?
클래스의 설계도대로 메모리에 인스턴스를 만들었다고 하자. 그리고 이 인스턴스는 레퍼런스 변수가 그 주소를 가지고 있다.
이를 바이트 배열로 만드는 것이 Serialization 이다. 이 바이트들은 클래스 정보, 인스턴스 값, **그리고 버전이 기록된다.** 버전 정보에는 클래스 정보가 있다. 인스턴스 값, 그리고 버전 정보를 가진다.

Serialize 과정을 거쳐 만들어진 byte[]들로 instance를 만드려고 하면 그냥 무조건 만들 수 있는 게 아니다. 클래스 정보에 기록된, 그 클래스로 만드려는 것인지 검사를 한다. 이것을 Serial Version UID 라는 static field 에 기록된 long 값으로 확인한다.

인스턴스의 메모리 주소와는 다르게, serialVersionUID 필드는 개발자가 직접 추가할 수도 있다. 개발자가 추가하지 않으면 컴파일러가 자동으로 추가한다.
개발자가 필드를 추가해도 값을 지정하지 않으면 컴파일러가 필드와 메서드 정보에 기반하여 값을 자동으로 넣는다.

단 위 내용들에는 조건이 하나 있다. java.io.Serializable 클래스에 자동 추가되는 스태틱 필드다. 당연히 Serializable Interface를 구현하는 클래스여야 SerialVersionUID 필드를 의미있게 사용할 것이다. Deserialize 과정에서 의미가 있는 필드이기 때문이다.

### SerialVersionUID의 목적은? 
출력된 데이터(Output)의 유효성을 검사하는 것이 목적이다. (그 중에서도 Serialization을 통해서 만들어진 output)

이상한 걸 Deserialization을 하여 문제가 발생하는 것을 막기 위해 확인할 수 있는 장치를 추가한 것이며 ,이것이 없었다면 유효하지 않은 데이터를 Deserialization하여 발생하는 문제를 알아채기까지 더 많은 시간이 걸릴 것이다.

### 그럼 deserialize 에서 오류 나는 때는 언제인가? *불일치 문제
(serialize한 인스턴스를 만든) 클래스에 변화(필드/메서드 수정 등)가 생기면 컴파일 될 때 SerialVersionUID도 변한다. 수정 -> 컴파일 -> 이후 deserialize 하면 오류가 생긴다.

### SerialVersionUID 불일치 문제를 해결하는 방법은?
인스턴스 주소와 다르게 SerialVersionUID는 그 값을 직접 확인도 가능하고 변경도 가능하다. 
 
1. 개발자가 직접 SerialVersionUID를 지정해준다.
필드나 메서드가 변경되더라도개발자가 판단하기에 기존 방식으로 읽어도 상관없다면 SerialVersioUID를 그대로 유지하는 것이다.
이 경우 명시적으로 serialVersionUID 변수를 명시적으로 추가해줘야 한다.

### 잘 안쓴다는데 인터페이스 구현하고 serialVersionUID 필드도 있어요
잘 안쓰지만 Serialize 하게 될 경우가 있을 수 있기에 코드를 구현해두고 사용하지 않는 경우가 많다.


### 약어 
UID = Unique IDentification 이다.

# 자습 계획
1. [x] 이것이 자바다 - 컬렉션 (640-692)
2. [x] 헤드퍼스트 디자인패턴 - 데코레이터 패턴 (114-142)
3. [x] 백준 3문제
4. [x] 코드트리 125xp

