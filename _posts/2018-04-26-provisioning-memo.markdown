---
title: 프로비저닝 잡담
layout: post
---

# 잡담

`VirtualBox` + `Vagrant` + `Ansible` 조합으로 로컬 개발환경을 구축하고 여기에 사용한 `playbook`을 서버 환경에 적용할 생각이었다.

1. `CentOS 7` VM 구성.
1. `HAProxy`를 사용해 ELB나 Cloud Load Balancing을 흉내낸다.
1. `Nginx`로 리버스 프록시 구성.
1. `Apache HTTP Server` + `PHP 7.2`으로 웹서버 구성.
1. `MySQL 5.7`로 DB 구성.
1. `NFS`로 공유 파일 스토리지 구성.

각각 서로 다른 `10.0.1x.10x` IP를 주고 `HAProxy`만 Host-only로 네트워크 어댑터를 설정해서 호스트OS의 브라우저로 파일 및 PHP, DB 연동은 끝냈다.

그런데 이렇게 만들고 보니

1. 게스트OS에 `vagrant`라는 유저가 생긴다.
1. `VirtualBox`의 인터널 네트워크를 만들어서 버추얼박스 내부의 네트워크로 만들었는데 `ifconfig`에는 여러개의 네트워크가 잡힌다.
1. `Vagrant`가 쓰기위한 포트포워딩 네트워크가 잡힌다.
1. `Vagrant`가 프로비저닝을 하다보니 앤서블 플레이북도 `vagrant` 유저로 실행된다.
1. `VirtualBox`는 NIC 타입을 지정할 수 있는데 `Vagrant`는 거기까진 커스터마이징을 지원하지 않는다.
1. `Vagrant`는 스크립트가 루비 소스코드인데 개발툴의 도움을 받기 어렵다.

이게 단순히 내가 처음이라 몰라서 못 하는 건지, 없는 건지.

`IaC`가 트렌드이고 조만간 일반적이고 당연한 개념이 될 것 같은데 인프라 개발도구가 나와서 단위 테스트와 디버깅이 됐으면 좋겠다.
로컬 서버 띄우듯이 로컬 백엔드 띄워서 테스트 하고 배포하게.
