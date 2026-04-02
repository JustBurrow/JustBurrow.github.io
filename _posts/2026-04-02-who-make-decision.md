---
title: 누가 아키텍처를 결정하는가?
layout: post
---

# 누가 아키텍처를 결정하는가?

미래의 아키텍트는 어떤 스킬셋을 요구할 것인가?

## 요약(TL;DR)

1. 온라인 서비스에서 아키텍트를 배출한 기술은 사용자에게서 먼 쪽에서 사용자 쪽으로 이동해왔다.
2. 하드웨어 성능 향상과 고레벨 프레임워크의 보급으로 서버 애플리케이션 개발자 출신의 아키텍트가 많은 상태이다.
3. 앞으로 아키텍트는 서버 애플리케이션과 클라이언트 모델 레이어 일부를 통합한 백엔드 개발자가 될 것이다.

## 아키텍처 결정 권력의 이동

아키텍트는 직함이 아니라 역할이다. 아무리 복잡한 서비스라도 아키텍처를 최종 결정하는 책임은 한 사람이 질 수밖에 없다. 이 글에서 말하는 아키텍트는 명함에 아키텍트라고 적힌 사람이 아니라 아키텍처를 결정할 책임과 권한을 가진 사람이다.

여기서 대상으로 삼는 서비스는 기업이 개발·운영하는 B2C 온라인 서비스다. B2B나 비영리 서비스도 본질은 크게 다르지 않다. 다만 온라인 게임은 논외다. UI/UX의 가치, 실시간 동시접속, 게임플레이 로직, 클라이언트 렌더링 같은 게임의 핵심 가치가 일반 온라인 서비스와 근본적으로 다르기 때문이다.

결정권이 이동하는 데는 공통된 패턴이 있다. 시기마다 가장 희소하고 값비싼 자원이 달랐고, 그 자원을 다루는 전문가의 ROI가 가장 높았기 때문에 결정권도 따라 이동했다. ROI가 높거나, 미션크리티컬하거나, 법적 책임이 있는 기술 스택의 전문가가 아키텍트가 됐다.

1. **~1990년대 후반: 메인프레임 시대** — 하드웨어 자체가 가장 값비싼 자원이었다. 시스템 관리자가 하드웨어를 효율적으로 사용하는 기술을 독점했고, 결정권도 가졌다. 개별 애플리케이션은 상대적으로 사소한 위상이었다. 하드웨어, 네트워크, OS, 시스템 패키지, 사용자, 사용자 그룹 관리가 중요한 기술이었다.
2. **1990년대 후반~2000년대 후반: 닷컴·x86서버 시대** — x86 서버가 메인프레임을 대체하면서 다수의 하드웨어를 묶어주는 미들웨어(WAS, ESB)가 희소 자원이 됐다. 이후 여러 서버가 하나의 DB를 공유하는 구조가 안정화되자, DB가 서비스 전체의 단일 장애점(SPOF)이 됐고 DBA의 ROI가 올라갔다. 드물게 PL/SQL 같은 프로시저로 비지니스 로직을 구현하는 경우도 있었다. OLTP, RDB 정규화, 쿼리 튜닝이 중요한 기술이었다.
3. **2010년대 초반~현재: 모바일·클라우드 시대** — 클라우드 API와 IaC(Infrastructure as Code)가 인프라 관리를 대체하고, ORM과 하드웨어 성능 향상이 SQL 최적화의 필요성을 낮췄다. 인프라와 DB 전문가의 ROI가 내려가면서 서버 애플리케이션 개발자가 결정권자로 부상했다. 서버 애플리케이션의 빠른 성장, 다수의 서버 애플리케이션 통합, 네트워크 및 인프라 비용 최적화가 중요한 기술이었다.

이 세 번의 교체는 모두 같은 방향이었다. 하드웨어 관리(사용자에게서 가장 먼 쪽)에서 미들웨어·DB를 거쳐 서버 애플리케이션(사용자에게 점차 가까운 쪽)으로 — 결정권은 항상 사용자 쪽으로 이동했다.

이 ROI 이동은 기업의 인력 구조에도 연쇄작용을 일으켰다. 결정권의 중심 기술이 교체될 때마다 기존 중심에 투입하던 예산은 크게 줄었고, 인원수는 예산보다 더 큰 폭으로 줄었다. 확보된 자금은 새로운 중심 기술 쪽에 투입돼 인원이 늘었다. 그 결과 사용자에게서 먼 쪽(전 중심 기술)의 전문가는 인원이 줄어들지만 희소해진 만큼 평균 급여는 오히려 상승했고, 가까운 쪽(새 중심 기술)은 인원은 크게 늘지만 공급이 많아지면서 급여는 상대적으로 낮아졌다.

