# SSH

## 建立 ssh key

```
ssh-keygen -t rsa -b 4096 -C "kj@example.com"
```

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

## 在本機透過 ssh tunnel 直接連線主機

```shell
ssh -t -p <port> -l <account> <ssh_tunnel_server> bash -ic 'ssh_internal_server <ip_address>'
```

```shell
ssh -t -p 22 -l kejyun server.kj.com bash -ic 'ssh_kj 1.1.1.1'
```


## 常見問題

### 在 MacOS Mojave 版本產生的 ssh key 無法使用

Mojave 的 ssh key 產生的 key 是新版本的 `OPENSSH` private key，所以在許多還沒支援新版本的平台使用此金鑰會無法被識別

```
ssh-keygen -t rsa -b 4096 -C "kj@kejyun.com"
```

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEA3Vnc+Hc5835QtKskY/oC9pFrnv6JSnMi7gQHc6aLZOCBE5IKklgq
8P+O8aImqQUEv2GhLIIkMMmRm60U7t78/JiNtLHP07BIqxXslcTaWi9D1bUWcooBgqB/qy
Hvg8J0QAmYBwbgaYzU8vgOckUgxQ/ssiFb1VkCsyuDvjCpjCJ0nwXNPlR2P1iYEuIpen7D
2uS+0iSCRmmJAAAAGXJheS5jaGVuZ0B0aGVuZXdzbGVucy5jb20B
-----END OPENSSH PRIVATE KEY-----
```

為了解決這個問題可以加入 `-m PEM` 參數，產生 `PEM` 格式的 key

```
ssh-keygen -m PEM -t rsa -b 4096 -C "kj@kejyun.com"
```

```
-----BEGIN RSA PRIVATE KEY-----
MIIJKQIBAAKCAgEAr+QHvQMVt5i2ZzkHMxl/PRXxkFD2rHdLx1RT+9E9Hie/FeO3
5DJMU+ZKZiPCK3ya/hDj8xJukL7jLfjhmGNKFDnkCGTzyhU5jWlORDEez72NKHGP
eAt4eO6w6MpvRyhg7FZakKsTvLbGu7P8ceBA2yy6LHWUUDWuXPx4ZvarSr9q5Vkf
qgGfICykPJEa0g68kyFJORLfaHrqgsrzmuHlJwokP8Jetw53MF+dciA/nE42tW4M
rsiU0zzbEUYpS15uuR31QZjeNzGduwOU+gPBuhTdB3IaXyXD7KfSs6vXsaIB
-----END RSA PRIVATE KEY-----
```


## 執行遠端 ssh 登入並執行遠端主機指令

在連線主機後若要立即在遠端主機執行指令的話，可以在 `ssh 指令` 後面用單引號（`'`）或雙引號（`"`）將要執行的指令包起來，這樣在 ssh 連線後就會立即在遠端主機執行該指令


```shell
ssh kejyun@127.0.0.1 '<remote_scripts>'
ssh kejyun@127.0.0.1 "<remote_scripts>"
ssh kejyun@127.0.0.1 '/home/kejyun/shell.sh'
ssh kejyun@127.0.0.1 "sudo /home/kejyun/shell.sh argument1 argument2"
```


## 使用 ssh 複製遠端檔案(scp)


**使用 ssh**

```
scp <your_username>@<your-server-domain>:/remote/dir/file.txt /local/dir
```

**使用金鑰**

```
scp -i <your-key> <your_username>@<your-server-domain>:/remote/dir/file.txt /local/dir
```

```
scp -i ~/.ssh/id_rsa kejyun@127.0.0.1:/home/remote-kejyun/myfile /home/kejyun
```

**複製目錄**

複製目錄加一個 `-r` 的參數即可

```
scp -r -i <your-key> <your_username>@<your-server-domain>:/remote/dir/path /local/dir
```



## ssh config

*User*

> 登入帳號


*PreferredAuthentications*

> 偏好授權方式

 - publickey
 - password


 *PasswordAuthentication*

 > 是否使用密碼驗證，預設 yes

 - yes
 - no


 *PubkeyAuthentication*

 > 是否使用 public key 驗證，預設 yes

 - yes
 - no

 *IdentitiesOnly*

 > 是否只使用 command line 或 config 的 ssh key，預設 no，所以會去嘗試抓其他不同的 key，像是 `ssh-agent` 的 key

 - yes
 - no


## 測試 ssh 連線

```
ssh -T git@github.com
ssh -T git@bitbucket.org
```




## 參考資料
* [Generating a new SSH key and adding it to the ssh-agent - User Documentation](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
* [ssh - How to check sshd log? - Server Fault](https://serverfault.com/questions/130482/how-to-check-sshd-log)
* [ssh免密码登陆设置时Authentication refused: bad ownership or modes错误解决方法 - 博学无忧](https://www.bo56.com/ssh%E5%85%8D%E5%AF%86%E7%A0%81%E7%99%BB%E9%99%86%E8%AE%BE%E7%BD%AE%E6%97%B6authentication-refused-bad-ownership-or-modes%E9%94%99%E8%AF%AF%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)
* [openssl - ssh-keygen does not create RSA private key - Server Fault](https://serverfault.com/questions/939909/ssh-keygen-does-not-create-rsa-private-key)
* [Run / Execute Command Using SSH - nixCraft](https://www.cyberciti.biz/faq/unix-linux-execute-command-using-ssh/)
* [linux - How to download a file from server using SSH? - Stack Overflow](https://stackoverflow.com/questions/9427553/how-to-download-a-file-from-server-using-ssh)
* [Use multiple ssh-keys for different GitHub accounts on the same computer](https://medium.com/@xiaolishen/use-multiple-ssh-keys-for-different-github-accounts-on-the-same-computer-7d7103ca8693)
* [How to manage multiple SSH key pairs | Enable Sysadmin](https://www.redhat.com/sysadmin/manage-multiple-ssh-key-pairs)
* [OpenSSH Config File Examples - nixCraft](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/)
* [如何用config管理多個網站的ssh key和如何不用每一組輸入ssh的Pass Phrase @ Alan Tsai 的學習筆記｜An Asp .Net Mvc Web Developer Blog](https://blog.alantsai.net/posts/2016/03/ssh-config-ssh-agent-passphrase-management)
