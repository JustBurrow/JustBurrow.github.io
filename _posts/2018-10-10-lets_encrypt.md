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

## 참고

- [Getting certificates (and choosing plugins)](https://certbot.eff.org/docs/using.html#manual)
- [How to Set Up Free SSL Certificates from Let's Encrypt using Docker and Nginx](https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx)