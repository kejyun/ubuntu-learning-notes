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

## Worker 設定

worker_processes 可以設定 worker 行程的數量，可以設定成 CPU 的核心數

```
# 4 核心的 CPU
worker_processes 4;
```

設為自動偵測 CPU 核心數

```
# 自動偵測 CPU 核心數
worker_processes auto;
```

設定每個 worker 可同時處理的連線數上限值

```
events {
  # 同時可處理 1024 條連線
  worker_connections 1024;
}
```

**連線數上限值**

> worker_processes * worker_connections

##  參考資料
* [Redirect all HTTP requests to HTTPS with Nginx](https://bjornjohansen.no/redirect-to-https-with-nginx)
