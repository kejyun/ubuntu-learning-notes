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

## 是否允許密碼登入

```shell
# /etc/ssh/sshd_config
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes
```

## 僅允許使用 key 登入

為了安全性，避免主機被使用帳號密碼 try，可以僅使用 key 登入，將密碼 ssh 登入方式關閉

```shell
# /etc/ssh/sshd_config
PasswordAuthentication no
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile      %h/.ssh/authorized_keys
```

**在自己電腦產生 key**

```shell
ssh-keygen -t rsa -b 4096 -C "kj@kejyun.com"
```

* id_rsa.pub：公開金鑰（public key），放在 Ubuntu 主機
* id_rsa：私密金鑰（private key），不可外洩的個人金鑰


**將 id_rsa.pub 公開金鑰放在 ~/.ssh/authorized_keys***

```shell
~$ mkdir .ssh
~$ cd .ssh
~/.ssh$ vim authorized_keys
```

**重新啟動 ssh 服務**

```shell
service ssh restart
```

**使用 key 登入**

```shell
ssh -p <ssh_port> -i /Users/kejyun/.ssh/id_rsa -l <account> <ip_address>
ssh -p 22 -i /Users/kejyun/.ssh/id_rsa -l kj 127.0.0.1
```

## SSH Log

SSH Log 檔案放在 `/var/log/auth.log`

```shell
grep 'sshd' /var/log/auth.log

tail -f /var/log/auth.log
```


## Authentication refused: bad ownership or modes for directory

ssh 登入為了安全性，對於 `.ssh` 目錄的讀寫權限有限制，需要為 `775` 或 `700`

`id_rsa.pub` 與 `authorized_keys` 權限為 `644`

`id_rsa` 權限為 `600`


## 指定 key 及帳號

在做 ssh 連線 alias 指令時，想要直接接著需要連線的 IP，那麼就必須要把參數先指定好，後面才可以直接接著 IP

```shell
ssh -p <port> -i <ssh_key_path> -l <account>
```

* -p：port
* -i：ssh 金鑰路徑
* -l：帳號名稱

```shell
ssh -p 22 -i ~/.ssh/id_rsa -l kejyun
```

**使用 shell function 做 ssh 登入**

```shell
f() { ssh <account>@$1 -i "<ssh_key_path>"; }; f
```

```shell
f() { ssh kejyun@$1 -i "~/.ssh/id_rsa"; }; f
```

## 參考資料
* [Generating a new SSH key and adding it to the ssh-agent - User Documentation](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
* [ssh - How to check sshd log? - Server Fault](https://serverfault.com/questions/130482/how-to-check-sshd-log)
* [ssh免密码登陆设置时Authentication refused: bad ownership or modes错误解决方法 - 博学无忧](https://www.bo56.com/ssh%E5%85%8D%E5%AF%86%E7%A0%81%E7%99%BB%E9%99%86%E8%AE%BE%E7%BD%AE%E6%97%B6authentication-refused-bad-ownership-or-modes%E9%94%99%E8%AF%AF%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)
