# 硬碟空間

## 查詢硬碟使用狀況

```
df -h
```

## 查詢資料夾所佔硬碟空間

```
sudo du -hs {查詢目錄}
```


```
sudo du -hs /home/web/laravel/*
2.6M	/home/web/laravel/app
4.0K	/home/web/laravel/artisan
36K     /home/web/laravel/bootstrap
4.0K	/home/web/laravel/bower.json
4.0K	/home/web/laravel/composer.json
196K	/home/web/laravel/composer.lock
124K	/home/web/laravel/config
132K	/home/web/laravel/database
4.0K	/home/web/laravel/package.json
4.0K	/home/web/laravel/phpunit.xml
7.7G	/home/web/laravel/public
4.0K	/home/web/laravel/readme.md
1.6M	/home/web/laravel/resources
276K	/home/web/laravel/routes
4.0K	/home/web/laravel/server.php
827M	/home/web/laravel/storage
28K     /home/web/laravel/tests
64M     /home/web/laravel/vendor
4.0K	/home/web/laravel/webpack.mix.js
```


## 參考資料
* [Day 15 ubuntu 查詢硬碟使用量df指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10160087)
* [Day 16 ubuntu 查詢檔案或目錄的磁碟使用空間du指令 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10160234)
* [Linux 檢查硬碟使用量 df 指令教學與指令稿範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-df-command-check-disk-space-usage-tutorial-script-example/)
