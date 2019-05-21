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


## 參考資料
* [GPG error: https://dl.yarnpkg.com/debian stable InRelease NO_PUBKEY E074D16EB6FF4DE3 · Issue #4453 · yarnpkg/yarn · GitHub](https://github.com/yarnpkg/yarn/issues/4453)
