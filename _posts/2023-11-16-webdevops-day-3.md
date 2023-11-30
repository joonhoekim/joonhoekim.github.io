---
title: (Day3) - 형상관리에 관하여 (1)
author: 김준회
date: 2023-11-16 23:00:00 +0900
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


> 이 글은 제가 교육을 수강하며 기록한 내용입니다.
> **강사님과 무관하게** 잘못된 내용이 있을 수 있습니다.
{: .prompt-warning}

---


# 클라우드 기반 웹 데브옵스 프로젝트 개발자 교육 과정 (5기)

* 비트캠프 엄진영 강사님 (https://github.com/eomjinyoung/)
* 훈련기관 : 네이버클라우드주식회사
* 기간: 2023-11-14 ~ 2024-5-22
* 남은 일자 : ***126*** 일 ( 3/129 )

# 3일(2023-11-16,목)

## 버전 관리 시스템의 유형별 특징을 설명할 수 있는가?
> RCS ➡️ CVCS ➡️ DVCS 순으로 등장했다.
> Revision Control System
> Centralized Version Control System
> Distributed Version Control System

| 버전 관리 시스템 (VCS) | 설명                                                         |
|-------------------------|--------------------------------------------------------------|
| RCS                     | 제일 먼저 등장, 로컬에서 버전 관리함. 파일 단위 변경 이력 관리                 |
| CVCS                    | RCS 이후 등장. 중앙 서버에 변경이력 및 소스코드 저장하며 협업               |
| DVC                     | 중앙 서버 뿐만 아니라 로컬에도 저장소 분산, 각 사용자들이 로컬 저장소를 가져 소스코드와 변경이력 저장함.         |




## git client를 사용하여 git server로부터 저장소를 복제할 수 있는가?

```bash
$ git clone $URL
```
좀 더 자세한 내용을 알고 싶다면? https://git-scm.com/book/en/v2
![](https://git-scm.com/images/progit2.png)



## 깃 저장소의 .git 폴더와 working directory의 관계를 설명할 수 있는가?

> .git 폴더는 git이 버전관리를 위해 사용하는 폴더이다. 기본적으로는 숨겨진 상태이며, 소스 코드, 변경 이력(커밋 내역) 등을 저장하고 있다.
> .git 폴더는 working directory 내부에 생성된다.
> .git 폴더에는 index 파일이 있는데, 현재 작업중인 working directory 에서 어떤 파일을 로컬 저장소에 저장할지 리스트를 저장한다. 

.git 폴더 내부가 궁금한가?
: git의 중요한 역할 중 하나가 변경 이력의 무결성을 보장하는 것이다.
변경 이력은 삭제되어서도, 수정되어서도 안된다. 
변경 이력이 진실이라는 것이 보장되어야 한다.
그래서 git 폴더 내부를 굳이 궁금해 할 필요는 없다.
index 파일이 어떤 역할을 하는지 정도만 알면 된다.

## 깃 저장소의 working directory에 있는 파일의 변경 상태를 알아내는 명령을 실행할 수 있는가?

> ```bash
> $ git status
> $ git status --short
> $ git status --long
> ```

좋은 글...
: https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%88%98%EC%A0%95%ED%95%98%EA%B3%A0-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0

## 깃 저장소의 working directory에 있는 파일의 변경 상태 세가지를 설명할 수 있는가?

![파일의 라이프사이클.](https://git-scm.com/book/en/v2/images/lifecycle.png)

| Git 파일 상태     | 설명                                                         |
|-------------------|--------------------------------------------------------------|
| Untracked         | Git이 해당 파일을 추적하지 않음. 새로 생성된 파일이나 디렉토리   |
| Tracked/Unmodified| Git이 해당 파일을 추적하고 있으며 변경되지 않음                |
| Modified          | 추적 중인 파일이 변경되었음                                  |


## 깃 저장소의 `staging area`에 대해 설명할 수 있는가?

Local Repository로 커밋할 대상은 `git add` 명령어를 통해  `staging area`에 올라간다.

`백업을 하는 것`을 흔히 `사진을 찍어 남긴다(Take a snapshot)` 이라고 표현하는데, `백업 대상이 되는 것`을 마찬가지로 사진찍기에 비유하여 `(사진을 찍는) 무대에 올라간다` 고  하여 `staging area`에 파일이 들어간다고 표현하는 것이다.

## 깃 명령 add, commit, push, pull에 대해 설명할 수 있는가?

![깃을 위한 다이어그램](https://support.nesi.org.nz/hc/article_attachments/360004194235/Git_Diagram.svg) 

| Git 명령어 | 설명                                                |
|------------|-----------------------------------------------------|
| `git add`  | 변경된 파일을 스테이징 영역에 추가                  |
| `git commit`| 스테이징 영역에 있는 변경 사항을 로컬 저장소에 저장 |
| `git push`  | 로컬 저장소의 변경 사항을 원격 저장소에 업로드      |
| `git pull`  | 원격 저장소에서 변경된 내용을 로컬 저장소로 가져옴   |



## 깃 저장소에 커밋된 기록을 조회하고 Working Directory로 꺼낼 수 있는가?

> ```bash
> #Commit 내역 및 ID 확인
> $ git log
> 
> #Commit ID 기준으로 Working Directory에 복구
> $ git checkout $Commit_ID
> ```

해당 커밋 ID 때 백업된 내용을 Working Directory에 덮어쓴다.

자세한 내용은 ...
: https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0


Commit 이라는 용어는...
:  "Commit"은 약속이나 서약을 의미한다. 소프트웨어 개발에서는 코드 변경 사항을 저장소에 ***영구적으로 기록하고 저장하는 행위*** 를 나타낸다.


## 로컬 깃 사용자의 이름과 이메일 정보를 조회하고 설정할 수 있는가?

```bash
$ git config --global user.name "Your Name" 
$ git config --global user.email you@example.com
```
저장소별 설정
: Repository 별로 사용자의 이름과 이메일 정보를 설정해 줄 수도 있다.


## github.com에서 원격 깃 저장소에 접근할 때 사용할 토큰을 발급할 수 있는가?

> 아래의 링크에서 토큰을 발급할 수 있다. 
>  https://github.com/settings/tokens
>  
> 윈도우는 브라우저를 통해 로그인할 수 있는데, 다른 경우에는 토큰을 이용해야 한다.
> (비밀번호로 로그인 불가)




# 강의 내용

## 소프트웨어 형상관리 개념 및 주요 도구 소개(2)

## 버전 관리 시스템의 유형 별 특징

## CVS, SVN, Git 버전 관리 도구 소개

## git 클라이언트 설치 및 설정, 사용법(6)

## Windows에서 설치

## 저장소 디렉토리 구조: .git/ 폴더와 작업 폴더(working directory)

## config, status, clone, add, commit, push, pull, log, checkout 등 깃 명령 사용법