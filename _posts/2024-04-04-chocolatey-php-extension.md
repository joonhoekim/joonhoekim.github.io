---
title: chocolatey에서 설치한 php의 extension 활성화하기
author: 김준회
date: 2024-04-29 16:00:00 +0900
categories: [TIL]
tags: [TIL, Web, php]
pin: true
math: true
mermaid: true
image:
  path: /commons/today-i-learned.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: TIL image
---
# chocolatey mysqli php
chocolatey에서 php를 설치한 경우, mysqli 같은 extension이 꺼져 있네요.

C:\tools\php83\php.ini 파일에서 플러그인을 활성화해주시면 됩니다.

```
;extension=exif      ; Must be after mbstring as it depends on it
extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
```