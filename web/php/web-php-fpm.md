# PHP FPM

## 檢測


```
/etc/init.d/php7.0-fpm status
/etc/init.d/php5-fpm status

* php-fpm7.0 is running
```

```
ps aux | grep php-fpm
root      1030  0.0  0.6 326692 23604 ?        Ss    5月03   0:03 php-fpm: master process (/etc/php/7.0/fpm/php-fpm.conf)
www-data 13671 17.9  0.8 337704 32332 ?        S    07:13   0:06 php-fpm: pool www
www-data 13672 16.5  0.7 335796 29880 ?        S    07:13   0:05 php-fpm: pool www
www-data 13692 18.8  0.6 333608 26172 ?        S    07:14   0:01 php-fpm: pool www
www-data 13695 40.0  0.7 335568 27848 ?        S    07:14   0:00 php-fpm: pool www
ubuntu   13697  0.0  0.0  12968   956 pts/0    S+   07:14   0:00 grep --color=auto php-fpm
```

```
netstat -an | grep :9000

nmap localhost -p 9000
```


## 參考資料
* [PHP FPM - check if running - Stack Overflow](http://stackoverflow.com/questions/14915147/php-fpm-check-if-running)