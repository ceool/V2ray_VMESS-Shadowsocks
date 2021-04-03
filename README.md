
## V2ray
<br>
<Strong>마지막 확인 Date: 2021. 04. 03
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
# vi /usr/local/etc/v2ray/config.json

V2ray 폴더 파일을 참고하여 내용 작성
```

systemctl start v2ray

<br>
<br>

## UFW (방화벽)
### UFW 설치
```
Ubuntu: apt-get install ufw
CentOS: yum install ufw
```

systemctl start ufw

<br>

### UFW 설정
```
# ufw allow 22/tcp

-- VMess Only --
# ufw allow 80/tcp
# ufw allow 443/tcp
```

ufw status <br>
![1](https://user-images.githubusercontent.com/62891711/113473707-2ddef300-94a6-11eb-9da5-94faefd57070.png)



<br>
<br>

## Nginx (VMess Only)

### Nginx 설치
```
Ubuntu: apt-get install nginx
CentOS: yum install nginx
```

<br>

### letsencrypt 인증서 발급
```
## 'apt-get install letsencrypt' or 'apt-get install certbot'
letsencrypt certonly -d DNS_HOST
```

<br>

### nginx 설정 (301 다이렉트[80 -> 443], DNS 443 TLS 암호화 적용)
```
# vi /etc/nginx/conf.d/default.conf

Nginx 폴더 파일을 참고하여 내용 작성
 - ceool.com 4곳 수정
```

systemctl start nginx

<br>
<br>

## 자동실행 등록
```
systemctl enable v2ray
systemctl enable ufw

-- VMess Only --
systemctl enable nginx
```
