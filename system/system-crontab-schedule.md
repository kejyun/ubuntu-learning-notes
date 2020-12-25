# 排程

## 進入排程設定檔案

```
sudo crontab -l
sudo crontab -e
```

## 排程格式

```
* * * * * /bin/execute/this/script.sh
```

`*` 位置代表意義

| 位置（由左至右） | 說明  | 範圍  |
|---|---|---|
| 1 | 分鐘 | 0-59 |
| 2 | 小時 | 0-23 |
| 3 | 每月第幾天 | 1-31 |
| 4 | 第幾個月 | 1-12 |
| 5 | 星期幾 | 0-6，0: 禮拜日 |

> `*` 代表每次條件都符合

## 特殊字元

```
@reboot     開機後執行一次
@yearly     一年執行一次           "0 0 1 1 *"
@annually   (與 @yearly 功能相同)
@monthly    一個月執行一次         "0 0 1 * *"
@weekly     一週執行一次           "0 0 * * 0"
@daily      一天執行一次           "0 0 * * *"
@midnight   (與 @daily 功能相同)
@hourly     一小時執行一次         "0 * * * *"
```

## 將執行的結果丟到垃圾桶不顯示

```
*/10 * * * * /bin/execute/this/script.sh > /dev/null 2>&1
```

## 測試 crontab

直接執行 `crontab` 指令就可以知道 crontab 是否有設定正確

```shell
crontab path/to/crontab/file
```

如果有錯誤的狀況發生就會顯示相關的錯誤訊息

```shell
"/etc/crontab":165: bad day-of-month
errors in crontab file, can\'t install.
```


# 參考資料
* [Schedule Tasks on Linux Using Crontab | kvz.io](https://kvz.io/schedule-tasks-on-linux-using-crontab.html)
* [Crontab.guru - The cron schedule expression editor](https://crontab.guru/#)
* [cron - Is there a way to validate /etc/crontab’s format? - Server Fault](https://serverfault.com/questions/43733/is-there-a-way-to-validate-etc-crontab-s-format)
