# Nginx Log

## Access Log

### 依照日期儲存

`$time_iso8601` 格式

```
2014-05-04T18:12:02+02:00
```

#### 擷取時間 YYYY-MM-DD

```
if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})") {
    set $year $1;
    set $month $2;
    set $day $3;
}

access_log /var/log/nginx/$year-$month-$day-access.log;
```

#### 使用 Perl 擷取時間

```
if ($time_iso8601 ~ "^(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})") {}

access_log /var/log/nginx/$year-$month-$day-access.log;
```

#### 擷取 YYYY-MM-DD HH:II:SS

```
if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2}):(\d{2})") {
    set $year $1;
    set $month $2;
    set $day $3;
    set $hour $4;
    set $minutes $5;
    set $seconds $6;
}
```


## Log 路徑

設定檔：

> /etc/nginx/nginx.conf

```
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```

## 參考資料
* [Log rotation directly within Nginx configuration file - Cambus.net](http://www.cambus.net/log-rotation-directly-within-nginx-configuration-file/)