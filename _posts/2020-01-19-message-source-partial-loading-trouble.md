---
title: MessageSource 부분적 로딩 문제
layout: post
tags: [intellij, spring, springmvc, i18n, cache]
---

IntelliJ IDEA에서 Spring Boot MVC를 개발하고 있었다.
다국어 지원을 위해 `application.yml`에 `spring.messages` 속성을 지정하고, 메시지 `*.properties` 파일을 만들었다.

**그런데 부분적으로 메시지를 인식하지 못하는 문제가 발생했다.**

`account.validate-fail.title` 메시지는 잘 불러와서 출력하는데,
`account.validate-fail.case1` 메시지는 읽지 못하고 `??account.validate-fail.case1_ko??`로 나오는 문제가 생겼다.

## 해결책

IntelliJ 캐시를 삭제한다.

`IntelliJ IDEA` > `File` > `Invalidate Caches / Restart...` > 클릭 `Invalidate and Restart`