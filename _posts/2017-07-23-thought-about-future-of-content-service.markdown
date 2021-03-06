---
title: 컨텐츠 서비스의 미래에 대한 생각
layout: post
---

1.	컨텐츠 서비스 분야는 2020년을 기점으로 크게 바뀔 것이다.
2.	2020년 동경 올림필을 통해 5G 네트워크가 마케팅을 하력고 할 것이기 때문이다.
3.	5G를 가장 기다리고 있는 분야는 바로 자동주행차량 분야이다.
4.	자동중행차량은 AI가 필수이다.
5.	AI가 정확한 판단을 하기 위해서는 자세한 차량 주변 정보가 필요하다.
6.	근시일 내에 차량에 탑재할 수 있는 컴퓨팅 능력으로는 충분한 AI 성능을 얻을 수 없다. 따라서 별도의 중앙 AI 센터가 필요하다.
7.	현재의 4G 통신망으로는 차량이 센서 데이터를 AI 센터에 실시간으로 보낼 수 없다.
8.	그래서 자동주행차량 분야는 5G 통신이 필수 요소이다.
9.	문제는 5G 통신망은 자동주행차량에 필요한 통신속도 이상을 가지고 있다.
10.	차량은 이동수단 이외에도 외부와 격리된 개인 공간의 의미도 크다.
11.	여유있는 통신능력, 필수적으로 할 일이 사라진 개인 공간은 새로운 컨텐츠 소비 공간이 될 것이다.
12.	대중교통보다는 넓지만 거실보다 좁고 혼자만의 공간인 자동주행 차량은 VR 컨텐츠를 소비하기에 최적의 공간이다.
 * 대량의 센서 운용, AI 센터 통신, 환경규제, 환경 우선에 대한 대중의 인식을 생각하면 자동주행차량은 전기자동차가 될 수밖에 없으며, 일상적으로는 주행 이외에도 여유있는 전력 공급이 가능할 것이다. 사용자는 전력에 대해 아예 신경 쓰지도 않겠지만.
 * 거실VR은 걸어서 움직이는 컨텐츠에도 대응해야 한다.
 * VR 보급의 가장 큰 장애 요소는 타인의 시선이다. 배터리 문제도 있지만, 모바일 시대의 가장 큰 컨텐츠 소비 공간인 대중교통에서 사용하지 않는 이유가 이것이다. 지금도 핸즈프리 사용을 막는 것이 타인의 시선이다.
13.	따라서 2020년 이후의 컨텐츠 서비스 플랫폼이 되기 위해서 3종류의 컨텐츠에 대응하거나 특화해야 한다.
 * 도큐먼트
   * HTML, XML, JSON 등의 텍스트 기반 컨텐츠.
 * 바이너리
   * 이미지, 업데이트 파일 등의 파일 컨텐츠.
 * 스트리밍
   * 대용량 컨텐츠의 pre-fetch(cache).
   * 생중계 컨텐츠.

## 대응방법

너무 뻔한 소리에 불과하지만...

컨텐츠 서비스의 수익모델을 크게 다음과 같다고 다음을 예상한다.

* 컨텐츠 무료 제공, 광고 노출.
* 유료 컨텐츠 제공.
* 컨텐츠 라이센싱.

앞서 말한 흐름에서 어떤 것을 개발해야 할지는 알 수 없다. 하지만 측정할 수 있는 KPI로 체류시간과 클릭을 생각한다면, 다음 요소가 필수 특성이 될 것이라 생각한다.

1. 개인화한 컨텐츠 추천.
2. 낮은 컨텐츠 생산 비용.
3. 유연한 컨텐츠 확장.

따라서 다음의 기술이 필요하다.

1. 개인화된 컨텐츠 추천을 위한 **AI**.
2. AI 개발, 트레이닝에 필요한 인력과 데이터를 확보할 수 있는 **BigData 시스템**.
3. BigData 시스템에 적합한 컨텐츠와 로그를 확보할 수 있는 **오브젝트기반 CMS**.
4. 오브젝트기반 CMS를 개발할 수 있는 **OOP 개발능력**.

## 덧

