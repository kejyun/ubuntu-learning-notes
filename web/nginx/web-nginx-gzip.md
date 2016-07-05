# Nginx Gzip

## 參數說明

| 參數  |  說明 |
|---|---|
| gzip on | on = enable, off = disable  |
| gzip_buffers 16 8k | 壓縮回應內容  |
| gzip_comp_level 6 | 壓縮等級，可設定 1 ~ 9 的整數，數字越大表示壓縮的量越大，也越吃資源，請看個人環境而定  |
| gzip_disable "msie6" | 設定不壓縮的條件，視 Request Header 中的 User-Agent 而定。可以設定 Regular Expression，例如: msie[4-6]  |
| gzip_min_length 1k | 文件內容小於 n 以下不壓縮，這裡的文件內容指的是 Response Header 的 Content-Length，一樣視個人需求而定，沒有標準值  |
| gzip_http_version 1.1 | 設定至少接受的 Http Version  |
| gzip_proxied any | 決定對來自 Proxy 的 Request 如何處理  |
| gzip_types [mime-types] | 指定需要壓縮的 MIME types，或用 * 表示所有 MIME types |
| gzip_vary on | 要不要在 Response Header "Vary: Accept-Encoding" |

### gzip_comp_level - 壓縮等級

#### text/html - phpinfo():

```
0    55.38 KiB (100.00% of original size)
1    11.22 KiB ( 20.26% of original size)
2    10.89 KiB ( 19.66% of original size)
3    10.60 KiB ( 19.14% of original size)
4    10.17 KiB ( 18.36% of original size)
5     9.79 KiB ( 17.68% of original size)
6     9.62 KiB ( 17.37% of original size)
7     9.50 KiB ( 17.15% of original size)
8     9.45 KiB ( 17.06% of original size)
9     9.44 KiB ( 17.05% of original size)
```

#### application/x-javascript - jQuery 1.8.3 (Uncompressed)

```
0    261.46 KiB (100.00% of original size)
1     95.01 KiB ( 36.34% of original size)
2     90.60 KiB ( 34.65% of original size)
3     87.16 KiB ( 33.36% of original size)
4     81.89 KiB ( 31.32% of original size)
5     79.33 KiB ( 30.34% of original size)
6     78.04 KiB ( 29.85% of original size)
7     77.85 KiB ( 29.78% of original size)
8     77.74 KiB ( 29.73% of original size)
9     77.75 KiB ( 29.74% of original size)
```

> 如果有更多的 CPU 資源，可以將壓縮等級設為 9，因為壓縮比例在 1 之後並沒有差異太大，對大部分的網站，gzip 設為 2 應該就足夠了

## Example

```
http {
    gzip on;
    gzip_min_length 1000;
    gzip_types  *;
    gzip_comp_level 2;
    gzip_proxied any;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
}
```


## 參考資料
* [Enable gzip compression with nginx](http://blog.norman-chen.me/post/16)
* [Module ngx_http_gzip_module](http://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_types)
* [What is the best nginx compression gzip level? - Server Fault](http://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level)