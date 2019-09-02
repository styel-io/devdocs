# nginx 설정

전체 코드

```bash
server {
root /var/www/html;
        index index.html index.htm;
server_name styel.io www.styel.io;

location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    listen [::]:443 ssl http2 ipv6only=on; # managed by Certbot
    listen 443 ssl http2; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/styel.io/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/styel.io/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
        add_header X-Frame-Options SAMEORIGIN;
        add_header X-Content-Type-Options nosniff;
        add_header X-XSS-Protection "1; mode=block";
        add_header Referrer-Policy "same-origin";
        add_header Feature-Policy "geolocation 'none';midi 'none';sync-xhr 'none';microphone 'none';camera 'none';magnetometer 'none';gyroscope 'none';speaker 'self';fullscreen 'self';payment 'none';";
        add_header Content-Security-Policy "default-src 'self' styel.s3.amazonaws.com example.com www.gravatar.com use.fontawesome.com styel.io styel.s3.ap-northeast-2.amazonaws.com fonts.gstatic.com styel.io; style-src 'unsafe-inline' fonts.googleapis.com cdn.jsdelivr.net; font-src 'self' *  data: fonts.gstatic.com; script-src 'self' use.fontawesome.com; connect-src *;";

}

server {
root /var/www/html;
        index index.html index.htm;
server_name styel.co www.styel.co;


location /admin {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }


    listen 443 ssl http2; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/styel.io/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/styel.io/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    if ($host = www.styel.io) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = styel.io) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    if ($host = www.styel.co) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = styel.co) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

        listen 80;
        listen [::]:80;
server_name styel.io www.styel.io styel.co www.styel.co;
    return 404; # managed by Certbot
}
```

## 도메인 라우팅 부분 코드

```bash
server {
    ...
server_name styel.io www.styel.io;

location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

server {
    ...
server_name styel.co www.styel.co;

location /admin {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## TLS(SSL) 적용부분

```bash
server {
    ...
server_name styel.io www.styel.io;

    listen [::]:443 ssl http2 ipv6only=on; # managed by Certbot
    listen 443 ssl http2; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/styel.io/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/styel.io/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
```

## Security Headers

응답헤더를 통한 보안 설정

```bash
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
        add_header X-Frame-Options SAMEORIGIN;
        add_header X-Content-Type-Options nosniff;
        add_header X-XSS-Protection "1; mode=block";
        add_header Referrer-Policy "same-origin";
        add_header Feature-Policy "geolocation 'none';midi 'none';sync-xhr 'none';microphone 'none';camera 'none';magnetometer 'none';gyroscope 'none';speaker 'self';fullscreen 'self';payment 'none';";
        add_header Content-Security-Policy "default-src 'self' styel.s3.amazonaws.com example.com www.gravatar.com use.fontawesome.com styel.io styel.s3.ap-northeast-2.amazonaws.com fonts.gstatic.com styel.io; style-src 'unsafe-inline' fonts.googleapis.com cdn.jsdelivr.net; font-src 'self' *  data: fonts.gstatic.com; script-src 'self' use.fontawesome.com; connect-src *;";
```

Strict-Transport-Security  
=> HTTP 대신 HTTPS만을 사용하여 통신해야한다고 웹사이트가 브라우저에 알리는 보안 기능.

X-Frame-Options SAMEORIGIN  
=> `<frame> 또는<iframe>, <object>`등을 통한 클릭하이재킹 공격 방어

X-Content-Type-Options nosniff  
=> 브라우저가 MIME 유형 스니핑을 수행하지 못하게한다.

X-XSS-Protection "1; mode=block"  
=> 브라우저의 내장 XSS Filter를 통해 공격을 방지

Referrer-Policy "same-origin"  
=> 동일 사이트 출처에 대해서는 리퍼러 전송

Feature-Policy  
=> 브라우저 기능 사용을 허용 및 거부

Content-Security-Policy  
=> 컨텐츠 보안 정책
