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



## 將使用者加入 sudo

```shell
sudo adduser 帳號名稱 sudo
```
