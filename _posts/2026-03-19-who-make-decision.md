# 누가 구조를 결정하는가?

_**이 글은 학문적 검증을 거치지 않은 개인적 경험과 추론에 기반한 사견임.**_

## TL;DR

1. **아키텍처 결정권자의 변화** : 가장 값비싼 자원을 다루는 사람이 결정권을 쥐어왔고 가장 값비싼 자원은 계속 변화했다.
2. **서버 애플리케이션 개발자가 아키텍처 결정권자가 된 이유** : UI/UX의 사회 과학적 성격과 기술적, 사업적, 법적 환경으로 인해 서버 애플리케이션 개발자가 전체 서비스의 아키텍처 결정권자가 됐다.
3. **기업의 데이터 스키마 결정권자가 아키텍처 결정권자가 될 것이다** : API 캡슐화 기술과 개발용 AI를 활용하면서 데이터 스키마를 정의하고 발전시킬 수 있는 개발자가 클라이언트 UI/UX를 제외한 대부분의 영역을 다룰 수 있게 되면서 결정권자가 될 것이다.

## 아키텍처 결정권자의 변화

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
   - 양쪽 모두 네트워크나 WAS 설정이 중요하지 않은 경우나 분산 시스템 구조가 안정화된 후에는 DB 관리자가 구조를 결정했다.
     - DBA가 결정권자인 경우, 비즈니스 로직을 procedure(PL/SQL 등)로 개발하는 경우도 있었다. 이 경우 애플리케이션 개발자는 프론트엔드 개발자와 변별점이 사라진다.
3. 클라우드 서비스 시기에는 애플리케이션 개발자가 구조를 결정한다.
   - 2010년대 초반 ~ 현재
   - 하드웨어나 미들웨어 관리가 클라우드 API/CLI와 IaC(Infrastructure as Code)로 대체되면서 개발자가 직접 관리할 수 있게 되었다.
     - 하드웨어 관리자와 미들웨어 관리자는 하나로 통합되면서 인프라 관리자가 되었다.
     - "하드웨어 + 미들웨어"의 예산은 줄고 인원은 더 크게 줄었으며, 줄어든 자원은 애플리케이션 개발에 투입됐다.
     - Kubernetes 같은 컨테이너 오케스트레이션은 설정 코드와 시스템 서비스의 관계를 프로그래밍 언어와 실행 환경의 관계처럼 만들었다.
   - WAS는 애플리케이션이 쓰는 라이브러리로 위상이 낮아졌다.
     - OS 프로세스의 관리 권한이 WAS에서 애플리케이션으로 이동했다.
     - WAS를 운영하기 위한 긴 시스템 명령어와 체크리스트를 애플리케이션의 의존성 설정 코드 몇 줄이 대체했다.
   - ORM의 자동 매핑이 SQL 작성을 상당 부분 대체하고, 엔티티 캐시(1st/2nd Lv. cache)가 반복 쿼리를 완화하면서 SQL 튜닝 없이도 충분한 성능을 내는 경우가 많아졌다.
     - 같은 테이블에 여러 종류와 횟수로 접근하는 쿼리를, 하나의 최적화된 쿼리와 호출 횟수 감소로 대체할 수 있게 됐다.
   - ORM을 효율적으로 활용하기 위해 RDB 정규화를 부분적으로 완화하는 것도 선택지가 되었다.
     - 정규화는 데이터 중복을 줄이고 정합성을 높이기 위한 원칙이지만, ORM의 지연 로딩(lazy loading)과 N+1 문제를 피하기 위해 의도적으로 비정규화하거나 중복 컬럼을 두는 방식이 실용적으로 사용된다.
     - DBA가 설계 주도권을 쥐던 시기에는 허용되기 어려웠던 이 접근이, 애플리케이션 개발자가 결정권을 가지면서 실용적 선택지로 자리잡았다.

## 서버 애플리케이션 개발자가 결정권자가 된 이유

- 아무리 복잡한 서비스라도 아키텍처를 최종 결정하는 책임은 한 사람이 질 수밖에 없다.
- 프론트엔드 개발자는 빠른 시행착오를 반복하며 최적의 사용자 경험을 찾아가는 방식으로 일한다.
- UI/UX는 비즈니스 로직에 비해 사회과학적 성격이 강하다. 어떤 방식이 사용자에게 더 유효한지는 실제로 해보지 않으면 결론 내리기 어렵고, 결론이 나오더라도 최선이라는 보장이 없으며, 최선이라 하더라도 시간이 흘러 환경이나 맥락이 달라지면 그 가치도 달라진다.
- 백엔드는 빠르게 변하는 프론트엔드의 요구를 수용하면서도 데이터 일관성과 서비스 안정성을 유지해야 한다.
- 백엔드는 예측 불가능한 비즈니스 모델의 변화와 프론트엔드의 변화에 대응해야 하면서도 데이터 스키마의 일관성을 유지해야 하기 때문에 필연적으로 높은 추상화 수준을 갖춰야 하며, 프론트엔드의 요구사항을 이해하고 구현할 수 있어야 서비스 전반의 유지·보수가 가능하다.
- 일반적인 웹서비스에서 프론트엔드의 기술적 영향 범위는 UI 렌더링과 사용자 인터랙션에 집중되는 경향이 있는 반면, 백엔드는 영속화 기술(DB, 트랜잭션, 데이터 정합성 등)과 비즈니스 로직을 담당하며 변경 시 보안, 연동된 클라이언트, 서드파티 연동 전반에 영향을 미친다.
- 이런 특성상 일반적인 웹서비스에서 백엔드 개발자는 프론트엔드 개발자보다 시스템 전반을 파악하기 쉬운 위치에 있으며, 그 결과 아키텍처의 최종 결정권을 맡는 경우가 많다.

