---
title: GKE에서 파일 공유 시도와 실패.
layout: post
tags: [gcp, pd, gcs, fuse, gke]
---

## 목표

GKE(Google Kubernetes Engine)로 다수의 웹 서버 컨테이너로 정적 리소스(css, javascript, 이미지 파일)를 서비스한다.

서비스할 파일은 배포서버가 실시간으로 갱신한다.

## 시도 1 - Persistent Disk

GCE의 기본 스토리지로 여러 가상머신(GCE 인스턴스)에서 공유할 수 있다.

공유 자체는 가능하지만, 배포 서버가 배포할 수가 없다. 읽기 모드로는 여러 가상머신이 공유할 수 있지만, 읽기-쓰기 모드는 공유 파일 시스템으로 사용할 수 없다.

### 실패 이유

영구 디스크를 사용한다면 배포 프로세스가 "신규 디스크 생성 - 리소스 배포(패키징) - GKE 오브젝트 생성 - 컨테이너 롤링 업데이트"로 복잡해진다.

## 시도 2 - Cloud Storage

GCE의 오브젝트 스토리지. AWS라면 S3에 해당한다.

파일 시스템이 아니라 웹서버의 도큐먼트 루트로 사용할 수 없지만, Cloud Storage FUSE를 사용하면 파일시스템처럼 마운트할 수 있다.

읽기-쓰기 모드로 여러 인스턴스(가상머신, 컨테이너)에 마운트 할 수 있다.

### 실패 이유

1. Kubernetes에 직접 볼륨 오브젝트를 만들 수 없다.
2. `gcsfuse`를 사용해서 마운트를 하려면 인증을 해야 한다. 컨테이너를 하나 올릴 때마다 인증 받는 방법을 못 찾음.

## 결론

NFS인 [Cloud Filestore](https://cloud.google.com/filestore)를 사용할 수 밖에 없어 보인다.

분산 파일시스템이 아니라는 이유로 NFS를 피해왔는데, 아무래도 무리인 듯 싶다.

## 참고

- [GCP Persistent Disk](https://cloud.google.com/persistent-disk)
- [여러 인스턴스에서 영역 영구 디스크 공유](https://cloud.google.com/compute/docs/disks/add-persistent-disk#use_multi_instances)
- [Cloud Storage 문서](https://cloud.google.com/storage/docs)
- [Cloud Storage FUSE](https://cloud.google.com/storage/docs/gcs-fuse)
- [FUSE volumes #7890](https://github.com/kubernetes/kubernetes/issues/7890)

<details>
<summary>memo</summary>
<pre>
- 목표
  - GKE에 Nginx로 리버스 프록시를 실행한다.
  - 정적 리소스는 리버스 프록시가 직접 서비스 한다.
  - 정적 리소스는 공유 파일 스토리지에 배포한다.
- 시도
  - PD
    - "배포 서버에 읽기 쓰기, GKE 노드/컨테이너는 일기"로 마운트 할 수가 없다.
    - 읽기 모드는 여러 인스턴스에 마운트 할 수 있지만 읽기-쓰기 모드는 하나의 인스턴스에만 마운트 할 수 있다.
  - Cloud Storage
    - 읽기-쓰기 모드로 여러 인스턴스에 마운트 할 수 있지만, 인증이 필요하다.
    - 컨테이너 실행시 인증 할 수 있는 방법은?
  - Cloud Filestore
    - 관리형 NFS
</pre>
</details>