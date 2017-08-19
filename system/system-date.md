# 時間

## 時區設定

### 依選項選擇你在的時區

```shell
sudo tzselect
```

### 更換時區檔

```shell
sudo cp /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```

### 對時


```shell
sudo apt-get install ntpdate
sudo ntpdate time.stdtime.gov.tw
```

### 時間寫入 BIOS

```shell
sudo hwclock -w
```



### 每日對時

```shell
sudo crontab -e
```
*cronjob*

```shell
0 0 * * *  /usr/sbin/ntpdate time.stdtime.gov.tw > /dev/null
```

> 若 ntpdate 路徑與我不太一樣，可以使用 `which ntpdate` 去查看看您的真實路徑為何


## 參考資料
* [[Ubuntu]Ubuntu 更改時區 | Philip@Swarchy](https://philipatswarchy.wordpress.com/2007/03/19/ubuntu-change-time-zone/)
