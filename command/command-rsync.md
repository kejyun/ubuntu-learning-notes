# Rsync

| 參數 | 說明 |
|---|---|
|  --progress  |  顯示同步進度  |
|  --delete  |  刪除存在於目的端，但不存在於來源端的檔案  |
|  --exclude-from  |  排除指定檔案不要刪除  |
|  -a  |  遞迴備份所有子目錄下的目錄與檔案，保留連結檔、檔案的擁有者、群組、權限以及時間戳記  |
|  -v  |  顯示詳細同步的資訊  |
|  -h  |  數字以比較容易閱讀的格式輸出  |


## --exclude-from

```shell
rsync --delete --exclude-from ./rsyn_exclusion_file
```

## 參考資料
* [Linux 使用 rsync 遠端檔案同步與備份工具教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)
* [rsync參數詳解--20100209 @ 暉獲無度的步烙閣 :: 隨意窩 Xuite日誌](https://blog.xuite.net/jyoutw/xtech/20025390-rsync%E5%8F%83%E6%95%B8%E8%A9%B3%E8%A7%A3--20100209)
