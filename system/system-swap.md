# Swap

## Swap 建立指令

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
```

> if 參數: 輸入的檔名，/dev/zero代表的是空的檔案

> of 參數: 輸出的檔名

> bs 參數: block size，每個區塊大小單位

> count 參數: swap 檔案要佔用的單位大小

> Swap 大小: bs * count = 1M * 1024 = 1024M

### 將已建好的檔案形式的儲存空間設定為 swap 的檔案形態

```shell
sudo /sbin/mkswap /var/swap.1
```


### 立即啟動檔案形式的swap檔案空間

```shell
sudo /sbin/swapon /var/swap.1
```

### 讓 Swap file 重開機後也能自動啟動

```shell
sudo echo "/var/swap.1    none    swap    sw    0    0" >> /etc/fstab
```

### 完整指定

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1
```

### 調整 swappiness

`swappiness` 參數是一個介於 0~100 的數值，當數值越接近100，代表系統越積極使用Swap，當數值越接近0，系統將會盡量使用RAM。預設值是 60

***取得現有 swappiness 數值 ***

```shell
cat /proc/sys/vm/swappiness
```

***設定 swappiness 數值 ***

```shell
sysctl vm.swappiness=10
```

*** 重開機後也能生效設定的 swappiness 數值 ***

```shell
sudo echo "vm.swappiness = 10" >> /etc/sysctl.conf
```


## 移除 Swap

```shell
sudo swapoff /var/swap.1
sudo rm /var/swap.1
sudo gedit /etc/fstab  # 並將 /var/swap.1 swap swap defaults 0 0 那一行資料料刪除
```

## 參考資料
* [在Linux系統中動態增加swap檔案空間的方法](http://xx3d2ybnf.pixnet.net/blog/post/143381208-%E5%9C%A8linux%E7%B3%BB%E7%B5%B1%E4%B8%AD%E5%8B%95%E6%85%8B%E5%A2%9E%E5%8A%A0swap%E6%AA%94%E6%A1%88%E7%A9%BA%E9%96%93%E7%9A%84%E6%96%B9%E6%B3%95)
* [在 Ubuntu VPS 上設定虛擬記憶體 (Swap) 來解決RAM不夠用的問題 « 峰哥的技術日誌](http://fonger.logdown.com/posts/2015/02/01/setting-swap-for-ubuntu-to-solve-out-of-memory)
* [How To Add Swap Space on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)