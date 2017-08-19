# SSH

## 變更 ssh port

開啟設定檔案

```shell
sudo vim /etc/ssh/sshd_config
```

找尋 Port 並變更

```shell
# What ports, IPs and protocols we listen for
Port 22
```

重新啟動 ssh 服務即可

```shell
sudo service ssh restart
```