# 主機名稱 Hostname

## 查詢主機名稱

直接在命令列輸入 `hostname` 就可以看到目前的主機名稱

```shell
hostname
```

## 變更主機 hostname

**1. 修改主機名稱**

使用編輯器修改 `/etc/hostname` 檔案，將 hostname 改為您要呈現的主機名稱

```shell
sudo vim /etc/hostname
```

修正完的 hostname 會長這樣

```shell
# /etc/hostname
kejyun-host-name
```

**2. 加入主機名稱對應 ip**

在修改主機名稱後，因為有些服務 IP 是對應到舊的主機名稱，所以新的名稱需要自行指定到本機 `127.0.0.1`

```shell
sudo vim /etc/hosts
```

```shell
# /etc/hosts
127.0.0.1 kejyun-host-name
```

**3. 強制更改主機名稱**

修改完成後，可以下指令強制更新主機名稱，然後再重新登入就可以看到主機名稱已經被修改了

```shell
sudo hostname -F /etc/hostname
```

```shell
kj@kejyun-host-name:~$
```


## 參考資料
* [ubuntu 14.04 設定hostname](http://computer.jges.mlc.edu.tw/index.php/ubuntu/112-ubuntu-14-04-%E8%A8%AD%E5%AE%9Ahostname)
