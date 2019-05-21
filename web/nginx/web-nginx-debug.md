# Nginx 除錯

## 回傳參數

```
location ~ \.php$ {
    return 200 $args; add_header Content-Type text/plain;
    return 200 $uri; add_header Content-Type text/plain;
    return 200 $document_root; add_header Content-Type text/plain;
    return 200 $request_uri; add_header Content-Type text/plain;
}
```

* $args：get 參數（e.g. ?page=2）
* $uri：網址路徑（e.g. /tag/laravel）
* $document_root：網頁路徑（e.g. /var/web/laravel/public）
* $request_uri：完整請求 URI（/tag/laravel?page=2）

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

## could not build server_names_hash, you should increase server_names_hash_bucket_size: 64

在新增 Nginx 主機時，發現網址名稱有過長的問題


```
$ systemctl status nginx.service

● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Thu 2019-03-07 08:28:21 UTC; 7s ago
  Process: 9595 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
  Process: 9827 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=1/FAILURE)
 Main PID: 31527 (code=exited, status=0/SUCCESS)

Mar 07 08:28:21 tnl-dev systemd[1]: Starting A high performance web server and a reverse proxy server...
Mar 07 08:28:21 tnl-dev nginx[9827]: nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size: 64
Mar 07 08:28:21 tnl-dev nginx[9827]: nginx: configuration file /etc/nginx/nginx.conf test failed
Mar 07 08:28:21 tnl-dev systemd[1]: nginx.service: Control process exited, code=exited status=1
Mar 07 08:28:21 tnl-dev systemd[1]: Failed to start A high performance web server and a reverse proxy server.
Mar 07 08:28:21 tnl-dev systemd[1]: nginx.service: Unit entered failed state.
Mar 07 08:28:21 tnl-dev systemd[1]: nginx.service: Failed with result 'exit-code'.
```

此時可以到 `/etc/nginx/nginx.conf` 將 `server_names_hash_max_size` 設定更高的數值

```
server_names_hash_max_size 512;
server_names_hash_max_size 1024;
```

設定完後重新啟動 Nginx 就可以正常啟動了

## 參考資料
* [解決 Nginx 錯誤: 413 Request entity too large](https://www.phpini.com/linux/fix-nginx-error-413-request-entity-too-large)
* [nginx配置server_names_hash_max_size放在什么地方？ - SegmentFault 思否](https://segmentfault.com/q/1010000004853184)
