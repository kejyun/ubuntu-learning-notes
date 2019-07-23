# 群組


## 查詢使用者群組

```shell
groups user1
user1 : <other-groups> www-data
```

```shell
getent group www-data
www-data:x:33:kejyun,group1

getent group www-data  | awk -F: '{print $4}'
kejyun,group1
```

## 將使用者加入群組

### 指令加入群組

```shell
sudo adduser $USER www-data
```

### 編輯群組檔案

```shell
sudo vim /etc/group
```

在每個群組最後面加入相關的使用者，並用逗號隔開

```shell
www-data:x:33:kejyun,group1,group2
```



## 將使用者加入 sudo root

```shell
sudo adduser 帳號名稱 sudo
```


## 變更檔案群組

除了 `root` 使用者外，沒有人可以將擁有者變更為其他使用者，所以若要變更檔案擁有者的話，必須加上 `sudo` 權限

> Non-privileged users (not root) cannot chown files to other user names. To use chown, a user must have the privileges of the target user. In other words, only root can give a file to another user.

```
sudo chown -R root:root ./folder
```

## 參考資料
* [linux - chown - Operation not permitted - Super User](https://superuser.com/questions/697608/chown-operation-not-permitted)
