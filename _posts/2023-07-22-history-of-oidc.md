---
title: OIDC의 역사
layout: post
---

최근의 모바일 앱은 OIDC(OpenID Connect)사용해 거대 서비스에 연동하는 것을 기본으로 하고 있다.
하지만 왜 OIDC를 개발하고 채용해야 했던 이유는 잘 알려지지 않은 것 같아 정리한다.

## 모바일 이전

1. `2002/01` 3G 상용화 시작.
   - SKT가 최초.
2. `2004/02/04` Facebook 런칭.
   - 이때는 PC 기반.
3. `2006/06/13` 공개 SSO 표준으로 OpenID가 개발.
   - 사용자의 정보를 안제하게 제공하는 게 목적.
4. `2006/06` 4G 상용화 시작.
5. `2006/07/15` SMS(Short Message Service) 기반 SNS인 Twitter가 런칭.
   - 출시 후에도 한동안은 2G 폰으로 글을 쓰고 PC로 타임라인을 읽는 방식이 일반적.
6. `2006/08` Facebook Log-in 출시.
7. `2007/01/09` iPhone 1세대 출시.
   - 많은 서비스가 작은 화면에서 회원가입을 유도해야 할 동기가 생김.
8. `2007/10/03` Twitter 주축으로 OAuth 1.x 개발.
   - 각종 웹 서비스가 CPL(Cost Per Lead 회원 획득 비용) 절감 수단으로 채용.
   - `HTTP` 기반.
9.  `2009/02/09` Facebook Like 버튼 출시.
   - 이때부터 급성장 해서 Twitter를 제침.
10. `2009/11/17` Facebook 로그인에 OAuth 1 채용.

## 모바일 이후

1. 2000년대 후반부터 `HTTP`에서 `HTTPS`로 표준 표준 프로토콜이 이동하기 시작.
   1. 2001년 구글은 검색결과에 `HTTPS`를 우선 노출.
2. `2012/10/06` OAuth 2 공개.
   - 보안을 `HTTPS`에 의존. 중간자공격(Man-in-the-middle attack)에 간단히 개인정보가 유출된다는 이유로 반발이 심했다.
   - 보안을 `HTTPS`에 의존하면서 개발이 단순해짐.
   - OAuth 1.x는 `HTTP` 기반이었기 때문에 클라이언트의 리퀘스트 암호화와 서버의 검증 로직이 복잡하고 부하가 컸다.
3. `2012/05/22` JWT 공개.
   - 사용자 정보를 얻기 위해선 IO-bound 작업이 필요한 세션(공유 세션 스토리지), OAuth 1.x를 대신한 인증 방식.
   - VM화, 클라우드화로 공유 세션 스토리지에 접속하는 WAS도 폭증, 사용자 정보 획득이 보틀넥으로 작동한다. IO(특히 네트워크IO) 발생을 없애서 성능을 개선.
   - 보안을 `HTTPS`에 의존해야 하는 방식.
4. `2012/07/31` AWS에 SSD 도입.
   - "클라우드가 SSD를 도입 -> 클라우드 인프라 성능 개선 -> 클라우드 인프라 채용 증가 -> 세션 스토리의 요구 커넥션 증가"의 과정으로 JWT 같은 connection-less 사용자 정보 제공 기술의 필요성 증가.
   - 다수의 3rd 파티 서비스를 지원해야 하는 플랫폼은 막대한 사용자 정보 요청에 직면.
5. `2014/06/18` Kubernetes 공개.
   - 가상화 기술이 VM에서 컨테이너로 바뀌면서 유저 정보가 필요한 서버 애플리케이션이 더욱 증가, connection-less 사용자 정보 제공 기술의 필요성이 더욱 증가.
6. `2014/11/08` OIDC(OpenID Connect) 공개.
   - OAuth 2 Authorization Code Flow + JWT
   - "플랫폼(ID Provider)의 낮은 부하 + 낮은 개발 난이도 + 앱 클라이언트 or 서버 클라이언트 지원 + 상대적으로 쉬운 개발 난이도 + 3rd 파티의 비교적 단순한 권한 모델"에 적절한 기술.
7. `2019/04` 5G 상용화.

## 정리

