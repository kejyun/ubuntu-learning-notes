# Virtual Host


```shell
server {
    listen 80;
    server_name kejyun.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name kejyun.com;

    root "/home/kejyun/tnl/web/tnl_frontend_laravel/public";
    charset utf-8;
    index index.htm index.html index.php;
    error_page 404 /404.html;

    # add_header X-Frame-Options SAMEORIGIN;
    #add_header X-W "if you are reading this, come talk to us. job(at)thenewslens.com";

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }


    location ~ [^/]\.php(/|$) {
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            if (!-f $document_root$fastcgi_script_name) {
                    return 404;
            }
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_pass unix:/run/php/php7.1-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
    }


    location ~* \.(jpg|jpeg|gif|png|css|js|ico|xml)$ {
        access_log  off;
        log_not_found off;
    }

    error_log  /var/log/nginx/kejyun.com-error.log error;
    access_log /var/log/nginx/kejyun.com.access.log;

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_dhparam /etc/ssl/certs/dhparam.pem;
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM    -SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES    256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES25    6-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3    -SHA:!KRB5-DES-CBC3-SHA';
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:50m;

    ssl_certificate /etc/letsencrypt/live/kejyun.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/kejyun.com/privkey.pem; # managed by Certbot
}
```
