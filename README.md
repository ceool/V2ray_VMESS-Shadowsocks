
## V2ray
<br>
<Strong>마지막 확인 Date: 2021. 04. 03</Strong>
<br>
<br>
https://www.v2fly.org/en_US/guide/install.html#supported-os-platforms <br>
(https://github.com/v2fly/fhs-install-v2ray)

<br>

#### 설치 및 업데이트 (V2Ray)
```
# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

#### 최신 베타 버전 (geoip.dat 와 geosite.dat)
```
# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

#### 삭제 (V2ray)
```
# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```

<br>

### V2ray 설정 (VMess + Shadowsocks)
```
$ vi /usr/local/etc/v2ray/config.json

V2ray 폴더 파일을 참고하여 내용 작성
```

systemctl start v2ray

<br>
<br>

## UFW (방화벽)
### UFW 설치
```
Ubuntu: $ apt-get install ufw
CentOS: $ yum install ufw
```

systemctl start ufw

<br>

### UFW 설정
```
$ ufw allow 22/tcp

-- Shadowsocks Only --
$ ufw allow 23456/tcp

-- VMess Only --
$ ufw allow 443/tcp

-- Option --
$ ufw allow 80/tcp
```

ufw status <br>
![1](https://user-images.githubusercontent.com/62891711/113473707-2ddef300-94a6-11eb-9da5-94faefd57070.png)



<br>
<br>

## Nginx (VMess Only)

### Nginx 설치
```
Ubuntu: $ apt-get install nginx
CentOS: $ yum install nginx
```

 - 도메인 등록
```
$ vi /etc/nginx/sites-available/default

# server_name _; 의 _ 부분을
# 자신의 도메인을 넣음.
# server_name example.com www.example.com;
```
systemctl start nginx <br>
(webroot로 인증서 발급을 위해 구동 필요)


<br>

### letsencrypt
- 인증서 발급
```
## 'apt-get install letsencrypt' or 'apt-get install certbot'

명령어
$ letsencrypt certonly -d DNS_HOST

Webroot 인증 및 WEB 경로 미리 지정
$ certbot certonly --webroot --webroot-path=/var/www/html -d DNS_HOST
```

<br>

- 인증서 갱신 (crontab 사용)
```
$ certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"

인증서 갱신 테스트
$ certbot renew --dry-run
```
```
# 리다이렉션 설정을 했을 경우, /etc/nginx/conf.d/default.conf 파일 수정 후 적용
## return 301 https://$server_name$request_uri; 주석처리

$ systemctl restart nginx
$ certbot renew
```

<br>

### nginx 설정 (301 다이렉트[80 -> 443], DNS 443 TLS 암호화 적용)
```
$ vi /etc/nginx/conf.d/default.conf

Nginx 폴더 파일을 참고하여 내용 작성
 - ceool.com 5곳 수정
```

systemctl restart nginx

<br>
<br>

## 자동실행 등록
```
systemctl enable v2ray
systemctl enable ufw

-- VMess Only --
systemctl enable nginx
```