OOP 개발능력에 필수적인 요소는 도메인 지식과 유지 보수에 대한 장기 예측 능력이라고 생각한다. 구체적이고 경험적으로 이 능력을 키우기 위해선 **지식과 예측이 없으면 매우 불편한**, 컴파일이 필요한 하드 타이핑 언어(Java, C++ 같은)가 제격이라고 생각한다. 그런데 어째서인지 이런 언어는 철지난 구닥다리에 불편하고 무거워서 생산성이 낮아 쓸 게 못 되니 레거시 프로그램의 유지보수가 아닌 이상 고려할만한 선택이 아니란 인식이 너무 강하게 퍼져있다.

그래서 보통 (코드 생략에 의한)생산성과 유연성을 위해 선택하는 것이 PHP, Ruby on Rails, NodeJS 등등의 인터프리터 언어를 선택한다. 그리고 생산성을 최초로 눈에 보이는 무언가를 만들 때 까지만 고려할 뿐, 설계나 유지보수에 필요한 비용은 없는 셈 치는 경향이 지나치게 강하다.

이런 인터프리터/동적 타이핑 언어의 유연성과 코드 생략(code over convention 정도 되려나?)이 장점이 되려면 해결하려는 문제가 뭔지 짐작조차 할 수 없어 프로토타이핑을 통해 구체화하려는 단계이거나, 해결하려는 문제가 너무나 명확(도메인 지식이 충분)하거나, 인수인계 비용이 낮거나(인력도 충분한), 수정시 고려해야 할 사항이 아주 적은 경우에나 성립한다고 생각한다. 그리고 하드타이핑 언어에 비행 매우 많은 테스트 코드는 필수이다.

DB 스키마까지 보지 않으면 이 클래스의 오브젝트는 대체 어떤 데이터를 가지고 있을지 알 수 없는 게, PHP나 Ruby 프로그램에서는 보통이었다. 이 경우 당연히, 반드시 존재하지만 무시하는 유지보수 및 장애 대응 비용은 누구도 말하지 않는다.

## 덧2 - 불만

어떤 소프트웨어든 1회용이나 PoC가 아닌 이상 유지보수는 발생한다. 릴리즈 이전에도 유지보수는 이어진다. 문제는 유지보수 비용은 경시하거나, 무시하거나, 걱정할 필요가 없는 문제로 여기는 경우가 많다는 점이다. 더 당황스러운 경우는 개발자가 유지보수 비용을 신경쓰는 걸 월권행위로 여기는 경우도 있다는 점이다. 개발자를 말단 월급 노예 정도로 여기는 사고방식이다. 개발자의 프로그램은 개발자의 시간을, 수명을, 생명을 써서 나온 결과이다. 그게 좀 더 나은 존재이길 바라는 건 당연한 것 아닌가. 그리고 쓰레기(레거시)에 허덕이느라 날새는 건 싫은 게 당연한 거 아닌가.

## 덧3 - 레거시 코드

어떤 책에서 그랬다. 레거시 코드란 테스트 코드가 없는 코드라고. 어제 작성한 코드라도 테스트 코드가 없으면 레거시 코드라고. 숫자로 판단할 수 있는 기준이라면 이 이상의 정의는 없을 것 같다. 감각적으로는 "이걸 이렇게 바꾸려면 어딜 어떻게 손대야 하니까 어느 정도 시간이 필요한지" 감이 안오는 게 레거시 코드라고 생각한다. **레거시 코드는 다른 말로 쓰레기 코드다**. 버리자. 제발 버리고 재개발 하자. 쓰레기 코드를 재개발 하지 않도록 도메인 지식을 반영한 설계 좀 하고. 짬은 똥구녕으로 처먹나. 재개발 해서 잘되는 걸 못 봤다거나 잘난 척(기존 개발자를 무시) 하지 말라는 헛소리 하지 말고. 제품은 개발자만 만드나. 개발자 끼리만 머리 굴려서 만들 거면 나머지 인력은 그냥 묻어가는 장식품인가. 개발자만 있어서 될 것 같으면 그 인원으로 회사 차리지. 당연한 것 아닌가.
