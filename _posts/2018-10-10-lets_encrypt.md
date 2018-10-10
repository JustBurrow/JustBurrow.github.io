---
title: Let's Encrypt
layout: post
---
# Let's Encrypt

## 목표

1. [https://lul.kr]용 TLS 인증서 발급받기.
1. GKE(Google Cloud Platform의 Kubernetes 클러스터 서비스)를 사용해서 발급받기.
1. GKE를 사용해서 스태틱 웹 서비스 하기.

## 단계

1. 로컬에서 컨테이너로 웹 서버 실행하기.
1. GKE에서 웹 서버 실행하기.
1. GKE에서 인증서 발급하기.

## NGINX Docker Container

`Dockerfile` :
```docker
FROM nginx:1.14.0
```

실행 명령 :
```bash
➜  nginx git:(docker-nginx) ✗ docker build -t justburrow/nginx . && docker run -p 8080:80 justburrow/nginx
```

컨테이너 실행 확인 :
```bash
curl localhost:8080
```

NGINX 기본 설정 : `/etc/nginx/nginx.conf`
```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

NGINX 설정 : `/etc/nginx/conf.d/default.conf`
```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

파일
- `/usr/share/nginx/html/50x.html`
- `/usr/share/nginx/html/index.html`

### 리소스 변경하기

구조
```
➜  nginx git:(docker-nginx) ✗ tree
.
├── Dockerfile
└── html
    └── index.html

1 directory, 2 files
```

`Dockerfile` :
```docker
FROM nginx:1.14.0

COPY ./html /usr/share/nginx/html
```

## GKE 실행

내가 만든 도커 이미지를 사용해 GKE에서 실행한다.

1. [GCP 콘솔](https://console.cloud.google.com/home/dashboard) 접속.
1. [탐색 메뉴 > Kubernetes Engine > 클러스터](https://console.cloud.google.com/kubernetes/list)
1. "클러스터 만들기" : 저렴하게 설정.
  - 이름 : 적당히
  - 위치 유형 : `영역`(기본값)
  - 영역 : `asia-northeast1-a`
  - 노드풀 > 맞춤설정 > 고급 수정
    - 노드 수 : `1`
    - 코어 : `1` vCPU
    - 메모리 : `1` GB
    - 부팅 디스크 유형 : `표준 영구 디스크`
    - 부팅 디스크 크기(GB) : `40`
1. "만들기"
1. `docker build -t gcr.io/[GCP 프로젝트ID]/nginx .`
  - 이미지 태그를 이 형식으로 해야만 받아주는 듯? `justburrow/nginx`를 사용했을 땐 `denied` 였다.
1. `gcloud` 설치
  1. `brew cask install google-cloud-sdk`
  1. `gcloud init`
1. `gcloud docker -- push gcr.io/[GCP 프로젝트ID]/nginx`
1. 탐색메뉴 > 도구 > Container Registry
  - Docker 컨테이너 이미지가 올라온 것을 확인할 수 있다.

## 참고

- [Getting certificates (and choosing plugins)](https://certbot.eff.org/docs/using.html#manual)
- [How to Set Up Free SSL Certificates from Let's Encrypt using Docker and Nginx](https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx)
- [Docker - Build, Ship, and Run Any App, Anywhere](https://www.docker.com)
- [Docker Hub](https://hub.docker.com)
- [GCP Kubernetes Engine](https://console.cloud.google.com/kubernetes)

### GKE 클러스터

클러스터 정보 보기 : `gcloud container clusters list`

클러스터 일시 정지 : `gcloud container clusters resize cluster-1 --size=0 --region=asia-northeast1-a`
```zsh
➜  nginx git:(docker-nginx) ✗ gcloud container clusters resize cluster-1 --size=0 --region=asia-northeast1-a
Pool [default-pool] for [cluster-1] will be resized to 0.

Do you want to continue (Y/n)?  Y

Resizing cluster-1...done.
Updated [https://container.googleapis.com/v1/projects/[GCP 프로젝트ID]/zones/asia-northeast1-a/clusters/cluster-1].
```