# 虛擬記憶體（SWAP）

## 確認記憶體使用狀況

```shell
free -m
```

```shell
$ free -m
              total        used        free      shared  buff/cache   available
Mem:            990         164         197          38         628         584
Swap:             0           0           0

$ free -m
             total       used       free     shared    buffers     cached
Mem:           992        867        125         20         63        516
-/+ buffers/cache:        287        704
Swap:         1023          0       1023
```

## 設定 Swap File

設定 4G 的 SWAP 檔案

```shell
sudo -s
cd /var
fallocate -l 4G swapfile.1
chmod 600 swapfile.1
```

設定此檔案只能被 root 讀寫，以防安全性問題

## 啟用 SWAP

```
mkswap /var/swapfile.1
swapon /var/swapfile.1
```

## 查詢目前 SWAP 路徑

```shell
$ swapon -s
```

```shell
$ swapon -s
Filename				Type		Size	Used	Priority
/var/swapfile.1                        	file    	4194300	0	-1
```

## 讓 Swap file 重開機後也能自動啟動

請確認您的 swap 檔案路徑為 `/var/swapfile.1`，如果有自定義其他 swap 檔案名稱，請將 swap 檔案路徑改為您的路徑，否則會導致無法開機

```shell
echo "/var/swapfile.1    none    swap    sw    0    0" >> /etc/fstab
```

## 使用 htop 監控記憶體使用狀況

```shell
sudo apt-get install htop
```

## 設定 swappiness 調整 swap 使用優先權

`swappiness` 數值介於 0~100，越接近 100 則系統越常使用 swap，越接近 0 則系統會越常使用 RAM，預設為 60，為了系統效能，可以盡量將此設定值調低

**查詢 swappiness**

```
cat /proc/sys/vm/swappiness
```

**設定 swappiness**

```
sysctl vm.swappiness=10
```

**設定重新開機時 swappiness 數值**

```
echo "vm.swappiness = 10" >> /etc/sysctl.conf
```



## 參考資料
* [在 Ubuntu VPS 上設定虛擬記憶體 (Swap) 來解決RAM不夠用的問題 « 峰哥的技術日誌](http://fonger.logdown.com/posts/2015/02/01/setting-swap-for-ubuntu-to-solve-out-of-memory)
* [memory - How do I find out if I have a swap partition on my hard drive? - Ask Ubuntu](https://askubuntu.com/questions/159783/how-do-i-find-out-if-i-have-a-swap-partition-on-my-hard-drive)
