# 開機自動執行


## 開機自動執行指定程式

將開機需要執行的指定程式寫入 `/etc/rc.local` 即可

```shell
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

# 開機自動啟動
bash /home/ubuntu/scripts/boot.sh > /dev/null 2>&1
# 自動部署程式
bash /home/ubuntu/scripts/deploy.sh > /dev/null 2>&1
```

輸入下列指令可以檢查目前這個服務執行的狀態

```
systemctl status rc.local.service
```


```
$ systemctl status rc.local.service
● rc-local.service - /etc/rc.local Compatibility
   Loaded: loaded (/lib/systemd/system/rc-local.service; enabled-runtime; vendor preset: enabled)
  Drop-In: /lib/systemd/system/rc-local.service.d
           └─debian.conf
   Active: active (exited) since Wed 2019-09-25 15:52:07 CST; 33min ago
     Docs: man:systemd-rc-local-generator(8)
  Process: 793 ExecStart=/etc/rc.local start (code=exited, status=0/SUCCESS)

Sep 25 15:54:33 ip-172-31-25-154 rc.local[793]: Deploy Done
```


## 使用 crontab 重新開機後自動執行

```
crontab -e
```


```
# 開機自動啟動
@reboot bash /home/ubuntu/scripts/boot.sh > /dev/null 2>&1
# 自動部署程式
@reboot bash /home/ubuntu/scripts/deploy.sh > /dev/null 2>&1
```


# 參考資料
* [boot - Run bash script on startup - Raspberry Pi Stack Exchange](https://raspberrypi.stackexchange.com/questions/15475/run-bash-script-on-startup)
* [Schedule Tasks on Linux Using Crontab | kvz.io](https://kvz.io/schedule-tasks-on-linux-using-crontab.html)
*