1. `2002/01` 3G 상용화.
2. `2004/02/04` Facebook 런칭.
3. `2006/06/13` OpenID 공개.
4. `2006/06` 4G 상용화.
5. `2006/07/15` 트위터 런칭
6. `2006/08/31` FB 로그인 2006년 8월. 날짜 불확실.
7. `2007/01/09` iPhone 출시.
8. `2007/10/03` OAuth1 공개.
9. `2009/02/09` FB 좋아요 버튼
10. `2009/11/17` FB API에 OAuth 1.x 지원
11. `200x` HTTPS 우대 정책 확산 시작.
12. `2012/10/06` OAuth2 공개.
13. `2012/05/22` JWT 공개.
14. `2012/07/31` AWS에 SSD 도입.
15. `2014/06/18` Kubernetes 공개.
16. `2014/11/08` OIDC 공개.
17. `2019/04` 5G 상용화.

## 참고 자료

### OAuth 1.x

- OAuth1
  > • [OAuth Core 1.0](https://oauth.net/core/1.0) was released December 4, 2007.
  - [OAuth 1 — OAuth](https://oauth.net/1)
- 더 정확한 릴리즈 2007/10/03
  > On October 3rd, 2007 the OAuth Core 1.0 final draft was released.
  - [Introduction — OAuth](https://oauth.net/about/introduction)
- OAuth1 릴리즈는 트위터에서 시작.
  - [OAuth](https://en.wikipedia.org/wiki/OAuth)

## OAuth 2.x

- [OAuth 2.0 — OAuth](https://oauth.net/2)
- [RFC 6819: OAuth 2.0 Threat Model and Security Considerations](https://datatracker.ietf.org/doc/html/rfc6819.html)
- 릴리즈 2012/10/06
  > OAuth 2.0 Threat Model and Security Considerations RFC 6819 Informational	2012-10-06
  - [Specs — OAuth](https://oauth.net/specs)
- 플로우 종류별 비교
  - [Google Trends](https://trends.google.com/trends/explore?date=2012-01-01%202023-07-19&q=oauth%20code%20flow,oauth%20implicit%20flow,oauth%20password%20flow,oauth%20credential%20flow)
  - ![oauth flow trends.png]({{ site.url }}/assets/history-of-oidc/oauth_flow_trends.png)

## OpenID

- OpenID 1.1 : `<head>`에 2006/06/13이니 마지막 날짜로 사용.
  > D. Recordon B. Fitzpatrick May 2006
  - [OpenID Authentication 1.1](https://openid.net/specs/openid-authentication-1_1.html)
- [OpenID - OpenID Foundation](https://openid.net/)
- 2007년 12월 OpenID 2.0 발표
  > The final version of OpenID is OpenID 2.0, finalized and published in December 2007.[[5]](https://en.wikipedia.org/wiki/OpenID#cite_note-5) The term *OpenID* may also refer to an identifier as specified in the OpenID standard; these identifiers take the form of a unique [Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) (URI), and are managed by some "OpenID provider" that handles authentication.
  - [OpenID](https://en.wikipedia.org/wiki/OpenID)

## OIDC

- [OpenID Connect Protocol](https://auth0.com/docs/authenticate/protocols/openid-connect-protocol)
- 1.0 릴리즈 2014/11/08
  > Final	N. Sakimura NRI J. Bradley Ping Identity M. Jones Microsoft B. de Medeiros Google C. Mortimore Salesforce November 8, 2014
  - [Final: OpenID Connect Core 1.0 incorporating errata set 1](https://openid.net/specs/openid-connect-core-1_0.html)
- JWT 릴리즈 2012/05/22
  > OAuth Working Group M. Jones Internet-Draft Microsoft Intended status: Standards Track J. Bradley Expires: November 23, 2012 Ping Identity N. Sakimura NRI May 22, 2012
  - [JSON Web Token (JWT)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-json-web-token-00)

## 기타

- [3G](https://en.wikipedia.org/wiki/3G) 2002년 1월 첫 상용화.
  > The first network to go commercially live was by SK Telecom in South Korea on the CDMA-based 1xEV-DO technology in January 2002. By May 2002, the second South Korean 3G network was by KT on EV-DO and thus the South Koreans were the first to see competition among 3G operators.
  - ![Cellular network standards and generation timeline.](https://en.wikipedia.org/wiki/3G#/media/File:Cellular_network_standards_and_generation_timeline.svg)
- [4G](https://en.wikipedia.org/wiki/4G) 2006년 6월 첫 상용화.
  > In June 2006, the world's first commercial mobile WiMAX service was opened by KT in Seoul, South Korea.
- [5G](https://en.wikipedia.org/wiki/5G#Deployment) 2019년 4월
  > The first fairly substantial deployments were in April 2019. In South Korea, SK Telecom claimed 38,000 base stations, KT Corporation 30,000 and LG U Plus 18,000; of which 85% are in six major cities.
- 아이폰 발매 2007년 1월 9일
  > MACWORLD SAN FRANCISCO—January 9, 2007—Apple® today introduced iPhone
  - [Apple Reinvents the Phone with iPhone](https://www.apple.com/newsroom/2007/01/09Apple-Reinvents-the-Phone-with-iPhone/)
- Facebook 런칭 2004년 2월 4일
  > Launched	February 4, 2004; 19 years ago
  - [Facebook](https://en.wikipedia.org/wiki/Facebook)
- FB 좋아요 버튼 2009년 2월 9일
  > The like button is a [feature](https://en.wikipedia.org/wiki/List_of_Facebook_features) of social networking service Facebook, where users can like content such as status updates, comments, photos and videos, links shared by friends, and advertisements. The feature was activated February 9, 2009.[[2]](https://en.wikipedia.org/wiki/Facebook_like_button#cite_note-2)
  - [Facebook like button](https://en.wikipedia.org/wiki/Facebook_like_button#Use_on_Facebook)
- FB로 로그인 2006년 8월 31일
  > In August 2006, we introduced the first version of the Facebook API, enabling users to share their information with the third party websites and applications they choose. Hundreds of companies have leveraged these APIs, allowing users to dynamically connect their identity information from Facebook, such as basic profile, friends, photos information and more, to third party websites, as well as desktop and mobile applications.
  - [Announcing Facebook Connect - Meta for Developers](https://developers.facebook.com/blog/post/2008/05/09/announcing-facebook-connect/)
- FB API OAuth 1.x 지원 2009/11/17
  > We all have made the OAuth Core 1.0a and OAuth WRAP specifications
  - [Evolving OAuth via the Open Web Foundation - Meta for Developers](https://developers.facebook.com/blog/post/335)
- 구글 검색 2011년 `https`를 기본으로 사용.
  > 사용자를 보호하고 더욱 강력한 업계 차원의 보안 표준을 마련하기 위해 Google에서는 로그인한 사용자의 경우 기본적으로 https://를 사용합니다. 검색어 및 검색결과 페이지가 암호화되기 때문에 사용자의 검색 내용을 악의적인 행위자로부터 보호할 수 있습니다.
  - [Search Through Time](https://www.google.com/search/howsearchworks/our-history)
- 크롬 브라우저는 v68부터 HTTP 주소를 안전하지 않음으로 표시. 2018/07/24
  > In Chrome 68, Chrome will show the “Not secure” warning on all HTTP pages.
  - [Stable Channel Update for Desktop](https://chromereleases.googleblog.com/2018/07/stable-channel-update-for-desktop.html)
- 트위터 런칭 2006/07/15
  > Launched July 15, 2006; 17 years ago
  - [Twitter](https://en.wikipedia.org/wiki/Twitter)
- [Announcing Provisioned IOPS for Amazon EBS](https://aws.amazon.com/about-aws/whats-new/2012/07/31/announcing-provisioned-iops-for-amazon-ebs) 2012년 7월 31일 SSD 기반 서비스 출시.
- [중간자 공격(Man-in-the-middle attack)](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
- [Google Open Sources Its Secret Weapon in Cloud Computing](https://www.wired.com/2014/06/google-kubernetes) 2014년 6월 18일 Kubernetes 공개.
- [Kubernetes](https://en.wikipedia.org/wiki/Kubernetes)
  > Kubernetes (κυβερνήτης kubernḗtēs, Greek for "steersman, navigator" or "guide", and the etymological root of cybernetics) was announced by Google in mid-2014. The project was created by Joe Beda, Brendan Burns, and Craig McLuckie, who were soon joined by other Google engineers, including Brian Grant and Tim Hockin.