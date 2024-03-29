---
title: 자산관리 도구
layout: post
---

토스뱅크 계좌를 만들면서 토스주식으로 주식을 시작했는데, 사고 팔고 하다 보니 작은 화면으로는 대체 얼마나 벌었는지 알 수가 없다.
그래서 하는 김에 은행까지 관리 해보자 해서 시도를 해봤다.
여러 종류의 통화(원화, 엔화, 달러)에 여러 종류의 계좌(현금, 은행, 주식)를 관리할 수 있어야 하고, 총 자산을 보려면 환율을 반영할 수도 있어야 한다.

## 스프레드시트

먼저 엑셀은 안 샀으니 [OpenOffice Calc](https://www.openoffice.org)로 시도를 해봤지만 실패.

주식 매매 기록을 입력하는 건 문제가 아닌데 이걸 종목, 구입/판매로 나눠서 통계를 내고 수익을 계산을 해야 하는데 수식이 점점 더 복잡해진다.

익숙하지 않은 수식으로 이걸 하려고 생각하니 못 하겠다.

## 노션

이것도 실패. 매매 기록에서 구입은 `12345원`, 판매는 `-12345원` 식으로 기록을 했는데, +/-만 모아서 합계 내는 방법을 모르겠다.

## 자산관리 서비스

검색을 해보니 금융기관 계정을 통합 관리해주는 것만 보이고 자산 관리용은 안보인다. 검색을 못 하는 건가.

## 결론

없으면 만들자. [공공데이터포털](https://www.data.go.kr)를 활용하면 만들 수 있을 것 같다. 은행 계좌를 연결하기는 힘들겠지만, 잔고를 기준으로 볼 수는 있겠지.
