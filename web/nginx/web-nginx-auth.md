# 認證

## 設定需要驗證網址路徑

在網站設定中，設定需要驗證的目錄

```shell
location / {
    # 驗證方式
    auth_basic "Restricted";
    # 驗證檔案
    auth_basic_user_file htpasswd;
    try_files $uri $uri/ /index.php?$query_string;
}
```

##  驗證檔案

在 `/etc/nginx/` 目錄下建立 `htpasswd` 檔案（`/etc/nginx/htpasswd`）

```shell
/etc/nginx $ touch htpasswd
```



## 建立驗證密碼

使用 `openssl` 建立加密的密碼

```shell
openssl passwd
Password:
Verifying - Password:
ZEmF5bjIAXRb6
```

## 設定帳號密碼

`auth_basic_user_file` 檔案格式

```shell
帳號1:密碼1
帳號2:密碼2:註解
帳號3:密碼3
```

將建立的密碼設定在 `/etc/nginx/htpasswd` 檔案中

```shell
test:ZEmF5bjIAXRb6
```

設定完成之後，重新啟動 nginx，就可以使用 nginx 限制連線存取的帳號密碼了～



## 參考資料
* [NGINX 設定密碼認證與限制可存取的 IP 位址，控制頻寬與連線數 - G. T. Wang](https://blog.gtwang.org/linux/nginx-restricting-access-authenticated-user-ip-address-tutorial/)