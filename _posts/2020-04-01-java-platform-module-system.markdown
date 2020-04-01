---
title: Java Platform Module System
layout: post
tag: [java, jigsaw, jpms]
---

접근제어
  모듈에서 특정 패키지만 공개 가능.
  인터페이스만 패키지로 묶어서 공개하고 구현은 완전히 숨길 수 있음.

## 소개

- 프로젝트 Jigsaw에서 개발.
- Java9에서 릴리스.
- 이전 버전의 자바와 구분되는 가장 큰 특징.

## 특징

- `jlink`를 사용해 커스텀 JRE 생성.
- `module-info.java`를 사용해 모듈 수준의 캡슐화.

## 커스텀 런타임

- `jlink`를 사용해 필요한 모듈만 가진 JRE 생성.
  - JRE 바이너리, 윈도우와 리눅스용 스크립트를 생성함.
- 적은 용량으로 실행 가능한 Java 애플리케이션 작성이 가능해짐.
  - Docker 같은 컨테이너 환경에 유리.
  - 런타임 : 286MB(JDK 11) => 48MB(`java.base` 모듈만)
  - 컨테이너 이미지 : 1G(`openjdk:11-jdk`) => 100MB(`alpine` + `jlink` JRE)
  - |타입|프로세서|메모리|
    |---|----|------|
    |HW|물리 CPU 1+|물리 메모리|
    |VM|물리 코어 1+|물리 메모리|
    |Container|프로세스 1|컨테이너 프로세스 메모리|

## 접근제어

- 패키지 단위로 모듈(라이브러리) 외부에서 접근 가능한 코드를 제어할 수 있음.
  - 모듈 내부처리용 코드는 비공개 패키지로 분리해서 캡슐화.
- 모듈 내부에서는 기존의 클래스 단위 접근제어.

![sample 모듈]({{ base.url }}/assets/java-platform-module-system/sample-module.png)

## 참고

- [Project Jigsaw: JDK modularization](http://openjdk.java.net/projects/jigsaw/doc/jdk-modularization.html)
- [Introduction to Project Jigsaw](https://www.baeldung.com/project-jigsaw-java-modularity)
- [Guide to jlink](https://www.baeldung.com/jlink)
- [OpenJDK11のdokcerイメージ(1GB)が大きいのでalpine linux+ jlinkで小さいイメージ(85MB)を作成する](https://qiita.com/h-r-k-matsumoto/items/294eeb838cfd062d75b6)
  - [How to create a small docker image of openjdk 11(ea) application ( 1GB→85MB )](https://qiita.com/h-r-k-matsumoto/items/1725fc587ce127671560)