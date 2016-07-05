# Nginx Cache

## site conf 設定

```
http {
    # 靜態檔案快取規則

    # 快取設定及 HTML 檔案及資料
    location ~* \.(?:manifest|appcache|html?|xml|json)$ {
      expires -1;
      # access_log logs/static.log; # I don't usually include a static log
    }

    # Feed
    location ~* \.(?:rss|atom)$ {
      expires 1h;
      add_header Cache-Control "public";
    }

    # 媒體：圖片、圖示、影片、音樂
    location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
      expires 1M;
      access_log off;
      add_header Cache-Control "public";
    }

    # CSS 及 Javascript
    location ~* \.(?:css|js)$ {
      expires 1y;
      access_log off;
      add_header Cache-Control "public";
    }
}
```

## 參考資料

* [Nginx Caching - Servers for Hackers](https://serversforhackers.com/nginx-caching)