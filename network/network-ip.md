# 查詢 ip

## 用指令查詢目前主機 ip

執行下列指令，就可以取得目前所在主機的對外 ip

```shell
dig +short myip.opendns.com @resolver1.opendns.com
```

```shell
dig TXT +short o-o.myaddr.l.google.com @ns1.google.com
```

# 參考資料
* [How To Find My Public IP Address From Command Line On a Linux - nixCraft](https://www.cyberciti.biz/faq/how-to-find-my-public-ip-address-from-command-line-on-a-linux/)