# 軟體

## add-apt-repository 指令找不到

```shell
sudo: add-apt-repository: command not found
```

在出現上述指令找不到時，可以安裝下列套件，就可以找到這個指令了

```shell
sudo apt-get install software-properties-common
```


## GPG error: https://dl.yarnpkg.com/debian stable InRelease NO_PUBKEY E074D16EB6FF4DE3

在執行 `sudo apt-get update` 時，出現 `NO_PUBKEY` 的錯誤訊息

```
$ sudo apt-get update
Hit:1 https://deb.nodesource.com/node_8.x xenial InRelease
Hit:3 http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu xenial InRelease
Get:4 http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu xenial-updates InRelease [109 kB]
Get:5 http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu xenial-backports InRelease [107 kB]
Err:2 https://dl.yarnpkg.com/debian stable InRelease
  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 23E7166788B63E1E
Get:6 http://ap-northeast-1.ec2.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [957 kB]
Get:7 http://security.ubuntu.com/ubuntu xenial-security InRelease [109 kB]
Hit:8 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial InRelease
Hit:9 http://ppa.launchpad.net/chris-lea/redis-server/ubuntu xenial InRelease
Hit:10 http://ppa.launchpad.net/ondrej/php/ubuntu xenial InRelease
Fetched 1282 kB in 1s (744 kB/s)
Reading package lists... Done
W: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: https://dl.yarnpkg.com/debian stable InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 23E7166788B63E1E
W: Failed to fetch https://dl.yarnpkg.com/debian/dists/stable/InRelease  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 23E7166788B63E1E
W: Some index files failed to download. They have been ignored, or old ones used instead.
```

執行更新 key 即可修復這個問題

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```

## 列出所有已安裝套件

```
sudo apt list --installed
sudo apt list --installed | less
```

## 強制安裝套件，略過 cli 詢問是否安裝

在安裝套件時，常常會遇到 cli 需要詢問是否繼續安裝，像是：

```
$ sudo apt-get install php7.2-xml
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libzip4 php7.2-bcmath php7.2-cli php7.2-common php7.2-curl php7.2-dev php7.2-fpm php7.2-gd php7.2-gmp php7.2-imap php7.2-intl php7.2-json php7.2-ldap php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-pgsql php7.2-readline php7.2-soap php7.2-sqlite3 php7.2-zip
Suggested packages:
  dh-php
The following packages will be upgraded:
  libzip4 php7.2-bcmath php7.2-cli php7.2-common php7.2-curl php7.2-dev php7.2-fpm php7.2-gd php7.2-gmp php7.2-imap php7.2-intl php7.2-json php7.2-ldap php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-pgsql php7.2-readline php7.2-soap php7.2-sqlite3 php7.2-xml
  php7.2-zip
22 upgraded, 0 newly installed, 0 to remove and 405 not upgraded.
Need to get 5,739 kB of archives.
After this operation, 64.5 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```

如果要跳過詢問的話，可以在參數加入 `-y`，這樣就可以跳過被詢問的步驟直接安裝

```
sudo apt-get install -y php7.2-xml
```

> -y, --yes, --assume-yes

> Automatic yes to prompts; assume "yes" as answer to all prompts and
  run non-interactively. If an undesirable situation, such as
  changing a held package, trying to install a unauthenticated
  package or removing an essential package occurs then apt-get will
  abort. Configuration Item: APT::Get::Assume-Yes.




## 參考資料
* [GPG error: https://dl.yarnpkg.com/debian stable InRelease NO_PUBKEY E074D16EB6FF4DE3 · Issue #4453 · yarnpkg/yarn · GitHub](https://github.com/yarnpkg/yarn/issues/4453)
* [How to List Installed Packages on Ubuntu | Linuxize](https://linuxize.com/post/how-to-list-installed-packages-on-ubuntu/)
