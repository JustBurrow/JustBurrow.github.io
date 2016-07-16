---
layout: post
title: Google Cloud Compute Engine에 Spring Boot 웹서버 띄우기.
---

## VM 생성 & OS 부팅

1. 고급 옵션을 열면 Compute Engine이 아니라 App Engine으로 나오지만 무시하고 만든다.
![프로젝트 만들기]({{ site.url }}/assets/google-cloud-compute-engine-spring-boot/1-create-project.png)
2. 프로젝트를 만들 때는 App Engine이지만, Compute Engine으로 만들어졌다.
![프로젝트 만들기 결과]({{ site.url }}/assets/google-cloud-compute-engine-spring-boot/2-create-project-result.png)
3. VM 생성할 때 옵션 지정하기. [참고글](#ref-1)보다 최신 우분투를 선택했다.
![VM 옵션]({{ site.url }}/assets/google-cloud-compute-engine-spring-boot/3-vm-option.png)
  * 이름 : `ubuntu-instance`
  * 영역 : `asia-east1-a` 대만에 있다.
  * 머신 유형 : `초소형(공유 vCPU 1개)`
  * 부팅 디스크 : `Ubuntu 16.04 LTS` LTS 최신 버전으로 선택. 뭐가 다른지 모르니까...
  * 방화벽 : `HTTP 트래픽 허용` 선택, `HTTPS 트래픽 허용` 선택.
4. 생성 후 1분 정도 부팅 과정을 거치면
![VM 생성 & 부팅중]({{ site.url }}/assets/google-cloud-compute-engine-spring-boot/4-vm-create.png)
5. 완료되고 CPU 사용량 정보가 보인다.
![VM 부팅 완료]({{ site.url }}/assets/google-cloud-compute-engine-spring-boot/5-vm-boot.png)
6. 당장은 SSH 키를 만들기 귀찮으니 브라우저 SSH를 연다. VM 인스턴스 옆의 `SSH`를 클릭.
![브라우저 SSH 터미널 여는 중]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/6-browser-ssh-open.png)
7. 연결 완료.
![브라우저 SSH 터미널 연결]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/7-browser-ssh-connected.png)

## 패키지 준비

### JDK8

1. `sudo apt-get update` 스크린샷은 2번째 업데이트한 거라 내용이 적지만, 원래는 많았다.
![sudo apt-get update]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/8-apt-get-update.png)
2. `apt-cache show default-jdk` 설치할 수 있는 기본 JDK 정보 보기. JDK8이 있다.
![apt-cache show default-jdk]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/9-apt-cache-show-default-jdk.png)
3. `sudo apt-get install default-jdk` 테스트니까 그냥 기본 JDK로 설치.
![sudo apt-get install default-jdk]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/10-sudo-apt-get-install-default-jdk.png)
4. Java 버전 확인
![Java 버전 확인]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/11-jdk-version-check.png)

### Maven

1. `apt-cache show maven` 설치할 수 있는 메이븐 버전 확인.
![apt-cache show maven]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/12-apt-cache-show-maven.png)
2. `sudo apt-get install maven` 사실 그냥 있는 거 설치할 거였으니 버전 확인은 필요없었다.
![sudo apt-get install maven]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/13-apt-get-install-maven.png)
3. `mvn -v` 설치한 메이븐 버전 확인.
![mvn -v]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/14-mvn-v.png)
4. `apt-cache show git`
![apt-cache show git]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/15-apt-cache-show-git.png)
5. `sudo apt-get install git` & `git --version`
![sudo apt-get install git & git --version]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/16-git--version.png)

## 애플리케이션 설치 및 기동

1. 참고자료([2](#ref-3), [3](#ref-4))를 참고해서 간단한 Spring Boot 애플리케이션을 만든다.
2. [애플리케이션 푸시](https://github.com/JustBurrow/google-cloud/commit/d435996ea2b3be12657ecca9fdc39db514565b5f).
3. `git clone https://github.com/JustBurrow/google-cloud.git` 샘플 애플리케이션 클론.
![git clone https://github.com/JustBurrow/google-cloud.git]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/17-git-clone.png)
4. `mvn package` 샘플 애플리케이션 빌드
![mvn package]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/18-mvn-package.png)
5. JAR 파일 이동.
![mkdir & cp]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/19-mkdir-and-cp.png)
6. 설정파일 추가.
![add application.yml]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/20-add-config-file.png)
7. `sudo java -jar google-cloud-0.0.1-SNAPSHOT.jar` 애플리케이션 실행. `sudo`를 사용하지 않으면 권한 에러로 실행되지 않는다.
![애플리케이션 실행]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/21-run-application.png)
8. HTTP 접속 확인. HTTPS 접속은 설정은 해뒀지만 인증서 등이 없으므로 접속할 수 없다.
![HTTP 접속 확인]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/22-http-access-check.png)

## 덧

* `sudo apt-get update` & `sudo apt-get upgrade` 우분투 기본 패키지 갱신.
![우분투 패키지 갱신]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/23-sudo-apt-get-upgrade.png)
* `sudo apt-get install tree` `tree`를 설치한다. 편하다.
![tree 설치]({{ base.url }}/assets/google-cloud-compute-engine-spring-boot/24-install-tree.png)

## 참고

1. <span id="ref-1"/>[구글 클라우드 생성하기 - VM 생성과 접속](http://bcho.tistory.com/1107)
2. <span id="ref-2"/>[Building an Application with Spring Boot](https://spring.io/guides/gs/spring-boot) : Spring Boot 튜토리얼
3. <span id="ref-3"/>[63.1 Including the plugin](http://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html#build-tool-plugins-include-maven-plugin) : 나머진 Spring Boot 튜토리얼로 되지만, 실행 가능한 jar 파일을 만들기 위한 메이븐 설정은 이쪽을 참조해서 플러그인을 설정한다.
4. <span id="ref-4"/>[JustBurrow/google-cloud](https://github.com/JustBurrow/google-cloud) : 샘플 Spring Boot 애플리케이션 저장소.
