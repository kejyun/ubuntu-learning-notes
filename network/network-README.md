# 網路

## 列出所有服務 port

```shell
sudo lsof -iTCP -sTCP:LISTEN -P
```

## 安裝 letsencrypt for nginx

```shell
./certbot-auto --nginx
```
