# Nginx 參數

## error_page

Nginx 自訂錯誤頁

```
server {
    error_page 404 https://kejyun.com/404; # 錯誤頁
}
```

## proxy_intercept_errors

若被代理伺服器回傳 400 或大於 400 的狀態碼，啟用設定的 error_page 設定，預設為 `off` 不啟用

> Determines whether proxied responses with codes greater than or equal to 300 should be passed to a client or be intercepted and redirected to nginx for processing with the error_page directive.


```
server {
    proxy_intercept_errors on;  
}
```

## fastcgi_intercept_errors


若被 FastCGI 伺服器回傳 300 或大於 300 的狀態碼，啟用設定的 error_page 設定，預設為 `off` 不啟用

> Determines whether FastCGI server responses with codes greater than or equal to 300 should be passed to a client or be intercepted and redirected to nginx for processing with the error_page directive.

```
server {
    proxy_intercept_errors on;  
}
```


## 參考資料
* [Module ngx_http_proxy_module](http://nginx.org/en/docs/http/ngx_http_proxy_module.html)
* [Module ngx_http_fastcgi_module](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html)
* [Nginx代理功能与负载均衡详解 - 张龙豪 - 博客园](https://www.cnblogs.com/knowledgesea/p/5199046.html)
