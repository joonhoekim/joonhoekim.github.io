---
title: (Day	70)
author: 김준회
date: 2024-02-23 17:00:00 +0900
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
코드트리 문제가 점점 어려워지고 있다. 재밌다~

# Review
* HTTP Method (GET / POST)
* refresh, redirect
  * 일정 시간 뒤에 보내기, 바로 보내기
  * 뒤 내용을 보내냐 보내지 않느냐
  * 버퍼 다 차면 HTTP 응답 헤더에 refresh/redirect 적용되지 않을 수 있음. 
* forward/include

# Forward / Include 서블릿끼리 객체를 공유
Request, Response 객체를 공유한다.
setAttribute 메서드와 getAttribute 메서드로 값을 공유할 수 있다.

# 쿠키
쿠키를 이용하여 로그인 할 때 입력한 이메일을 보관하기
쿠키는 브라우저 개발자가 의도한 곳에 저장된다. 로컬에 저장된다.
서버 응답을 통해 쿠키를 줄 수 있다. 이 경우는 

## 쿠키를 주고받는 빈도
클라이언트 브라우저측에서 사이트에 접속할 때마다.

# 프로젝트에 대해서
분석/설계에 대한 비중이 Spiring framework 가 대세가 되면서 신입에게 업무 비중이 많이줄어든 감이 있음. 기존 설계에 대한 이해와, 각 구성요도에 대한 이해 및 구현능력이 신입 개발자에게 중요함.


# 팀프로젝트 절차
* 프로젝트 주제 선정
  * 현황 및 문제점 (이 프로젝트를 하게 된 이유)
  * 해결 방안 및 이점 (이 프로젝트의 장점)
  * 주요 기능 소개 (간단한 UI 프로토타입)
* 요구사항 정의
  * 유즈케이스모델
    * 액터와 유즈케이스
    * UI 프로토타이핑
* 분석 및 설계
  * DB모델
    * DDL
    * Sample Data
* 구현
  * 코드
* 발표
  * 주요기능 소개
  * 시연
  * 소감

보통 1개월 정도 프로젝트 진행. 유즈케이스: 화면을 빠트리지 않고 구현하는데 도움이 됨. 누가 어떤식으로 쓸 것인지를 정해두는 것이 유즈케이스.