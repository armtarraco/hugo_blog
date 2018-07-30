+++
title =  "Nginx as Static Web Server"
date = 2018-07-30T18:12:34+02:00
featured_image = ""
description = ""
tags = []
categories = ['nginx', 'devops']
+++


<!-- more -->

Supose the code is hosted in `/var/www/static-page`

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/static-page;
        index index.html;
        server_name <server-ip-addr> <domain>;

        # if it's part of another server and root location is already taken and static-url is the url
        location /static-url/ {
                try_files $uri $uri/ =404;
                rewrite ^/static-url/(.*)$ /$1 break;
        }
}
```