## 현재 상황

먼저 용어 정리가 필요하다. "프론트엔드"와 "백엔드"는 시기와 환경에 따라 의미가 다르다. 컴파일러 설계에서 유래한 이 용어는 웹 개발에서 "브라우저=프론트엔드, 서버=백엔드"로 굳어졌지만, 실제로는 논리적 레이어(프론트엔드·백엔드)와 물리적 위치(클라이언트·서버)가 반드시 일치하지 않는다. 지금도 React Server Components처럼 그 경계는 계속 재정의되고 있다. 일반적으로 서비스 기술 스택에서 아키텍트를 배출하는 기술부터 사용자에게서 더 먼쪽이 백엔드, 그보다 사용자 가까운 기술이 프론트엔드가 된다.

서버 애플리케이션 개발자가 결정권자가 된 데는 두 가지 측면이 있다.

하나는 **환경의 변화**다. 서버 하드웨어 고성능화, 클라우드 인프라의 보급, 모바일 환경의 확산이 맞물리면서 인프라·미들웨어 전문성의 ROI가 낮아졌다. WAS는 애플리케이션이 쓰는 라이브러리로 위상이 낮아졌고(Embedded Tomcat, Nodejs 등), 클라우드는 하드웨어·네트워크 관리를 설정 코드 몇 줄로 대체했다. 한편 클라이언트 하드웨어의 고성능화는 클라이언트 애플리케이션 설계 패턴에도 변화를 가져왔다. MVVM은 ViewModel이 반응형 상태(observable state)를 메모리에 유지하고 뷰가 상태 변화를 지속적으로 구독하기 때문에(예: Android의 StateFlow·LiveData, iOS의 Combine Publisher), MVC나 MVP보다 메모리와 CPU 자원을 더 많이 소모한다. 스마트폰 초기에는 이 비용이 부담이었지만 클라이언트 하드웨어 성능이 충분히 올라오면서 MVVM이 기본 패턴으로 자리잡았다. 이는 이후 프론트엔드·백엔드 경계 재정의를 이해하는 데 중요한 전제가 된다.

다른 하나는 **영역의 비대칭성**이다. UI/UX는 사회과학적 성격이 강하다. 어떤 방식이 사용자에게 더 유효한지는 해보기 전엔 알 수 없고, 결론이 나와도 맥락이 바뀌면 달라진다. 빠른 시행착오가 미덕이고, 잘못된 결정의 수정 비용이 낮다. 반면 백엔드는 **데이터 스키마**·비즈니스 로직·법적 책임을 담당한다.

여기서 말하는 데이터 스키마는 DB 테이블 구조만이 아니다. 서비스가 생산하고 활용하는 모든 정보의 도식이자 논리적 설계 — API 계약, 이벤트 구조, 캐시 설계, 도메인 모델 전체를 포괄한다. 이 관점에서 데이터 스키마는 서비스의 아키텍처·라이프사이클·비즈니스 모델의 동의어이며, 기업 문화·업무 프로세스·조직 구조 결정의 핵심 요소다.

데이터 스키마 변경(마이그레이션) 비용은 높고, 잘못된 설계는 보안 사고나 법적 분쟁으로 이어진다. 이 비대칭성이 백엔드 아키텍처 결정에 더 높은 비중을 두게 만들었고, 결과적으로 서버 애플리케이션 개발자가 전체 서비스의 아키텍처를 결정하는 위치에 자연스럽게 놓이게 됐다.

## 앞으로의 변화

앞서 정의한 서비스 전체의 데이터 스키마 관리 책임과 권한이 서버 개발자 쪽으로 이동하는 것을 가능하게 하는 세 가지 기술이 동시에 성숙하고 있다.

1. **API 정의 언어와 컴파일러** (gRPC, GraphQL) — API 계약을 단일 소스로 관리하고 서버·클라이언트 코드를 자동 생성한다. 전체 데이터 스키마를 알고 있는 서버 개발자가 클라이언트용 라이브러리까지 함께 제공하기 쉬운 환경이 만들어지고 있다.
2. **멀티플랫폼 프로그래밍 언어** (Kotlin Multiplatform 등) — 서버 개발자가 작성한 코드가 iOS, Android, 데스크탑, 웹에서 동시에 실행된다. 플랫폼별 클라이언트 라이브러리를 별도로 개발하지 않아도 된다.
3. **AI 코드 생성 도구** — UI는 눈에 보이는 결과물이기 때문에 AI 생성 도구의 효과가 직관적으로 드러나며, 관련 도구 발전이 집중되고 있다. 반면 영속화된 데이터 관리 로직은 도메인별 비즈니스 규칙이 기업마다 달라 퍼블릭 AI의 범용 패턴으로 일반화하기 어렵고, 법적 책임이 수반되기 때문에 사람의 직접 검토가 필요한 영역으로 남는다.

