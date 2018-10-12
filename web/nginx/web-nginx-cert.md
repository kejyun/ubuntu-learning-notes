# Nginx 憑證


## 建立放憑證目錄

```
sudo mkdir /etc/nginx/ssl
```

## 建立憑證

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt
```

```
Country Name (2 letter code) [AU]:TW
State or Province Name (full name) [Some-State]:Taiwan
Locality Name (eg, city) []:Taipei
Organization Name (eg, company) [Internet Widgits Pty Ltd]:My Company
Organizational Unit Name (eg, section) []:My Unit
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:kj@kejyun.com
```

## 加入憑證至設定檔

```
server {
  listen 80 default_server;
  listen [::]:80 default_server;

  # 加入 SSL 設定
  listen 443 ssl default_server;
  listen [::]:443 ssl default_server;

  # 憑證與金鑰的路徑
  ssl_certificate /etc/nginx/ssl/nginx.crt;
  ssl_certificate_key /etc/nginx/ssl/nginx.key;

  # ...
}
```

```
sudo service nginx restart
```

## 參考資料
* [NGINX 設定 HTTPS 網頁加密連線，建立自行簽署的 SSL 憑證 - G. T. Wang](https://blog.gtwang.org/linux/nginx-create-and-install-ssl-certificate-on-ubuntu-linux/)
