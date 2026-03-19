# 누가 구조를 결정하는가?

## 백엔드 개발자가 아키텍처를 결정하는 이유

- 아무리 복잡한 서비스라도 구조(architecture)를 최종 결정하는 건 1명이 할 수 밖에 없다.
- 그리고 이 1명은 보통 최종 사용자 접촉면(frontend)의 뒷쪽(backend) 출신이다.
- 사용자 접촉면(UI/UX) 개발자는 빠르게 다양한 기능을 개발해서 try & error를 반복하면서 최적안을 찾는 방식으로 진행되기 쉽기 때문이다.
- 접촉면의 뒷쪽은 빠르게 변하는 접촉면에도 불구하고 안정적으로 데이터를 유지, 관리하며 기능을 제공할 수 있어야 한다.
- 따라서 백엔드는 접촉면에 비해 높은 추상화를 달성하면서 접촉면의 필요를 이해하고 구현해야지 서비스의 제공과 유지, 보수가 가능해진다.
- 일반적인 웹서비스에서 백엔드 개발자는 사업적으로 영향이 큰 영속화 기술(DB, 트랜잭션, 데이터 정합성 등)과 비즈니스 로직에 대해 기술 수준이 높고, 접촉면의 요구사항을 파악하고 있으면서, 접촉면에서 이해할 필요도 코드를 볼 필요도 없는 종류의 기술을 사용하게 된다.
- 일반적인 웹서비스에서 프론트엔드 개발자의 기술적 영향 범위는 상대적으로 좁다. 프론트엔드의 변경은 UI 렌더링과 사용자 인터랙션에 국한되는 반면, 백엔드의 변경은 데이터 정합성, 보안, 연동된 다수의 클라이언트, 서드파티 연동 전반에 영향을 미친다.
- 결과적으로 일반적인 웹서비스에서 프론트 개발자는 백엔드 개발자를 대신하기 매우 어렵지만, 백엔드 개발자는 비교적 프론트 개발자를 쉽게 대신할 수 있다.
- 대체 불가능한 존재인 백엔드 개발자 중에서 1명이 구조를 결정하게 된다.

## 아키텍처 결정권이 백엔드로 이동해온 과정

1. 메인프레임을 사용하던 시절에는 시스템 관리자가 구조를 결정했다.
   - ~1990년대 후반
   - 이 시기에 대한 평가는 경험이 없기에 추측에 불과하다는 전제로 봐야 한다.
   - 하드웨어(서버) 자체가 가장 값비싼 자원이었다.
   - 하드웨어를 효율적으로 사용할 수 있게 하는 기술을 가진 사람이 결정권자였다.
   - 상대적으로 각각의 애플리케이션의 위상은 지금에 비교하자면 상당히 사소했다.
   - 백엔드 개발은 시스템 패키지의 개발/설치/관리, 사용자와 사용자 그룹의 정의와 관리를 뜻했다.
2. 닷컴 버블 ~ 가상서버(VMWare 등) 시기에는 미들웨어 개발자가 구조를 결정했다.
   - 1990년대 후반 ~ 2000년대 후반
   - x86 서버가 메인프레임을 대체하면서 다수의 하드웨어를 하나로 묶어주는 미들웨어가 가장 값비싼 자원이 된다.
   - 하드웨어는 비교적 손쉽게 추가하거나 뺄 수 있는 자원으로 그 위상이 낮아졌다.
   - 1대 혹은 소수의 하드웨어를 사용하는 경우 WAS(Web Application Server. IBM WebSphere, Apache Tomcat 등)를 관리할 수 있는 개발자가 구조를 결정했다.
   - 여러 대의 하드웨어를 사용하는 경우 네트워크 관리 미들웨어(ESB 등) 개발자 혹은 드물게 네트워크 하드웨어 관리자가 구조를 결정했다.
   - 양쪽 모두 네트워크나 WAS 설정이 중요하지 않은 경우나 분산 시스템 구조가 안정화된 후에는 DB 관리자가 구조를 결정 했다.
     - DBA가 결정권자인 경우, 비지니스 로직을 procedure(PL/SQL 등)로 개발하는 경우도 있었다. 이 경우 애플리케이션 개발자는 프론트엔드 개발자와 변별점이 사라진다.
