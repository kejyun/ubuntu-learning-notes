# 群組


## 查詢使用者群組

```sh
groups user1
user1 : <other-groups> www-data
```

```sh
getent group www-data
www-data:x:33:kejyun,group1

getent group www-data  | awk -F: '{print $4}'
kejyun,group1
```

## 將使用者加入群組

```sh
sudo vim /etc/group
```

在每個群組最後面加入相關的使用者，並用逗號隔開

```sh
www-data:x:33:kejyun,group1,group2
```
