---
title: Gradle implementation을 subprojects에서 쓸 때 생기는 의존성 오류
layout: post
---

프로토타이핑 하느라고 이렇게 `build.gradle`을 만들었다.

```gradle
// ... 생략 ...

dependencies {
  implementation 'org.springframework.boot:spring-boot-starter-web'
  implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'

  implementation 'org.apache.commons:commons-lang3'

  annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'

  testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

`implementation`은 `compile` 대신으로 나온 건데, 어차피 새로 만드는 프로젝트니까 Gradle 최신버전을 쓰면서 `implementation`을 사용했다.

그리고 `subproject`를 사용해서 `core`, `web`, `runner` 3개의 서브프로젝트로 분할했다.

```
.
├── LICENSE
├── README.md
├── build.gradle
├── core
│   ├── build.gradle
│   ├── out
│   └── src
├── gradle
│   └── wrapper
├── gradlew
├── gradlew.bat
├── runner
│   ├── build.gradle
│   ├── out
│   └── src
├── settings.gradle
└── web
    ├── build.gradle
    ├── out
    └── src
```

```gradle
// settings.gradle
include 'core',
    'web',
    'runner'
```
```gradle
// build.gradle
// ... 생략 ...
allprojects {
  group 'kr.lul.kobalttown'
  version '0.0.1.SNAPSHOT'
}

subprojects {
  apply plugin: 'java'
  apply plugin: 'io.spring.dependency-management'
  apply plugin: 'org.springframework.boot'

  sourceCompatibility = 11
  targetCompatibility = 11

  repositories {
    mavenCentral()
    jcenter()
  }

  dependencies {
    // ...
  }

  bootJar.enabled = false
  jar.enabled = true
}
```
```gradle
// web/build.gradle
dependencies {
  implementation project(':core')
  // ...
}
```
```gradle
// runner/build.gradle
dependencies {
  implementation project(':web')
  // ...
}
bootJar.enabled = true
```

**그런데 이렇게 하면 IntelliJ IDEA의 Gradle 리프레시는 성공하는데, 빌드(`./gradlew build`)는 실패한다.**

`runner`는 `web`의 클래스를 임포트 할 수 없고, `web`은 `core`를 임포트 할 수 없는, **의존성이 반영되지 않는 문제가 생기는 거다**.

매우 삽질을 했는데... 예전에 했던 프로젝트를 따라서 `implementation`을 `compile`로, `testImplementation`을 `testCompile`로 바꾸면 잘 된다.
Gradle 5.0/5.2는 `subprojects`에서 `implementation`을 지원하지 않는 모양이다.