## 다음 결정권자는 누구인가

### 상황

- 서버-클라이언트 간 API를 정의하고 캡슐화하는 기술이 성숙하고 있다.
  1. gRPC와 GraphQL이 확산되면서 API 정의가 공통 스키마 파일로 명시화되고, 서버/클라이언트 코드를 자동 생성할 수 있게 됐다.
  2. GraphQL은 클라이언트가 서버의 데이터 연관관계를 따라가며 필요한 정보에만 접근하는 방식을 제공한다. 단순한 직렬화/역직렬화를 넘어 데이터 접근 경계를 정의하는 수단이 된다.
  3. 전체 데이터 스키마를 알고 있는 서버 개발자가 클라이언트용 통신 모듈까지 함께 제공하기 쉬운 환경이 만들어지고 있다.
- GUI 수준의 멀티플랫폼 기술이 안정화되고 있다.
  1. Flutter, Compose Multiplatform 같은 기술이 프로덕션 수준에 도달한 것으로 보인다.
  2. 멀티플랫폼 GUI가 있다는 것은 비즈니스 로직을 공유할 수 있는 멀티플랫폼 개발 언어가 존재한다는 의미이기도 하다.
- AI 기반 코드 생성 도구의 등장으로 UI 개발 비용이 낮아지고 있다.
  1. UI는 눈에 보이는 결과물이기 때문에 AI 생성 도구의 효과가 직관적으로 드러나며, 관련 도구 발전이 집중되고 있다.
  2. 반면 영속화된 데이터 관리 로직은 법적 책임이 있어서 사람이 직접 검토하고 결정해야 하는 영역으로 남는다.

### 예상

1. 각 클라이언트가 다룰 수 있는 데이터 스키마는 전체 데이터 스키마의 서브셋이 될 것이다.
   - 전체 데이터 스키마를 모든 클라이언트에 노출하는 것은 보안, 성능, 결합도 측면에서 문제가 된다.
   - 각 클라이언트(모바일, 웹, 파트너 API 등)는 자신이 필요한 서브셋만 접근할 수 있는 API 계층을 갖게 된다.
   - GraphQL의 스키마 퍼미션이나 gRPC의 서비스 정의가 이 경계를 표현하는 수단이 된다.
2. 기업은 여러 클라이언트를 동시에 운영하며 최적의 사용자 경험을 찾게 될 것이다.
   - AI 기반 UI 생성 도구로 새로운 클라이언트 실험 비용이 낮아진다. A/B 테스트가 페이지 단위를 넘어 클라이언트 전체 수준으로 확장될 수 있다.
   - 고객 세그먼트(연령대, 기기, 구독 등급 등)별로 다른 UI/UX를 제공하는 것이 기술적으로 현실적인 선택지가 된다.
   - 모든 클라이언트가 동일한 서버 API 라이브러리를 공유한다면, 클라이언트 다양성이 서버 복잡도 증가로 이어지지 않는다.
3. 전체 데이터 스키마를 도메인 그룹으로 나누고, 각 그룹에서 gRPC·GraphQL 같은 기술을 사용해 서버 구현과 클라이언트 API 모듈을 함께 제공하게 될 것이다.
   - 클라이언트 종류가 늘어날수록 각 클라이언트에 맞는 REST 엔드포인트를 별도로 유지하는 방식은 지속 가능하지 않다.
   - 스키마 변경 시 서버와 클라이언트 코드를 동기화하는 비용이 팀 규모와 클라이언트 수에 비례해서 커지므로, API 계약(contract)을 단일 소스로 관리하고 코드를 자동 생성하는 방식으로 전환하는 압력이 생긴다.
   - 마이크로서비스의 도메인 분리와 결합되어, 각 도메인 팀이 API 스키마와 클라이언트 SDK를 함께 소유하는 구조가 된다.
   - protobuf 정의나 GraphQL 스키마 파일이 서버-클라이언트 간 계약 문서이자 코드 생성의 원천이 된다.
