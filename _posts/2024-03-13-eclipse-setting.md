---
title: 이클립스 설정하기
author: 김준회
date: 2024-03-12 17:00:00 +0900
categories: [TIL, 비트캠프]
tags: [TIL, Web, 비트캠프, 네이버클라우드]
pin: true
math: true
mermaid: true
image:
  path: https://ih1.redbubble.net/image.373803446.4778/st,small,507x507-pad,600x600,f8f8f8.u1.jpg
  alt: TIL image
---

이클립스는 플러그인 몇 개 설치하면, 다음부터 켜는 데 1분은 걸리는 것 같다. 이 불편함을 해결해보자.

가장 중요한 건 시작할 때 플러그인을 activate 하는 것을 최소화 하는 것이다.

그 다음은 선택인데, 윈도우 기준으로 `C:\Users\USERNAME\eclipse\jee-2023-12\eclipse\eclipse.ini` 파일을 변경해준다. 이클립스가 시작할 때 차지할 램 크기, 그리고 실행 중 차지할 수 있는 최대 램 크기를 변경할 수 있다. (이클립스는 자바로 구동되는 프로그램이며, 이클립스를 구동하는 JVM의 초기/최대 힙 메모리를 변경하는 것이다.)

```
-startup
plugins/org.eclipse.equinox.launcher_1.6.600.v20231106-1826.jar
--launcher.library
C:\Users\jh\.p2\pool\plugins\org.eclipse.equinox.launcher.win32.win32.x86_64_1.2.800.v20231003-1442
-product
org.eclipse.epp.package.jee.product
-showsplash
C:\Users\jh\.p2\pool\plugins\org.eclipse.epp.package.common_4.30.0.20231201-1200
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vmargs
-Dorg.eclipse.ecf.provider.filetransfer.excludeContributors=org.eclipse.ecf.provider.filetransfer.httpclientjava
-Dosgi.requiredJavaVersion=17
-Dosgi.instance.area.default=@user.home/eclipse-workspace
-Dosgi.dataAreaRequiresExplicitInit=true
-Dorg.eclipse.swt.graphics.Resource.reportNonDisposed=true
-Declipse.e4.inject.javax.warning=false
-Dsun.java.command=Eclipse
-Xms2048m
-Xmx10240m
-XX:+UseG1GC
-XX:+UseStringDeduplication
-Xshare:on
--add-modules=ALL-SYSTEM
-Djava.security.manager=allow
-Declipse.p2.max.threads=20
-Doomph.update.url=https://download.eclipse.org/oomph/updates/milestone/latest
-Doomph.redirection.index.redirection=index:/->https://raw.githubusercontent.com/eclipse-oomph/oomph/master/setups/
```

아래 옵션도 고려할 수 있다.
```
-Xshare:on
-Xquickstart
-Xverify:none
```