3. 클라우드 서비스 시기에는 애플리케이션 개발자가 구조를 결정한다.
   - 2010년대 초반 ~ 현재
   - DB가 저장소 하드웨어를 HDD에서 SSD로 바꾸고, DB 서버를 포함해 서버가 대용량 메모리를 사용할 수 있게 되면서 SQL 튜닝의 중요도가 상대적으로 낮아졌다.
   - 하드웨어나 미들웨어 관리는 클라우드 관리 API 혹은 클라우드 CLI를 사용해서 개발자가 관리할 수 있게 되었다.
     - 하드웨어 관리자와 미들웨어 관리자는 하나로 통합되면서 인프라 관리자가 되었다.
     - "하드웨어 + 미들웨어"의 예산은 줄고 인원은 더 크게 줄었다.
     - 줄어든 인원과 예산은 애플리케이션에 투입했다.
   - WAS는 애플리케이션이 쓰는 라이브러리로 위상이 낮아졌다.
     - OS 프로세스의 관리 권한을 WAS가 가지고 있었지만, 애플리케이션이 가지도록 변했다.
     - WAS를 운영하기 위한 긴 시스템 명령어와 체크리스트를 애플리케이션의 의존성 설정 코드 몇 줄이 대체했다.
   - 네트워크 관리는 클라우드 API/CLI 코드로 대체됐다.
   - DB 쿼리는 ORM의 자동 매핑이 대체하고, 반복적인 쿼리 호출은 ORM의 엔티티 캐시(1st/2nd Lv. cache)가 완화했다.
     - 같은 테이블에 접근하는 여러 종류와 횟수의 쿼리 최적화를 기본적으로 하나의 쿼리를 최적화하고 횟수를 줄이는 방식으로 전체 성능을 끌어올렸다.
   - ORM을 효율적으로 사용하기 위해 RDB 정규화를 부분적으로 포기하는 것도 선택지가 되었다.
     - 정규화는 데이터 중복을 줄이고 정합성을 높이기 위한 원칙이지만, ORM의 지연 로딩(lazy loading)과 N+1 문제를 피하기 위해 의도적으로 비정규화하거나 조인 대신 중복 컬럼을 두는 방식이 실용적으로 사용된다.
     - 이는 DBA가 설계의 주도권을 쥐던 시기에는 허용되기 어려웠던 접근이며, 애플리케이션 개발자가 결정권을 가지게 된 변화를 보여준다.

## 다음 결정권자는 누구인가

### 상황

