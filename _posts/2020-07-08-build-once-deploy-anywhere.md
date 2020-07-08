---
title: Build Once, Deploy Anywhere
layout: post
---

## 개요

한번만 빌드하면 어떤 환경(인프라)에도 배포, 실행할 수 있도록 CI/CD 시스템을 만드는 전략.

### 특징

- 빌드 결과물은 환경 의존적인 코드를 포함하지 않는다.
- 빌드 및 배포를 애플리케이션이 담당하지 않는다.
- 다수의 저장소를 사용한다.

## 필요성

- 애플리케이션은 환경에 의존적이다. 대표적인 환경 요소로 DB 스키마가 있다.
- Git의 보급으로 서로 다른 변경을 동시에 진행하는 경우가 늘어났다. 하지만 서로 다른 변경이 리소스를 공유할 수 있다는 보장은 없다.
- 클라우드(GCP, AWS, Azure), 컨테이너(Docker), 컨테이너 클러스터(Kubernetes)의 보급으로 환경을 추가하거나 복제하기 쉬워졌다.
- Nodejs와 같은 정적 분석이 어려운 스크립트 언어가 보급되어 자동화된 테스트의 중요도가 높아졌다.
- 인프라를 소스코드로 관리하는 IaC 개념이 보급되었다.

## 실행

### 가정

- MySQL을 사용하는 Spring Boot 기반의 애플리케이션.
- 빌드 도구는 Gradle을 사용한다.
- GitHub로 버전 관리.
- CircleCI로 배포.
- `dev`, `prod` 2개 환경에 배포.

### 작업

- Spring Boot 애플리케이션 소스코드를 관리할 `app` 저장소와 환경 의존적인 소스코드를 관리할 `app-env-dev`, `app-env-prod` 저장소를 만든다.
- 테스트용 CircleCI 코드(`.circleci/config.yml`)는 `app` 저장소에 작성한다. `./gradlew test` 태스크를 실행한다.
- 배포용 CircleCI 코드는 `app-env-dev`, `app-env-prod` 저장소에 작성한다. `./gradlew bootJar` 태스크와 배포 스텝을 실행한다.
- DB 스키마(DDL) 파일은 `app` 저장소에 작성한다. 현재의 코드에 필요한 스키마를 유지하도록 관리한다.
- DB 스키마 변경은 `app` 배포에 연동할 수 있도록 `app-env-dev`, `app-env-prod`에서 관리한다.

## 영향

## 참고

