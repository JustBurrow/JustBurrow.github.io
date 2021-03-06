---
title: 애플리케이션 UI/UX 디자인 순서
layout: post
---

모듈을 잘게 쪼개서 만들던 앱이 있다.
그런데 모듈이 너무 많아져서 모듈 사이의 관계와 기능을 파악하기가 어려워졌다.
그래서 큰 덩어리만 나눠서 새로 만들기로 했다.

겸사겸사 와이어프레임부터 차근차근 만들어보기로 했다.

1. 서비스 핵심 가치 정의.
2. 핵심 가치를 알 수 있도록 가상의 FAQ 작성.
3. 필요한 기능을 정의.
4. 유저 저니 맵 / 사이트 맵 정의.
5. 제목만 있는 빈 HTML 페이지 작성.
6. 각 페이지에 표시할 데이터, 제공할 기능을 정의. (로우 피델리티 와이어프레임)
7. 와이어프레임 정밀도 향상.
    - 실제 서비스와 유사한 레이아웃, 링크, 폼 추가해서 Thymeleaf 템플릿화 할 수 있는 수준으로 작성. (하이 피델리티 와이어프레임)
8. 와이어프레임에서 사라진 표시할 데이터, 제공할 기능을 프론트 개발문서로 작성.
    - 와이어프레임을 `<iframe>` 태그로 표시하는 정적 웹 문서로 작성.
9. 서비스 핵심 가치애 맞는 용어로 와이어프레임 정리.
    - 이 정도면 와이어프레임이 아니라 공돌이 냄새나는 디자인 파일이다.

대략 이정도 하는데 3일(10시간 정도) 걸렸다.

1 ~ 4는 필기 앱으로 그리면서 하고 나머지는 HTML 코드를 작성하면서 했다.
적당한 와이어프레임 툴을 모르고, 찾아서 익히기엔 학습비용이 과하다 싶어 포기했다.

3 ~ 5 사이, 늦어도 7 이전에 애플리케이션이 다뤄야 할 도메인 오브젝트를 설계했어야 하지만 미룸.

이상적인 애플리케이션 개발 흐름을 해보려고 최소한의 기능으로 시도 해봤는데 생각보다 시간이 오래 걸린다.
어떤 소스코드를 만들어야 하는지 이미 알고 있으니 나도 모르게 건너뛰는 부분도 많고.