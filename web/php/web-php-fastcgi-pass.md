# fastcgi_pass

> `fastcgi_pass` 是指定用來解譯 PHP 程式的端口

## 設定檔

路徑：

> /etc/nginx/site-enabled/xxx

```
location ~ \.php$ {
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    # fastcgi_pass 127.0.0.1:9000;
}
```

路徑：

> /etc/php/7.0/fpm/pool.d/www.conf

```
listen = /run/php/php7.0-fpm.sock
;listen = 127.0.0.1:9000
```



## 參考資料
* [Understanding and Implementing FastCGI Proxying in Nginx | DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-and-implementing-fastcgi-proxying-in-nginx)
* [PHP FastCGI Example | NGINX](https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/)