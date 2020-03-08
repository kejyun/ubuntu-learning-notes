# Nginx Multiple Root

在做網站改版的時候，可能因為專案太大而無法一次全部改版，所以希望能夠針對部分的 頁面或API 進行改版，然後慢慢的全部迭代改版完成，為了做到這樣的效果，可以透過 Nginx 指定不同的網址路徑，去執行不同的程式，這邊以 PHP Laravel Framework 為例。

由於舊專案是 `Laravel 4.2` 但新專案是 `Laravel 5.5`，程式改版過程中將改版完成的網址路徑導向至 `Laravel 5.5`，但尚未完成改版的項目則還是繼續導向至 `Laravel 4.2`，由 nginx 判斷請求路徑，去控制判斷要執行哪一部分的程式。


```shell
server {
    listen 80;
    server_name _;
    # 預設使用 Laravel 5.5
    root "/home/deploy/laravel55/public";
    charset utf-8;
    index index.htm index.html index.php;
    error_page 404 /404.html;


    if ($request_uri ~* "^(.*/)index\.php/*(.*)") {
        return 301 $1$2;
    }

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location /elb-status {
        access_log off;
        return 200;
    }

    access_log /var/log/nginx/web-access.log cloudflare;
    error_log  /var/log/nginx/web-error.log error;
    location = /robots.txt { access_log off; log_not_found off; }
    location = /favicon.ico { access_log off; log_not_found off; }

    location ~ ^/assets/ {
        try_files $uri $uri/ /index.php?$args;
        expires max;
        add_header Cache-Control public;
        access_log off;
    }


    location ~ ^/(api|old-uri-path1|old-uri-path2)(.*)$ {
        # 指定舊網址，變更網站路徑為 Laravel 4
        root "/home/deploy/laravel4/current/public";

        # Rewrite $uri=/api/v1/xyz back to just $uri=/xyz
        rewrite ^(.*)$ /$1 break;

        # Try to send static file at $url or $uri/
        # Else try /index.php (which will hit location ~\.php$ below)
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ [^/]\.php(/|$) {
        set $newurl $request_uri;
        # 預設使用 php 7.1 fpm 執行

        fastcgi_pass unix:/run/php/php7.1-fpm.sock;

        if ($newurl ~ ^/(api|old-uri-path1|old-uri-path2)(.*)$) {
            # 變更網站路徑為 Laravel 4.2
            root "/home/deploy/laravel4/current/public";
            # 舊網址使用 php 7.0 fpm 執行
            fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param REQUEST_URI $newurl;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
    }


    proxy_intercept_errors on;
    fastcgi_intercept_errors on;

    # gzip
    gzip on;
    gzip_min_length 1000;
    gzip_types  *;
    gzip_comp_level 3;
    gzip_proxied any;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
}
```

預設是執行 `Laravel 5.5`，而設定的例外路徑則是存取 `Laravel 4.2`

## 參考資料
* [Nginx config for multiple laravel sites based on /api/v1 url paths](https://gist.github.com/mreschke/27bfafb84add38d3bab8)
* [Setting up Django and your web server with uWSGI and nginx — uWSGI 2.0 documentation](https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)
* [教學課程：使用以路徑為基礎的路由與 Application Load Balancer 搭配使用 - Elastic Load Balancing](https://docs.aws.amazon.com/zh_tw/elasticloadbalancing/latest/application/tutorial-load-balancer-routing.html)
* [node.js - nginx + nodejs + php - Stack Overflow](https://stackoverflow.com/questions/13999069/nginx-nodejs-php)
* [debian - nginx server config with multiple locations does not work - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/134087/nginx-server-config-with-multiple-locations-does-not-work)
* [Nginx 的 try_files 指令使用实例 - 运维之美](https://www.hi-linux.com/posts/53878.html)
* [Nginx的try_files参数保证能懂的讲解-10536390-51CTO博客](https://blog.51cto.com/10546390/1754757)
* [Nginx - Changing the server root make location root not working - Stack Overflow](https://stackoverflow.com/questions/35220769/nginx-changing-the-server-root-make-location-root-not-working)
* [php - nginx configuration with multiple location blocks - Stack Overflow](https://stackoverflow.com/questions/34082831/nginx-configuration-with-multiple-location-blocks?rq=1)
* [Understanding Nginx Server and Location Block Selection Algorithms | DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)
* [How to proxy web apps using nginx? · GitHub](https://gist.github.com/soheilhy/8b94347ff8336d971ad0)
* [Nginx multiple server setting](https://blog.bluehat-lab.org/nginx-multiple-server-setting-with-single-ip/)
