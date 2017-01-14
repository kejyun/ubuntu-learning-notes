# Swap

```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1
```

> if 參數: 輸入的檔名，/dev/zero代表的是空的檔案

> of 參數: 輸出的檔名

> bs 參數: block size，每個區塊大小單位

> count 參數: swap 檔案要佔用的單位大小

> Swap 大小: bs * count = 1M * 1024 = 1024M


## 參考資料
* [在Linux系統中動態增加swap檔案空間的方法](http://xx3d2ybnf.pixnet.net/blog/post/143381208-%E5%9C%A8linux%E7%B3%BB%E7%B5%B1%E4%B8%AD%E5%8B%95%E6%85%8B%E5%A2%9E%E5%8A%A0swap%E6%AA%94%E6%A1%88%E7%A9%BA%E9%96%93%E7%9A%84%E6%96%B9%E6%B3%95)
* [How To Add Swap Space on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)