- 네트워크를 캡슐화 하는 기술이 늘어난다.
   1. API 개발을 공통화, 자동화 하는 기술로 주로 서버용으로 [gRPC](https://grpc.io), 클라이언트용으로 [GraphQL](https://graphql.org)가 확산중이다.
   2. GraphQL은 단순히 직렬화/역직렬화 하는 기술이 아니라, 공통의 데이터 스키마로 클라이언트가 서버에서 관리하는 데이터의 연관관계를 따라가 필요한 특정 정보에 접근하는 방법을 통제할 수 있는 방법을 제공한다.
   3. 서버/클라이언트 구조에서 네트워크를 캡슐화 하는 기술은 필연적으로 서버와 클라이언트가 공통으로 사용하는 API 정의가 필요하다.
   4. 그럼에도 불구하고 정확하게 필요한 데이터에 접근하기 위해서는 데이터의 구조를 알고 있어야 한다.
   5. 서버 개발자가 공통 API 정의 파일과 함께 클라이언트용 통신 코드를 개발하는 역할을 맡기 쉬운 환경이 조성되었다.
- GUI 수준의 멀티 플랫폼 기술이 안정화 되어간다.
  1. [Compose Multiplatform](https://kotlinlang.org/docs/multiplatform/compose-multiplatform.html), [Flutter](https://flutter.dev) 같은 기술은 상당히 안정적이고 프로덕션 수준을 달성한 걸로 보인다.
  2. 하드웨어의 성능 향상으로 눈에 보이는 것과 동작 처리 코드를 더욱 직관적으로 만들어주는 MVVM 패턴을 지원하는 GUI 프레임워크가 등장했다.
  3. 멀티플랫폼 GUI가 있다는 것은 비지니스 로직을 작성할 수 있는 멀티플랫폼 개발 언어가 존재한다는 뜻이기도 하다.
- 기업은 특성상 끊임없이 신규 고객 발굴, 고객 확장, 최적화를 시도한다.
  1. 기업 전체의 데이터 스키마는 끊임없이 확장, 변화할 수 밖에 없다.
  2. 클라이언트에 필요한 API의 스키마 또한 확장, 변화할 수 밖에 없다.
  3. 전체 데이터 스키마를 관리하는 서버 개발자가 API 스키마를 관리할 수 밖에 없다.
  4. 서버 개발자가 멀티플랫폼 언어로 API를 캡슐화 하는 모듈을 제공한다면 클라이언트의 개발 효율을 높이기 쉬워진다.
- 개발용 AI의 등장으로 UI 개발이 많이 쉬워졌다.
  1. 개발용 AI의 시연은 UI로 하는 경우가 많다.
  2. 영속화된 데이터 관리 로직은 법적 문제의 가능성도 있기 때문에 책임자의 검토가 필요하다.
  3. 잘 관리된 API 캡슐화 모듈(코드)가 있다면 AI로 UI를 입히는 것으로 서버-클라이언트 개발이 가능해졌다.

### 예상

1. 기업 전체의 데이터 스키마를 다룰 수 있는 사람이 CTO를 맡게 될 것이다.
   - 클라우드 시기 이후 인프라 관리는 코드화되고 범용화되었기 때문에, 더 이상 인프라 전문성이 CTO의 핵심 역량이 아니다.
   - 데이터 스키마는 기업의 비즈니스 모델 자체를 반영한다. 스키마를 설계하고 진화시키는 능력이 곧 사업 전략을 기술로 번역하는 능력이다.
   - AI 도구가 UI와 반복적인 코드 생성을 담당하게 될수록, 인간 개발자의 희소 가치는 "무엇을 만들지"를 구조적으로 결정하는 능력에 집중된다.

2. 전체 데이터 스키마의 서브셋이 클라이언트가 다룰 수 있는 데이터 스키마가 될 것이다.
   - 클라이언트에 전체 스키마를 노출하는 것은 보안, 성능, 결합도 측면에서 모두 문제가 된다.
   - 각 클라이언트(모바일, 웹, 파트너 API 등)는 자신이 필요한 서브셋만 접근할 수 있도록 범위가 제한된 API 계층을 갖게 된다.
   - GraphQL의 스키마 퍼미션이나 gRPC의 서비스 정의가 이 서브셋 경계를 명시적으로 표현하는 수단이 된다.

3. 기업은 최적화된 클라이언트를 찾기 위해 동시에 여러 클라이언트와 서비스를 운영하게 될 것이다.
   - AI 기반 UI 생성 도구로 인해 새로운 클라이언트 실험의 비용이 낮아진다. A/B 테스트가 페이지 단위를 넘어 클라이언트 전체 수준으로 확장된다.
   - 고객 세그먼트(연령대, 사용 기기, 구독 등급 등)에 따라 다른 UI/UX를 제공하는 것이 기술적으로 현실적인 선택지가 된다.
   - 이 모든 클라이언트가 동일한 서버측 API 라이브러리를 공유한다면, 클라이언트 다양성이 서버 복잡도 증가로 이어지지 않는다.

4. 전체 데이터 스키마를 몇 개의 도메인 그룹으로 나누고, 각 그룹에서 `GraphQL`, `gRPC` 같은 기술을 사용해 서버 구현과 클라이언트 API 모듈을 함께 제공할 수 있다.
   - 클라이언트 종류가 늘어날수록 각 클라이언트에 맞는 REST 엔드포인트를 별도로 유지하는 방식은 지속 가능하지 않다.
   - 스키마 변경 시 서버와 클라이언트 코드를 동기화하는 비용이 팀 규모와 클라이언트 수에 비례해서 커지기 때문에, API 계약(contract)을 단일 소스로 관리하고 서버/클라이언트 코드를 자동 생성하는 방식으로 전환하는 압력이 생긴다.
   - 이는 마이크로서비스의 도메인 분리와 결합되어, 각 도메인 팀이 자신의 API 스키마와 클라이언트 SDK를 함께 소유하는 구조로 이어진다.
   - `gRPC`의 protobuf 정의나 `GraphQL`의 스키마 파일이 서버-클라이언트 간 계약 문서이자 코드 생성의 원천이 된다.
   - 도메인 경계가 명확할수록 각 모듈의 독립적 배포와 버전 관리가 가능해진다.

5. 전체 데이터 스키마에 대해 충분히 잘 모듈화한 API와 라이브러리가 있다면 클라이언트 개발은 필요한 API 라이브러리 선택과 UI 구현으로 정의할 수 있다.
   - 클라이언트 개발자는 데이터 레이어를 직접 구현하지 않고 서버 팀이 제공하는 SDK를 소비하는 역할로 전환된다.
   - 비즈니스 로직의 중력이 서버측으로 더욱 이동하고, 클라이언트는 표현(presentation)에 집중하게 된다.
   - AI 도구가 UI 코드 생성을 상당 부분 담당하게 되면, 클라이언트 개발에서 인간의 역할은 "어떤 API로 어떤 UX를 만들 것인가"를 결정하는 것으로 좁아진다.

6. API 라이브러리는 네트워크 통신을 넘어 로컬 캐시, 오프라인 동작, 미디어나 기기 센서 등 기기 전용 기능을 활용하는 것까지 확장할 수도 있다.
   - ORM이 엔티티 캐시로 DB 접근 횟수를 줄이듯, API 라이브러리도 네트워크 접근 횟수와 지연을 줄이기 위한 로컬 캐시 전략을 내장하게 된다.
   - 서버의 공유 캐시(Redis 등)가 서버 내 요청을 최적화하듯, 클라이언트측 캐시가 네트워크 경계를 최적화하는 역할을 맡는다.
   - 이 구조가 성숙하면 클라이언트에서 "언제 서버를 호출할지"는 라이브러리가 판단하고 클라이언트 코드는 데이터의 의미에만 집중할 수 있게 된다.

7. 그렇게 된다면 프론트엔드는 UI/UX 영역으로 의미가 축소되고 나머지 영역(MVVM의 Model ~ 서버)이 백엔드 영역으로 확장된다.
   - 현재의 "프론트엔드 개발자"는 UI 전문가(디자인 시스템, 접근성, 애니메이션)와 데이터/상태 관리 담당자로 분화될 가능성이 있다.
   - 후자는 점점 백엔드 개발자의 역할과 중첩되면서 풀스택 혹은 API 소비자 역할로 수렴할 것이다.

8. 서버-클라이언트의 물리적 구분이 백엔드-프론트엔드의 구분이던 전통적 구분이 힘을 잃고 UI와 나머지를 뜻하는 말로 바뀔 것이다.
   - 단일 하드웨어 시스템에서 UI와 그 외 영역을 나누던 것이 프론트엔드-백엔드의 원래 의미였다. 인터넷 보급으로 이것이 클라이언트-서버의 물리적 구분으로 치환되었을 뿐이다.
   - API 라이브러리가 클라이언트 코드 안으로 깊이 들어오면, "이 코드가 어느 기기에서 실행되는가"보다 "이 코드가 사용자에게 보이는가"가 프론트엔드와 백엔드의 실질적 경계가 된다.
   - 결국 프론트엔드는 다시 원래 의미로 돌아간다: 사용자와 맞닿는 표현 계층.

---

## 참고 자료

### 용어의 기원: 컴파일러 설계

- **[Compilers: Principles, Techniques, and Tools](https://en.wikipedia.org/wiki/Compilers:_Principles,_Techniques,_and_Tools)** — Alfred V. Aho, Ravi Sethi, Jeffrey D. Ullman (Addison-Wesley, 1986)
  - 흔히 "Dragon Book"으로 불리는 컴파일러 교재. 1970년대 후반 PQCC(Production Quality Compiler-Compiler) 프로젝트에서 정립된 원칙을 바탕으로 front-end(소스 분석, 중간 표현 생성)와 back-end(목적 코드 생성)의 구분을 표준화했다. 이것이 frontend/backend라는 용어의 원형이다.
  - > The Production Quality Compiler-Compiler (PQCC), in the late 1970s, introduced the principles of compiler organization that are still widely used today (e.g., a front-end handling syntax and semantics and a back-end generating machine code).
  - > Limited memory capacity of early computers led to substantial technical challenges when the first compilers were designed, requiring the compilation process to be divided into several small programs, with front end programs producing analysis products used by back end programs to generate target code.

- **[Front end and back end - Wikipedia](https://en.wikipedia.org/wiki/Front_end_and_back_end)**
  - > Terms like 'front-end' and 'back-end' had already been used in large enterprise systems, where the front-end would be what people saw on their terminal and the back-end was basically the mainframe itself.
  - 단, 이 인용이 특정 1차 문헌에서 나온 것인지는 확인되지 않은 2차 자료다.

### 웹의 클라이언트-서버 모델 확립

- **[RFC 1945 – Hypertext Transfer Protocol HTTP/1.0](https://datatracker.ietf.org/doc/html/rfc1945)** — Tim Berners-Lee, Roy T. Fielding, Henrik Frystyk (IETF, 1996)
  - HTTP 표준에서 client와 server를 공식 정의한 문서. 웹 개발에서 frontend=브라우저(클라이언트), backend=웹 서버라는 등식이 이 시기에 굳어지기 시작했다. RFC 자체는 "frontend/backend"라는 단어를 쓰지 않고 "client/server"를 사용한다.
  - > **Client:** An application program that establishes connections for the purpose of sending requests.
  - > **Server:** An application program that accepts connections in order to service requests by sending back responses.

- **[Presentation Domain Data Layering](https://martinfowler.com/bliki/PresentationDomainDataLayering.html)** — Martin Fowler (2015)
  - 프레젠테이션-도메인-데이터의 3계층 분리를 설명하면서, 이것이 물리적 배포 계층(tier)과 다른 논리적 계층(layer)임을 강조한다. 클라이언트/서버 경계와 frontend/backend 경계가 반드시 일치하지 않음을 시사한다.
  - > These layers are **logical layers not physical tiers.** A developer can run all three layers on a laptop, run the presentation and domain model in a desktop with a database on a server, or split the presentation with a rich client in the browser and a Backend For Frontend on the server.
  - > Developers don't have to be full-stack but teams should be.

### 경계의 재정의: JAMstack과 BFF 패턴

- **[The Evolution Of Jamstack](https://www.smashingmagazine.com/2021/05/evolution-jamstack/)** — Smashing Magazine (2021)
  - 2015년 Netlify CEO Mathias Biilmann이 만든 JAMstack 개념을 설명. frontend를 UI/정적 자산 계층으로, backend를 API/데이터 계층으로 명확히 재분리했다. 인터넷 시대에 클라이언트=frontend로 뭉뚱그려진 것을 다시 UI 영역으로 좁힌 흐름이다.
  - > Unlike the large legacy apps, Jamstack projects neatly separate the frontend pages and UI from the backend apps and databases. Freed from backend servers, the frontend can then be deployed globally, directly to a CDN.
  - > Decoupling the frontend from back-end services and platforms enforces a clear contract for how your UI communicates with the rest of the system.

- **[The Back-end for Front-end Pattern (BFF)](https://philcalcado.com/2015/09/18/the_back_end_for_front_end_pattern_bff.html)** — Phil Calçado (2015, SoundCloud 경험 기반)
  - BFF 패턴은 frontend를 단순히 "브라우저"가 아닌 "특정 UI 경험(iOS, Android, Web 등)에 종속된 전체 스택"으로 개념을 확장했다.
  - > At this stage we realised that the BFF wasn't an API *used* by the application. **The BFF was part of the application.**

- **[Backends For Frontends](https://samnewman.io/patterns/architectural/bff/)** — Sam Newman
  - BFF 패턴의 아키텍처적 근거와 적용 방식을 체계적으로 정리한 문서.

- **[BFF – Backend for frontends (Technology Radar)](https://www.thoughtworks.com/en-us/radar/techniques/bff-backend-for-frontends)** — ThoughtWorks

### Frontend/Backend 구분이 안티패턴이 되는 시대

- **[Dividing frontend from backend is an antipattern](https://www.thoughtworks.com/en-us/insights/blog/dividing-frontend-backend-antipattern)** — ThoughtWorks (2018)
  - 현대 프론트엔드 개발의 복잡도가 백엔드와 동일한 수준으로 높아졌으며, frontend/backend를 직군으로 분리하는 것이 오히려 해롭다는 주장. 역설적으로, 이 주장 자체가 frontend의 기술적 영역이 넓어졌음을 전제로 한다.
  - > Frontend engineers now solve the same kinds of problems as their backend counterparts, using the same kinds of solutions, and it is harmful to continue arbitrarily dividing us.
  - > Contemporary frontend work has evolved in complexity to the extent that we should no longer separate frontend from backend roles.
  - > Testability, maintainability, persistence, asynchronicity, state, and API design [are] the daily work of client-side developers.

### 코드 수준의 경계 재정의: React Server Components

- **[Understanding React Server Components](https://vercel.com/blog/understanding-react-server-components)** — Vercel (2023)
  - React Server Components(RSC)는 코드 수준에서 `"use client"` / `"use server"` 지시어로 frontend와 backend의 경계를 브라우저/서버가 아닌 **인터랙티비티 여부**로 재정의했다.
  - > The server, far more powerful and physically closer to your data sources, deals with compute-intensive rendering and ships to the client just the interactive pieces of code.

- **[Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)** — Next.js 공식 문서 (2026)
  - > Use **Server Components** when you need: Fetch data from databases or APIs close to the source. Use API keys, tokens, and other secrets without exposing them to the client.
  - > Use **Client Components** when you need: State and event handlers... Browser-only APIs...