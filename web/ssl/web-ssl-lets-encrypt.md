# Let's Encrypt

## 安裝

```shell
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
```

**Nginx**

```shell
$ sudo apt-get install python-certbot-nginx
```

**Apache**

```shell
$ sudo apt-get install python-certbot-apache
```

## 執行憑證驗證

```shell
$ sudo certbot
```

## 自動更新憑證

<!-- ```shell
./certbot-auto renew --pre-hook "service nginx stop" --post-hook "service nginx start"
``` -->

**更新憑證**

```shell
sudo certbot renew
```

**測試更新憑證**

```shell
sudo certbot renew --dry-run
```

*Crontab 自動更新*

需要輸入完整執行檔案名稱，然後指定需要自動更新時間

<!-- ```shell
30 2 * * 1  /full-path-to-certbot/certbot-auto renew --pre-hook "service nginx stop" --post-hook "service nginx start"
``` -->

```shell
30 2 * * 1  certbot renew
```

## /etc/ssl/certs/dhparam.pem

在安裝 SSL 憑證時，會出現下列訊息時，可以使用 `openssl` 去新增 `dhparam.pem` 憑證

```shell
nginx: [emerg] BIO_new_file("/etc/ssl/certs/dhparam.pem") failed (SSL: error:02001002:system library:fopen:No such file or directory:fopen('/etc/ssl/certs/dhparam.pem','r') error:2006D080:BIO routines:BIO_new_file:no such file)
```

使用下列指令可以新增 `dhparam.pem` 憑證

```shell
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```


## 移除指定憑證

如果不需要 SSL 憑證了，可以使用下列指令將網域移除

```
sudo certbot delete --cert-name kejyun.com
```


## 解決 certbot-auto 版本過舊問題

在使用舊版 `certbot-auto` 安裝憑證時，因為 `certbot-auto` 會自動更新自己的版本，導致發生 python 版本過舊導致無法驗證憑證

```
Installing Python packages...
/opt/eff.org/certbot/venv/bin/python: No module named pip.__main__; 'pip' is a package and cannot be directly executed
Traceback (most recent call last):
  File "/tmp/tmp.OGGbtieawI/pipstrap.py", line 177, in <module>
    sys.exit(main())
  File "/tmp/tmp.OGGbtieawI/pipstrap.py", line 149, in main
    pip_version = StrictVersion(check_output([python, '-m', 'pip', '--version'])
  File "/usr/lib/python2.7/subprocess.py", line 544, in check_output
    raise CalledProcessError(retcode, cmd, output=output)
subprocess.CalledProcessError: Command '['/opt/eff.org/certbot/venv/bin/python', '-m', 'pip', '--version']' returned non-zero exit status 1
```

可以使用 `--no-self-upgrade` 參數，讓 `certbot-auto` 不要自動更新版本


```
wget https://raw.githubusercontent.com/certbot/certbot/75499277be6699fd5a9b884837546391950a3ec9/certbot-auto
chmod +x ./certbot-auto
./certbot-auto --no-self-upgrade
```



## 參考資料
* [How to install Certbot on Ubuntu 16.04 (Auto Cert Renew!) - Ceos3c](https://www.ceos3c.com/open-source/install-certbot-ubuntu-16-04-auto-cert-renew/)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04#step-5-%E2%80%94-verifying-certbot-auto-renewal)
* [Get Certbot — Certbot 0.25.0.dev0 documentation](https://certbot.eff.org/docs/install.html)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)
* [How To Secure Apache with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)
* [SSL and nginx missing file: /etc/ssl/certs/dhparam.pem · Issue #10 · ummjackson/mastodon-guide](https://github.com/ummjackson/mastodon-guide/issues/10)
* [Correct Way to Delete a Certbot SSL Certificate – Matthew Hagemann – Medium](https://medium.com/@mhagemann/correct-way-to-delete-a-certbot-ssl-certificate-e8ee123e6e01)
* [With 0.32.0, certbot-auto stopped working for some EOL distributions · Issue #6824 · certbot/certbot · GitHub](https://github.com/certbot/certbot/issues/6824#issuecomment-470440436)
