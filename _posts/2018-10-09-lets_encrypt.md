---
title: Let's Encrypt
layout: post
---
# Let's Encrypt

## 목표

1. [https://lul.kr]용 TLS 인증서 발급받기.
1. GKE를 사용해서 발급받기.
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
docker build -t justburrow/nginx . && docker run -p 8080:80 justburrow/nginx
```

컨테이너 실행 확인 :
```bash
curl localhost:8080
```

## 참고

- [Getting certificates (and choosing plugins)](https://certbot.eff.org/docs/using.html#manual)
- [How to Set Up Free SSL Certificates from Let's Encrypt using Docker and Nginx](https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx)