# PHP FPM Max Children

在 php fpm 的 error log 發現執行緒不太夠用，需要設定開啟新執行緒

```
[24-Oct-2018 07:43:58] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 08:27:52] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 10:57:17] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 11:37:32] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
```

*設定檔路徑*

```
/etc/php/7.1/fpm/pool.d/www.conf
```

在設定檔中可以看到類似以下的設定方式

```
pm.max_children = 10
pm.start_servers = 4
pm.min_spare_servers = 2
pm.max_spare_servers = 8
```

## Process Manager 行程調配規則參數說明



| 參數  | 說明  |
|---|---|
| pm | PHP-FPM Process Manager 行程調配規則 |
| pm.start_servers | PHP-FPM 服務在一開始啟動時，要配置多少個行程 |
| pm.min_spare_servers | PHP-FPM 最小閒置行程的數量 |
| pm.max_spare_servers | PHP-FPM 最大閒置行程的數量 |
| pm.max_requests  | 單一 PHP-FPM 最多可以處理多少個連線，當一個工作行程處理的連線數達到這個值的時候，就會強制關閉此行程，重新產生另一個新的行程  |


```
pm = dynamic
```

| 參數  | 說明  |
|---|---|
| static | 固定行程數量（數量為 pm.max_children） ，效能很好，但很佔記憶體|
| dynamic | 動態行程數量（根據 pm.max_children、pm.start_servers、pm.min_spare_servers、pm.max_spare_servers 動態調整），根據使用量用多少開多少，但當使用量比較低時，會保留一些行程，隨時等著接收新的連線 |
| ondemand | 動態行程數量（根據 pm.max_children、pm.start_servers、pm.min_spare_servers、pm.max_spare_servers 動態調整），用多少開多少 |


## pm.max_children 最大執行程序數量

最大執行程序數量是針對自己的主機規格不同去做設定的，當你主機有較大的記憶體，則可以設定更多的執行程序


> Total Max Processes = (Total Ram - (Used Ram + Buffer)) / (Memory per php process)

> 最大執行程序 = (總記憶體 - (已使用記憶體 + Buffer)) / (每個 php 執行緒需要記憶體)

> pm.max_children 數量：可用記憶體(MB) / 大約 50MB

*確認每個 php 執行緒需要記憶體*

因為每個 php 執行的記憶體及效率會針對不同的程式環境不同，而有不同的消耗記憶體，可以使用下列指令去取得 php-fpm 執行緒消耗的記憶體數量

> ps -ylC `php-fpm 執行緒完整名稱` --sort:rss

*範例*

其中的 `php-fpm 執行緒完整名稱` 要看看你系統是用哪個版本的 php，而會有不同版本名稱的 php-fpm，像是：

```
ps -ylC php-fpm --sort:rss
ps -ylC php-fpm7.0 --sort:rss
ps -ylC php-fpm7.1 --sort:rss
```

在列出來 php-fpm 使用的資源後，可以看到 `RSS` 欄位為使用的記憶體大小，`31692` 表示為 `31692 kb`，大概就是 `31M`

```
$ ps -ylC php-fpm7.1 --sort:rss
S   UID   PID  PPID  C PRI  NI   RSS    SZ WCHAN  TTY          TIME CMD
S    33 22275 16692  1  80   0 31692 212882 -     ?        00:00:00 php-fpm7.0
S    33 22092 16692  1  80   0 40708 231684 -     ?        00:00:02 php-fpm7.0
S    33 22023 16692  1  80   0 41136 231888 -     ?        00:00:05 php-fpm7.0
S    33 22038 16692  1  80   0 41204 231744 -     ?        00:00:04 php-fpm7.0
```

但每個 php-fpm 程序的記憶體不盡相同，我們可以用下列指令去計算平均使用的記憶體大小是多少

> ps --no-headers -o "rss,cmd" -C `php-fpm 執行緒完整名稱`  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'


也是一樣針對你系統是用哪個版本的 php，而會有不同版本名稱的 php-fpm，像是：

```
ps --no-headers -o "rss,cmd" -C php-fpm  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'
ps --no-headers -o "rss,cmd" -C php-fpm7.0  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'
ps --no-headers -o "rss,cmd" -C php-fpm7.1  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'
```

抓出來的平均使用記憶體大概為 `44 MB`，但為了抓 Buffer，我們大概抓 `50 MB` 左右


```
$ ps --no-headers -o "rss,cmd" -C php-fpm7.0  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'
44Mb
```


*確認可用的記憶體數量*

> php-fpm 可以用的記憶體為：剩餘還可用的記憶體 + (php-fpm 平均使用記憶體 * 目前 php-fpm 開啟數量)

使用 `free -m` 取得目前的記憶體使用狀況，總記憶體是 `3272 M`，已經使用了 `1539M`，剩餘還可用的記憶體有 `1021 M`

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:           3762        1539        1021          84        2417        2686
Swap:             0           0           0
```

而從下列指令中，取得 php-fpm 平均使用記憶體為 `44M`

> ps --no-headers -o "rss,cmd" -C `php-fpm 執行緒完整名稱`  | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'

從下列指令中列出目前所有 php-fpm 執行緒數量為 `41 個`（直接去計算清單出現幾行 php-fpm 資源狀況即可知道總共開了幾個 php-fpm 執行緒）

> ps -ylC `php-fpm 執行緒完整名稱` --sort:rss

所以 php-fpm 可以用的記憶體為 `1021 M` + `44 M` * `41 個` = 2743M


> 最大執行程序數量 = php-fpm 可以用的記憶體 / 每個 php 執行緒需要記憶體(Buffer 數值)

2743M / 50M = 54.86

為了不要讓所有記憶題全部消耗完，而沒有空間去執行其他非 php 的程序，所以我們這邊會抓 `pm.max_children` 大概最多使用個 `50 個` 執行緒

```
pm.max_children = 50
```

start_server 官方建議算法是 min_spare_servers + (max_spare_servers - min_spare_servers) / 2

min_spare_servers 跟 max_spare_servers 就取個大概就好，不要讓他一直開開關關即可

```
pm.start_servers = 20
pm.min_spare_servers = 13
pm.max_spare_servers = 26
```

## 防止 php-fpm 執行緒處理過多 request 導致 Memory leak

```
pm.max_requests = 500
request_terminate_timeout = 30s
```

讓每個子執行緒同時處理超過 500 個 request 就自動移除此執行緒，避免 Memory Leak 執行緒卡在那邊動彈不得

而執行時間超過 30 秒的請求則自動中斷掉，建議設定為與 `php.ini` 的 `max_execution_time` 相同

最後設定值會像是：

```
pm = dynamic
pm.max_children = 50
pm.start_servers = 20
pm.min_spare_servers = 13
pm.max_spare_servers = 26
pm.max_requests = 500
request_terminate_timeout = 30s
```

## 記錄觀察

為了持續觀察 php-fpm 執行序地處理狀況，我們可以把請求執行時間過慢的 Request 記錄下來，在之後後續可以持續做除錯改進，這裡設定紀錄執行超過 10 秒的執行緒狀況

```
; slowlog = /var/log/php7.0-fpm-$pool.log.slow
slowlog = /var/log/php7.1-fpm-$pool.log.slow
request_slowlog_timeout = 10s
```

設定完成後可以進行測試，確認改過的設定檔案沒有問題

```
php-fpm7.0 -t
php-fpm7.1 -t
```

```
$ php-fpm7.1 -t
[03-Jul-2019 15:53:15] NOTICE: configuration file /etc/php/7.1/fpm/php-fpm.conf test is successful
```

確認後記得重啟 php-fpm 服務即可生效


```
sudo service php7.0-fpm restart
sudo service php7.1-fpm restart
```


## 參考資料
* [Nginx 與 PHP-FPM 最佳化效能設定教學與技巧 - G. T. Wang](https://blog.gtwang.org/linux/nginx-php-fpm-configuration-optimization/)
* [Determining the correct number of child processes for PHP-FPM on NGinx](https://www.kinamo.be/en/support/faq/determining-the-correct-number-of-child-processes-for-php-fpm-on-nginx)
* [【整理】php-fpm.conf中的pm.max_children到底应该设置为多少 – 在路上](https://www.crifan.com/php_fpm_conf_pm_max_children_should_set_which_value/)
* [PHP-FPM: Process Management | Servers for Hackers](https://serversforhackers.com/c/php-fpm-process-management)
* [Determining the correct number of child processes for PHP-FPM on NGinx](https://www.kinamo.be/en/support/faq/determining-the-correct-number-of-child-processes-for-php-fpm-on-nginx)
* [How To: Solve PHP-FPM server reached max_children | Webcore Community | Webcore Cloud](https://community.webcore.cloud/tutorials/how_to_solve_php_fpm_server_reached_max_children/)
