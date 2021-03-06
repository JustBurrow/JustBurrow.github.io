---
title: Dropbox를 사용한 파일 교환 애플리케이션
layout: post
tag: [file exchange, dropbox, cloud storage]
---

회사에서 협력사와 데이터 파일 교환에 SFTP + NFS 조합을 쓰는 걸 보고 생각해본 서비스 아이디어.

애플리케이션에서 컨트롤 할 수 없는 SFTP, NFS 같은 시스템 서비스나 인프라 서비스를 매우 싫어하는 게 가장 큰 이유이지만.

경우에 따라 괜찮은 선택이지만, 대부분은 별로다. 특히 Kubernetes 같은 노드간 자동 부하분산을 실행하는 컨테이너 클러스터에서는 파일 조작은 최악의 선택이다.

## 기존 설계의 문제점

- sftp, nfs는 애플리케이션에서 관리할 수 없다.
- 관리포인트 및 스테이크홀더가 늘어난다.
- 확장이 어렵다.

## 설계

## 사용 시나리오

1. 파트너가 업로드 디렉토리를 요청한다.
1. 시스템이 드랍박스에 디렉토리를 생성하고 파트너 계정에 업로드 권한을 부여한다.
1. 생성된 디렉토리 정보를 파트너에게 전달한다.
1. 파트너가 디렉토리에 파일을 업로드한다.
1. Dropbox가 웹훅을 실행해 시스텡에 업로드 정보를 전달한다.
1. 시스템이 파일을 다운로드 해서 처리한다.
1. 파일 처리 결과를 Dropbox에 업로드한다.
1. 파트너가 처리 결과를 다운로드한다.

### Use case

![use case]({{ base.url }}/assets/file_exchange_application_with_dropbox/usecase.png)

### BPMN

플로우차트보단 알아보기 쉬울 것 같아 BPMN 표기법으로 시나로오를 정리해봤다.

BPMN은 잘 모른다. 엄격한 표기법을 사용한 건 아니고, 그냥 알아볼 수 있는 정도로만 정리.

![BPMN]({{ base.url }}/assets/file_exchange_application_with_dropbox/bpmn.png)

## 참고

- Dropbox API 문서
    - [`/create_folder`](https://www.dropbox.com/developers/documentation/http/documentation#files-create_folder)
    - [`/add_folder_member`](https://www.dropbox.com/developers/documentation/http/documentation#sharing-add_folder_member)
    - [`/download`](https://www.dropbox.com/developers/documentation/http/documentation#files-download)
    - [`/download_zip`](https://www.dropbox.com/developers/documentation/http/documentation#files-download_zip)
- [Dropbox Webhooks](https://www.dropbox.com/developers/reference/webhooks)