- [Build Once, Deploy Anywhere!](https://www.openshift.com/blog/build-once-deploy-anywhere) : Docker 이미지가 대상이지만, 원칙면에서 Java의 JAR 파일도 다르지 않다.
    - > One of the fundamental principles of Continuous Delivery is **Build Binaries Only Once**. Subsequent deployments, testing and releases should be never attempt to build the binary artifacts again, instead reusing the already built binary. In many cases, the binary is built at each stage using the same source code, and is considered to be “the same”. But it is not necessarily the same because of different environmental configuration or other factors.
- [Build once, deploy everywhere — part 1](https://medium.com/buildit/build-once-deploy-everywhere-part-1-706d7affaf0f)
  - > In the DevOps community, there is common agreement on avoiding environment-specific builds. One aims to **build the deployment package once, for any environment**, and set configuration data at deploy time. Such configuration data can be stored in one or more locations; typical examples are launch parameters, environment variables, configuration files, distributed services like etcd, ZooKeeper or Consul.
  - [Build once, deploy everywhere — part 2](https://medium.com/buildit/build-once-deploy-everywhere-part-2-1e70df05cea5)
- [Build cloud native Build once, deploy anywhere](https://www.ibm.com/downloads/cas/KBMQLAOM) : 영업용 자료네.
- [Build Once Deploy Anywhere – Configuring the Build Side](https://hamersmithblog.wordpress.com/2016/10/14/build-once-deploy-anywhere-configuring-the-build-side) : 빌드 시스템에서 배포 대상 서버의 설정 파일을 관리하는 방식에 대해 설명.

<details>
<summary>memo</summary>
<pre>
- 빌드는 한번만 하고, 롤백도 과거 커밋으로 빌드하는 것이 아니라 보관중이던 빌드 패키지를 다시 배포한다.
- 동일한 커밋을 빌드하더라도, 빌드 시스템의 설정, 패키지 등의 차이로 결과물은 달라질 수 있다.
  - JDK 버전이 바뀔 때, 결과물이 동일하다고 보장할 수 있는가? 동일하지 않더라도 실행 결과는 동일하다고 가정할 수 있는가?
  - 빌드 사이에 Maven 저장소의 아티팩트가 변경되지 않았다고 보장할 수 있는가?
- CD(Continous Delivery)는 빌드 패키지와 설정파일을 배포한다. 빌드는 빌드 패키지가 없는 최초 배포할 때만 한다.
- 빌드의 결과물은 소스코드 + 빌드 시스템의 환경.
- `dev`,  `prod` 같은 환경만 있다가 `test`, `stg` 같은 환경이 추가되었을 경우, 대응할 수 있는가? 추가된 환경에 과거의 빌드 패키지를 배포해야 할 경우 버전 히스토리는 어떤 형상을 가지는가?
- 문제가 있을 경우, 원인이 소스코드에 있을까, 환경에 있을까 범위를 축소할 수 있어야 한다.
  - 모든 환경에서 문제가 생긴다면 소스코드의 문제.
  - 특정 환경에만 문제가 생긴다면 환경 의존적 리소스 혹은 설정의 문제.
- 클라우드 인프라의 등장으로 환경 자체를 추가하기 쉬워졌다.
  - Kubernetes 같은 컨테이너 클러스터가 보급되서 새로운 환경을 만드는 게 쉬워졌다.
- 클라우드 인프라와 함께 MSA의 보급으로 테스트의 환경 의존성이 높아졌다.
  - 특정 코드를 테스트하기 위해서는 특정 버전의 마이크로서비스가 필요하다.
- Git의 보급으로 동시적, 독립적인 개발 비용이 낮아졌다. 그 결과 환경 호환성이 없는 개발이 동시에 진행될 가능성이 높아졌다.
- Docker와 같은 경량 가상화 기술의 보급으로 많은 리소스, 외부 API 등이 있는 서버 환경을 로컬에서도 인프라를 구성할 수 있게 되었다. 로컬 인프라의 수 만큼 환경이 늘어난다.
- 이상적으로는 검증된 빌드 패키지를 모든 환경에서 사용하는 것.
  - 현실적으로는 환경 독립적인 소스코드, 리소스와 환경 의존적인 설정, 리소스를 구분할 수 있게 되는 것이다.
    - 그 결과로 개발 업무와 운영 업무에서 파악해야 할 범위를 나눌 수 있게 된다.
    - 개발자는 자신의 코드가 환경 의존적인지 환경 독립적인지 완전히 파악할 수 있는가?
- 실절적으로 환경 의존적인 부분을 외부 설정 파일로 관리할 수 있도록 개발해야 한다.
  - 예를들어 로컬 환경이나 서버환경이나 MySQL을 사용하며 데이터베이스의 이름도 같지만, DB 호스트와 유저 이름이 다를 경우에 어떻게 해야 하는가?
    - Spring Boot는 JDBC URL은 외부 설정파일(`config/application.yml`)에 정의하고 JDBC Driver 클래스 이름은 JAR 파일 내부의 `application.yml`에 정의한 후에 내외부 설정을 합쳐서 `DataSource` 빈을 만들 수 있다.
- CI/CD를 자동화할 경우, CI/CD 코드는 환경 의존적이다.
  - 배포 대상 서버의 주소.
  - 빌드에 사용할 아티팩트 저장소 위치.
  - 단위테스트가 사용하는 외부 API 주소.
  - 프록시 서버 정보.
- 환경 의존적인 배포 코드와 환경별 설정파일을 함께 관리.
  - 배포 코드
  - DB, 외부 API 정보 등.
  - 로그 포맷, 로그 레벨
- 보안 정보가 환경 의존적이라 환경 독립적인 저장소에서 분리했다면, 환경 독립적인 저장소는 공개 저장소로 만들 수 있다.
  - 공개 저장소는 인원과 관점 모두 더 폭넓은 코드 리뷰가 가능해진다.
</pre>
</details>