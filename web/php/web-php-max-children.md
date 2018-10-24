# PHP FPM Max Children

在 php fpm 的 error log 發現執行緒不太夠用，需要設定開啟新執行緒

```
[24-Oct-2018 07:43:58] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 08:27:52] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 10:57:17] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 11:37:32] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
```

設定檔路徑

```
/etc/php/7.1/fpm/pool.d/www.conf
```

> pm.max_children 數量：主機記憶體(MB) / 60MB



```
pm.max_children = 40
pm.start_servers = 15
pm.min_spare_servers = 15
pm.max_spare_servers = 25
pm.max_requests = 500
pm = dynamic
```

## Process Manager 行程調配規則參數說明



| 參數  | 說明  |
|---|---|---|---|---|
| pm | PHP-FPM Process Manager 行程調配規則 |
| pm.start_servers | PHP-FPM 服務在一開始啟動時，要配置多少個行程 |
| pm.min_spare_servers | PHP-FPM 最小閒置行程的數量 |
| pm.max_spare_servers | PHP-FPM 最大閒置行程的數量 |
| pm.max_requests  | 單一 PHP-FPM 最多可以處理多少個連線，當一個工作行程處理的連線數達到這個值的時候，就會強制關閉此行程，重新產生另一個新的行程  |


```
pm = dynamic
```

| 參數  | 說明  |
|---|---|---|---|---|
| static | 固定行程數量（數量為 pm.max_children） ，效能很好，但很佔記憶體|
| dynamic | 動態行程數量（根據 pm.max_children、pm.start_servers、pm.min_spare_servers、pm.max_spare_servers 動態調整），根據使用量用多少開多少，但當使用量比較低時，會保留一些行程，隨時等著接收新的連線 |
| ondemand | 動態行程數量（根據 pm.max_children、pm.start_servers、pm.min_spare_servers、pm.max_spare_servers 動態調整），用多少開多少 |


設定完成後記得重啟 php-fpm 服務

```
sudo service php7.1-fpm restart
```


## 參考資料
* [Nginx 與 PHP-FPM 最佳化效能設定教學與技巧 - G. T. Wang](https://blog.gtwang.org/linux/nginx-php-fpm-configuration-optimization/)
* [Determining the correct number of child processes for PHP-FPM on NGinx](https://www.kinamo.be/en/support/faq/determining-the-correct-number-of-child-processes-for-php-fpm-on-nginx)
* [【整理】php-fpm.conf中的pm.max_children到底应该设置为多少 – 在路上](https://www.crifan.com/php_fpm_conf_pm_max_children_should_set_which_value/)
