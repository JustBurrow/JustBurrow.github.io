---
title: Clean Architecture
layout: post
---

## 메모

- 디자인 패턴이 아니다. 포스팅의 동심원은 레이어의 역할.
- 주로 나오는 클래스 다이어그램은 MVC 디자인 패턴을 적용한 웹 프레임워크에 클린아키텍처의 원칙을 적용한 결과.
- 책은 대충 SOLID, DRY를 어떻게 적용할지 사례를 들어 설명하는 듯. 읽다 말았다.
- ORM을 사용할 때 생기는 문제.
  - 설계는 프로덕션 코드와 테스트 코드를 모두 포함해야 한다.
  - mockup 사용의 문제.
  - 데이터, 오브젝트 수준의 정합성 문제.
- 스프링프레임워크 기준으로 설명.
- [ ] 원본 블로그의 다이어그램.
- [ ] 원본 블로그 정리
  - 설계 원칙의 목표
    1. **Independent of Frameworks.** The architecture does not depend on the existence of some library of feature laden software. This allows you to use such frameworks as tools, rather than having to cram your system into their limited constraints.
    2. **Testable.** The business rules can be tested without the UI, Database, Web Server, or any other external element.
    3. **Independent of UI.** The UI can change easily, without changing the rest of the system. A Web UI could be replaced with a console UI, for example, without changing the business rules.
    4. **Independent of Database.** You can swap out Oracle or SQL Server, for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
    5. **Independent of any external agency.** In fact your business rules simply don’t know anything at all about the outside world.
  - 레이어
    1. Entity : Enterprise business rules.
       - global : static(interface / class)
       - instance specific : object
    2. Use Cases : Application business rules.
    3. Interface Adapters
       - JPA, MVC 웹 프레임워크는 여기에 해당.
    4. Framework & Drivers
- [ ] 클린 아키텍처라고 돌아다니는 MVC 클래스 다이어그램.
- [ ] 테스트 코드를 포함하는 아키텍처.

## 참고

- [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Clean Architecture A Craftsman's Guide To Software Structure And Design](https://archive.org/details/CleanArchitecture)
- [주니어 개발자의 클린 아키텍처 맛보기](https://woowabros.github.io/tools/2019/10/02/clean-architecture-experience.html)
- [Clean Architecture는 모바일 개발을 어떻게 도와주는가? - (1) 경계선: 계층 나누기](https://medium.com/@justfaceit/clean-architecture%EB%8A%94-%EB%AA%A8%EB%B0%94%EC%9D%BC-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%84%EC%99%80%EC%A3%BC%EB%8A%94%EA%B0%80-1-%EA%B2%BD%EA%B3%84%EC%84%A0-%EA%B3%84%EC%B8%B5%EC%9D%84-%EC%A0%95%EC%9D%98%ED%95%B4%EC%A4%80%EB%8B%A4-b77496744616)