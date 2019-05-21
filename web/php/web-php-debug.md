# PHP FPM 除錯


## recv() failed (104: Connection reset by peer) while reading response header from upstream


*Nginx 錯誤 Log*

在 nginx 發現 php-fpm.sock 的錯誤

```
2018/10/24 03:47:05 [error] 1231#1231: *324 recv() failed (104: Connection reset by peer) while reading response header from upstream, client: 172.31.22.62, server: _, request: "POST /post/article HTTP/1.1", upstream: "fastcgi://unix:/run/php/php7.1-fpm.sock:", host: "kejyun.com"
```


*PHP 錯誤 log*

在 php fpm error log 發現 `pm.max_children setting (5), consider raising it` 的錯誤

```
sudo vim /var/log/php7.1-fpm.log
```

```
[24-Oct-2018 01:09:33] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 07:20:16] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 07:43:58] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[24-Oct-2018 08:27:52] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
```


*提高請求中斷時間（request_terminate_timeout）*

```
sudo vim /etc/php/7.1/fpm/pool.d/www.conf
```

```
request_terminate_timeout = 300
```

*提高 Server fastcgi read timeout*

```
vim /etc/nginx/sites-available/kejyun.com
```

```
location ~ \.php$ {
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
}
```

*提高 php fpm max_children*

請參考 [PHP FPM Max Children](./web-php-max-children.md) 這篇文章，根據自己主機的狀況需求去提高可以使用的 php-fpm children 數量，提高存取效率


## 使用 `sudo add-apt-repository ppa:ondrej/php` 發生錯誤


```
$ sudo add-apt-repository ppa:ondrej/php
 Co-installable PHP versions: PHP 5.6, PHP 7.x and most requested extensions are included. Only Supported Versions of PHP (http://php.net/supported-versions.php) for Supported Ubuntu Releases (https://wiki.ubuntu.com/Releases) are provided. Don't ask for end-of-life PHP versions or Ubuntu release, they won't be provided.

Debian oldstable and stable packages are provided as well: https://deb.sury.org/#debian-dpa

You can get more information about the packages at https://deb.sury.org

BUGS&FEATURES: This PPA now has a issue tracker:
https://deb.sury.org/#bug-reporting

CAVEATS:
1. If you are using php-gearman, you need to add ppa:ondrej/pkg-gearman
2. If you are using apache2, you are advised to add ppa:ondrej/apache2
3. If you are using nginx, you are advise to add ppa:ondrej/nginx-mainline
   or ppa:ondrej/nginx

PLEASE READ: If you like my work and want to give me a little motivation, please consider donating regularly: https://donate.sury.org/

WARNING: add-apt-repository is broken with non-UTF-8 locales, see
https://github.com/oerdnj/deb.sury.org/issues/56 for workaround:

# LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
 More info: https://launchpad.net/~ondrej/+archive/ubuntu/php
Press [ENTER] to continue or ctrl-c to cancel adding it

gpg: keyring `/tmp/tmpw76p12wa/secring.gpg' created
gpg: keyring `/tmp/tmpw76p12wa/pubring.gpg' created
gpg: requesting key E5267A6C from hkp server keyserver.ubuntu.com
gpg: /tmp/tmpw76p12wa/trustdb.gpg: trustdb created
gpg: key E5267A6C: public key "Launchpad PPA for Ond\xc5\x99ej Sur�" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
Exception in thread Thread-1:
Traceback (most recent call last):
  File "/usr/lib/python3.4/threading.py", line 920, in _bootstrap_inner
    self.run()
  File "/usr/lib/python3.4/threading.py", line 868, in run
    self._target(*self._args, **self._kwargs)
  File "/usr/lib/python3/dist-packages/softwareproperties/SoftwareProperties.py", line 687, in addkey_func
    func(**kwargs)
  File "/usr/lib/python3/dist-packages/softwareproperties/ppa.py", line 370, in add_key
    return apsk.add_ppa_signing_key()
  File "/usr/lib/python3/dist-packages/softwareproperties/ppa.py", line 261, in add_ppa_signing_key
    tmp_export_keyring, signing_key_fingerprint, tmp_keyring_dir):
  File "/usr/lib/python3/dist-packages/softwareproperties/ppa.py", line 210, in _verify_fingerprint
    got_fingerprints = self._get_fingerprints(keyring, keyring_dir)
  File "/usr/lib/python3/dist-packages/softwareproperties/ppa.py", line 202, in _get_fingerprints
    output = subprocess.check_output(cmd, universal_newlines=True)
  File "/usr/lib/python3.4/subprocess.py", line 609, in check_output
    output, unused_err = process.communicate(inputdata, timeout=timeout)
  File "/usr/lib/python3.4/subprocess.py", line 947, in communicate
    stdout = _eintr_retry_call(self.stdout.read)
  File "/usr/lib/python3.4/subprocess.py", line 491, in _eintr_retry_call
    return func(*args)
  File "/usr/lib/python3.4/encodings/ascii.py", line 26, in decode
    return codecs.ascii_decode(input, self.errors)[0]
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc5 in position 92: ordinal not in range(128)

$
```

由於當系統不是 UTF-8 的 locales，會發生這個錯誤，只要在指令前方加入 locales 指令即可

```
$ sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
```

## `sudo apt-get update` 時出現 `Failed to fetch http://ppa.launchpad.net/ondrej/php5-5.6/ubuntu/dists/trusty/main/binary-amd64/Packages  403  Forbidden` 訊息

```
$ sudo apt-get update

W: Failed to fetch http://ppa.launchpad.net/ondrej/php5-5.6/ubuntu/dists/trusty/main/binary-amd64/Packages  403  Forbidden

E: Some index files failed to download. They have been ignored, or old ones used instead.
```

由於 php 資源庫的位置已更新，可以移除資源庫清單資料即可

```
sudo rm /etc/apt/sources.list.d/ondrej-php5-5_6-trusty.list
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get -y update
```



## 參考資料
* [Increase PHP script execution time with Nginx](https://easyengine.io/tutorials/php/increase-script-execution-time/)
* [php-fpm超時時間設置request_terminate_timeout分析 - 掃文資訊](https://hk.saowen.com/a/00daab29e1dfee5e8354f200fe27332a159c0de9dff687600e312585ef630f7a)
* [php-fpm中的request_terminate_timeout最好不要设置](http://www.onepx.com/php-fpm-request-terminate-timeout.html)
* [解决 recv() failed (104: Connection reset by peer) while reading response header from upstream](http://www.apkfuns.com/-e8-bd-ac-e8-a7-a3-e5-86-b3-recv-failed-104-connection-reset-by-peer-while-reading-response-header-from-upstream/)
* [经验之谈：nginx php 502 bad gateway 解决方法_壹聚教程网](http://www.111cn.net/sys/nginx/56291.htm)
* [解决 添加 ppa:ondrej/php 时出现无法解码的报错 - 墨客](https://mokiee.com/code/150)
* [ubuntu - apt-get install php repository 403 « tedshd's DevNote](http://tedshd.logdown.com/posts/2472689)
* [How To Upgrade to PHP 7 on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-upgrade-to-php-7-on-ubuntu-14-04)
