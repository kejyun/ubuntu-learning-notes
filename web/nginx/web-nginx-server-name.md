# Nginx Server Name

## 取得所有的 Server Name

使用 `_` 特殊符號取得所有傳進來的 Server Name

```
server {
    listen       80;
    server_name  example.org
                 www.example.org
                 ""
                 192.168.1.1
                 ;
    ...
}
```

```
server {
    listen       80  default_server;
    server_name  _;
    return       444;
}
```


# 參考資料
* [Nginx Server names](http://nginx.org/en/docs/http/server_names.html)