4. 잘 모듈화된 API 라이브러리가 갖춰지면, 클라이언트 개발은 API 라이브러리 선택과 UI 구현으로 정의될 것이다.
   - 서버 개발자가 멀티플랫폼 언어로 API 캡슐화 모듈을 제공하면, 클라이언트 개발자는 데이터 레이어를 직접 구현하지 않고 이를 소비하는 역할로 전환된다.
   - 비즈니스 로직의 중심이 서버 쪽으로 더 이동하고, 클라이언트는 표현(presentation)에 집중하게 된다.
   - AI 도구가 UI 코드 생성을 상당 부분 담당하게 되면, 클라이언트 개발에서 사람의 역할은 "어떤 API로 어떤 UX를 만들 것인가"를 결정하는 데 집중된다.
5. API 라이브러리는 통신 최적화를 목적으로 로컬 캐시와 백그라운드 동기화를 담당하고, 이 과정에서 네트워크 상태·알림 등 클라이언트 플랫폼의 기능에 접근하는 형태로 확장될 수 있다.
   - ORM이 엔티티 캐시로 DB 접근을 최적화하듯, API 라이브러리도 로컬 캐시로 네트워크 접근 횟수와 지연을 줄일 수 있다.
   - 이 구조가 성숙하면 "언제 서버를 호출할지"는 라이브러리가 판단하고, 클라이언트 코드는 데이터의 의미와 표현에만 집중하게 된다.
6. 앞의 구조가 갖춰진다면, 프론트엔드와 백엔드의 경계를 물리적 위치(서버/클라이언트)가 아닌 '사용자에게 보이는 영역 여부'로 재편하는 기술적 선택이 가능해진다.
   - 인터넷 보급 이전에는 프론트엔드-백엔드가 UI와 나머지 처리 영역의 구분이었다. 웹의 등장으로 이것이 클라이언트-서버의 물리적 구분과 동일시되었다.
   - 이 경계를 다시 그을 경우, API 라이브러리 개발과 서버 개발은 같은 팀에 속하게 되고, 클라이언트 개발자의 역할은 UI/UX에 집중하는 방향으로 재편된다. 직군 구분과 팀 구성 기준도 달라진다.
   - 결과적으로 프론트엔드는 원래 의미인 "사용자와 맞닿는 표현 계층"으로, 백엔드는 "데이터 모델에서 클라이언트 API까지"를 포괄하는 영역으로 수렴한다.
7. 기업 전체의 데이터 스키마를 설계하고 진화시킬 수 있는 사람이 아키텍처 결정권자가 될 것이다.
   - 클라우드와 IaC의 보편화로 인프라 관리 전문성은 더 이상 결정권의 핵심 요소가 아니다.
   - 데이터 스키마는 기업의 비즈니스 모델 그 자체이자 핵심 조직 구조를 결정한다. 데이터 스키마를 설계하고 진화시키는 능력이 사업 전략을 기술로 번역하는 능력이 된다.

### 보충: 기업의 성장과 스키마 변화

- 기업은 끊임없이 신규 고객 발굴, 고객 확장, 최적화를 시도한다.
  1. 기업 전체의 데이터 스키마는 끊임없이 확장·변화할 수밖에 없다.
  2. 클라이언트에 필요한 API 스키마 역시 그에 따라 변화한다.
  3. 전체 데이터 스키마를 관리하는 서버 개발자가 API 스키마 변화를 주도하게 된다.

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

### 시스템 구조와 조직 구조: 콘웨이의 법칙

- **[How Do Committees Invent?](https://www.melconway.com/Home/Committees_Paper.html)** — Melvin E. Conway (Datamation, 1968)
  - 콘웨이의 법칙의 원문. "시스템을 설계하는 조직은 필연적으로 그 조직의 커뮤니케이션 구조를 복제한 시스템을 만든다"는 명제를 최초로 제시했다. 역으로 보면, 데이터 스키마와 서비스 구조의 경계가 팀 구성과 협업 방식에 압력을 가하게 된다.
  - > Organizations which design systems (in the broad sense used here) are constrained to produce designs which are copies of the communication structures of these organizations.

- **[Conway's Law](https://martinfowler.com/bliki/ConwaysLaw.html)** — Martin Fowler (2022)
  - 콘웨이의 법칙에 대한 현대적 해석. 아키텍처와 조직 구조의 상호작용, 그리고 원하는 시스템 구조에 맞게 조직을 의도적으로 재편하는 "역 콘웨이 전략(Inverse Conway Maneuver)"을 설명한다.

- **[Demystifying Conway's Law](https://www.thoughtworks.com/en-us/insights/articles/demystifying-conways-law)** — Sam Newman (ThoughtWorks, 2014)
  - 마이크로서비스 아키텍처에서 콘웨이의 법칙이 어떻게 작동하는지를 구체적인 사례로 설명한다. 도메인 경계를 기준으로 팀을 나누면 시스템 경계도 자연스럽게 정렬된다는 점을 강조한다.
