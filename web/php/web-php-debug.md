# PHP FPM 除錯


## recv() failed (104: Connection reset by peer) while reading response header from upstream


*Nginx 錯誤 Log*

在 nginx 發現 php-fpm.sock 的錯誤

```
2018/10/24 03:47:05 [error] 1231#1231: *324 recv() failed (104: Connection reset by peer) while reading response header from upstream, client: 172.31.22.62, server: _, request: "POST /post/article HTTP/1.1", upstream: "fastcgi://unix:/run/php/php7.1-fpm.sock:", host: "kejyun.com"
```


*PHP 錯誤 log*

在 php fpm error log 發現 `pm.max_children setting (5), consider raising it` 的錯誤

```
sudo vim /var/log/php7.1-fpm.log
```

```
[24-Oct-2018 01:09:33] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 07:20:16] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 07:43:58] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 08:27:52] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
```


*提高請求中斷時間（request_terminate_timeout）*

```
sudo vim /etc/php/7.1/fpm/pool.d/www.conf
```

```
request_terminate_timeout = 300
```

*提高 Server fastcgi read timeout*

```
vim /etc/nginx/sites-available/kejyun.com
```

```
location ~ \.php$ {
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
}
```

*提高 php fpm max_children*

請參考 [PHP FPM Max Children](./web-php-max-children.md) 這篇文章，根據自己主機的狀況需求去提高可以使用的 php-fpm children 數量，提高存取效率



## 參考資料
* [Increase PHP script execution time with Nginx](https://easyengine.io/tutorials/php/increase-script-execution-time/)
* [php-fpm超時時間設置request_terminate_timeout分析 - 掃文資訊](https://hk.saowen.com/a/00daab29e1dfee5e8354f200fe27332a159c0de9dff687600e312585ef630f7a)
* [php-fpm中的request_terminate_timeout最好不要设置](http://www.onepx.com/php-fpm-request-terminate-timeout.html)
* [解决 recv() failed (104: Connection reset by peer) while reading response header from upstream](http://www.apkfuns.com/-e8-bd-ac-e8-a7-a3-e5-86-b3-recv-failed-104-connection-reset-by-peer-while-reading-response-header-from-upstream/)
* [经验之谈：nginx php 502 bad gateway 解决方法_壹聚教程网](http://www.111cn.net/sys/nginx/56291.htm)
