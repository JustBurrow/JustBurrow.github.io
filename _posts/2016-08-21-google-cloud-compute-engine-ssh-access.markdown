---
layout: post
title: Google Cloud Compute Engine에 SSH 접속하기
---

## 환경

* 키 생성 : MacOS X
* 접속할 VM : [compute-engine](#ref-1)
* SSH 클라이언트(?) : Terminal.app

## SSH 키 생성하기

Compute Engine의 서버 인스턴스를 설정할 때 필요한 RSA 공개키를 내 컴퓨터(**MacBook Pro Retina**)에서 만드는 과정.

1. 터미널 앱을 실행해서 SSH 키 생성.<br/>
`ssh-keygen -t rsa -C "just_burrow@compute-engine"`<br/>
예의상 한번정도 비밀번호 틀려주는 것도 좋다.<br/>
![ssh-keygen]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/ssh-keygen-rsa.png)<br/>
키 파일 경로를 `/User/justburrow/.ssh/compute_engine`이 아니라 `~/.ssh/compute_engine`이라고 하면 찾을 수 없다면서 안만들어지더라. 왜 때문이지...
2. 생성된 키 파일 확인하기.
![SSH 키 파일]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/ssh-key-file.png)

## SSH 키 추가하기

구글 클라우드 Compute Engine의 서버(이 경우엔 Ubuntu Linux) 인스턴스를 웹브라우저가 아닌 별도의 SSH 클라이언트로 접속할 수 있도록 서버 인스턴스를 설정하는 과정.

1. 기존 SSH 키 목록. 웹 SSH를 사용할 때 생성한 키 인 듯.
![SSH 키 메뉴]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/metadata-sshkey.png)
2. 필요없으니까 전부 삭제하자.<br/>
**`SSH키`** 탭 아래의 `수정` 버튼을 누른 후, 기존 키를 모두 삭제한다.<br/>
아마도 웹브라우저 SSH 클라이언트를 사용할 때 자동으로 만든 키로 보인다.<br/>
![기존 SSH 키 삭제]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/delete-old-keys.png)
3. 로컬에서 생성한 키 추가하기.<br/>
`~/.ssh/compute_engine.pub` 파일의 내용. 터미널에서 바로 복사하는 경우, 사용한 터미널 앱에 따라서 출력 화면의 줄바꿈까지 복사되는 것 같다. 꼭 텍스트 앱으로 열어서 복사하자.<br/>
터미널에서 파일 내용을 바로 클립보드로 복사할 수 있는 기능이 있을 것도 같은데, 모른다.
![키 추가]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/add-ssh-key.png)
4. 새 SSH 키 목록.
![새 SSH 키 목록]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/new-sshkey-list.png)

## SSH 접속하기

실재로 SSH 클라이언트로 서버 인스턴스에 접속해서, 내가 원하던 바로 그 유저로 접속했는지 확인하는 과정.

1. SSH 접속하기
![SSH 접속]({{ base.url }}/assets/google-cloud-compute-engine-ssh-access/ssh-connection.png)
2. 짜란~ `ls`로 확인한 디렉토리 목록이 바로 서버 인스턴스 관리자 계정의 홈 디렉토리의 내용이다.

## 참고

1. <span id="ref-1"/>[Google Cloud Compute Engine에 Spring Boot 웹서버 띄우기.](/2016/07/15/google-cloud-compute-engine-spring-boot.html)
2. <span id="ref-2"/>[구글 클라우드 VM으로 SSH 접속하기](http://bcho.tistory.com/1103)

## 기타

* SSH 키를 만들 때 [참고 2](#ref-2)의 내용 처럼 꼭 구글 계정을 사용할 필요는 없다. 내 경우 구글 계정은 `just.burrow@lul.kr`인데, Compute Engine의 관리자 계정은 `just_burrow`이다. 구글 계정을 사용해야 한다면 유효한 키 자체를 만들 수 없다(아마도). 단, SSH 접속시 유저명으로 키를 만들 때 Compute Engine의 유저를 사용하고(호스트는 자유), 사용한 그 유저(`just_burrow`) + @ + `호스트` 형태로 접속해야 한다. 이 경우엔 `just_burrow@111.111.111.111`(IP는 비밀 😘).
* SSH 키를 Compute Engine에서 만들어준 유저가 아닌 구글 계정을 사용하면 메일 주소에서 유저 부분을 뽑아서 Compute Engine 인스턴스에 새로 유저를 만들어준다. 문제는 권한도 그룹도 홈디렉토리도 기존의 관리자 계정이 아니라는 점. 이거 때문에 SSH 키 생성시 덮어쓰기 할 거냐고 묻는 뿌분이 나왔다.
* 22 포트 막아놓은 통신사 짜증난다. 휴대용 WiFi는 통신사가 포트를 막아놔서(타임아웃 발생) 핸드폰 테더링을 사용했다.
* 7월 23일에 쓰기 시작한 걸 미루고 미루다 8/21에 완성한다.
* 이 글은 몇 번의 삽질 결과를 정리한 것이기 때문에, 가급적 최종 결과를 기준으로 작성했음에도 불구하고, 그 이전에 시도한 내용이 끼어들어갔을 가능성이 있지만, 그렇다고 내가 처음부터 다시 따라하면서 검사하기엔 너무나도 귀찮다. 따라서, 오류를 발견한 운나쁜 사람은 Github의 훌륭한, 소스코드 코멘트 기능을 활용해주길 바란다. 코멘트를 언제 확인할지는 모르지만...
* 중간에 키를 만들면서 실수로 비밀번호 없이 `~/.ssh/id_rsa`파일을 덮어써버렸다. 새로 발급하긴 했지만 쓸 수 있으려나... =_=);;;
