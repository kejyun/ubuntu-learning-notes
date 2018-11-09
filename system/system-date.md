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


## /usr/share/zoneinfo/iso3166.tab: No such file or directory

```
$ sudo tzselect
/usr/bin/tzselect: line 180: /usr/share/zoneinfo/iso3166.tab: No such file or directory
/usr/bin/tzselect: time zone files are not set up correctly
```

若在設定時區時出現 `/usr/share/zoneinfo/iso3166.tab: No such file or directory` 問題，可以重新安裝時區套件即可解決

```shell
sudo apt-get install tzdata
```

## 參考資料
* [\[Ubuntu\]Ubuntu 更改時區 | Philip@Swarchy](https://philipatswarchy.wordpress.com/2007/03/19/ubuntu-change-time-zone/)
* [Debian User Forums • View topic - Setting time zone](http://forums.debian.net/viewtopic.php?t=15348)
