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
bash /home/ubuntu/scripts/boot.sh
# 自動部署程式
bash /home/ubuntu/scripts/deploy.sh
```
