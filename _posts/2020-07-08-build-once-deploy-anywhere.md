---
title: Build Once, Deploy Anywhere
layout: post
---

- 빌드는 한번만 하고, 롤백도 과거 커밋으로 빌드하는 것이 아니라 보관중이던 빌드 패키지를 다시 배포한다.
- 동일한 커밋을 빌드하더라도, 빌드 시스템의 설정, 패키지 등의 차이로 결과물은 달라질 수 있다.
  - JDK 버전이 바뀔 때, 결과물이 동일하다고 보장할 수 있는가? 동일하지 않더라도 실행 결과는 동일하다고 가정할 수 있는가?
  - 빌드 사이에 Maven 저장소의 아티팩트가 변경되지 않았다고 보장할 수 있는가?
- CD(Continous Delivery)는 빌드 패키지와 설정파일을 배포한다. 빌드는 빌드 패키지가 없는 최초 배포할 때만 한다.
- 빌드의 결과물은 소스코드 + 빌드 시스템의 환경.
- `dev`,  `prod` 같은 정해진 환경만 있다가 `test`, `stg` 같은 환경이 추가되었을 경우, 대응할 수 있는가? 추가된 환경에 과거의 빌드 패키지를 배포해야 할 경우 버전 히스토리는 어떤 형상을 가지는가?
- 부분적인 설정 외부화가 구현에 필요한 기능.
- 문제가 있을 경우, 원인이 소스코드에 있을까, 환경에 있을까 범위를 축소할 수 있어야 한다.
  - 모든 환경에서 문제가 생긴다면 소스코드의 문제.
  - 특정 환경에만 문제가 생긴다면 환경 의존적 리소스 혹은 설정의 문제.
- 클라우드 인프라의 등장으로 환경 자체를 추가하기 쉬워졌다.
- Git의 보급으로 동시적, 독립적인 개발 비용이 낮아졌다. 그 결과 환경 호환성이 없는 개발이 동시에 진행될 가능성이 높아졌다.
- Docker와 같은 경량 가상화 기술의 보급으로 많은 리소스, 외부 API 등이 있는 서버 환경을 로컬에서도 인프라를 구성할 수 있게 되었다. 로컬 인프라의 수 만큼 환경이 늘어난다.

## 참고

- [Build Once, Deploy Anywhere!](https://www.openshift.com/blog/build-once-deploy-anywhere) : Docker 이미지가 대상이지만, 원칙면에서 Java의 JAR 파일도 다르지 않다.
    - > One of the fundamental principles of Continuous Delivery is **Build Binaries Only Once**. Subsequent deployments, testing and releases should be never attempt to build the binary artifacts again, instead reusing the already built binary. In many cases, the binary is built at each stage using the same source code, and is considered to be “the same”. But it is not necessarily the same because of different environmental configuration or other factors.
- [Build once, deploy everywhere — part 1](https://medium.com/buildit/build-once-deploy-everywhere-part-1-706d7affaf0f)
  - > In the DevOps community, there is common agreement on avoiding environment-specific builds. One aims to **build the deployment package once, for any environment**, and set configuration data at deploy time. Such configuration data can be stored in one or more locations; typical examples are launch parameters, environment variables, configuration files, distributed services like etcd, ZooKeeper or Consul.
  - [Build once, deploy everywhere — part 2](https://medium.com/buildit/build-once-deploy-everywhere-part-2-1e70df05cea5)
- [Build cloud native Build once, deploy anywhere](https://www.ibm.com/downloads/cas/KBMQLAOM) :