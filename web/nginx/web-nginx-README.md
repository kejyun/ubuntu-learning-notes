# nginx

## 強制重新導向 HTTP 到 HTTPS

修改 `/etc/nginx/site-available/default` 裡的網址設定

### 重新導向所有網址

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    return 301 https://$host$request_uri;
}
```

### 重新導向指定網址

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}
```