# Nginx 安全性


## 不顯示 Nginx 版本

```
http {
  # 不顯示 Nginx 版本
  server_tokens off;
}
```

## 禁止存取隱藏檔

```
server {
  location ~ /\. {
    access_log off;
    log_not_found off;
    deny all;
  }
}
```

## 其他

```
events {
  # 使用 epoll 效能較好
  use epoll;

  # 可同時接受多條連線
  multi_accept on;
}
```

## 參考資料
* [Nginx 與 PHP-FPM 最佳化效能設定教學與技巧 - G. T. Wang](https://blog.gtwang.org/linux/nginx-php-fpm-configuration-optimization/)
