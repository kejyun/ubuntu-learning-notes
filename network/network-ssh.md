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
ssh-keygen
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
