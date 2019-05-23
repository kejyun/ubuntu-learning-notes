# 安裝 PHP 7.1

## 加入 ondrej/php

```shell
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

如果指令無法執行，請先安裝下列套件

```shell
sudo apt-get install software-properties-common python-software-properties
```

## 列出 PHP 套件

```shell
apt-cache pkgnames | grep php7.1
```

## 安裝 php

```shell
sudo apt-get install php7.1 php7.1-common
```

```shell
sudo apt-get install php7.1-curl php7.1-xml php7.1-zip php7.1-gd php7.1-mysql php7.1-mbstring
```

## 測試 PHP

```shell
php -v
PHP 7.1.8-2+ubuntu16.04.1+deb.sury.org+4 (cli) (built: Aug  4 2017 13:04:12) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.1.8-2+ubuntu16.04.1+deb.sury.org+4, Copyright (c) 1999-2017, by Zend Technologies
```

## 在 ubuntu 安裝並執行不同 php 版本

## 安裝其他 php 版本

```
------------ Install PHP Modules ------------
$ sudo apt install php5.6-cli php5.6-xml php5.6-mysql
$ sudo apt install php7.0-cli php7.0-xml php7.0-mysql
$ sudo apt install php7.1-cli php7.1-xml php7.1-mysql
$ sudo apt install php7.2-cli php7.2-xml php7.2-mysql
$ sudo apt install php7.3-cli php7.3-xml php7.3-mysql  
```

## 執行指定 php 版本

```
/usr/bin/php -v
/usr/bin/php7.0 -v
/usr/bin/php7.1 -v
```

### 變更預設 php 執行路徑

```
------------ For PHP 5.6 ------------
$ sudo update-alternatives --set php /usr/bin/php5.6
$ php -i | grep "Loaded Configuration File"

------------ For PHP 7.0 ------------
$ sudo update-alternatives --set php /usr/bin/php7.0
$ php -i | grep "Loaded Configuration File"

------------ For PHP 7.1 ------------
$ sudo update-alternatives --set php /usr/bin/php7.1
$ php -i | grep "Loaded Configuration File"

------------ For PHP 7.2 ------------
$ sudo update-alternatives --set php /usr/bin/php7.2
$ php -i | grep "Loaded Configuration File"

------------ For PHP 7.3 ------------
$ sudo update-alternatives --set php /usr/bin/php7.3
$ php -i | grep "Loaded Configuration File"

```

## 參考資料
* [How to upgrade to PHP 7.1 on Ubuntu | Ayesh Karunaratne](https://ayesh.me/Ubuntu-PHP-7.1)
* [How to Install and Configure PHP 7.0 or PHP 7.1 on Ubuntu 16.04 - Vultr.com](https://www.vultr.com/docs/how-to-install-and-configure-php-70-or-php-71-on-ubuntu-16-04)
* [How to Install Different PHP (5.6, 7.0 and 7.1) Versions in Ubuntu](https://www.tecmint.com/install-different-php-versions-in-ubuntu/)
