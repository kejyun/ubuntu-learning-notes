# 語系

## 解決語系 `perl: warning: Falling back to the standard locale ("C").` 錯誤訊息

```shell
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_CTYPE = "UTF-8",
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
```

**Solution**

在命令列輸入下列指令即可

```shell
export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
locale-gen en_US.UTF-8
sudo dpkg-reconfigure locales
```


## 參考資料
* [solve perl: warning: Setting locale failed. · GitHub](https://gist.github.com/panchicore/1269109)
