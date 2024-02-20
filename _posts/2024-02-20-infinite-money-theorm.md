---
title: (Day	67)
author: 김준회
date: 2024-02-20 17:00:00 +0900
categories: [TIL, Algorithm]
tags: [TIL, Algorithm]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---
# 무한 원숭이 정리
[이 글](https://namu.wiki/w/%EB%AC%B4%ED%95%9C%20%EC%9B%90%EC%88%AD%EC%9D%B4%20%EC%A0%95%EB%A6%AC) 을 보다가 갑자기 구현해보고 싶어져서 작성했다.
무한 원숭이 정리는 보통 진화론과 생명의 발생에서 언급되는데, 원숭이가 뭔가를 이해하지 않고 계속 타자를 치면 결국에 어떤 것이든지 (의미있는 것을) 만들어 낼 수 있을 것이라는 정리다.

## 코드
```java
//infinite monkey theorm

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {

  static int trial;
  static int wordLength;

  static long callMonkey(String target, BufferedReader br, BufferedWriter bw) throws Exception {
    StringBuilder sb = new StringBuilder();
    int index = -1;
    long trialTimes = 0;
    for (int i = 0; i < trial; i++) {
      for (int j = 0; j < wordLength; j++) {
        char random = (char) (Math.random() * 26 + 'a');
        sb.append(random);
      }
      //결과만 보려면 아래 출력을 주석처리
      bw.write(i + " " + sb + "\n");
      if (sb.toString().contains(target)) {
        index = sb.toString().indexOf(target);
        trialTimes = (i - 1) * (long) wordLength + index;
        bw.write(
            "\n찾을때까지 입력한 알파벳 수: " + trialTimes
                + "\n라인의 개수: " + i
                + "\n찾아낸 단어의 시작 인덱스 :  " + index
                + "\n");
        break;
      }

      sb.setLength(0);
    }
    return trialTimes;
  }

  public static void main(String[] args) throws Exception {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    bw.write("\n각 실험당 반복 횟수: ");
    bw.flush();
    trial = Integer.parseInt(br.readLine());

    bw.write("\n한 라인의 길이: ");
    bw.flush();
    wordLength = Integer.parseInt(br.readLine());

    bw.write("\n나오길 바라는 단어(알파벳 소문자만 가능): ");
    bw.flush();
    String target = br.readLine();
    bw.write("\n실험 횟수: ");
    bw.flush();
    int population = Integer.parseInt(br.readLine());
    long[] answers = new long[population];

    for (int i = 0; i < population; i++) {
      answers[i] = callMonkey(target, br, bw);
    }

    int count = -1;
    for (int i = 0; i < population; i++) {
      if (++count == 10) {
        bw.write("\n");
        count = 0;
      }
      bw.write(String.format("%15d", answers[i]) + "|");
    }

    bw.flush();
    bw.close();
    br.close();


  }
}
```

## 설명


```
각 실험당 반복 횟수: 100

한 라인의 길이: 100

나오길 바라는 단어(알파벳 소문자만 가능): book

실험 횟수: 100

```

[100 글자를 랜덤하게 찍고 `char random = (char) (Math.random() * 26 + 'a');` 그 안에 `book` 이라는 단어가 있는지를 100번의 반복을 통해 확인하는 실험] 을  100번 진행했다.
```
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
           8108|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
```
100번의 실험 중 단 한번의 실험에서 81번째 100개의 글자에서 `book`이 발견되었다.
10000개의 길이 100인 문자열에서 book을 포함하는 문자열은 단 하나가 나왔다.
다시 동일 조건으로 해보니 100번의 실험에서 3번의 실험에서 발견되기도 했다.
```
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|           7729|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|           4494|              0|
              0|              0|              0|           2318|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
              0|              0|              0|              0|              0|              0|              0|              0|              0|              0|
```

## 느낀 점
무한 원숭이 정리가 재미있어서, 코드로 직접 구현해보고 싶었다.

무한이 참 멀게도 느껴졌다. 어떤 원하는 부분문자열이, 길이 100인 랜덤한 문자열 안에 존재할 확률은 부분문자열의 길이에 따라서 지수적으로 낮아진다.

알파벳 소문자라면, 확률은 부분문자열의 길이가 n, 랜덤문자열의 길이가 m인 경우 다음과 같다.

$(m-n-1) * 1/26^n$

앞의 m-n-1 은 부분문자열이 전체 문자열에 들어갈 수 있는 자리의 수이며, 뒤 $1/26^n$ 부분은 원하는 문자열이 만들어질 확률이다.

위에서 실험해 본 것 값을 넣어보면, 길이 100의 랜덤 문자열에 n자의 특정 단어가 있을 확률은, $(100-n) * 1/26^n$ 이다.
원하는 부분문자열이 한 글자 늘어나면 발견될 확률은 대략 1/26 배 이하로 감소한다.

정리하면, 한 랜덤한 길이 m의 문자열에서 n글자의 부분문자열이 존재하는지 L번(라인) 확인해보는 실험의라면, 그 실험에서 부분문자열이 발견될 확률은 $L*(m-n-1) * 1/26^n$ 이다.

m=100, n=6, L=100 인 경우 0.003% 정도다. n=7 이라면 0.00011% 정도가 된다. n=8인 경우 0.000004% 가 된다...
무한 원숭이 정리가 무언가 가치있는 정보를 줄까? '무한'이라는 개념은 지구에게 적용될 수 있는 개념도 아니다.
아주 작은 부분문자열은 쉽게 발견되듯이, 자기복제성을 가진 아주 작은 무언가가 먼저 발견되고, 유한한 시간 내에서 지금까지 변화해왔으리라는 생각이 든다.
'무한한 속도를 가진 컴퓨터'가 있다면 랜덤한 문자들의 배열에서 셰익스피어 전집을 얻어낼수도 있겠지만, 
그 속도가 무한하지 않다면 아마 1경년이 지나도 셰익스피어 전집은 못 만들 것이다...
알파벳을 10자라고 하고, 셰익스피어가 작성한 문서가 100자, 100페이지, 100 권이라고 한다면
그 확률은...
$$
\frac{1}{10}
$$
