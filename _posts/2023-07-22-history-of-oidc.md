---
title: OIDC의 역사
layout: post
---

## 정리

1. `2006/06/13` OpenID 릴리즈
2. `2006/07/15` 트위터 런칭
3. `2006/08/31` FB 로그인 2006년 8월. 날짜 불확실.
4. `2007/01/09` iPhone 출시
5. `2007/10/03` OAuth1 릴리즈
6. `2009/02/09` FB 좋아요 버튼
7. `2009/11/17` FB API에 OAuth 1.x 지원
8. HTTPS 우대 정책
    - 200x년 부터 확산.
    - 구글 검색은 2011년부터 `https`를 기본으로.
    - `2018/07/24` 크롬 브라우저가 `http` 페이지를 “안전하지 않음”으로 표시.
9. `2012/10/06` OAuth2 릴리즈
10. `2012/05/22` JWT
11. `2014/11/08` OIDC

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

- 아이폰 발매 2007년 1월 9일
  > MACWORLD SAN FRANCISCO—January 9, 2007—Apple® today introduced iPhone
  - [Apple Reinvents the Phone with iPhone](https://www.apple.com/newsroom/2007/01/09Apple-Reinvents-the-Phone-with-iPhone/)
- FB 좋아요 버튼 2009/02/09
  > The like button is a [feature](https://en.wikipedia.org/wiki/List_of_Facebook_features) of social networking service Facebook, where users can like content such as status updates, comments, photos and videos, links shared by friends, and advertisements. The feature was activated February 9, 2009.[[2]](https://en.wikipedia.org/wiki/Facebook_like_button#cite_note-2)
  - [Facebook like button](https://en.wikipedia.org/wiki/Facebook_like_button#Use_on_Facebook)
- FB로 로그인 2006/08/31
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
