---
title: Java Platform Module System
layout: post
tag: [java, jigsaw, jpms]
---

간단 소개
  Project Jigsaw에서 개발 자바9에 도입
  모듈 시스템 공개와 함께 JRE 없어짐.
    Oracle에서도 jlink를 사용해 필요한 런타임을 포함해서 빌드할 것을 추천.
특징
  커스텀 런타임
  접근 제어
커스텀 런타임
  작은 크기
  jlink를 사용해서 생성.
접근제어
  모듈에서 특정 패키지만 공개 가능.
  인터페이스만 패키지로 묶어서 공개하고 구현은 완전히 숨길 수 있음.

## 접근제어

![sample 모듈]({{ base.url }}/assets/java-platform-module-system/sample-module.png)

## 참고

- [Project Jigsaw: JDK modularization](http://openjdk.java.net/projects/jigsaw/doc/jdk-modularization.html)
- [Introduction to Project Jigsaw](https://www.baeldung.com/project-jigsaw-java-modularity)