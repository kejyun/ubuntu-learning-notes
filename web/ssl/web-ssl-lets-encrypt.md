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

## 參考資料
* [How to install Certbot on Ubuntu 16.04 (Auto Cert Renew!) - Ceos3c](https://www.ceos3c.com/open-source/install-certbot-ubuntu-16-04-auto-cert-renew/)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04#step-5-%E2%80%94-verifying-certbot-auto-renewal)
* [Get Certbot — Certbot 0.25.0.dev0 documentation](https://certbot.eff.org/docs/install.html)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)
* [How To Secure Apache with Let's Encrypt on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04)