이 세 기술이 맞물리면 프론트엔드와 백엔드의 경계가 재정의될 것이다. MVVM의 View·ViewModel은 UI 렌더링과 화면 상태 관리 — 확실한 프론트엔드 영역으로 남을 것이다. 하지만 Model의 통신 레이어, 로컬 스토리지(키-값 저장소·경량 DB 등, 예: DataStore, Room, UserDefaults, SQLite), 데이터 동기화 로직은 API 라이브러리를 통해 서버 개발자가 제공하는 백엔드 영역으로 재분류될 것이다. ORM이 DB 접근을 추상화했듯, API 라이브러리가 네트워크와 로컬 스토리지 접근을 추상화하게 된다.

이 추상화 시도가 새로운 것은 아니다. 닷컴 시기에도 CORBA·DCOM·Java RMI·SOAP/WSDL이 같은 목표를 향했다 — 네트워크 통신을 메서드 호출처럼 추상화하는 것. 하지만 당시의 시도는 플랫폼 종속성, 과도한 복잡성, 성능 문제로 대중화에 실패했다. 지금의 시도가 그때와 다른 점은 API 정의 언어의 표준화(gRPC·GraphQL), 멀티플랫폼 컴파일러, AI 코드 생성 도구가 동시에 성숙했다는 것이다.

결과적으로 프론트엔드는 사회과학적 특성이 강한 UI/UX 영역으로 축소될 것이고, 클라이언트 개발의 상당 부분이 백엔드 영역으로 흡수될 것이다. 클라이언트 개발자의 역할은 "어떤 API로 어떤 UX를 만들 것인가"를 결정하는 데 집중될 것이다.

이 구조가 성숙하면, 기업 전체의 데이터 스키마를 설계하고 진화시킬 수 있는 개발자가 클라이언트 UI/UX를 제외한 대부분의 영역을 다룰 수 있게 된다. **사업 모델을 기술로 번역하는 능력이 바로 데이터 스키마를 설계·진화시키는 능력**이고, 그 사람이 다음 아키텍트가 될 것이다.

미래의 아키텍트에게 요구되는 핵심 스킬셋은 다음과 같다.

- **데이터 스키마 설계·진화**: 전체 서비스의 도메인 모델을 정의하고, 비즈니스 변화에 맞게 스키마를 안전하게 발전시키는 능력.
- **API 계약 기술 활용(언어 + 컴파일러)**: gRPC·GraphQL 스키마 같은 API 정의 언어로 서버-클라이언트 간 계약을 명시화하고, 컴파일러로 양쪽 코드를 자동 생성한다. 이를 통해 네트워크 통신을 클라이언트 라이브러리 뒤에 캡슐화하여 클라이언트 개발자가 네트워크를 직접 다루지 않고 타입이 보장된 메서드 호출로 데이터에 접근하게 한다.
- **멀티플랫폼 라이브러리 개발**: iOS·Android·웹에서 동시에 실행되는 클라이언트 라이브러리를 작성하는 능력. 로컬 캐싱, 로깅, 백그라운드 작업, 플랫폼(OS) 기능을 캡슐화한 API 확장이나 플랫폼 독립적인 유틸리티 라이브러리를 포함한다.

## 참고 자료

### GUI 아키텍처 패턴: MVC·MVP·MVVM

