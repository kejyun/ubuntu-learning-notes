# 指令


## 指令結果不要輸出

*>/dev/null 2>&1*

* /dev/null：空文件
* >：代表重新導向到哪里，例如：echo "123" > /home/123.txt
* 1：表示 stdout 標準輸出，系統預設值是1，所以 `>/dev/null` 等同於 `1>/dev/null`
* 2：表示 stderr 標準錯誤
* &：表示等同於的意思，2>&1，表示 2 的輸出重新導向等同於1


```shell
renice -20 `cat /home/yoursrcdspath/srcds.pid` >/dev/null 2>&1
```

## 參考資料
* [Linux Shell 1>/dev/null 2>&1 含义 - CSDN博客](https://blog.csdn.net/ithomer/article/details/9288353)
* [孤島日誌: Unix 重新導向跟 2>&1](http://ibookmen.blogspot.com/2010/11/unix-2.html)
