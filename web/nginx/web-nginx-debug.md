# Nginx 除錯

## 測試 `site-enabled` 設定

```
nginx -t
```

## 413 Request entity too large

Nginx 預設檔案上傳大小為 100M，可以修改設定值提高上傳檔案大小

```shell
sudo vim /etc/nginx/nginx.conf
```

打開 `nginx.conf` 後，在 `http` 設定值中加入 `client_max_body_size` 並修改上傳檔案大小

> 若沒有此設定值可自行加入

```
http {
    client_max_body_size 100M;
}
```

## 參考資料
* [解決 Nginx 錯誤: 413 Request entity too large](https://www.phpini.com/linux/fix-nginx-error-413-request-entity-too-large)