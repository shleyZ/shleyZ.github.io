---
title: nginx 启用websocket连接
date: 2019-09-02 14:11:22
categories: Linux
tags: nginx
---
## nginx配置ws代理
```js
server {
  listen 8888;
  server_name XXXXXXXX;

  location /api {
      proxy_pass http://XXX.XXX.XX.XX:XXXX/;
  }
  // 启用支持websocket连接
  location /ws {
      proxy_pass http://XXX.XXX.XX.XX:XXXX/;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
  }
  location / {
      root   /root/www/XXX;
      index  index.html index.htm;
      try_files $uri $uri/ /index.html;
  }
}
```

## 本地webpack配置ws代理:

```js
"/ws": {
    changeOrigin: true,
    ws: true, 
    pathRewrite: {
        "/ws": "/"
    }
},
```