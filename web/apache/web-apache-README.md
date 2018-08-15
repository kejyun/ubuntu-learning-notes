# Apache

## 測試設定檔

```shell
apachectl configtest
```

## 啟動關閉 Apache 服務

```shell
sudo service apache2 stop

sudo service apache2 start

sudo service apache2 restart
```


## 關閉 Apache 避免開機執行 Apache

```shell
# 關閉
sudo update-rc.d -f apache2 remove
```

```shell
# 恢復自動啟動
sudo update-rc.d apache2 defaults
```

```shell
sudo update-rc.d apache2 disable
sudo update-rc.d apache2 enable
```

## 參考資料
* [How to disable apache2 server from auto starting on boot - Ask Ubuntu](https://askubuntu.com/questions/170640/how-to-disable-apache2-server-from-auto-starting-on-boot)
* [Microsoft Word - Installing_and_working_with_ Apache_web server](https://www.ce.teiep.gr/e-class/modules/document/file.php/129/Linux_networking/Installing_and_working_with__Apache_web_server.pdf)
