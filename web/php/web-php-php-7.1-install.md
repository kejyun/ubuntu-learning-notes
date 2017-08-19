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

## 參考資料
* [How to upgrade to PHP 7.1 on Ubuntu | Ayesh Karunaratne](https://ayesh.me/Ubuntu-PHP-7.1)
* [How to Install and Configure PHP 7.0 or PHP 7.1 on Ubuntu 16.04 - Vultr.com](https://www.vultr.com/docs/how-to-install-and-configure-php-70-or-php-71-on-ubuntu-16-04)