- **[GUI Architectures](https://martinfowler.com/eaaDev/uiArchs.html)** — Martin Fowler (2006)
  - MVC, MVP, Presentation Model(MVVM의 전신) 등 주요 GUI 아키텍처 패턴을 비교·설명한 글. MVVM에서 ViewModel이 반응형 상태를 메모리에 유지하고 뷰가 이를 구독하는 구조의 기원을 이해하는 데 필요한 배경이다.
  - > Model-View-Presenter was created at Taligent in the early 1990s and described by Mike Potel.

- **[Guide to app architecture](https://developer.android.com/topic/architecture)** — Android Developers 공식 문서
  - Android에서 권장하는 MVVM 기반 아키텍처를 설명한다. UI 레이어(ViewModel + UI), 도메인 레이어(UseCase), 데이터 레이어(Repository)로 구성하며 데이터 레이어가 Model에 해당한다. `StateFlow`·`LiveData` 같은 observable data holder의 사용과 단방향 데이터 흐름(UDF) 패턴을 구체화한다.

### 레포지토리 패턴

- **[Repository - Catalog of Patterns of Enterprise Application Architecture](https://martinfowler.com/eaaCatalog/repository.html)** — Martin Fowler (PoEAA, 2003)
  - 도메인 레이어와 데이터 매핑 레이어 사이를 중재하는 Repository 패턴을 정의한다. 클라이언트 관점에서 데이터 접근의 구체적인 구현(네트워크, 로컬 스토리지, 캐시)을 추상화하는 인터페이스로, MVVM의 Model 경계가 서버와 클라이언트 간 계약이 되는 구조의 근거다.
  - > A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection.

### API 정의 언어와 컴파일러

- **[Protocol Buffers Documentation](https://protobuf.dev/)** — Google
  - gRPC의 인터페이스 정의 언어(IDL)이자 직렬화 포맷. `.proto` 파일로 API 계약을 정의하면 서버·클라이언트 양쪽의 코드를 자동 생성(`protoc`)할 수 있다.

- **[gRPC Documentation](https://grpc.io/docs/)** — gRPC 공식 문서
  - Protocol Buffers를 IDL로 사용하는 RPC 프레임워크. 코드 생성이 기본 내장돼 있어 서버 개발자가 정의한 서비스 명세에서 클라이언트 스텁을 자동으로 생성한다.

- **[GraphQL Specification](https://spec.graphql.org/)** — GraphQL Foundation
  - 스키마로 API 계약을 명시화하는 쿼리 언어. 스키마에서 클라이언트 코드를 자동 생성하려면 Apollo Codegen·graphql-codegen 같은 별도 도구가 필요하다(gRPC와 달리 코드 생성은 기본 내장이 아니다).

### 멀티플랫폼 프로그래밍

- **[Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html)** — Kotlin 공식 문서
  - 비즈니스 로직을 Kotlin으로 한 번 작성하고 Android, iOS, 데스크탑, 웹(Wasm/JS)에서 공유하는 기술. 공유 코드는 각 플랫폼 네이티브로 컴파일되며, UI는 각 플랫폼 프레임워크를 그대로 사용한다. 서버 개발자가 작성한 도메인 모델·Repository 구현을 클라이언트 라이브러리로 배포하는 경로를 제공한다.

### 기타

- **[Compilers: Principles, Techniques, and Tools](https://en.wikipedia.org/wiki/Compilers:_Principles,_Techniques,_and_Tools)** — Alfred V. Aho, Ravi Sethi, Jeffrey D. Ullman (Addison-Wesley, 1986)
  - 흔히 "Dragon Book"으로 불리는 컴파일러 교재. 1970년대 후반 PQCC(Production Quality Compiler-Compiler) 프로젝트에서 정립된 원칙을 바탕으로 front-end(소스 분석, 중간 표현 생성)와 back-end(목적 코드 생성)의 구분을 표준화했다. 이것이 frontend/backend라는 용어의 원형이다.
  - > The Production Quality Compiler-Compiler (PQCC), in the late 1970s, introduced the principles of compiler organization that are still widely used today (e.g., a front-end handling syntax and semantics and a back-end generating machine code).
  - > Limited memory capacity of early computers led to substantial technical challenges when the first compilers were designed, requiring the compilation process to be divided into several small programs, with front end programs producing analysis products used by back end programs to generate target code.
- **[RFC 1945 – Hypertext Transfer Protocol HTTP/1.0](https://datatracker.ietf.org/doc/html/rfc1945)** — Tim Berners-Lee, Roy T. Fielding, Henrik Frystyk (IETF, 1996)
  - HTTP 표준에서 client와 server를 공식 정의한 문서. 웹 개발에서 frontend=브라우저(클라이언트), backend=웹 서버라는 등식이 이 시기에 굳어지기 시작했다. RFC 자체는 "frontend/backend"라는 단어를 쓰지 않고 "client/server"를 사용한다.
  - > **Client:** An application program that establishes connections for the purpose of sending requests.
  - > **Server:** An application program that accepts connections in order to service requests by sending back responses.
- **[Presentation Domain Data Layering](https://martinfowler.com/bliki/PresentationDomainDataLayering.html)** — Martin Fowler (2015)
  - 프레젠테이션-도메인-데이터의 3계층 분리를 설명하면서, 이것이 물리적 배포 계층(tier)과 다른 논리적 계층(layer)임을 강조한다. 클라이언트/서버 경계와 frontend/backend 경계가 반드시 일치하지 않음을 시사한다.
  - > These layers are **logical layers not physical tiers.** A developer can run all three layers on a laptop, run the presentation and domain model in a desktop with a database on a server, or split the presentation with a rich client in the browser and a Backend For Frontend on the server.
  - > Developers don't have to be full-stack but teams should be.