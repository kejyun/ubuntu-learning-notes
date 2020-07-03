# 更新憑證

在執行 curl 的時候，主機噴出 `server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt` 憑證過期的錯誤訊息，此時需要重新更新主機憑證資訊

```shell
$ curl https://www.google.com
curl: (60) server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
More details here: http://curl.haxx.se/docs/sslcerts.html

curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn\'t adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you\'d like to turn off curl\'s verification of the certificate, use
 the -k (or --insecure) option.
```

*更新主機套件*

因為主機套件沒有更新的話，抓取的憑證資料可能還會是舊的憑證，所以在更新憑證之前要先更新主機套件版本

```shell
sudo apt-get update
```

*更新主機憑證*

更新完主機套件後，就可以重新安裝憑證套件程式，讓主機抓取最新的憑證

```shell
sudo apt-get install --reinstall ca-certificates
```

若套件已是最新的，要直接更新憑證，可以使用下列指令直接更新即可

```shell
sudo update-ca-certificates
```

# 參考資料
* [服务器证书验证失败CAfile:/etc/ssl/certs/ca certificates.crt CRLfile: 无_ssl-certificate_酷徒编程知识库](https://hant-kb.kutu66.com/ssl-certificate/post_12157107)
* [各系統更新 ca (root)根憑證的方法 – Mr. 沙先生](https://shazi.info/%E5%90%84%E7%B3%BB%E7%B5%B1%E6%9B%B4%E6%96%B0-ca-root%E6%A0%B9%E6%86%91%E8%AD%89%E7%9A%84%E6%96%B9%E6%B3%95/)
